# Troubleshooting Guide - Robin Dark Web Tool

Common issues and solutions when using Robin.

---

## Table of Contents

1. [Tor Connection Issues](#tor-connection-issues)
2. [Docker Problems](#docker-problems)
3. [API Key Errors](#api-key-errors)
4. [Web Interface Issues](#web-interface-issues)
5. [Search Problems](#search-problems)
6. [Performance Issues](#performance-issues)
7. [General Tips](#general-tips)

---

## Tor Connection Issues

### Problem: "Connecting to Tor..." Never Completes

**Symptoms:**
- Robin stuck on "Connecting to Tor network..."
- Web interface shows "Waiting for Tor connection"
- Searches fail immediately

**Solutions:**

**1. Wait Longer (Most Common)**
From Chuck's experience in the video:
- Tor connections can take 30-60 seconds to fully establish
- The web interface may start before Tor connects
- Be patient and wait 1-2 minutes before troubleshooting

**2. Check Tor Service**
```bash
# Check if Tor is running
ps aux | grep tor

# If not running, start it
sudo service tor start

# Or on macOS
brew services start tor
```

**3. Restart Robin Container**
```bash
docker-compose down
docker-compose up -d
```

**4. Check Tor Configuration**
Verify Tor is installed correctly:
```bash
tor --version
```

If missing, reinstall:
```bash
# Linux
sudo apt install tor

# macOS
brew install tor
```

---

### Problem: Tor Version Incompatibility

**Symptoms:**
- Docker build fails with Tor-related errors
- Robin can't connect to Tor network despite Tor being installed

**Solution:**

Chuck mentioned this issue in the video. The Dockerfile may reference an outdated Tor repository.

**Fix:**
1. Check Apurv's GitHub for updates to the Dockerfile
2. Look for issues or pull requests addressing Tor version
3. Alternative: Manually update the Dockerfile's Tor source

**Check for updates:**
```bash
cd robin
git pull origin main
```

---

### Problem: Tor Circuits Breaking During Searches

**Symptoms:**
- Searches start but fail partway through
- "Circuit broken" or "Connection reset" errors
- Some sites load, others don't

**This is NORMAL behavior on Tor.**

**From the video:**
> "The connection does break. So you have to recreate the circuit, then restart your script." - Apurv

**Solutions:**

**1. Retry the Search**
Simply run the search again. Tor will establish new circuits.

**2. Wait Between Retries**
Give Tor 10-20 seconds to stabilize before retrying.

**3. Expect Failures**
Not all sites will load. This is the nature of Tor:
- Sites go offline randomly
- Operators only run sites 2 days/week sometimes
- Relay servers fail (remember Beatrice and her extension cord!)

---

## Docker Problems

### Problem: "docker: command not found"

**Symptom:**
```bash
$ docker --version
bash: docker: command not found
```

**Solution:**
Docker is not installed.

**Install Docker:**
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows/Mac)
- [Docker Engine](https://docs.docker.com/engine/install/) (Linux)

After installation, verify:
```bash
docker --version
```

---

### Problem: Permission Denied Errors

**Symptom:**
```bash
$ docker build -t robin .
permission denied while trying to connect to the Docker daemon socket
```

**Solutions:**

**Linux:**
```bash
# Add your user to the docker group
sudo usermod -aG docker $USER

# Log out and back in, then verify
docker ps
```

**macOS:**
- Ensure Docker Desktop is running
- Check the Docker icon in menu bar

**Windows (WSL):**
- Start Docker Desktop on Windows
- Ensure WSL integration is enabled in Docker Desktop settings

---

### Problem: Docker Build Fails

**Symptoms:**
- `docker build` command fails
- Errors about missing dependencies
- Package installation failures

**Solutions:**

**1. Check Internet Connection**
Docker needs to download packages during build.

**2. Clear Docker Cache**
```bash
docker system prune -a
```

Then rebuild:
```bash
docker build -t robin .
```

**3. Check Dockerfile Syntax**
Ensure you haven't accidentally modified the Dockerfile.

**4. Update Docker**
```bash
# Check version
docker --version

# Update Docker Desktop or Docker Engine
```

---

### Problem: Port Already in Use

**Symptom:**
```bash
Error: bind: address already in use
```

**Solution:**

Another service is using port 8501 or 8000.

**Find the process:**
```bash
# Linux/macOS
lsof -i :8501
lsof -i :8000

# Kill the process (replace PID with actual process ID)
kill -9 PID
```

**Or change Robin's port:**
Edit `docker-compose.yml`:
```yaml
ports:
  - "8502:8501"  # Changed from 8501:8501
```

Then access Robin at `http://localhost:8502`.

---

## API Key Errors

### Problem: "API Key Invalid" or "Authentication Failed"

**Symptoms:**
- Robin won't start search
- Error messages about API authentication
- "Invalid API key" in logs

**Solutions:**

**1. Verify API Key Format**

Check your `.env` file:
```bash
cat .env
```

**OpenAI keys** should start with:
```
sk-proj-...
```
or
```
sk-...
```

**Anthropic keys** should start with:
```
sk-ant-...
```

**2. Check for Extra Spaces**

Open `.env` in a text editor:
```bash
nano .env
```

Ensure no spaces around the `=`:
```
OPENAI_API_KEY=sk-your-key-here    ✓ Correct
OPENAI_API_KEY = sk-your-key-here  ✗ Wrong (spaces)
```

**3. Regenerate API Key**

If the key is correct but still fails:
1. Go to your AI provider's dashboard
2. Generate a new API key
3. Update `.env` file
4. Restart Robin

```bash
docker-compose down
docker-compose up -d
```

**4. Check API Credits**

Ensure you have available credits on your AI provider account:
- OpenAI: [platform.openai.com/account/billing](https://platform.openai.com/account/billing)
- Anthropic: [console.anthropic.com/settings/billing](https://console.anthropic.com/)

---

### Problem: Ollama Connection Failed

**Symptoms:**
- Selected Ollama but searches fail
- "Connection refused" errors
- Can't reach Ollama base URL

**Solutions:**

**1. Verify Ollama is Running**
```bash
ollama list
```

If not installed:
```bash
# Install from https://ollama.ai/
curl -fsSL https://ollama.com/install.sh | sh
```

**2. Check Ollama Base URL**

Your `.env` should have:
```
OLLAMA_BASE_URL=http://localhost:11434
```

Verify Ollama is listening:
```bash
curl http://localhost:11434/api/tags
```

**3. Pull a Model**
```bash
ollama pull llama3.1
```

**4. Check Docker Network**

If running Ollama and Robin both in Docker, they need to be on the same network or use `host.docker.internal`:

```
OLLAMA_BASE_URL=http://host.docker.internal:11434
```

---

## Web Interface Issues

### Problem: Can't Access http://localhost:8501

**Symptoms:**
- Browser shows "Connection refused"
- "This site can't be reached"
- Page doesn't load

**Solutions:**

**1. Check Robin is Running**
```bash
docker ps
```

You should see the Robin container listed and status "Up".

**2. Try Alternative Port**

Robin might be on a different port:
```
http://localhost:8000
http://localhost:8501
http://localhost:8080
```

Check Docker logs to confirm:
```bash
docker logs robin
```

Look for lines like:
```
Streamlit running on port 8501
```

**3. Check Docker Port Mapping**
```bash
docker ps
```

Look at the PORTS column:
```
0.0.0.0:8501->8501/tcp
```

**4. WSL Users (Windows)**

If using WSL, access from Windows browser using:
```
http://localhost:8501
```

NOT the WSL IP address.

---

### Problem: Web Interface Loads but Shows Errors

**Symptoms:**
- Interface appears but no search functionality
- Error messages in the UI
- Blank or broken layout

**Solutions:**

**1. Check Browser Console**

Open browser Developer Tools (F12) and check Console for errors.

**2. Clear Browser Cache**

```
Ctrl+Shift+Delete (Windows/Linux)
Cmd+Shift+Delete (Mac)
```

Clear cache and cookies, then reload.

**3. Try Different Browser**

Test in:
- Chrome
- Firefox
- Edge

**4. Check Docker Logs**
```bash
docker logs robin
```

Look for Python errors or stack traces.

---

## Search Problems

### Problem: Search Returns No Results

**Symptoms:**
- Search completes but finds 0 results
- "No relevant content found"

**Possible Causes:**

**1. Too Specific Query**

Try broader search terms:
- Instead of: `Conti ransomware v3.2 exploit code`
- Try: `Conti ransomware`

**2. Tor Connection Issues**

Verify Tor is connected (see [Tor Connection Issues](#tor-connection-issues)).

**3. Search Engines Down**

Dark web search engines go offline frequently. Try again later.

**4. Overly Aggressive Filtering**

AI filtering might be too strict. Try rephrasing your query.

---

### Problem: Robin Refines Query But Then Stops

**From Chuck's Experience:**

**Symptoms:**
- "Refining query..." completes
- Search appears to hang
- Nothing happens for minutes

**Solution:**

**Wait for Tor to fully connect.**

Chuck experienced this in the video:
1. Web interface started before Tor fully connected
2. Query refinement worked (API call successful)
3. Dark web search failed (Tor not ready)
4. **Solution:** Wait 30-60 seconds and try again

**To verify:**
- Check Docker logs for "Tor connection established"
- Wait until you see confirmation before searching

---

### Problem: Some Sites Don't Load

**This is completely normal.**

**From Apurv in the video:**
- Sites operate only 2 days per week sometimes
- Tor circuits break constantly
- Relay servers go offline (Beatrice's mom unplugs the power!)

**What Robin Does:**
- Attempts to scrape all filtered sites
- Skips sites that fail to load
- Continues with successful scrapes
- Provides results from whatever loaded successfully

**No action needed** - this is expected behavior.

---

## Performance Issues

### Problem: Searches Take 5+ Minutes

**This is normal.**

**Why:**
1. **Tor is slow by design** - Multiple relay hops
2. **15 search engines** - Querying all simultaneously
3. **AI filtering** - Processing hundreds of results
4. **Multi-threaded scraping** - Downloading 20 sites over Tor
5. **AI analysis** - Generating comprehensive summary

**From the video:**
Manual research takes 6-8 hours. Robin reduces this to ~30 minutes.

**If searches take longer than 10 minutes:**

**1. Check Tor Connection**
Ensure Tor is stable (see [Tor Connection Issues](#tor-connection-issues)).

**2. Check AI Provider**
Slow API responses from OpenAI/Anthropic can add time.

**3. Reduce Timeout Values**
If Robin has configurable timeouts, adjust them (check documentation).

---

### Problem: High CPU/Memory Usage

**Symptoms:**
- Computer becomes slow
- Fan noise increases
- Docker using lots of resources

**This is expected during searches.**

**Why:**
- Multi-threaded scraping
- AI model processing
- Docker container overhead

**Solutions:**

**1. Close Other Applications**
Free up resources during searches.

**2. Adjust Docker Resources**

Docker Desktop → Settings → Resources:
- Increase CPU allocation
- Increase memory allocation

**3. Use Less Resource-Intensive AI**

Ollama with smaller models uses less memory than large OpenAI models.

**4. Wait for Search to Complete**

Resource usage will drop once the search finishes.

---

## General Tips

### Check Docker Logs First

Most issues show up in logs:
```bash
docker logs robin
```

For real-time monitoring:
```bash
docker logs -f robin
```

---

### Restart Robin

Often fixes transient issues:
```bash
docker-compose down
docker-compose up -d
```

---

### Verify All Prerequisites

Make sure you have:
- [ ] Docker installed and running
- [ ] Tor installed
- [ ] Valid API key in `.env`
- [ ] Correct port mapping
- [ ] Internet connection
- [ ] VPN connected (recommended)

---

### Update Robin

Check for updates to fix known issues:
```bash
cd robin
git pull origin main
docker-compose down
docker build -t robin .
docker-compose up -d
```

---

### Check GitHub Issues

Visit Apurv's repository:
```
https://github.com/APURV-USERNAME/robin/issues
```

Search for your problem - others may have encountered it.

---

### When All Else Fails

1. **Stop everything:**
   ```bash
   docker-compose down
   docker system prune -a
   ```

2. **Start fresh:**
   ```bash
   docker build -t robin .
   docker-compose up -d
   ```

3. **Wait 60 seconds** for Tor to connect

4. **Try a simple search** like "cybersecurity news"

---

## Getting Help

If you're still stuck:

1. **Check the video** - [NetworkChuck Episode 480](https://www.youtube.com/watch?v=VIDEO_ID)
2. **Review the documentation** - README.md, INSTALLATION.md, USAGE.md
3. **Check GitHub Issues** - Someone may have solved your problem
4. **Ask in YouTube comments** - Community support
5. **Create a GitHub Issue** - Report bugs to Apurv

**When asking for help, include:**
- Your operating system
- Docker version (`docker --version`)
- Error messages (from `docker logs robin`)
- Steps to reproduce the issue

---

**Remember: Tor is inherently unstable. Many "problems" are just Tor being Tor. Be patient and retry.**

"Tour is janky." - NetworkChuck
