# 🌐 HESS / DFH — “What Goes in Each Anchor” (Human Version)
### One root file. Ten anchors. A simple first place for AI + search to start.

Hess (Semantic First-Hop) / DFH (Deterministic First-Hop) is **not a truth engine**.
It’s a **deterministic “start here” layer** so machines don’t guess where meaning and provenance begin.

🧱 HESS / DFH — The 5 Mandatory Meaning Anchors (Implementer Guide)



Goal: publish one deterministic “first hop” for meaning at:



https://yourdomain.com/.well-known/stack



That single file points to 5 meaning anchors machines can fetch immediately.



DNS tells machines where to go.


HESS / DFH tells machines what it means when they get there.

✅ The 5 Mandatory Meaning Anchors (What each anchor answers)
Anchor	Answers	What goes inside (in plain English)


/type	“What class of thing is this topic?”	Taxonomy / ontology classification (JSON-LD)


/entity	“What is the noun / entity?”	The primary entity record(s) with stable IDs


/url	“Where does this meaning live?”	Canonical URL bindings for the entity / key routes


/canonical	“What do we call it — and what is it NOT?”	Canonical label + aliases + ambiguity boundaries


/sitemap	“What is the crawl surface (start here)?”	A declared list of crawl entrypoints / conceptual surfaces (NOT a URL dump)


Rule: These anchors declare meaning + intent, not “truth.”


Downstream systems can accept, reject, weight, or override.

📁 Minimal Directory Layout
yourdomain.com/
├─ .well-known/
│  └─ stack                      <-- root descriptor (JSON-LD)
├─ type/
│  └─ index.jsonld
├─ entity/
│  └─ index.jsonld
├─ url/
│  └─ index.jsonld
├─ canonical/
│  └─ index.jsonld
├─ sitemap/
│  └─ index.jsonld               <-- DFH sitemap anchor (JSON-LD)
└─ sitemap.xml                   <-- traditional XML sitemap (optional but common)

1) /.well-known/stack — Root Descriptor (Bootstrap)

This is the only mandatory discovery file.
It should stay tiny and stable.

{
  "@context": { "dfh": "https://example.org/ns/dfh#" },
  "@id": "https://yourdomain.com/.well-known/stack",
  "@type": "dfh:DeterministicSemanticRoot",
  "dfh:anchors": {
    "dfh:type": "https://yourdomain.com/type/index.jsonld",
    "dfh:entity": "https://yourdomain.com/entity/index.jsonld",
    "dfh:url": "https://yourdomain.com/url/index.jsonld",
    "dfh:canonical": "https://yourdomain.com/canonical/index.jsonld",
    "dfh:sitemap": "https://yourdomain.com/sitemap/index.jsonld"
  }
}

2) /type — What class of thing is this?

Purpose: declare the topic’s classification using stable vocabularies.

{
  "@context": {
    "schema": "https://schema.org/",
    "dfh": "https://example.org/ns/dfh#"
  },
  "@id": "https://yourdomain.com/type/index.jsonld",
  "@type": "dfh:TypeAnchor",
  "dfh:domainRepresents": [
    { "@id": "schema:Thing" },
    { "@id": "schema:CreativeWork" }
  ],
  "dfh:primaryTopic": "beer"
}

3) /entity — What is the noun / entity?

Purpose: define the primary entity with a stable ID.

{
  "@context": {
    "schema": "https://schema.org/",
    "dfh": "https://example.org/ns/dfh#"
  },
  "@id": "https://yourdomain.com/entity/index.jsonld",
  "@type": "dfh:EntityAnchor",
  "dfh:items": [
    {
      "@id": "urn:dfh:entity:beer",
      "@type": "schema:Product",
      "schema:name": "Beer",
      "schema:description": "A fermented malt beverage produced from cereal grains, water, hops, and yeast, typically containing alcohol by volume as defined by applicable law."
    }
  ]
}

4) /url — Where meaning lives (domain-owned)

Purpose: bind the entity to the canonical URLs you control.

