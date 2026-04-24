# Step 6 - Test Nemotron Inference

## Connect to the Sandbox

Before running any inference commands, you must be **inside** the sandbox —
`inference.local` is a virtual hostname that only resolves within the
sandbox network namespace.

```bash
openshell sandbox connect collabnix
```

Your prompt should change to `sandbox@collabnix:~$`. If it still shows your
host shell (e.g. `coder@...`), the next commands will not work.

## Test Basic Inference

Send a request to `inference.local` — the Privacy Router will intercept it,
inject your NVIDIA NIM credentials, and forward to Nemotron:

```bash
curl http://inference.local/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3-nano-30b-a3b",
    "messages": [
      {"role": "user", "content": "Hello! Who are you and what can you do?"}
    ]
  }'
```

You should get a JSON response with Nemotron's reply including a
`reasoning_content` field (Nemotron 3's exposed chain of thought).

> **Note on the response `model` field:** You may see the response come back
> from `nvidia/nemotron-3-super-120b-a12b` even though you requested
> `nemotron-3-nano-30b-a3b`. This is the Privacy Router honouring the
> **operator-configured** model (set via `openshell inference set`) rather
> than the caller's requested model. The agent asks; the operator decides.

## Test with Streaming

```bash
curl http://inference.local/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3-nano-30b-a3b",
    "stream": true,
    "messages": [
      {"role": "user", "content": "Explain what OpenShell does in 3 sentences."}
    ]
  }'
```

## Test Tool Use

Nemotron supports function calling. Try this:

```bash
curl http://inference.local/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3-nano-30b-a3b",
    "messages": [
      {"role": "user", "content": "What is 42 * 137?"}
    ],
    "tools": [
      {
        "type": "function",
        "function": {
          "name": "calculator",
          "description": "Perform arithmetic",
          "parameters": {
            "type": "object",
            "properties": {
              "expression": {"type": "string"}
            }
          }
        }
      }
    ]
  }'
```

## List Available Models

```bash
curl http://inference.local/v1/models
```

## Verify Privacy Router is Working

Notice that your request to `inference.local` never included an NVIDIA API key
— the Privacy Router injected it automatically at the proxy layer. The key
never entered the sandbox filesystem.

```bash
# This works — Privacy Router adds the key
curl http://inference.local/v1/models
```

> **What just happened?**
>
> `Sandbox → Egress Proxy → Policy Engine (allow) → Privacy Router (inject key) → NVIDIA NIM → Nemotron → back to you`
>
> Your API key never touched the sandbox filesystem. 🔐

## Exit the Sandbox

```bash
exit
```
