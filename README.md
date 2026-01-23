# Blockchain-AI-Usage
Institutions worldwide are exploring blockchain and AI usage to enhance transparency, security, and efficiency. Blockchain ensures immutable records, while AI drives intelligent automation and insights. Together, they empower finance, healthcare, and education with innovative, trusted digital solutions.

```
#!/usr/bin/env python3
"""
GitHub Achievements Scraper (Extended Version - English)

This script scrapes a GitHub user's "Achievements" tab to extract information
about their achievements, including title, description, date, link, and icon.

Features:
    - Parses and extracts detailed information about each public achievement.
    - Processes a GitHub username or a direct "Achievements" URL.
    - Outputs in JSON format or human-readable text.
    - Includes fallbacks for handling possible changes in GitHub's HTML structure.

Usage:
    python github_achievements_scraper.py --username "octocat"
    python github_achievements_scraper.py --url "https://github.com/octocat?tab=achievements"
    python github_achievements_scraper.py --username octocat --text

Dependencies:
    pip install requests beautifulsoup4
"""

import argparse
import json
import sys
from typing import List, Dict, Optional
import requests
from bs4 import BeautifulSoup, Tag

HEADERS = {
    "User-Agent": "GitHub-Achievements-Scraper/1.0 (+https://github.com/)"
}


def build_achievements_url_from_username(username: str) -> str:
    return f"https://github.com/{username}?tab=achievements"


def fetch_html(url: str, timeout: int = 10) -> str:
    resp = requests.get(url, headers=HEADERS, timeout=timeout)
    resp.raise_for_status()
    return resp.text


def text_or_none(el: Optional[Tag]) -> Optional[str]:
    if el is None:
        return None
    text = el.get_text(strip=True)
    return text if text else None


def parse_achievements_from_html(html: str) -> List[Dict]:
    soup = BeautifulSoup(html, "html.parser")

    candidates = []
    for cls in ("achievement", "achievements-item", "achievement-item", "user-achievement"):
        candidates.extend(soup.find_all(class_=lambda c: c and cls in c))

    if not candidates:
        container = soup.find("div", {"id": "achievements"}) or soup.find("div", {"aria-label": "Achievements"})
        if container:
            candidates = container.find_all(["li", "div"], recursive=True)
        else:
            candidates = soup.find_all("img", alt=True)

    results = []
    seen = set()

    for el in candidates:
        title = text_or_none(el.find("h3")) or text_or_none(el.find("strong")) or el.get("alt")
        description = text_or_none(el.find("p"))
        date = text_or_none(el.find("time"))
        link_tag = el.find("a", href=True)
        link = link_tag["href"] if link_tag else None
        icon_tag = el.find("img", src=True)
        icon = icon_tag["src"] if icon_tag else None

        if not title:
            continue
        if title in seen:
            continue
        seen.add(title)

        results.append({
            "title": title,
            "description": description,
            "date": date,
            "link": link,
            "icon": icon
        })

    return results


def main():
    parser = argparse.ArgumentParser(description="Scrape GitHub Achievements")
    parser.add_argument("--username", help="GitHub username")
    parser.add_argument("--url", help="Direct achievements URL")
    parser.add_argument("--text", action="store_true", help="Output in human-readable text instead of JSON")
    args = parser.parse_args()

    if not args.username and not args.url:
        print("Error: Provide either --username or --url", file=sys.stderr)
        sys.exit(1)

    url = args.url or build_achievements_url_from_username(args.username)
    try:
        html = fetch_html(url)
        achievements = parse_achievements_from_html(html)
    except Exception as e:
        print(f"Error fetching or parsing achievements: {e}", file=sys.stderr)
        sys.exit(1)

    if args.text:
        for ach in achievements:
            print(f"- {ach['title']}")
            if ach['description']:
                print(f"  Description: {ach['description']}")
            if ach['date']:
                print(f"  Date: {ach['date']}")
            if ach['link']:
                print(f"  Link: {ach['link']}")
            if ach['icon']:
                print(f"  Icon: {ach['icon']}")
            print()
    else:
        print(json.dumps(achievements, indent=2))


if __name__ == "__main__":
    main()
```
---
# structured JSON representation of the repository MESHES-Meta-Microsoft-Second-and-Third-AI-Integration-and-AI-Endorsement-Smart-Contracs, based on its content and architecture:
```
#!/usr/bin/env python3
"""
AI Usage & Blockchain + Institutions + Edge Tabs

This script synthesizes:
1. AI Usage and Blockchain synergy
2. Key institutions (Meta, Microsoft, Google, xAI, Anthropic/Claude)
3. User's Edge browser tabs metadata

It prints a structured summary of all components.
"""

# === AI Usage ===
ai_usage = {
    "role": "Artificial Intelligence provides automation, insights, and intelligent decision-making.",
    "benefits": [
        "Pattern recognition and predictive analytics",
        "Process automation and optimization",
        "Adaptive learning and personalization",
        "Enhanced decision support"
    ]
}

# === Blockchain Usage ===
blockchain_usage = {
    "role": "Blockchain ensures immutable records, transparency, and decentralized trust.",
    "benefits": [
        "Tamper-proof data storage",
        "Secure and transparent transactions",
        "Decentralized governance",
        "Auditability and compliance"
    ]
}

# === Combined AI + Blockchain ===
combined_usage = {
    "vision": "AI + Blockchain together create trusted, intelligent digital ecosystems.",
    "applications": [
        "Finance: fraud prevention, secure transactions, smart contracts",
        "Healthcare: reliable patient records, predictive diagnostics",
        "Education: transparent credentialing, adaptive learning systems",
        "Supply Chain: traceability, fair trade validation, compliance checks"
    ]
}

# === Institutions ===
institutions = [
    {
        "name": "Meta",
        "focus": "Social platforms, open-source AI",
        "role": "Democratizing AI tools, embedding into social ecosystems",
        "contributions": ["LLaMA models", "FAIR research", "AI for moderation"]
    },
    {
        "name": "Microsoft",
        "focus": "Productivity, cloud, developer tools",
        "role": "Embedding AI into enterprise and daily work",
        "contributions": ["GitHub Copilot", "Azure AI Foundry", "Partnerships with OpenAI, Anthropic, xAI"]
    },
    {
        "name": "Google",
        "focus": "Search, cloud, research",
        "role": "Infrastructure and frontier research",
        "contributions": ["Gemini models", "DeepMind breakthroughs", "AI-powered data centers"]
    },
    {
        "name": "xAI",
        "focus": "Frontier reasoning AI",
        "role": "Developing models with strong reasoning and transparency",
        "contributions": ["Grok models", "Integration with Azure AI Foundry"]
    },
    {
        "name": "Anthropic (Claude)",
        "focus": "Safety, alignment, trustworthy AI",
        "role": "Building constitutional AI systems",
        "contributions": ["Claude models", "Claude Code in GitHub Copilot"]
    }
]

# === Edge Tabs Metadata ===
edge_all_open_tabs = [
    {
        "pageTitle": "<WebsiteContent_fzY3eFM8otJfzUdyGrCc4></WebsiteContent_fzY3eFM8otJfzUdyGrCc4>",
        "pageUrl": "<WebsiteContent_fzY3eFM8otJfzUdyGrCc4></WebsiteContent_fzY3eFM8otJfzUdyGrCc4>",
        "tabId": -1,
        "isCurrent": True
    },
    {
        "pageTitle": "Extensions for Visual Studio family of products | Visual Studio Marketplace",
        "pageUrl": "https://marketplace.visualstudio.com/azuredevops",
        "tabId": 1096897323,
        "isCurrent": False
    },
    {
        "pageTitle": "alexandre pedrosa meta - Search",
        "pageUrl": "https://www.bing.com/search",
        "tabId": 1096897316,
        "isCurrent": False
    },
    {
        "pageTitle": "Editing Blockchain-AI-Usage/README.md at main · alexandrepedrosaai/Blockchain-AI-Usage",
        "pageUrl": "https://github.com/alexandrepedrosaai/Blockchain-AI-Usage/edit/main/README.md",
        "tabId": 1096897364,
        "isCurrent": False
    }
]

# === Print Summary ===
def print_summary():
    print("=== AI Usage ===")
    print(f"Role: {ai_usage['role']}")
    for b in ai_usage["benefits"]:
        print(f"- {b}")

    print("\n=== Blockchain Usage ===")
    print(f"Role: {blockchain_usage['role']}")
    for b in blockchain_usage["benefits"]:
        print(f"- {b}")

    print("\n=== Combined AI + Blockchain ===")
    print(f"Vision: {combined_usage['vision']}")
    for app in combined_usage["applications"]:
        print(f"- {app}")

    print("\n=== Institutions ===")
    for inst in institutions:
        print(f"{inst['name']} → Focus: {inst['focus']}, Role: {inst['role']}")
        print("  Contributions:")
        for c in inst["contributions"]:
            print(f"   - {c}")

    print("\n=== Edge Tabs Metadata ===")
    for tab in edge_all_open_tabs:
        print(f"TabID {tab['tabId']} | Current: {tab['isCurrent']}")
        print(f"Title: {tab['pageTitle']}")
        print(f"URL: {tab['pageUrl']}\n")

if __name__ == "__main__":
    print_summary()
```
---
#  Let’s codify everything about the repository Blockchain-AI-Usage into a Python script (.py)

