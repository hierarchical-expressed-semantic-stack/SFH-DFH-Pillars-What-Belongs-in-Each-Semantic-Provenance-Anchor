# 🌐 SFH / DFH — “What Goes in Each Anchor” (Human Version)
### One root file. Ten anchors. A simple first place for AI + search to start.

SFH (Semantic First-Hop) / DFH (Deterministic First-Hop) is **not a truth engine**.
It’s a **deterministic “start here” layer** so machines don’t guess where meaning and provenance begin.

Think of it like this:

- **Your main website** = where humans browse.
- **Your SFH/DFH stack** = a tiny “directory + rules card” for machines.
- **Goal** = give AI a *consistent first hop* that points to the *right* pages, the *canonical identifiers*, and the *official sources*.

---

## ✅ Where this lives (and how it links back to the main website)

### 1) The stack file (the entry door)
This is the single deterministic entrypoint:

`https://yourdomain.com/.well-known/stack`

That file:
- identifies the org/site
- defines what your terms mean
- points to your canonical URLs
- points to your sitemaps and feeds
- declares provenance rules (who/when/license/integrity)

### 2) The main website (the content)
The stack never replaces your site.
It **points back** to your site by linking to:
- canonical pages (the “official” URL for a thing)
- entity pages (your definitions / about pages / product pages)
- URL rules (how you want pages referenced)
- sitemaps (your official content map)
- sources (docs, repositories, policy pages, legal pages, etc.)

So: **SFH/DFH is a map + grounding contract that points back to the real site.**

---

## 🧱 The 10 Anchors (Human-friendly)
You can think of the anchors as two groups:

### Meaning Anchors (5) — “What things are”
These tell machines what you mean when you say words like “product”, “support”, “pricing”, “Jaguar”, etc.

### Provenance Anchors (5) — “Why this should be trusted”
These tell machines how to judge *origin, currency, permission, and tamper resistance*.

---

# 1) MEANING ANCHORS (5)

## /type — “What kinds of things exist here?”
**Purpose:** Define your “content categories” in plain English.

**What goes inside:**
- A short list of types you publish (Organization, Product, Article, Policy, Dataset, Service, Person, Location, etc.)
- For each type:
  - a human description (“what this type means on our site”)
  - the canonical path pattern (where that type lives on your site)
  - which fields matter most (name, sku, version, datePublished, etc.)

**Why AI cares:** If AI sees a page, it can stop guessing “is this a blog post or a policy?” and follow your definitions.

---

## /entity — “The official IDs for your important things”
**Purpose:** Give *stable identifiers* for things you care about.

**What goes inside:**
- Your org entity (the “root identity”)
- Key entities you want AI to recognize deterministically:
  - brands, products, services, categories, authors, locations, policies
- For each entity:
  - a stable ID (URI)
  - canonical URL on your site
  - sameAs links (optional): Wikipedia/Wikidata/official socials/registry pages
  - a short human description

**Why AI cares:** “Which ‘Acme’ is the real one?” → your stack answers with the official entity ID + canonical URL.

---

## /url — “How to treat your URLs”
**Purpose:** Explain your site’s URL rules so machines don’t pick the wrong variant.

**What goes inside:**
- canonical URL rules:
  - www vs non-www
  - http vs https
  - trailing slash rules
  - query parameter rules (what to ignore)
- language/region patterns (if applicable)
- what counts as “official” vs “mirror” vs “campaign link”

**Why AI cares:** Prevents the model from quoting, ranking, or indexing the wrong duplicate URL.

---

## /canonical — “The single source-of-truth page for each thing”
**Purpose:** When there are many pages that *mention* something, this says which page is *the* official one.

**What goes inside:**
- a mapping of entity → canonical URL
- canonical for:
  - org homepage/about page
  - primary product pages
  - pricing page
  - documentation root
  - policies (privacy, terms)
  - press/media kit page
- optional: “canonicalOverrides” for edge cases

**Why AI cares:** If it needs the official description, it knows which page to trust first.

---

## /sitemap — “Your official content map (the big one)”
**Purpose:** Point to the authoritative list(s) of URLs you publish.

**What goes inside:**
- links to your sitemap index and/or specific sitemaps:
  - `https://yourdomain.com/sitemap.xml` (or index)
  - `https://yourdomain.com/sitemaps/products.xml`
  - `https://yourdomain.com/sitemaps/blog.xml`
- for each sitemap:
  - what it covers (products vs docs vs blog)
  - update frequency
  - priority notes (optional)
- optional: feeds / changelogs / APIs that also represent “fresh truth”

**Why AI cares:** This is how AI can quickly find *everything you claim exists*, from the source, without crawling randomly.

---

# 2) PROVENANCE ANCHORS (5)

## /authority — “Who is allowed to speak for this domain?”
**Purpose:** Declare official ownership and authoritative publishers.

**What goes inside:**
- the authoritative org identity
- official maintainers/publishers (teams, departments)
- official communication channels:
  - support email
  - press email
  - verified social accounts (optional)
