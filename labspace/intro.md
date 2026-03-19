# Running NVIDIA NemoClaw on OpenShell with Nemotron Inference

## Welcome!

In this lab you will set up **NVIDIA NemoClaw** on Linux and connect it to
**Nemotron-3-Super-120B** inference via NVIDIA NIM.

## What You'll Build
```
AI Agent (OpenClaw)
    ↓
OpenShell Sandbox (policy-enforced egress)
    ↓ https://inference.local
Privacy Router (credential stripping)
    ↓
NVIDIA NIM Cloud API → Nemotron-3-Super-120B
```

## What You'll Learn

- Install OpenShell and NemoClaw on Linux
- Create a gateway and sandbox with `nemoclaw onboard`
- Configure NVIDIA NIM inference
- Apply and hot-reload YAML network policies
- Test Nemotron inference from inside the sandbox

## Prerequisites

- A free NVIDIA API key from [build.nvidia.com](https://build.nvidia.com)
- Basic familiarity with the command line

> **Note:** Use `https://inference.local` (not `http://`) — the proxy uses TLS on port 443.

## Time to Complete

~30 minutes