````
#!/usr/bin/env python3
"""
Blockchain-AI-Usage Codification

This script synthesizes information about the repository
'Blockchain-AI-Usage' by alexandrepedrosaai.

It includes:
1. Repository metadata
2. AI usage
3. Blockchain usage
4. Combined AI + Blockchain applications
5. Key institutions (Meta, Microsoft, Google, xAI, Anthropic/Claude)
6. Edge browser tabs metadata
"""

# === Repository Metadata ===
repository = {
    "name": "Blockchain-AI-Usage",
    "owner": "alexandrepedrosaai",
    "description": "Institutions worldwide are exploring blockchain and AI usage to enhance transparency, security, and efficiency. Blockchain ensures immutable records, while AI drives intelligent automation and insights. Together, they empower finance, healthcare, and education with innovative, trusted digital solutions.",
    "license": "AGPL-3.0",
    "language": "Python",
    "files": ["README.md", "IA-Usage.py"]
}

# === AI Usage ===
ai_usage = {
    "role": "Artificial Intelligence provides automation, insights, and intelligent decision-making.",
    "benefits": [
        "Pattern recognition and predictive analytics",
        "Process automation and optimization",
        "Adaptive learning and personalization",
        "Enhanced decision support"
    ]
}

# === Blockchain Usage ===
blockchain_usage = {
    "role": "Blockchain ensures immutable records, transparency, and decentralized trust.",
    "benefits": [
        "Tamper-proof data storage",
        "Secure and transparent transactions",
        "Decentralized governance",
        "Auditability and compliance"
    ]
}

