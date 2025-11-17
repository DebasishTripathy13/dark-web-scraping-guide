# Usage Guide - Robin Dark Web Tool

Learn how to effectively use Robin for dark web research and threat intelligence gathering.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [The Robin Interface](#the-robin-interface)
3. [Conducting a Search](#conducting-a-search)
4. [Understanding Results](#understanding-results)
5. [Downloading Reports](#downloading-reports)
6. [Example Searches](#example-searches)
7. [Best Practices](#best-practices)
8. [Limitations](#limitations)

---

## Getting Started

### Before You Search

1. **Start your VPN** - Always connect to a VPN before using Tor
2. **Start Robin** - Run `docker-compose up -d` if not already running
3. **Wait for Tor** - Give Tor 30-60 seconds to fully connect
4. **Review Safety Guidelines** - Read [SAFETY.md](SAFETY.md) to understand what to avoid

### Access the Interface

Open your browser and navigate to:
```
http://localhost:8501
```

Or check your terminal output for the correct port number.

---

## The Robin Interface

### Left Sidebar: LLM Selection

Choose your AI provider:
- **OpenAI (ChatGPT)** - Fast, accurate, requires API key
- **Anthropic (Claude)** - Excellent analysis, requires API key
- **Ollama (Local)** - Privacy-focused, runs on your machine, slower

### Center Area: Search Box

This is where you enter your search queries. Robin will:
1. Refine your query using AI
2. Search multiple dark web engines
3. Filter and analyze results
4. Present findings with next steps

### Status Indicators

Watch for:
- **Tor Connection Status** - Wait until "Connected"
- **Search Progress** - Shows current operation
- **Results Count** - How many results found/filtered

---

## Conducting a Search

### Step 1: Enter Your Query

Type your search query in the search box. Be specific but not overly narrow.

**Examples:**
- `ransomware forums`
- `data breach credentials`
- `threat actor discussions`
- `malware samples`
- `cryptocurrency scams`

### Step 2: Query Refinement

Robin will automatically refine your query for better semantic matching.

**Example:**
- **Your query:** `ransomware`
- **Robin's refinement:** `ransomware forums threat actors tools techniques latest vulnerabilities exploitation`

This refinement helps match the **meaning** of what you're looking for, not just exact keywords.

### Step 3: Multi-Engine Search

Robin searches **15 dark web search engines** simultaneously, aggregating results.

**You'll see:**
```
Searching dark web engines...
Found 910 results
```

The number varies based on your query.

### Step 4: AI Filtering

Robin uses semantic analysis to filter results from hundreds down to ~20 most relevant.

**You'll see:**
```
Filtering results using AI...
910 results → 20 relevant sources
```

This removes:
- Irrelevant results
- Low-quality sources
- Likely scam/honeypot sites
- Duplicate content

### Step 5: Multi-Threaded Scraping

Robin scrapes all filtered sites in parallel, extracting actual content.

**You'll see:**
```
Scraping 20 sites...
Progress: 5/20
Progress: 15/20
Scraping complete
```

**Note:** Some sites may fail to load (broken circuits, sites down). This is normal on Tor.

### Step 6: Content Analysis

The AI analyzes scraped content and generates a comprehensive summary with:
- Key findings
- Referenced links and artifacts
- Threat intelligence insights
- Recommended next steps
- Additional search queries

---

## Understanding Results

### Results Structure

Each Robin search provides:

#### 1. Overview Summary
High-level summary of what was found and key themes.

#### 2. Referenced Sites and Links
Actual `.onion` URLs where information was found.

**Example:**
```
http://exampleabc123def456.onion - Ransomware Forum
http://anotheronion789xyz.onion - Threat Actor Marketplace
```

#### 3. Key Findings
Bullet points of important discoveries:
- Ransomware builders mentioned
- Cryptocurrency addresses
- Threat actor names/handles
- Tactics, techniques, and procedures (TTPs)
- Mentioned vulnerabilities

**Example from Chuck's Demo:**
```
- RCM ransomware builder
- Crypto addresses: DOGE, LTC, BTC, XMR
- Conti ransomware group discussions
- Darknet Army forum references
- Claims of $500 per hit
```

#### 4. Next Steps
AI-recommended actions to continue investigation:
- Monitor specific forums
- Profile certain threat actors
- Follow up searches to conduct
- Links to investigate further

#### 5. Additional Search Queries
Suggested related searches based on findings.

**Example:**
```
Recommended searches:
- "Conti ransomware operations 2024"
- "RCM ransomware builder download"
- "Darknet Army forum access"
```

---

## Downloading Reports

### Markdown Export

Robin generates downloadable markdown reports perfect for research tools.

**To download:**
1. Click the **"Download Report"** button (usually top-right or bottom of results)
2. Save the `.md` file to your computer
3. Import into Obsidian, Notion, or your preferred note-taking tool

### Report Contents

The markdown file includes:
- Search query and timestamp
- Complete summary
- All referenced links
- Key findings organized by category
- Next steps and recommendations
- Metadata (number of results, filters applied, etc.)

---

## Example Searches

### Example 1: Ransomware Research

**Query:** `ransomware`

**Results (from Chuck's demo):**
- Found 910 results across engines
- Filtered to 20 relevant sources
- Discovered:
  - RCM ransomware builder references
  - Conti ransomware group forum
  - Cryptocurrency wallets (BTC, XMR, DOGE, LTC)
  - Threat actor handles and marketplace listings

**Next Steps:**
- Monitor "Conti" forum
- Profile threat actors offering RCM
- Search for specific cryptocurrency addresses

### Example 2: Data Breach Search (Your Own Data)

**Query:** `yourname@email.com`

**Purpose:** Check if your email appears in dark web data breaches.

**Expected Results:**
- Breach databases mentioning your email
- Paste sites with credentials
- Forums discussing specific data leaks

**Action:** If found, change passwords immediately and enable 2FA.

### Example 3: Threat Actor Profiling

**Query:** `known-threat-actor-handle`

**Results:**
- Forum posts by the actor
- Marketplace listings
- Associated cryptocurrency addresses
- Connected personas/accounts
- Active discussion threads

---

## Best Practices

### Search Strategy

1. **Start Broad, Then Narrow**
   - First search: `ransomware`
   - Follow-up: `Conti ransomware builder`
   - Deep dive: `Conti affiliate program recruitment`

2. **Use Robin's Recommendations**
   - Always check the "Additional Search Queries" section
   - These are AI-generated based on actual findings

3. **Be Patient**
   - Tor is slow
   - Searches can take 2-5 minutes
   - Sites frequently go offline (totally normal)

4. **Verify Multiple Sources**
   - Don't trust a single result
   - Cross-reference findings
   - Look for corroboration across multiple sites

### Research Workflow

**For Professional Investigators:**

1. **Initial Broad Search** - Get the landscape
2. **Identify Key Forums/Sites** - Find the real communities
3. **Monitor Over Time** - Patience is key (days/weeks)
4. **Build Sock Puppet Identity** - Create consistent fake persona
5. **Engage Gradually** - Build trust slowly
6. **Document Everything** - Keep detailed notes in markdown reports
7. **Wait for Invites** - Private Telegram groups, exclusive forums

See [SAFETY.md](SAFETY.md) for detailed sock puppet and persona management guidance.

### Privacy Protection

1. **Always VPN + Tor** - VPN first, then Tor
2. **Never use real identity** - No real names, emails, or accounts
3. **Don't download files** - High malware risk
4. **Clear browser data** - Regularly clear cookies and cache
5. **Use dedicated machine** - Ideally a separate device for dark web research

---

## Limitations

### What Robin Can Do
- Search 15 dark web engines simultaneously
- Filter hundreds of results intelligently
- Extract content from accessible sites
- Provide AI-generated analysis and next steps
- Create research reports

### What Robin Cannot Do
- **Access invite-only sites** - Private forums require manual access
- **Bypass authentication** - Login-protected content is inaccessible
- **Guarantee 100% uptime** - Tor sites go offline frequently
- **Filter all illegal content** - Guardrails exist but aren't foolproof
- **Replace manual investigation** - Tool accelerates research but can't replace human judgment

### Dark Web Realities

From Apurv's Interview:

- **90% is fake** - Law enforcement honeypots and scams
- **Sites disappear** - Many operate only 2 days per week at unknown times
- **Circuits break constantly** - Tor is unstable by design
- **Trust takes months** - Real access requires patience
- **Persona consistency is critical** - One mistake can blow your cover

**Timeline Reality:** Professional investigations take days, weeks, or months - not hours.

---

## Homework (From Chuck)

1. **Test the tool** - Run a few benign searches
2. **Search for your own email** - See if you're on the dark web
3. **Search for your name** - Check for data breaches
4. **Comment what you find** - Share (SFW only!) in YouTube comments

---

## Next Steps

- Review [SAFETY.md](SAFETY.md) for critical legal and security guidelines
- Check [TROUBLESHOOTING.md](TROUBLESHOOTING.md) if you encounter issues
- Watch [NetworkChuck's Full Video](https://www.youtube.com/watch?v=VIDEO_ID) for visual walkthrough

---

**Remember:** This tool is for education, security research, and defensive purposes only.

"A dark web researcher gave me his AI tool, and I just gave it to you. Think about that. You now have the ability to use the same tools a real threat researcher has on the dark web." - NetworkChuck
