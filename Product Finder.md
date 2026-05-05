---
Name: Product-Finder
Description: >
  Find and compare products available online in India across all major Indian
  retailers (Amazon India, Flipkart, Croma, Reliance Digital, Tata CLiQ, Myntra,
  Vijay Sales, and others). Use this skill whenever the user wants to search for
  a product to buy online in India, check prices or availability, find the best
  deal, or discover alternatives. Trigger on any input resembling a product
  name, brand + description, or purchase intent — even casual phrasing like
  "looking for a Samsung TV" or "where can I buy a good sofa online". Always
  use this skill before answering product availability or pricing questions
  related to India.
---

# Product Finder — India

A skill for finding, comparing, and evaluating products available for purchase
online in India. Covers all product categories: consumer electronics, home &
furniture, specialty/niche items, and general merchandise.

---

## Workflow

### Step 1 — Parse & Validate Input

Accept any product description. The expected input level is **moderate**:
brand name + general description (e.g., "Sony soundbar", "Godrej refrigerator
double door", "Herman Miller chair"). Full model numbers are a bonus, not
a requirement.

**If input is ambiguous** (no brand, no category, or contradictory specs),
ask a single focused clarifying question before proceeding. Do not ask multiple
questions at once. Examples of ambiguity that warrant clarification:

- "laptop" → ask: "Any preferred brand or key spec (budget, screen size,
  use case)?"
- "good chair" → ask: "Office/gaming chair, or dining/occasional?"
- "TV 55 inch" → ask: "Any brand preference, or open to all options?"

If the input is reasonably specific (brand + product type), **search immediately
without asking**.

---

### Step 2 — Search

Use `web_search` to query across Indian retailers. Run **parallel searches**:

1. `<brand> <product> price India site:amazon.in`
2. `<brand> <product> price India site:flipkart.com`
3. `<brand> <product> buy online India Croma OR "Reliance Digital" OR "Tata CLiQ" OR "Vijay Sales"`
4. `<brand> <product> best price India 2025` (aggregator/comparison catch-all)

Fetch product pages with `web_fetch` when snippet data is insufficient to
confirm price, availability, or rating.

**Retailers to cover** (in priority order):
1. Amazon India (amazon.in)
2. Flipkart
3. Croma
4. Reliance Digital
5. Tata CLiQ
6. Vijay Sales
7. Myntra (apparel/lifestyle)
8. Other relevant niche retailers if applicable (e.g., Pepperfry for furniture)

---

### Step 3 — Ask About Alternatives

Before presenting results, **always ask**:

> "Would you like me to include alternative products alongside the exact
> matches?"

Wait for the user's response, then proceed to Step 4.

---

### Step 4 — Build the Comparison Table

Present results as a **structured markdown table**. Include all found listings
for the exact product, and (if requested) alternatives in a separate table
beneath.

#### Exact Match Table

| Retailer | Product Name | Price (₹) | In Stock | Rating | Link |
|----------|-------------|-----------|----------|--------|------|
| Amazon India | … | ₹X,XXX | ✅ Yes / ❌ No / ⚠️ Limited | X.X/5 | [View](...) |
| Flipkart | … | ₹X,XXX | ✅ Yes | X.X/5 | [View](...) |
| Croma | … | ₹X,XXX | ❌ Out of stock | — | [View](...) |

**Column rules:**
- **Price**: Show exact listed price in INR. If a sale/offer price exists,
  show it with the original struck through: ~~₹12,999~~ ₹9,999.
- **In Stock**: ✅ Yes / ❌ No / ⚠️ Limited (when "only X left" is shown).
  If unavailable to verify, write "—".
- **Rating**: Retailer's own star rating out of 5, with review count in
  parentheses where available: e.g., `4.3/5 (1,842)`. Write "—" if absent.
- **Link**: Always include a direct product page link.

#### Alternatives Table (if requested)

Use the same column format. Add one extra column:

| … | Why It's an Alternative |
|---|------------------------|
| … | Similar specs, ₹3,000 cheaper |
| … | Higher-rated, different brand |

---

### Step 5 — Summary Callout

After the table(s), add a brief 2–3 line summary:

- **Best price**: Retailer + price
- **Best availability**: Retailer with confirmed stock
- **Top-rated listing**: Retailer + rating
- **Recommended pick** (if clear winner exists): One sentence, no hedging.

---

## Rules & Constraints

- **Never fabricate prices, ratings, or availability.** If data cannot be
  confirmed via search/fetch, mark the cell as "—" and note the data gap.
- **Always link directly** to the product page, not to a search results page.
- **Currency**: Always use ₹ (INR). Never convert to other currencies unless
  explicitly asked.
- **Recency**: Prefer search results from the past 30 days. Flag if pricing
  data appears stale (older than 60 days).
- **Out-of-stock items**: Still include them in the table — clearly marked ❌ —
  so the user can make an informed decision or set up an alert.
- **Do not recommend retailers** over each other for any reason other than
  objective data (price, rating, availability).

---

## Trigger Phrases (Examples)

This skill should activate on inputs such as:

- "Find me a [brand] [product] online in India"
- "Where can I buy [product] in India?"
- "Best price for [product] on Flipkart or Amazon"
- "Is [product] available in India?"
- "Compare [product A] vs [product B] prices India"
- "Looking for a [category] under ₹X,XXX"
- Any product name/brand followed by purchase intent words: buy, price,
  available, cheapest, compare, deals

---

## Edge Cases

| Situation | Behavior |
|-----------|----------|
| Product not available in India | State clearly; offer to search grey-market or global alternatives if user agrees |
| Only one retailer has stock | Present single-row table; note limited availability |
| Price varies by seller/variant | Show price range (₹X – ₹Y) and note variant dependency |
| Product discontinued | State so; pivot immediately to alternatives |
| User provides a URL | Fetch it directly, extract data, then cross-check other retailers |