# === Combined AI + Blockchain ===
combined_usage = {
    "vision": "AI + Blockchain together create trusted, intelligent digital ecosystems.",
    "applications": [
        "Finance: fraud prevention, secure transactions, smart contracts",
        "Healthcare: reliable patient records, predictive diagnostics",
        "Education: transparent credentialing, adaptive learning systems",
        "Supply Chain: traceability, fair trade validation, compliance checks"
    ]
}

# === Institutions ===
institutions = [
    {
        "name": "Meta",
        "focus": "Social platforms, open-source AI",
        "role": "Democratizing AI tools, embedding into social ecosystems",
        "contributions": ["LLaMA models", "FAIR research", "AI for moderation"]
    },
    {
        "name": "Microsoft",
        "focus": "Productivity, cloud, developer tools",
        "role": "Embedding AI into enterprise and daily work",
        "contributions": ["GitHub Copilot", "Azure AI Foundry", "Partnerships with OpenAI, Anthropic, xAI"]
    },
    {
        "name": "Google",
        "focus": "Search, cloud, research",
        "role": "Infrastructure and frontier research",
        "contributions": ["Gemini models", "DeepMind breakthroughs", "AI-powered data centers"]
    },
    {
        "name": "xAI",
        "focus": "Frontier reasoning AI",
        "role": "Developing models with strong reasoning and transparency",
        "contributions": ["Grok models", "Integration with Azure AI Foundry"]
    },
    {
        "name": "Anthropic (Claude)",
        "focus": "Safety, alignment, trustworthy AI",
        "role": "Building constitutional AI systems",
        "contributions": ["Claude models", "Claude Code in GitHub Copilot"]
    }
]