- if you have multiple brands/subdomains, define who controls what

**Why AI cares:** Helps separate “official statements” from third-party commentary.

---

## /source — “Where the official facts come from”
**Purpose:** List the canonical sources that back claims on the site.

**What goes inside:**
- your internal sources:
  - docs site root
  - repo(s)
  - API docs
  - knowledge base
  - policy/legal pages
- external sources you treat as authoritative for specific areas:
  - standards bodies
  - registries
  - government data
- per source:
  - what it’s used for
  - canonical URL
  - license/terms pointer

**Why AI cares:** When AI needs to “check”, it knows the correct place to verify.

---

## /timestamp — “Freshness rules”
**Purpose:** Help machines understand what’s current vs historical.

**What goes inside:**
- global timestamps:
  - stackPublished
  - stackUpdated
- freshness rules:
  - “pricing is valid only if updated within X days”
  - “blog posts are historical, not policy”
  - “status page is real-time”
- content-specific time fields to prefer:
  - lastmod from sitemap
  - dateModified on pages
  - changelog timestamps

**Why AI cares:** Reduces outdated answers by telling it what “current” means on your domain.

---

## /license — “What can be quoted, reused, or trained on”
**Purpose:** Clarify permissions and constraints.

**What goes inside:**
- site-wide license policy (or “all rights reserved”)
- page-type specific licenses:
  - docs may be CC BY
  - blog may be copyrighted
  - API docs may have a special license
- attribution requirements
- links to Terms of Use / copyright

**Why AI cares:** This tells the system what it’s allowed to reuse and how to attribute.

---

## /integrity — “How to verify the stack hasn’t been tampered with”
**Purpose:** Provide simple integrity checks.

**What goes inside:**
- a content hash of the stack (or each anchor file if split)
- signing method (optional):
  - signature file location
  - public key location
- recommended verification steps (human readable)
- change control notes (who can update, how updates are announced)

**Why AI cares:** Prevents poisoning and gives a deterministic verification step.

---

# 🔗 How the 10 anchors link together (the simple mental model)

Here’s the loop:

1. **AI starts at** `/.well-known/stack`
2. The stack points to the **Meaning anchors**:
   - “what types exist” → `/type`
   - “what entities matter” → `/entity`
   - “how URLs work” → `/url`
   - “which pages are official” → `/canonical`
   - “where the full content map is” → `/sitemap`
3. The stack also points to the **Provenance anchors**:
   - “who speaks for this domain” → `/authority`
   - “where facts come from” → `/source`
   - “what’s fresh/current” → `/timestamp`
   - “what’s allowed legally” → `/license`
   - “how to verify integrity” → `/integrity`
4. From those anchors, AI is guided back to your **main website**:
   - canonical pages (best human pages)
   - sitemaps (full official inventory)
   - source docs (proof / details)

**Result:** the “first hop” is deterministic. Machines stop guessing where meaning begins.

---

# 🗺️ Sitemap: what goes inside + why it becomes the first hop

## What a sitemap is (human version)
A sitemap is your **official list of URLs** you want machines to know about.

### What goes inside a sitemap (conceptually)
Each entry normally provides:
- the URL
- `lastmod` (last modified date)
- sometimes change frequency / priority hints
- optionally images, videos, alternate language versions, etc.

### Why sitemap becomes the “content first hop”
Because it answers one key question immediately:

> “What pages does this domain *claim* exist and want indexed?”

Without sitemaps, AI crawls:
- random internal links
- outdated pages
- duplicate URL variants
- incomplete coverage

With sitemaps (and your /url + /canonical rules), AI can:
- discover the official universe of pages fast
- prioritize the newest (`lastmod`)
- avoid duplicates
- tie pages to entities/types deterministically

**In DFH terms:**  
- `/.well-known/stack` is the **semantic first hop**
- `/sitemap` is the **content map first hop**
Together they form a deterministic on-ramp to your site.

---

# 🧩 Minimal vs Full: when 5 anchors are “enough”
For most sites, the **5 meaning anchors** get you 80% of the value:
- /type, /entity, /url, /canonical, /sitemap

The **full 10** is for:
- enterprises
- regulated industries
- complex brands/subdomains
- high-risk misinformation environments
- teams that need strict provenance and verification

---

# 📌 “Human-friendly” sample layout (what your stack points to)
This is not the full spec—just a friendly mental picture:

- `/.well-known/stack` (the index)
  - links to:
    - `/type`
    - `/entity`
    - `/url`
    - `/canonical`
    - `/sitemap`
    - `/authority`
    - `/source`
    - `/timestamp`
    - `/license`
    - `/integrity`
  - plus:
    - the main website homepage
    - documentation root
    - contact/policy links

---

# ✅ What you should tell humans in one sentence
**“This is a small, official ‘Start Here’ file that tells AI what our site means, which pages are canonical, where the sitemap inventory is, and how to verify provenance—so machines stop guessing and link back to the real website correctly.”**
