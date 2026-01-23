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