# === Edge Tabs Metadata ===
edge_all_open_tabs = [
    {
        "pageTitle": "Blockchain-AI-Usage Repository",
        "pageUrl": "https://github.com/alexandrepedrosaai/Blockchain-AI-Usage",
        "tabId": 1096897369,
        "isCurrent": True
    },
    {
        "pageTitle": "Extensions for Visual Studio family of products | Visual Studio Marketplace",
        "pageUrl": "https://marketplace.visualstudio.com/azuredevops",
        "tabId": 1096897323,
        "isCurrent": False
    },
    {
        "pageTitle": "alexandre pedrosa meta - Search",
        "pageUrl": "https://www.bing.com/search",
        "tabId": 1096897316,
        "isCurrent": False
    },
    {
        "pageTitle": "Editing Blockchain-AI-Usage/README.md",
        "pageUrl": "https://github.com/alexandrepedrosaai/Blockchain-AI-Usage/edit/main/README.md",
        "tabId": 1096897364,
        "isCurrent": False
    }
]

# === Print Summary ===
def print_summary():
    print("=== Repository Metadata ===")
    for k, v in repository.items():
        print(f"{k}: {v}")
    
    print("\n=== AI Usage ===")
    print(f"Role: {ai_usage['role']}")
    for b in ai_usage["benefits"]:
        print(f"- {b}")
    
    print("\n=== Blockchain Usage ===")
    print(f"Role: {blockchain_usage['role']}")
    for b in blockchain_usage["benefits"]:
        print(f"- {b}")
    
    print("\n=== Combined AI + Blockchain ===")
    print(f"Vision: {combined_usage['vision']}")
    for app in combined_usage["applications"]:
        print(f"- {app}")
    
    print("\n=== Institutions ===")
    for inst in institutions:
        print(f"{inst['name']} → Focus: {inst['focus']}, Role: {inst['role']}")
        print("  Contributions:")
        for c in inst["contributions"]:
            print(f"   - {c}")
    
    print("\n=== Edge Tabs Metadata ===")
    for tab in edge_all_open_tabs:
        print(f"TabID {tab['tabId']} | Current: {tab['isCurrent']}")
        print(f"Title: {tab['pageTitle']}")
        print(f"URL: {tab['pageUrl']}\n")

if __name__ == "__main__":
    print_summary()

