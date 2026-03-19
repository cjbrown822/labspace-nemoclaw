# Congratulations! 🎉

You've successfully set up **NVIDIA NemoClaw on OpenShell** with **Nemotron-3-Nano**
inference on Linux!

## What You Accomplished

✅ Installed OpenShell and NemoClaw on Linux  
✅ Created a Gateway (K3s cluster inside Docker)  
✅ Configured NVIDIA NIM as an inference provider  
✅ Applied declarative YAML network policies  
✅ Tested Nemotron-3-Nano inference via `inference.local`  
✅ Ran Codex agent inside a policy-governed sandbox  
✅ Hot-reloaded policies without restarting the sandbox  

## The Stack You Built

```
Codex Agent
    ↓
OpenShell Sandbox
    ↓  (policy-enforced egress via 10.200.0.1:3128)
Policy Engine  ←→  Privacy Router
    ↓                    ↓
GitHub API        NVIDIA NIM Cloud
(read-only)       Nemotron-3-Nano 30B
```

## Key Concepts to Remember

| Concept | Description |
|---------|-------------|
| **Default deny** | All outbound traffic blocked until explicitly allowed |
| **Hot-reload** | Network + inference policies update live, no restart |
| **Privacy Router** | Strips agent credentials, injects backend keys |
| **inference.local** | Magic hostname that routes to your configured LLM |
| **Locked at creation** | Filesystem + process policies immutable after start |

## What's Next?

- **Add more agents:** Try Claude Code (`openshell sandbox create -- claude`)
- **GPU sandbox:** Add `--gpu` flag for local inference with NVIDIA GPU
- **Community sandboxes:** Explore `openshell sandbox create --from openclaw`
- **Policy generator:** Use the built-in skill to generate policies from plain English
- **Multi-sandbox:** Run multiple sandboxes with different policies simultaneously

## Resources

- 📚 [OpenShell Docs](https://docs.nvidia.com/openshell/latest/)
- 🐙 [OpenShell GitHub](https://github.com/NVIDIA/OpenShell)
- 🦀 [NemoClaw GitHub](https://github.com/NVIDIA/NemoClaw)
- 🤖 [NVIDIA NIM Models](https://build.nvidia.com)
- 🌐 [Collabnix Community](https://collabnix.com)

## macOS Users

Running on Apple Silicon? Check out:
- [NemoClaw macOS tracking issue #260](https://github.com/NVIDIA/NemoClaw/issues/260)
- [Ajeet Raina's Apple Silicon walkthrough](https://www.ajeetraina.com/can-i-run-nvidia-nemoclaw-on-apple-silicon/)

---

*Built with ❤️ by the [Collabnix](https://collabnix.com) community*
