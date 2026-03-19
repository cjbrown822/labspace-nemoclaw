# Step 3 - Configure Nemotron Inference

## How Inference Works in OpenShell

OpenShell's **Privacy Router** intercepts all LLM API calls from inside the
sandbox. When an agent calls `http://inference.local`, the Privacy Router:

1. Strips the caller's credentials
2. Injects the backend credentials (your NVIDIA NIM key)
3. Forwards the request to the configured model endpoint
4. Returns the response to the agent

This means your API keys are **never exposed inside the sandbox**.

## Configure the Inference Route

Set the gateway-level inference provider to use NVIDIA NIM with Nemotron-3-Nano:

```bash
openshell inference set \
  --provider nvidia-nim \
  --model nvidia/nemotron-3-nano-30b-a3b
```

You should see:

```
Gateway inference configured:

  Route: inference.local
  Provider: nvidia-nim
  Model: nvidia/nemotron-3-nano-30b-a3b
  Version: 1
  Validated Endpoints:
    - https://integrate.api.nvidia.com/v1/chat/completions
```

The **Validated Endpoints** line confirms your API key is working correctly.

## About Nemotron-3-Nano

The model we're using has some impressive capabilities:

| Property | Value |
|----------|-------|
| Parameters | 31.6B (MoE architecture) |
| Context length | 1M tokens |
| Quantization | Q4_K_M |
| Capabilities | completion, tools, **thinking** |

The **thinking** capability means Nemotron reasons through problems before
responding — you'll see `<think>` blocks in the output.

## Verify Inference Configuration

```bash
openshell inference get
```

This shows the currently configured inference route for your gateway.

> **Tip:** You can hot-reload the inference config at any time with
> `openshell inference set` — no sandbox restart needed!
