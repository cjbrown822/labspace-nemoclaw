# Step 1 - Install OpenShell and NemoClaw

## Install OpenShell

OpenShell is the safe, private runtime for autonomous AI agents. Install it
using the official install script:

```bash
curl -LsSf https://raw.githubusercontent.com/NVIDIA/OpenShell/main/install.sh | sh
```

Reload your shell to pick up the new PATH:

```bash
source ~/.bashrc
export PATH="/home/coder/.local/bin:$PATH"
```

Verify the installation:

```bash
openshell --version
```

## Install NemoClaw

NemoClaw is NVIDIA's agent plugin for OpenShell that adds Nemotron model
support and the NemoClaw agent harness:

```bash
# Fix npm global prefix BEFORE running NemoClaw install
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
export PATH="~/.npm-global/bin:$PATH"

# Then install NemoClaw
curl -LsSf https://raw.githubusercontent.com/NVIDIA/NemoClaw/main/install.sh | bash
```

Reload your shell again:

```bash
source ~/.bashrc
```

Verify NemoClaw:

```bash
nemoclaw --version
```

