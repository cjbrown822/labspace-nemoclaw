# Step 4 - Apply Network Policy

## Default Deny

OpenShell sandboxes start with **zero outbound access**. Every network request
from inside the sandbox goes through the egress proxy at `10.200.0.1:3128`.
Without a policy, everything returns `403 Forbidden`.

This is OpenShell's security model in action — explicit allow, implicit deny.

## Verify the Default Deny

Connect to your sandbox and try a request:

```bash
openshell sandbox connect <your-sandbox-name>
```

Inside the sandbox:

```bash
curl -v http://inference.local/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"nvidia/nemotron-3-nano-30b-a3b","messages":[{"role":"user","content":"hello"}]}'
```

You'll see `HTTP/1.1 403 Forbidden` — this is correct and expected!

Exit the sandbox:

```bash
exit
```

## Monitor Policy Decisions with the TUI

In a second terminal, launch the OpenShell terminal UI:

```bash
openshell term
```

This gives you a live k9s-style dashboard showing:
- Gateway health
- Sandbox status
- Live policy decisions with `l7_decision=allow` or `l7_decision=deny`

## Write the Policy File

Create a policy YAML that allows inference traffic:

```bash
cat > /tmp/nemoclaw-policy.yaml << 'EOF'
version: 1

filesystem_policy:
  include_workdir: true
  read_only: [/usr, /lib, /proc, /dev/urandom, /app, /etc, /var/log]
  read_write: [/sandbox, /tmp, /dev/null]
landlock:
  compatibility: best_effort
process:
  run_as_user: sandbox
  run_as_group: sandbox

network_policies:
  inference_local:
    name: inference-local
    endpoints:
      - host: inference.local
        port: 443
        protocol: rest
        tls: terminate
        enforcement: enforce
        access: read-write
    binaries:
      - { path: /usr/bin/curl }
EOF
```

## Apply the Policy

```bash
openshell policy set <your-sandbox-name> \
  --policy /tmp/nemoclaw-policy.yaml \
  --wait
```

The `--wait` flag blocks until the policy is fully activated. You'll see:

```
✓ Policy version 2 submitted (hash: xxxxxxxx)
```

## Verify Policy Status

```bash
openshell policy get <your-sandbox-name>
```

You should see `Status: Active`.

> **Key insight:** The `network_policies` section is **hot-reloadable** — you can
> update it on a running sandbox without restarting. The `filesystem_policy`,
> `landlock`, and `process` sections are locked at sandbox creation.