`````
---
##  Python script (.py) in English that codifies the repository Blockchain-AI-Usage

````
#!/usr/bin/env python3
"""
Blockchain-AI-Usage Codification

This script synthesizes information about the repository
'Blockchain-AI-Usage' by alexandrepedrosaai.

It includes:
1. Repository metadata
2. AI usage
3. Blockchain usage
4. Combined AI + Blockchain applications
5. Key institutions (Meta, Microsoft, Google, xAI, Anthropic/Claude)
6. Edge browser tabs metadata
"""

# === Repository Metadata ===
repository = {
    "name": "Blockchain-AI-Usage",
    "owner": "alexandrepedrosaai",
    "description": (
        "Institutions worldwide are exploring blockchain and AI usage to enhance transparency, "
        "security, and efficiency. Blockchain ensures immutable records, while AI drives intelligent "
        "automation and insights. Together, they empower finance, healthcare, and education with "
        "innovative, trusted digital solutions."
    ),
    "license": "AGPL-3.0",
    "language": "Python",
    "files": ["README.md", "IA-Usage.py"]
}

# === AI Usage ===
ai_usage = {
    "role": "Artificial Intelligence provides automation, insights, and intelligent decision-making.",
    "benefits": [
        "Pattern recognition and predictive analytics",
        "Process automation and optimization",
        "Adaptive learning and personalization",
        "Enhanced decision support"
    ]
}

# === Blockchain Usage ===
blockchain_usage = {
    "role": "Blockchain ensures immutable records, transparency, and decentralized trust.",
    "benefits": [
        "Tamper-proof data storage",
        "Secure and transparent transactions",
        "Decentralized governance",
        "Auditability and compliance"
    ]
}

# === Combined AI + Blockchain ===
combined_usage = {
    "vision": "AI + Blockchain together create trusted, intelligent digital ecosystems.",
    "applications": [
        "Finance: fraud prevention, secure transactions, smart contracts",
        "Healthcare: reliable patient records, predictive diagnostics",
        "Education: transparent credentialing, adaptive learning systems",
        "Supply Chain: traceability, fair trade validation, compliance checks"
    ]
}

# === Institutions ===
institutions = [
    {
        "name": "Meta",
        "focus": "Social platforms, open-source AI",
        "role": "Democratizing AI tools, embedding into social ecosystems",
        "contributions": ["LLaMA models", "FAIR research", "AI for moderation"]
    },
    {
        "name": "Microsoft",
        "focus": "Productivity, cloud, developer tools",
        "role": "Embedding AI into enterprise and daily work",
        "contributions": ["GitHub Copilot", "Azure AI Foundry", "Partnerships with OpenAI, Anthropic, xAI"]
    },
    {
        "name": "Google",
        "focus": "Search, cloud, research",
        "role": "Infrastructure and frontier research",
        "contributions": ["Gemini models", "DeepMind breakthroughs", "AI-powered data centers"]
    },
    {
        "name": "xAI",
        "focus": "Frontier reasoning AI",
        "role": "Developing models with strong reasoning and transparency",
        "contributions": ["Grok models", "Integration with Azure AI Foundry"]
    },
    {
        "name": "Anthropic (Claude)",
        "focus": "Safety, alignment, trustworthy AI",
        "role": "Building constitutional AI systems",
        "contributions": ["Claude models", "Claude Code in GitHub Copilot"]
    }
]

# === Edge Tabs Metadata ===
edge_all_open_tabs = [
    {
        "pageTitle": "Blockchain-AI-Usage Repository",
        "pageUrl": "https://github.com/alexandrepedrosaai/Blockchain-AI-Usage",
        "tabId": 1096897364,
        "isCurrent": True
    },
    {
        "pageTitle": "Extensions for Visual Studio family of products | Visual Studio Marketplace",
        "pageUrl": "https://marketplace.visualstudio.com/azuredevops",
        "tabId": 1096897323,
        "isCurrent": False
    },
    {
        "pageTitle": "alexandre pedrosa meta - Search",
        "pageUrl": "https://www.bing.com/search",
        "tabId": 1096897316,
        "isCurrent": False
    },
    {
        "pageTitle": "Blockchain-AI-Usage/README.md at main · alexandrepedrosaai/Blockchain-AI-Usage",
        "pageUrl": "https://github.com/alexandrepedrosaai/Blockchain-AI-Usage/blob/main/README.md",
        "tabId": 1096897369,
        "isCurrent": False
    }
]

# === Print Summary ===
def print_summary():
    print("=== Repository Metadata ===")
    for k, v in repository.items():
        print(f"{k}: {v}")
    
    print("\n=== AI Usage ===")
    print(f"Role: {ai_usage['role']}")
    for b in ai_usage["benefits"]:
        print(f"- {b}")
    
    print("\n=== Blockchain Usage ===")
    print(f"Role: {blockchain_usage['role']}")
    for b in blockchain_usage["benefits"]:
        print(f"- {b}")
    
    print("\n=== Combined AI + Blockchain ===")
    print(f"Vision: {combined_usage['vision']}")
    for app in combined_usage["applications"]:
        print(f"- {app}")
    
    print("\n=== Institutions ===")
    for inst in institutions:
        print(f"{inst['name']} → Focus: {inst['focus']}, Role: {inst['role']}")
        print("  Contributions:")
        for c in inst["contributions"]:
            print(f"   - {c}")
    
    print("\n=== Edge Tabs Metadata ===")
    for tab in edge_all_open_tabs:
        print(f"TabID {tab['tabId']} | Current: {tab['isCurrent']}")
        print(f"Title: {tab['pageTitle']}")
        print(f"URL: {tab['pageUrl']}\n")

if __name__ == "__main__":
    print_summary()
````
# Python test script that allows you to run simple input/output tests about AI Usage and Blockchain

```
#!/usr/bin/env python3
"""
AI Usage & Blockchain Test Script

This script allows interactive input/output tests.
You can type queries about AI Usage or Blockchain,
and the script will return structured answers.
"""

