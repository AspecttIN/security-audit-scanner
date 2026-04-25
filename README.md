# GitHub Security Scanner

A Flask web app that clones any public GitHub repository and scans it for common security vulnerabilities. Paste a repo URL, click Scan, and get a colour-coded report in seconds — plus a downloadable professional PDF audit report.

---

![Screenshot placeholder](screenshot.png)
*← Replace with a screenshot of the app*

---

## Features

- **Hardcoded secret detection** — passwords, API keys, tokens, AWS credentials, Bearer tokens
- **Dangerous function detection** — `eval`, `exec`, `os.system`, `subprocess shell=True`, `pickle.load`, `yaml.load`, disabled SSL verification, and more
- **Vulnerable dependency scanning** — checks `requirements.txt` against known-vulnerable versions of 12+ common packages
- **Security score** — overall score out of 100, colour-coded Critical / Needs Work / Good
- **Grouped findings** — deduplicated by type and collapsed into expandable cards
- **AI-powered fix advice** — "How to fix it" button on each finding calls an LLM for a plain-English explanation
- **PDF report generation** — professional penetration-testing style report with cover page, executive summary, donut chart, key findings, and a step-by-step remediation guide

---

## Installation

**Requirements:** Python 3.9+, Git (must be on your PATH)

```bash
# 1. Clone or download this project
cd "GitHub Security Scanner"

# 2. Create a virtual environment and install dependencies
python3 -m venv .venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate
pip install flask python-dotenv requests reportlab

# 3. Set up your API key (see below)
cp .env.example .env
# edit .env and add your key

# 4. Run the app
python app.py
```

Open **http://localhost:5000** in your browser.

---

## API Key Setup

The "How to fix it" button and PDF remediation guide use an LLM via [OpenRouter](https://openrouter.ai). You need a free OpenRouter API key.

1. Sign up at [openrouter.ai](https://openrouter.ai) and copy your API key.
2. Create a `.env` file in the project root (or copy `.env.example`):

```env
OPENROUTER_API_KEY=sk-or-v1-your-key-here
OPENROUTER_MODEL=openai/gpt-oss-20b:free
```

> **Note:** The app runs without an API key — scanning works fully. Only "How to fix it" and PDF remediation content require the key.

---

## Usage

1. Paste a public GitHub repo URL into the input field (e.g. `https://github.com/owner/repo`)
2. Click **Scan** — the app clones the repo and analyses it
3. Review findings grouped by severity (High → Medium → Low)
4. Click any finding card header to expand individual instances
5. Click **✦ How to fix it** on a card to get AI-generated remediation advice
6. Click **⬇ Download PDF Report** to generate a full audit report

---

## Project Structure

```
.
├── app.py              # Flask app — scanning logic, PDF generation, API routes
├── templates/
│   └── index.html      # Single-page UI
├── .env                # Your API keys (not committed)
└── README.md
```

---

## Severity Scoring

| Severity | Points deducted |
|----------|----------------|
| High     | −10 per finding |
| Medium   | −5 per finding  |
| Low      | −2 per finding  |

Score ranges: **0–40** Critical · **41–70** Needs Work · **71–100** Good
