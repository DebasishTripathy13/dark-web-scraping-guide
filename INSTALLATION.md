# Installation Guide - Robin Dark Web Tool

Complete step-by-step installation instructions for Linux, macOS, and Windows (via WSL).

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Platform-Specific Setup](#platform-specific-setup)
3. [Install Tor](#install-tor)
4. [Clone Robin Repository](#clone-robin-repository)
5. [Configure API Keys](#configure-api-keys)
6. [Build Docker Container](#build-docker-container)
7. [Run Robin](#run-robin)
8. [Access Web Interface](#access-web-interface)
9. [Verification](#verification)

---

## Prerequisites

Before you begin, ensure you have:

### Required Software

1. **Docker** - Container platform
   - [Install Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows/Mac)
   - [Install Docker Engine](https://docs.docker.com/engine/install/) (Linux)

2. **Git** - Version control system
   - Usually pre-installed on Linux/Mac
   - [Download Git for Windows](https://git-scm.com/download/win)

3. **Terminal Access**
   - Linux: Built-in terminal
   - macOS: Terminal app or iTerm2
   - Windows: WSL (Windows Subsystem for Linux) - [Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install)

### Required API Access

You need an API key for **one** of these AI providers:

- **OpenAI** (ChatGPT) - [Get API Key](https://platform.openai.com/api-keys)
- **Anthropic** (Claude) - [Get API Key](https://console.anthropic.com/)
- **Ollama** (Local models) - [Install Ollama](https://ollama.ai/)

### Recommended

- **VPN Service** - For privacy protection (use before connecting to Tor)

---

## Platform-Specific Setup

### Linux/macOS Users

You can proceed directly to [Install Tor](#install-tor).

### Windows Users

Windows users must install WSL (Windows Subsystem for Linux) first:

1. Open PowerShell as Administrator
2. Run:
   ```powershell
   wsl --install
   ```
3. Restart your computer
4. Open Ubuntu (or your chosen Linux distribution)
5. Complete the initial setup (create username/password)
6. Continue with the installation steps below inside WSL

For detailed WSL setup, see [NetworkChuck's WSL Video](https://www.youtube.com/watch?v=VIDEO_ID).

---

## Install Tor

Tor (The Onion Router) is required to access dark web sites.

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install tor
```

### macOS

Using Homebrew (install [Homebrew](https://brew.sh/) first if needed):

```bash
brew install tor
```

### Verify Tor Installation

```bash
tor --version
```

You should see output showing the Tor version number.

---

## Clone Robin Repository

Clone the Robin repository to your local machine:

```bash
git clone https://github.com/APURV-USERNAME/robin.git
cd robin
```

Replace `APURV-USERNAME` with the actual GitHub username.

---

## Configure API Keys

Robin needs an API key to use AI for semantic filtering and analysis.

### Step 1: Copy Example Environment File

Robin includes an example configuration file:

```bash
cp .env.example .env
```

### Step 2: View the Template

Check what's inside:

```bash
cat .env.example
```

You'll see something like:

```
ANTHROPIC_API_KEY=
OPENAI_API_KEY=
GOOGLE_API_KEY=
OLLAMA_BASE_URL=
```

### Step 3: Edit the .env File

Open the `.env` file with a text editor:

```bash
nano .env
```

### Step 4: Add Your API Key

Add your API key for **ONE** of the following:

#### Option A: OpenAI (ChatGPT)

```
OPENAI_API_KEY=sk-your-openai-api-key-here
```

#### Option B: Anthropic (Claude)

```
ANTHROPIC_API_KEY=sk-ant-your-anthropic-api-key-here
```

#### Option C: Ollama (Local Model)

If using Ollama with a local model like Llama 3.1:

```
OLLAMA_BASE_URL=http://localhost:11434
```

**Note:** You must have Ollama installed and running with a model pulled:
```bash
# Install Ollama first from https://ollama.ai/
ollama pull llama3.1
```

### Step 5: Save the File

In nano:
- Press `Ctrl + X`
- Press `Y` to confirm
- Press `Enter` to save

**Security Note:** Never commit your `.env` file to Git or share it publicly. Your API keys should remain private.

---

## Build Docker Container

Build the Robin Docker container from the Dockerfile:

```bash
docker build -t robin .
```

**What this does:**
- `-t robin` - Names the container "robin"
- `.` - Uses the current directory's Dockerfile

This process may take several minutes as Docker downloads and installs all dependencies, including:
- Python packages
- Beautiful Soup (web scraping library)
- Tor integration tools
- AI/LLM libraries

You'll see lots of output as packages install. Get your coffee ready!

---

## Run Robin

Start the Robin Docker container:

```bash
docker-compose up -d
```

Or use the docker run command if docker-compose isn't available:

```bash
docker run -d \
  --name robin \
  -p 8501:8501 \
  -v $(pwd):/app \
  --env-file .env \
  robin
```

**What happens next:**

1. Docker starts the Robin container
2. Robin connects to the Tor network
3. Web interface starts (usually on port 8501 or 8000)

**Important:** Wait 30-60 seconds for Tor to fully connect before accessing the web interface. Tor connections can be slow to establish.

You'll see output like:

```
Connecting to Tor network...
Tor connection established
Streamlit app starting on port 8501...
```

---

## Access Web Interface

Once Robin is running and connected to Tor:

1. Open your web browser
2. Navigate to one of:
   - `http://localhost:8501`
   - `http://localhost:8000`

**Which port?** Check the docker-compose output or Robin's documentation. The default is usually 8501 for Streamlit applications.

You should see:
- **Left sidebar**: LLM selection (OpenAI, Anthropic, Ollama)
- **Center area**: Search box
- **Top**: Status indicators

---

## Verification

### Test 1: Check Docker Container Status

```bash
docker ps
```

You should see the Robin container running.

### Test 2: Check Tor Connection

The web interface should show a Tor connection status. Wait until it shows "Connected" or similar.

**Note from Chuck's experience:** Sometimes the web app starts before Tor fully connects. If searches fail immediately, wait 30-60 seconds and try again.

### Test 3: Run a Simple Search

1. Select your LLM provider (OpenAI, Anthropic, or Ollama)
2. Enter a benign search query like "cybersecurity news"
3. Click Search

You should see:
- Query refinement happening
- Search engines being queried
- Results being filtered
- Content being scraped
- Summary being generated

**If you see "Refining query..." but nothing happens:** Wait another 30 seconds for Tor to fully connect, then try again.

---

## Post-Installation

### Stopping Robin

```bash
docker-compose down
```

Or:

```bash
docker stop robin
```

### Restarting Robin

```bash
docker-compose up -d
```

### Viewing Logs

```bash
docker logs robin
```

Or for real-time logs:

```bash
docker logs -f robin
```

---

## Next Steps

- Read the [Usage Guide](USAGE.md) to learn how to effectively use Robin
- **IMPORTANT:** Read [Safety Guidelines](SAFETY.md) before conducting any dark web research
- Check [Troubleshooting](TROUBLESHOOTING.md) if you encounter issues

---

## Important Reminders

1. **Always use a VPN** before connecting to Tor
2. **Never download** files from dark web sites
3. **Read SAFETY.md** before searching for anything
4. **Be patient** - Tor connections are slow and can break
5. **Keep API keys private** - Never share your .env file

---

**Installation complete! You now have the same tools professional threat researchers use.**

Remember: With great power comes great responsibility. Use this tool ethically and legally.