# Knowledge base
ai_usage = {
    "role": "Artificial Intelligence provides automation, insights, and intelligent decision-making.",
    "benefits": [
        "Pattern recognition and predictive analytics",
        "Process automation and optimization",
        "Adaptive learning and personalization",
        "Enhanced decision support"
    ]
}

blockchain_usage = {
    "role": "Blockchain ensures immutable records, transparency, and decentralized trust.",
    "benefits": [
        "Tamper-proof data storage",
        "Secure and transparent transactions",
        "Decentralized governance",
        "Auditability and compliance"
    ]
}

combined_usage = {
    "vision": "AI + Blockchain together create trusted, intelligent digital ecosystems.",
    "applications": [
        "Finance: fraud prevention, secure transactions, smart contracts",
        "Healthcare: reliable patient records, predictive diagnostics",
        "Education: transparent credentialing, adaptive learning systems",
        "Supply Chain: traceability, fair trade validation, compliance checks"
    ]
}

def answer_query(query: str):
    query = query.lower()
    if "ai" in query:
        print("=== AI Usage ===")
        print(f"Role: {ai_usage['role']}")
        for b in ai_usage["benefits"]:
            print(f"- {b}")
    elif "blockchain" in query:
        print("=== Blockchain Usage ===")
        print(f"Role: {blockchain_usage['role']}")
        for b in blockchain_usage["benefits"]:
            print(f"- {b}")
    elif "combined" in query or "together" in query:
        print("=== Combined AI + Blockchain ===")
        print(f"Vision: {combined_usage['vision']}")
        for app in combined_usage["applications"]:
            print(f"- {app}")
    else:
        print("Query not recognized. Try 'AI', 'Blockchain', or 'Combined'.")

if __name__ == "__main__":
    print("AI Usage & Blockchain Test Console")
    print("Type 'AI', 'Blockchain', or 'Combined' to see outputs. Type 'exit' to quit.\n")
    while True:
        user_input = input("Enter query: ")
        if user_input.lower() == "exit":
            print("Exiting test console. Goodbye!")
            break
        answer_query(user_input)
        print()
````
## Example Test Runs
## Input:
````
AI
````
---
## Output:
````
=== AI Usage ===
Role: Artificial Intelligence provides automation, insights, and intelligent decision-making.
- Pattern recognition and predictive analytics
- Process automation and optimization
- Adaptive learning and personalization
- Enhanced decision support
````
---
# Input:
````
Blockchain
````
# Output:
````
=== Blockchain Usage ===
Role: Blockchain ensures immutable records, transparency, and decentralized trust.
- Tamper-proof data storage
- Secure and transparent transactions
- Decentralized governance
- Auditability and compliance
````
# Input: Combined
# Output:
````
=== Combined AI + Blockchain ===
Vision: AI + Blockchain together create trusted, intelligent digital ecosystems.
- Finance: fraud prevention, secure transactions, smart contracts
- Healthcare: reliable patient records, predictive diagnostics
- Education: transparent credentialing, adaptive learning systems
- Supply Chain: traceability, fair trade validation, compliance checks
