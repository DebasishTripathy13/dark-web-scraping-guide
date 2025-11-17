# Dark Web Scraping with Robin AI Tool

## NetworkChuck Video Guide - Episode 480

> **WARNING**: This tool is for educational and security research purposes only. Accessing illegal content on the dark web can result in serious legal consequences. See [SAFETY.md](SAFETY.md) for critical legal and security information.

---

## Watch the Full Video Tutorial

[![Dark Web Expert's SECRET AI Tool (90% is FAKE)](https://img.youtube.com/vi/_KzObeom88Y/maxresdefault.jpg)](https://www.youtube.com/watch?v=_KzObeom88Y)

**[Watch on YouTube →](https://www.youtube.com/watch?v=_KzObeom88Y)**

---

## What is This Guide?

This repository provides a comprehensive walkthrough for installing and using **Robin**, an AI-powered dark web research tool created by cybersecurity expert Apurv. The tool was featured in NetworkChuck's Episode 480, where Chuck demonstrates how professional threat researchers find real content on the dark web.

### The Problem Robin Solves

**90% of the dark web is fake** - controlled by law enforcement honeypots or scam sites. Professional threat researchers like Apurv spend 6-8 hours manually searching through unreliable connections, broken circuits, and intermittent sites just to find legitimate threat intelligence.

Robin changes that equation dramatically:
- **Time Reduction**: 6-8 hours → 30 minutes
- **AI-Powered Filtering**: Searches 15 dark web search engines simultaneously
- **Semantic Analysis**: Uses AI to filter hundreds of results down to ~20 verified sources
- **Intelligent Scraping**: Multi-threaded content extraction with automatic summarization
- **Research Reports**: Generates markdown files for tools like Obsidian

---

## Quick Start

1. **[Install Prerequisites & Robin](INSTALLATION.md)** - Docker, Tor, and tool setup
2. **[Learn How to Use Robin](USAGE.md)** - Search the dark web safely and effectively
3. **[Read Safety Guidelines](SAFETY.md)** - Legal warnings and security protocols ⚠️
4. **[Troubleshoot Issues](TROUBLESHOOTING.md)** - Common problems and solutions

---

## How Robin Works

Robin uses a sophisticated multi-stage pipeline to find real dark web content:

### 1. Query Refinement
The AI improves your search query to better match semantic meaning rather than just keywords.

**Example:**
- You type: `ransomware`
- Robin refines: `ransomware forums threat actors tools techniques latest vulnerabilities`

### 2. Multi-Engine Search
Simultaneously searches **15 different dark web search engines**, aggregating hundreds of results.

**Example:** A search for "ransomware" might return 900+ results across all engines.

### 3. Semantic Filtering
Uses AI to analyze each result's relevance based on meaning, not just keyword matching. Filters the massive result set down to ~20 highly relevant sources.

**Example:** 900 results → 20 verified, relevant sources

### 4. Multi-Threaded Scraping
Rapidly scrapes all filtered sites in parallel, extracting actual content despite slow Tor connections and broken circuits.

### 5. Intelligent Analysis
The AI analyzes scraped content to identify:
- Referenced links and artifacts
- Key insights and threat intelligence
- Recommended next steps for investigation
- Additional search queries to continue research

### 6. Report Generation
Creates a downloadable markdown report with all findings, perfect for importing into research tools like Obsidian.

---

## What You Need

### Required
- **Docker** - Container platform to run Robin safely
- **Tor** - The Onion Router for dark web access
- **Git** - To clone the Robin repository
- **API Key** for one of:
  - OpenAI (ChatGPT)
  - Anthropic (Claude)
  - Ollama (local models like Llama 3.1)

### Recommended
- **VPN** - Essential for privacy (use before connecting to Tor)
- **Linux or macOS** - Or WSL (Windows Subsystem for Linux) on Windows

---

## Credits

### Created By
- **Tool Developer**: [Apurv](https://github.com/APURV-USERNAME) - Senior Threat Research Analyst
- **Video Tutorial**: [NetworkChuck](https://www.youtube.com/@NetworkChuck) - YouTube Creator
- **Guide Author**: NetworkChuck Community

### Original Robin Repository
The official Robin tool repository: [github.com/APURV-USERNAME/robin](https://github.com/APURV-USERNAME/robin)

**Named after:** The One Piece character Robin (Nico Robin), who has the ability to create eyes and ears anywhere to gather intelligence.

---

## Educational Use Only

This tool is designed for:
- Threat intelligence research
- Security professionals investigating cybercrime
- Understanding dark web threat landscapes
- Educational exploration of cybersecurity research techniques
- Searching for your own leaked data

This tool is **NOT** for:
- Accessing illegal marketplaces
- Downloading illegal content
- Facilitating criminal activity
- Evading law enforcement

**Read [SAFETY.md](SAFETY.md) before using this tool.**

---

## Table of Contents

- [Installation Guide](INSTALLATION.md) - Complete setup instructions
- [Usage Guide](USAGE.md) - How to use Robin effectively
- [Safety Guidelines](SAFETY.md) - Legal warnings and security protocols
- [Troubleshooting](TROUBLESHOOTING.md) - Common issues and solutions

---

## License

This guide is provided as-is for educational purposes. The Robin tool itself is licensed by its creator Apurv.

---

## Get Your Coffee Ready ☕

As Chuck always says - let's go explore the dark web... safely and legally!

**Now YOU have the same tools professional threat researchers use.**
