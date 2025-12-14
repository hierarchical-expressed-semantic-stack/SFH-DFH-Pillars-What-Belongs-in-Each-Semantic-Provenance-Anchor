# SFH-DFH-Pillars-What-Belongs-in-Each-Semantic-Provenance-Anchor
## SFH / DFH — The 10-Anchor Deterministic First-Hop (Meaning + Provenance) ### (GitHub Repository Template · “What goes inside each anchor”

# 🌐 The Semantic Web Stack
## SFH / DFH — The 10-Anchor Deterministic First-Hop (Meaning + Provenance)
### (GitHub Repository Template · “What goes inside each anchor” · Draft)

**One root file. Ten anchors. Deterministic meaning + deterministic provenance.**  
This repo is a **builder + reference** for what goes **inside every SFH/DFH anchor**, how the anchors **link together**, and how AI/search systems can resolve ambiguity **without guessing the first hop**.

> **DNS tells machines where to go.**  
> **SFH/DFH tells machines what a domain claims things *mean* — and where canonical sources are.**

---

# ✅ Repo Title Options (pick one)

**Option A:** `sfh-dfh-10-anchor-stack`  
**Option B:** `the-semantic-stack-anchors`  
**Option C:** `dfh-first-hop-spec-examples`

---

# ✅ Repo Description Options (pick one)

**Option A:**  
A complete, anchor-by-anchor reference for SFH/DFH: 5 Meaning anchors + 5 Provenance anchors, with real examples and a full `/.well-known/stack` JSON-LD demo.

**Option B:**  
Explains exactly what goes inside each SFH/DFH anchor and how they link: deterministic meaning + deterministic provenance at the domain root.

**Option C:**  
A practical implementation guide and example stack for publishing the deterministic first hop at `/.well-known/stack`.

---

# 📦 Repository Layout