{
  "@context": { "dfh": "https://example.org/ns/dfh#" },
  "@id": "https://yourdomain.com/url/index.jsonld",
  "@type": "dfh:UrlAnchor",
  "dfh:items": [
    { "entity": "urn:dfh:entity:beer", "url": "https://yourdomain.com/", "rel": "canonical" },
    { "entity": "urn:dfh:entity:beer", "url": "https://yourdomain.com/definition", "rel": "definition" },
    { "entity": "urn:dfh:entity:beer", "url": "https://yourdomain.com/types", "rel": "taxonomy" }
  ]
}


Key point: DFH only works if the first hop lands on a domain you control, because authority cannot be rooted in a domain you don’t own.

5) /canonical — What to call it, and what it is NOT (ambiguity boundary)

Purpose: prevent “Beer” from being mixed with other meanings or contexts.

{
  "@context": { "dfh": "https://example.org/ns/dfh#" },
  "@id": "https://yourdomain.com/canonical/index.jsonld",
  "@type": "dfh:CanonicalAnchor",
  "dfh:items": [
    {
      "entity": "urn:dfh:entity:beer",
      "canonicalLabel": "Beer",
      "aliases": ["Malt beverage", "Brewed beer"],
      "notThis": [
        "brewery (a business/organization)",
        "medical advice about alcohol",
        "legal guidance",
        "rankings/opinions/reviews"
      ]
    }
  ]
}

6) /sitemap — The part everyone confuses (so here’s the final rule)
✅ What /sitemap IS

/sitemap is the DFH “crawl declaration.”
It says:

“Start here. These are the official crawl entrypoints / conceptual surfaces for this topic.”

It is a semantic directory, not a URL list.

❌ What /sitemap is NOT

NOT your XML sitemap contents

NOT a page list

NOT navigation

NOT “SEO links”

NOT “every URL on the site”

✅ What you put in /sitemap

You put entrypoints — usually:

your sitemap.xml (traditional crawler entrypoint)

optionally other declared surfaces (like a taxonomy feed, entity feed, docs index, etc.)

Example /sitemap anchor (correct)
{
  "@context": { "dfh": "https://example.org/ns/dfh#" },
  "@id": "https://yourdomain.com/sitemap/index.jsonld",
  "@type": "dfh:SitemapAnchor",
  "dfh:entrypoints": [
    {
      "id": "xml-sitemap",
      "url": "https://yourdomain.com/sitemap.xml",
      "kind": "sitemap"
    },
    {
      "id": "types-surface",
      "url": "https://yourdomain.com/types",
      "kind": "concept-surface"
    },
    {
      "id": "definition-surface",
      "url": "https://yourdomain.com/definition",
      "kind": "concept-surface"
    }
  ]
}

What goes in sitemap.xml then?

That’s the normal XML sitemap: a list of URLs you want indexed.

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url><loc>https://yourdomain.com/</loc></url>
  <url><loc>https://yourdomain.com/definition</loc></url>
  <url><loc>https://yourdomain.com/types</loc></url>
</urlset>

🔗 Deterministic Fetch Flow (no confusion)

Fetch: https://yourdomain.com/.well-known/stack

Get the 5 anchor URLs

Fetch anchors:

/type → classification

/entity → entity identity

/url → canonical bindings

/canonical → naming + NOT-this boundaries

/sitemap → declared crawl entrypoints

Optional: then crawl the entrypoints (like sitemap.xml) normally

🍺 Beer Example (your “5 domains” phrasing, but clean)

If you want a simple mental model for humans:

Type = what class of thing

Entity = what noun it is

URL = where the authoritative meaning lives (your owned domain)

Canonical = what it’s called + what it is NOT

Sitemap = where to crawl first (declared entrypoints / concept surfaces)

The killer line that removes confusion:

/sitemap is a directory of “start here” entrypoints — not the sitemap itself.
It points to crawl surfaces; it does not enumerate them.

Think of it like this:

- **Your main website** = where humans browse.
- **Your HESS/DFH stack** = a tiny “directory + rules card” for machines.
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

So: **HESS/DFH is a map + grounding contract that points back to the real site.**

---

## 🧱 The 5 AND 10 Anchors (Human-friendly)
You can think of the anchors as two groups:

### Meaning Anchors (5) — “What things are” this covers 90 percent of topics
These tell machines what you mean when you say words like “product”, “support”, “pricing”, “Jaguar”, etc.

### Provenance Anchors (5) — “Why this should be trusted” this is for big companies or major topics that need provenance
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
