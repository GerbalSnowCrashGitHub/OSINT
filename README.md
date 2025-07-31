# OSINT Pentest Framework - JavaScript & Recon Analysis Guide

This guide details how to use the tools in this framework to crawl web targets, analyze JavaScript for insecure constructs, extract domain intel, and prepare findings for further pentesting or disclosure.

---

## 📁 Folder Structure

```
osint-pentest-framework/
├── scripts/
│   └── python/
│       ├── fetch_assets.py        # Web crawler & JS/asset downloader
│       ├── analyze_js.py          # JS fingerprinting + postMessage scanner
│       ├── dns_probe.py           # Passive & active DNS analysis
│       └── report_generator.py    # Converts findings to markdown/JSON
├── targets/
│   └── yoti.com/                  # Example target
│       ├── js/                    # Collected JavaScript files
│       ├── discoveries.txt        # Logged keywords, login pages, new domains
│       ├── js_findings.json       # JS scan output
│       └── dns_results.json       # DNS recon output
├── reports/                       # Auto-generated reports (optional)
```

---

## 🚀 Process Overview

### 1. 🔎 Crawl the Target Website
Downloads JS/HTML/WASM assets and logs discovery points (URLs, keywords, domains).

```bash
python3 scripts/python/fetch_assets.py yoti.com 2
```
- `yoti.com`: Target domain
- `2`: Crawl depth

➡️ Output: `targets/yoti.com/` with content and `discoveries.txt`

---

### 2. 🕵️ Analyze JavaScript for Security Issues
Scans JS files for common security smells:
- `postMessage` misuse
- `eval()`/`Function()` usage
- Biometric & privacy-sensitive terms
- Known fingerprinting patterns

```bash
python3 scripts/python/analyze_js.py targets/yoti.com/
```
➡️ Output: `js_findings.json`

---

### 3. 🌐 DNS Recon & Subdomain Discovery (Optional)
Probes passive and active DNS sources (DNSX, DNSRecon, etc).

```bash
python3 scripts/python/dns_probe.py yoti.com
```
➡️ Output: `dns_results.json`

---

## 📤 Using Output for Further Testing

### 🧠 Discovery Highlights
Check `discoveries.txt` for:
- URLs with `login`, `signup`, `admin`, `api`
- External JS or analytics domains

```bash
cat targets/yoti.com/discoveries.txt | grep domain | cut -d ':' -f2 > targets.txt
nmap -Pn -iL targets.txt -p- -sV -oA yoti-scan
```

### 📊 JS Findings
Review `js_findings.json` to:
- Flag dangerous functions
- Identify external trackers or biometric leaks

Use `report_generator.py` to create readable markdown summaries.

```bash
python3 scripts/python/report_generator.py targets/yoti.com/
```
➡️ Generates: `reports/yoti.com.md`

---

## 👥 Recommended Team Workflow

1. Assign domain to analyst
2. Run crawler: `fetch_assets.py`
3. Analyze JS: `analyze_js.py`
4. Run DNS recon (optional)
5. Generate report
6. Review findings for bug bounty eligibility

---

## 🛠 Planned Upgrades
- GitHub CI/CD scanner integration
- Multi-threaded asset download
- HackerOne/Intigriti scope-aware filters
- Shodan/FOFA/ZoomEye enrichment hooks

---

_For updates or issues, contact the framework maintainer or fork on GitHub._