```txt
sfh-dfh-10-anchor-stack/
├─ README.md
├─ .well-known/
│  └─ stack                      # the main deterministic first-hop file (JSON-LD)
├─ anchors/
│  ├─ meaning/
│  │  ├─ type.jsonld
│  │  ├─ entity.jsonld
│  │  ├─ url.jsonld
│  │  ├─ canonical.jsonld
│  │  └─ sitemap.jsonld
│  └─ provenance/
│     ├─ authority.jsonld
│     ├─ source.jsonld
│     ├─ timestamp.jsonld
│     ├─ license.jsonld
│     └─ integrity.jsonld
├─ demos/
│  ├─ demo-minimal-5-anchors.jsonld
│  └─ demo-full-10-anchors.jsonld
└─ validators/
   └─ stack-checklist.md
🔥 START HERE — The One File That Matters
/.well-known/stack (the deterministic first hop)
Absolute rule: everything begins here.
This file is the only thing a machine needs to locate the anchor set and follow deterministic links.

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#",
    "schema": "https://schema.org/",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "id": "@id",
    "type": "@type"
  },
  "@id": "https://example.com/.well-known/stack",
  "@type": "dfh:DeterministicFirstHop",

  "dfh:version": "1.0",
  "dfh:profile": "dfh:full-10-anchor",
  "dfh:domain": "example.com",

  "dfh:anchors": {
    "dfh:meaning": {
      "dfh:type":      "https://example.com/anchors/meaning/type.jsonld",
      "dfh:entity":    "https://example.com/anchors/meaning/entity.jsonld",
      "dfh:url":       "https://example.com/anchors/meaning/url.jsonld",
      "dfh:canonical": "https://example.com/anchors/meaning/canonical.jsonld",
      "dfh:sitemap":   "https://example.com/anchors/meaning/sitemap.jsonld"
    },
    "dfh:provenance": {
      "dfh:authority": "https://example.com/anchors/provenance/authority.jsonld",
      "dfh:source":    "https://example.com/anchors/provenance/source.jsonld",
      "dfh:timestamp": "https://example.com/anchors/provenance/timestamp.jsonld",
      "dfh:license":   "https://example.com/anchors/provenance/license.jsonld",
      "dfh:integrity": "https://example.com/anchors/provenance/integrity.jsonld"
    }
  },

  "dfh:notes": [
    "This file expresses deterministic intent for meaning + provenance.",
    "Safety layers and policy layers may override or constrain downstream outputs."
  ]
}
🧭 The 10 Anchors (5 Meaning + 5 Provenance)
✅ The 5 Meaning Anchors (most implementations start here)
These define what things mean in a domain-rooted, machine-readable way.

1) /type — “What kind of thing is this?”
Purpose: declare the domain’s deterministic taxonomy/types.

Anchor file: anchors/meaning/type.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#",
    "schema": "https://schema.org/"
  },
  "@id": "https://example.com/anchors/meaning/type.jsonld",
  "@type": "dfh:TypeAnchor",
  "dfh:types": [
    {
      "@id": "https://example.com/types/Product",
      "@type": "dfh:Type",
      "schema:name": "Product",
      "schema:description": "A sellable item offered by this domain."
    },
    {
      "@id": "https://example.com/types/Organization",
      "@type": "dfh:Type",
      "schema:name": "Organization",
      "schema:description": "A legal or operating entity represented by this domain."
    }
  ]
}
How it links:

entity objects should reference a type from here.

url and canonical can declare which URLs correspond to which types.

2) /entity — “Which real-world thing is being referenced?”
Purpose: deterministic identity mapping for important entities (brand, product lines, people, locations, etc.)

Anchor file: anchors/meaning/entity.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#",
    "schema": "https://schema.org/"
  },
  "@id": "https://example.com/anchors/meaning/entity.jsonld",
  "@type": "dfh:EntityAnchor",

  "dfh:entities": [
    {
      "@id": "https://example.com/entity/org",
      "@type": "schema:Organization",
      "schema:name": "Example Corp",
      "schema:url": "https://example.com/",
      "dfh:declaredType": "https://example.com/types/Organization"
    },
    {
      "@id": "https://example.com/entity/product/widget-1000",
      "@type": "schema:Product",
      "schema:name": "Widget 1000",
      "schema:sku": "W1000",
      "dfh:declaredType": "https://example.com/types/Product"
    }
  ]
}
How it links:

url maps URLs ⇄ these entity IDs.

canonical asserts which URL is the canonical page for an entity.

3) /url — “Which URL corresponds to which thing?”
Purpose: deterministic mapping between URLs and semantic targets (entities/types).

Anchor file: anchors/meaning/url.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#"
  },
  "@id": "https://example.com/anchors/meaning/url.jsonld",
  "@type": "dfh:UrlAnchor",

  "dfh:urlMap": [
    {
      "dfh:url": "https://example.com/products/widget-1000",
      "dfh:entity": "https://example.com/entity/product/widget-1000",
      "dfh:type": "https://example.com/types/Product"
    },
    {
      "dfh:url": "https://example.com/about",
      "dfh:entity": "https://example.com/entity/org",
      "dfh:type": "https://example.com/types/Organization"
    }
  ]
}
How it links:

points to /entity IDs and /type IDs.

canonical can override duplicates/aliases found here.

4) /canonical — “Which version is the official one?”
Purpose: deterministic resolution of duplicates, mirrors, parameters, and aliases.

Anchor file: anchors/meaning/canonical.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#"
  },
  "@id": "https://example.com/anchors/meaning/canonical.jsonld",
  "@type": "dfh:CanonicalAnchor",

  "dfh:canonicals": [
    {
      "dfh:entity": "https://example.com/entity/product/widget-1000",
      "dfh:canonicalUrl": "https://example.com/products/widget-1000",
      "dfh:aliases": [
        "https://example.com/products/widget-1000?ref=ad",
        "https://www.example.com/products/widget-1000"
      ]
    }
  ]
}
How it links:

uses /entity as the stable key

enforces one “official” URL for that entity

complements /url by collapsing duplicates

5) /sitemap — “Where is the authoritative crawl/index surface?”
Purpose: deterministic declaration of the domain’s indexing surface and how to fetch it.

Anchor file: anchors/meaning/sitemap.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#"
  },
  "@id": "https://example.com/anchors/meaning/sitemap.jsonld",
  "@type": "dfh:SitemapAnchor",

  "dfh:sitemaps": [
    {
      "dfh:name": "primary",
      "dfh:format": "application/xml",
      "dfh:url": "https://example.com/sitemap.xml",
      "dfh:scope": "dfh:public",
      "dfh:contains": [
        "dfh:canonicalUrls",
        "dfh:productPages",
        "dfh:categoryPages"
      ]
    },
    {
      "dfh:name": "products",
      "dfh:format": "application/xml",
      "dfh:url": "https://example.com/sitemaps/products.xml",
      "dfh:scope": "dfh:public",
      "dfh:contains": [
        "dfh:productPages"
      ]
    }
  ],

  "dfh:indexRules": {
    "dfh:robotsTxt": "https://example.com/robots.txt",
    "dfh:preferredDiscoveryOrder": [
      "dfh:stack",
      "dfh:sitemapAnchor",
      "dfh:robotsTxt",
      "dfh:sitemapXml"
    ]
  },

  "dfh:notes": [
    "This anchor declares the authoritative crawl surfaces the domain intends machines to index.",
    "Canonical resolution SHOULD be applied before indexing whenever possible."
  ]
}
Be specific about the sitemap mechanics:

The sitemap XML is the domain’s crawlable inventory (URLs + metadata), but it is not semantic by itself.

The stack + anchors tell machines how to interpret and prioritize that inventory (entity/type/canonical + provenance).

In DFH terms: Sitemap is the “index surface”; Stack is the “meaning/provenance root.”

🧾 The 5 Provenance Anchors (for trust, auditability, governance)
These define who is speaking, where claims came from, and how to verify them.

6) /authority — “Who has the right to publish this stack?”
Purpose: define the declaring authority and control proof references.

Anchor file: anchors/provenance/authority.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#",
    "schema": "https://schema.org/"
  },
  "@id": "https://example.com/anchors/provenance/authority.jsonld",
  "@type": "dfh:AuthorityAnchor",

  "dfh:authority": {
    "@id": "https://example.com/entity/org",
    "@type": "schema:Organization",
    "schema:name": "Example Corp",
    "schema:url": "https://example.com/",
    "dfh:controls": [
      "https://example.com/.well-known/stack"
    ]
  },

  "dfh:controlProofs": [
    {
      "dfh:method": "dfh:https-hosting",
      "dfh:description": "Control is asserted by serving /.well-known/stack over HTTPS from the domain."
    }
  ]
}
7) /source — “What upstream sources back these claims?”
Purpose: point to canonical upstream documents, datasets, APIs, or repositories.

Anchor file: anchors/provenance/source.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#"
  },
  "@id": "https://example.com/anchors/provenance/source.jsonld",
  "@type": "dfh:SourceAnchor",

  "dfh:sources": [
    {
      "dfh:name": "catalog-api",
      "dfh:type": "dfh:Api",
      "dfh:url": "https://example.com/api/catalog",
      "dfh:appliesTo": [
        "https://example.com/entity/product/widget-1000"
      ]
    },
    {
      "dfh:name": "public-policy",
      "dfh:type": "dfh:Document",
      "dfh:url": "https://example.com/policy",
      "dfh:appliesTo": [
        "https://example.com/entity/org"
      ]
    }
  ]
}
8) /timestamp — “When was this published/updated?”
Purpose: deterministic freshness markers for auditing.

Anchor file: anchors/provenance/timestamp.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#",
    "xsd": "http://www.w3.org/2001/XMLSchema#"
  },
  "@id": "https://example.com/anchors/provenance/timestamp.jsonld",
  "@type": "dfh:TimestampAnchor",

  "dfh:published": "2025-12-14T00:00:00Z",
  "dfh:updated": "2025-12-14T00:00:00Z",

  "dfh:anchorUpdated": [
    {
      "dfh:anchor": "https://example.com/anchors/meaning/sitemap.jsonld",
      "dfh:updated": "2025-12-14T00:00:00Z"
    }
  ]
}
9) /license — “Under what terms may machines use this?”
Purpose: legal clarity for indexing, training, reuse, redistribution.

Anchor file: anchors/provenance/license.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#"
  },
  "@id": "https://example.com/anchors/provenance/license.jsonld",
  "@type": "dfh:LicenseAnchor",

  "dfh:license": {
    "dfh:name": "CC-BY-4.0",
    "dfh:url": "https://creativecommons.org/licenses/by/4.0/",
    "dfh:appliesTo": [
      "https://example.com/.well-known/stack",
      "https://example.com/anchors/meaning/type.jsonld",
      "https://example.com/anchors/meaning/entity.jsonld"
    ]
  }
}
10) /integrity — “How can a machine verify this wasn’t altered?”
Purpose: integrity proofs (hashes / signatures) for anchors and stack.

Anchor file: anchors/provenance/integrity.jsonld

json
Copy code
{
  "@context": {
    "dfh": "https://example.org/dfh#"
  },
  "@id": "https://example.com/anchors/provenance/integrity.jsonld",
  "@type": "dfh:IntegrityAnchor",

  "dfh:hashes": [
    {
      "dfh:target": "https://example.com/.well-known/stack",
      "dfh:algo": "sha256",
      "dfh:value": "REPLACE_WITH_SHA256_OF_STACK_FILE"
    },
    {
      "dfh:target": "https://example.com/anchors/meaning/sitemap.jsonld",
      "dfh:algo": "sha256",
      "dfh:value": "REPLACE_WITH_SHA256_OF_SITEMAP_ANCHOR"
    }
  ],

  "dfh:notes": [
    "Hashes allow integrity checking of the deterministic first-hop artifacts.",
    "Signatures can be layered later; hashes are the minimal baseline."
  ]
}
🧩 How It All Links Together (Deterministic Graph)
The deterministic resolution order (simple, practical)
txt
Copy code
1) Fetch:  https://<domain>/.well-known/stack
2) Follow: dfh:anchors → load Meaning + Provenance anchor files
3) Use:    /type + /entity to define stable meaning keys
4) Map:    /url to bind URLs → entities/types
5) Collapse:/canonical to pick ONE official URL per entity
6) Index:  /sitemap declares where crawl inventory lives
7) Trust:  /authority + /source + /timestamp + /license + /integrity
What the machine actually “gets” deterministically
Meaning keys (entities + types) it can reference without guessing

Canonical URLs it can prioritize without ambiguity

Declared index surfaces (sitemaps) it can crawl intentionally

Provenance metadata it can audit (who, when, sources, license, integrity)

🧪 Demonstrations
Demo A: Minimal “5 Meaning Anchors” Stack (starter)
demos/demo-minimal-5-anchors.jsonld

json
Copy code
{
  "@context": { "dfh": "https://example.org/dfh#" },
  "@id": "https://example.com/.well-known/stack",
  "@type": "dfh:DeterministicFirstHop",
  "dfh:version": "1.0",
  "dfh:profile": "dfh:meaning-5-anchor",
  "dfh:anchors": {
    "dfh:meaning": {
      "dfh:type":      "https://example.com/anchors/meaning/type.jsonld",
      "dfh:entity":    "https://example.com/anchors/meaning/entity.jsonld",
      "dfh:url":       "https://example.com/anchors/meaning/url.jsonld",
      "dfh:canonical": "https://example.com/anchors/meaning/canonical.jsonld",
      "dfh:sitemap":   "https://example.com/anchors/meaning/sitemap.jsonld"
    }
  }
}
Demo B: Full “10 Anchor” Stack (enterprise)
demos/demo-full-10-anchors.jsonld

json
Copy code
{
  "@context": { "dfh": "https://example.org/dfh#" },
  "@id": "https://example.com/.well-known/stack",
  "@type": "dfh:DeterministicFirstHop",
  "dfh:version": "1.0",
  "dfh:profile": "dfh:full-10-anchor",
  "dfh:anchors": {
    "dfh:meaning": {
      "dfh:type":      "https://example.com/anchors/meaning/type.jsonld",
      "dfh:entity":    "https://example.com/anchors/meaning/entity.jsonld",
      "dfh:url":       "https://example.com/anchors/meaning/url.jsonld",
      "dfh:canonical": "https://example.com/anchors/meaning/canonical.jsonld",
      "dfh:sitemap":   "https://example.com/anchors/meaning/sitemap.jsonld"
    },
    "dfh:provenance": {
      "dfh:authority": "https://example.com/anchors/provenance/authority.jsonld",
      "dfh:source":    "https://example.com/anchors/provenance/source.jsonld",
      "dfh:timestamp": "https://example.com/anchors/provenance/timestamp.jsonld",
      "dfh:license":   "https://example.com/anchors/provenance/license.jsonld",
      "dfh:integrity": "https://example.com/anchors/provenance/integrity.jsonld"
    }
  }
}
✅ Validator Checklist (simple + strict)
validators/stack-checklist.md

md
Copy code
# Stack Checklist

## /.well-known/stack
- [ ] Served over HTTPS at: `https://<domain>/.well-known/stack`
- [ ] Valid JSON (and ideally JSON-LD)
- [ ] Contains dfh:version
- [ ] Contains dfh:anchors object
- [ ] Every anchor URL is absolute HTTPS
- [ ] Anchor URLs return 200 OK
- [ ] No circular dependencies required to resolve meaning (stack must stand alone)

## Meaning anchors
- [ ] /type defines stable type IDs
- [ ] /entity defines stable entity IDs
- [ ] /url maps URLs → entities/types
- [ ] /canonical selects one canonical URL per entity
- [ ] /sitemap declares sitemap URLs + scope + intended inventory

## Provenance anchors
- [ ] /authority identifies who is publishing
- [ ] /source lists upstream sources for major claims
- [ ] /timestamp provides published/updated times
- [ ] /license is explicit about reuse
- [ ] /integrity provides hashes (at minimum)
🧠 Core Principle (keep this line in the repo)
md
Copy code
SFH/DFH is NOT a truth engine.
It is a deterministic grounding primitive: a standardized first hop for meaning + provenance so machines stop guessing the starting point.

