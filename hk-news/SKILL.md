---
name: hk-news
description: Get latest Hong Kong news headlines from RTHK (Radio Television Hong Kong). Use when the user asks about Hong Kong news, current events, what's happening in HK, latest headlines, or wants a news briefing. Covers local, international, finance, and sports news in English and Chinese.
---

# HK News Skill

Get latest Hong Kong news from RTHK via RSS feeds. No API key needed.

## RSS Feeds

| Category | English | Chinese |
|----------|---------|---------|
| Local 本地 | `http://rthk9.rthk.hk/rthk/news/rss/e_expressnews_elocal.xml` | `http://rthk9.rthk.hk/rthk/news/rss/c_expressnews_clocal.xml` |
| International 國際 | `http://rthk9.rthk.hk/rthk/news/rss/e_expressnews_einternational.xml` | `http://rthk9.rthk.hk/rthk/news/rss/c_expressnews_cinternational.xml` |
| Finance 財經 | `http://rthk9.rthk.hk/rthk/news/rss/e_expressnews_efinance.xml` | `http://rthk9.rthk.hk/rthk/news/rss/c_expressnews_cfinance.xml` |
| Sport 體育 | `http://rthk9.rthk.hk/rthk/news/rss/e_expressnews_esport.xml` | `http://rthk9.rthk.hk/rthk/news/rss/c_expressnews_csport.xml` |

## How to Query

```bash
curl -sL "http://rthk9.rthk.hk/rthk/news/rss/e_expressnews_elocal.xml"
```

## Parsing RSS (Python)

```bash
curl -sL "http://rthk9.rthk.hk/rthk/news/rss/e_expressnews_elocal.xml" | python3 -c "
import sys, xml.etree.ElementTree as ET
root = ET.parse(sys.stdin).getroot()
for item in root.findall('.//item')[:10]:
    title = item.find('title').text
    link = item.find('link').text
    desc = item.find('description').text or ''
    print(f'📰 {title}')
    print(f'   {desc[:100]}')
    print(f'   {link}')
    print()
"
```

## Response Fields (per item)

- `title` — Headline
- `link` — Full article URL
- `description` — Brief summary
- `pubDate` — Publication date/time

## Presentation Tips

- Show 5-10 headlines by default
- Group by category if user asks for "all news"
- Include article links for further reading
- Use 📰 emoji for headlines
- Match language to user's preference (English feeds for English, Chinese for Chinese)
