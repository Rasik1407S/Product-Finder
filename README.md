<div align="center">
  <h1>🛒 Product Finder</h1>
  <p><em>Live price comparison across Amazon India, Flipkart, Croma, and more — inside Claude.</em></p>
</div>

<div align="center">

[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Claude%20AI-orange?style=flat-square)](https://claude.ai)
[![Type](https://img.shields.io/badge/type-Claude%20Skill-purple?style=flat-square)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](https://github.com/YOUR_USERNAME/product-finder/pulls)

</div>

<!-- Add a screenshot or GIF here: assets/demo.png -->

---

## 📋 Table of Contents

- [About](#about)
- [Features](#features)
- [How It Works](#how-it-works)
- [Usage](#usage)
- [Retailers Covered](#retailers-covered)
- [Output Format](#output-format)
- [Installation](#installation)
- [Contributing](#contributing)
- [License](#license)

---

## 🧠 About

Product Finder is a Claude skill that searches India's major online retailers and returns a structured price comparison table. Describe what you want — "Sony soundbar" or "Godrej double-door fridge" — and it queries Amazon India, Flipkart, Croma, Reliance Digital, Tata CLiQ, Vijay Sales, and others in parallel, then presents prices, stock status, ratings, and direct purchase links in one place.

It fetches live data for each search rather than pulling from a cached database, and it marks any value it cannot confirm as "—" rather than filling in a guess. Out-of-stock items still appear in the table, clearly flagged, so you can decide whether to wait or look elsewhere.

The skill covers all product categories — electronics, appliances, furniture, apparel, and niche goods. If it ships to an Indian address, this skill will look for it.

---

## ✨ Features

- 🔍 **Parallel multi-retailer search** — Amazon India, Flipkart, Croma, Reliance Digital, Tata CLiQ, Vijay Sales, and niche stores, all in one query
- 📊 **Structured comparison table** — price, stock, star rating, and a direct product link per retailer
- 💰 **Sale price detection** — shows original and sale price side-by-side: ~~₹12,999~~ ₹9,999
- ⚠️ **Granular stock flags** — ✅ In stock / ❌ Out of stock / ⚠️ Limited, not just a binary available/unavailable
- 🔄 **Optional alternatives table** — asks before generating, then shows similar products in a separate table
- 🧾 **Summary callout** — best price, best availability, and top-rated listing called out after every result
- 🚫 **No fabricated data** — unverified cells are marked "—", not filled with plausible-sounding numbers
- 📅 **Recency flagging** — pricing data older than 60 days is flagged as potentially stale

---

## ⚙️ How It Works

Product Finder is a Claude skill — a structured prompt workflow that gives Claude specific search and comparison behaviour. It uses Claude's built-in `web_search` and `web_fetch` tools to query retailer sites directly.

```
Input → Parse intent → Parallel search across retailers
      → Fetch product pages (price / stock / rating)
      → Ask about alternatives
      → Build comparison table
      → Output summary callout
```

---

## 🚀 Usage

Describe the product in plain language. A brand name plus product type is enough — exact model numbers help but are not required.

**Example prompts:**

```
Find me a Sony 65-inch TV online in India
Where can I buy a Herman Miller Aeron chair in India?
Best price for Dyson V15 on Flipkart or Amazon
Is the Bose QuietComfort 45 available in India?
Compare LG vs Samsung 1.5-ton split AC prices India
Looking for a Kindle under ₹10,000
```

The skill will search the major retailers, ask if you want alternatives included, then return the comparison table and a brief summary.

---

## 🏪 Retailers Covered

| Priority | Retailer | Categories |
|----------|----------|------------|
| 1 | Amazon India | All |
| 2 | Flipkart | All |
| 3 | Croma | Electronics, appliances |
| 4 | Reliance Digital | Electronics, appliances |
| 5 | Tata CLiQ | Electronics, fashion, home |
| 6 | Vijay Sales | Electronics, appliances |
| 7 | Myntra | Apparel, lifestyle |
| 8+ | Niche retailers (e.g. Pepperfry) | Furniture, specialty |

---

## 📄 Output Format

### Exact Match Table

| Retailer | Product Name | Price (₹) | In Stock | Rating | Link |
|----------|-------------|-----------|----------|--------|------|
| Amazon India | Sony HT-S40R | ~~₹19,990~~ ₹14,999 | ✅ Yes | 4.3/5 (1,842) | [View](https://amazon.in) |
| Flipkart | Sony HT-S40R | ₹15,490 | ✅ Yes | 4.1/5 (903) | [View](https://flipkart.com) |
| Croma | Sony HT-S40R | ₹16,999 | ❌ Out of stock | — | [View](https://croma.com) |

### Summary Callout

> **Best price:** Amazon India — ₹14,999  
> **Best availability:** Amazon India & Flipkart — both confirmed in stock  
> **Top-rated:** Amazon India — 4.3/5 from 1,842 reviews  
> **Recommended pick:** Amazon India at ₹14,999 with the highest rating and confirmed stock.

If alternatives are requested, a second table appears below with the same columns plus a "Why It's an Alternative" column explaining the trade-off.

---

## 📦 Installation

Product Finder is a single Markdown skill file. Drop it into your Claude environment and it activates automatically on product search queries.

### Option A — Claude.ai (Projects / Skills)

1. Open [claude.ai](https://claude.ai) and go to your Project or Skills settings.
2. Upload `Product_Finder.md` as a custom skill.
3. Start a conversation and describe any product you want to find.

### Option B — Claude API / Claude Code

```bash
# Clone this repository
git clone https://github.com/YOUR_USERNAME/product-finder.git

# Copy the skill file to wherever your Claude environment reads skills from
cp product-finder/Product_Finder.md /path/to/your/skills/
```

Then reference the skill in your system prompt or tool configuration.

---

## 🤝 Contributing

To add a retailer, improve search query patterns, or fix output formatting:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/add-nykaa-support`
3. Edit `Product_Finder.md` following the existing section structure
4. Commit: `git commit -m 'feat: add Nykaa to retailer list'`
5. Push and open a Pull Request

The core constraints are non-negotiable: no fabricated data, direct product page links only, all prices in ₹ INR.

---

## 📄 License

Distributed under the MIT License. See [LICENSE](LICENSE) for details.

---

## 👤 Author

**Your Name** — [@YOUR_USERNAME](https://github.com/YOUR_USERNAME)

Project Link: [https://github.com/YOUR_USERNAME/product-finder](https://github.com/YOUR_USERNAME/product-finder)
