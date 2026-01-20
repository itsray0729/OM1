# Windows Installation Guide for OM1

Complete guide for installing and running OM1 on Windows using WSL2.

## Prerequisites

- Windows 10/11 (64-bit)
- Administrator access
- Stable internet connection

## Installation Steps

### 1. Install WSL2 and Ubuntu 22.04

Open PowerShell as Administrator and run:
```powershell
wsl --install -d Ubuntu-22.04
```

**Important:** Restart your computer after installation.

After restart, Ubuntu will open automatically. Set up:
- Username (e.g., `user`)
- Password (typing won't show on screen - this is normal)

### 2. Update System and Install Dependencies

In Ubuntu terminal:
```bash
apt update && apt install -y git curl python3-pip ffmpeg portaudio19-dev
```

### 3. Install uv Package Manager
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"
```

### 4. Clone OM1 Repository
```bash
cd ~
git clone https://github.com/OpenMind/OM1.git
cd OM1
git submodule update --init
```

### 5. Create Virtual Environment and Install Dependencies
```bash
uv venv
source .venv/bin/activate
uv pip install -e .
```

This step takes 10-20 minutes as it downloads large packages like TensorFlow and PyTorch.

### 6. Configure API Key

Get your API key from https://portal.openmind.org/
```bash
cp env.example .env
nano .env
```

- Navigate to `OM_API_KEY=` line
- Add your API key after the equals sign
- Save: Ctrl+X, then Y, then Enter

### 7. Run OM1
```bash
python src/run.py spot
```

Open http://localhost:8000 in your browser to access WebSim interface.

## Restarting OM1

To run OM1 again after closing:

1. Open PowerShell
2. Run: `wsl -d Ubuntu-22.04`
3. Then:
```bash
cd ~/OM1
source .venv/bin/activate
python src/run.py spot
```

## Common Issues

### Cannot paste in Ubuntu terminal
- **Solution:** Right-click to paste, or use Ctrl+Shift+V

### Network interface errors (en0 not found)
- **Solution:** Edit config files to use `eth0` instead of `en0`
- Find your interface: `ip link show`

### Missing dependencies errors
- **Solution:** Reinstall with `uv pip install -e . --force-reinstall`

### Device shows offline in Portal
- **Note:** This is normal without physical robot hardware
- OM1 functions fully in local mode

## Stopping OM1

Press `Ctrl+C` in the Ubuntu terminal.

## System Requirements

- **Minimum:** 8GB RAM, 20GB free disk space
- **Recommended:** 16GB RAM, 50GB free disk space

## Support

- Discord: https://discord.gg/openmind
- Documentation: https://docs.openmind.org/
- Issues: https://github.com/OpenMind/OM1/issues
