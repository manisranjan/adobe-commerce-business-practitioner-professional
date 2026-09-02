# Adobe Commerce Business Practitioner Professional
## Section 5: Adobe Commerce Features and Services (18%)

> Weighting: **18%** of the exam — one of the heaviest sections. Expect scenario-based questions where you must pick the *best* structure or feature, not just recall a definition.

---

## 5.1 Website / Store / Store View Structure

### Core hierarchy (memorize this order)

```
Global (Default Config)
  └── Website
        └── Store (a.k.a. "Store Group")
              └── Store View
```

| Level | Also called | What it controls | Key rule |
|-------|-------------|------------------|----------|
| **Global** | Default config | System-wide defaults | One per installation |
| **Website** | — | **Customers, payment, shipping, tax, base currency** | Customer accounts are shared/scoped **here by default** |
| **Store** | Store Group | **Root category** (catalog structure) & default store view | Every store has exactly **one root category** |
| **Store View** | — | **Language / locale & presentation** | Each store view = one language/presentation of the same catalog |

> **⚠️ Exam Trap:** Adding a language means adding a store *view*, not a new website or store — reaching for extra websites here needlessly duplicates customers, currency, and catalog.

### What is scoped where (this is the #1 tested concept)

- **Website scope**: base currency, customer accounts (share option), payment methods, shipping, tax rules, product prices (in "Website" price scope), inventory (if not global).
- **Store (group) scope**: the **root category** — determines which catalog/category tree is available.
- **Store View scope**: **language/locale**, product name/description translations, CMS content translations, allowed currencies for display, product visibility per view.

> **⚠️ Exam Trap:** "Different currency" or "separate customer base" almost always signals **separate websites**, because base currency and customer accounts are both website-scoped — store views only change presentation.

### Decision scenarios

| Business need | Correct structure |
|---------------|-------------------|
| Same catalog, **multiple languages** (e.g. English + French, same products/prices) | **One website → one store → multiple store views** (one per language) |
| **Different catalogs / different brands** (separate product sets) | **Multiple websites** (or multiple stores with different root categories) |
| **Different base currency** per region | **Separate websites** (base currency is website-scoped) |
| **Separate customer bases** (customers of Brand A shouldn't log in to Brand B) | **Separate websites** (customer accounts are website-scoped) |
| Same brand, same currency, **different top categories** per region | One website → **multiple stores** (each with its own root category) |
| Share customer login across all sites (single sign-on across brands) | Set **Customer Account Sharing = Global** (Stores → Config → Customer Config) |

### Key configuration flags

- **Customer Account Sharing**: `Global` (one account works everywhere) vs `Per Website` (default — account tied to the website it was created on).
- **Catalog Price Scope**: `Global` (one price everywhere) vs `Website` (price can differ per website). Set under Stores → Config → Catalog → Catalog → Price.
- **Base URLs**: can be set per website/store view to route domains to the right scope.

---

## 5.2 Admin Roles and Permissions

### Where it lives
**System → Permissions → User Roles** and **System → Permissions → All Users**.

### The two building blocks

1. **User Role** — a named set of permissions (Access + Resources + optional scope restriction).
2. **User** — an admin account **assigned to exactly one role**.

### Creating a role — the parts

| Tab / field | Purpose |
|-------------|---------|
| **Role Info** | Name the role; re-enter *your* password to confirm changes. |
| **Role Resources → Resource Access** | `All` (everything) or `Custom` (tick specific ACL resources in the tree). |
| **Role Scopes** | Restrict role to specific **websites/store views** (Adobe Commerce feature for scoped admins). |

### Important rules & traps

- A user has **one role only**; permissions are the union defined by that role.
- **Resource Access = Custom** lets you build least-privilege roles (e.g. a "Catalog Manager" who only sees Catalog + Products).
- **Role Scopes** (limiting a role to certain websites) is an **Adobe Commerce** capability — useful for multi-brand teams where a marketer manages only one website.
- Changing your own role's resources requires **password confirmation** (a guard against lockout/mistakes).
- **Action Logs / Admin Actions Log** (Reports → find under Admin Actions) is **Adobe Commerce only** — tracks what each admin user did. Good for auditing.
- **Two-Factor Authentication (2FA)** is enforced by default for admin in modern versions (both Open Source and Commerce).
- **Admin Session Lifetime** and **password lifetime/lockout** are set in Stores → Config → Advanced → Admin → Security.

> **⚠️ Exam Trap:** **Custom Resource Access** limits *which features* an admin can use, while **Role Scopes** limits *which websites/store views* — don't answer a "restrict to one website" question with Resource Access.

---

## 5.3 Magento Open Source vs Adobe Commerce vs Adobe Commerce Cloud

### One-line definitions

- **Magento Open Source**: free, self-hosted, community-supported e-commerce platform (core features).
- **Adobe Commerce**: licensed (paid) — Open Source **plus** advanced B2B, marketing, targeting, and support. Can be self-hosted (on-prem) **or** cloud.
- **Adobe Commerce Cloud (Cloud infrastructure / PaaS)**: Adobe Commerce **delivered on a managed cloud platform** (hosting, CDN via Fastly, deployment pipeline, cloud tooling).

### Feature comparison (high-yield table)

| Feature | Open Source | Adobe Commerce | Commerce on Cloud |
|---------|:-----------:|:--------------:|:-----------------:|
| Cost | Free | Licensed | Licensed + hosting |
| Catalog & basic cart/checkout | ✅ | ✅ | ✅ |
| Customer segmentation & targeting | ❌ | ✅ | ✅ |
| **Price rules — Cart & Catalog** (basic) | ✅ | ✅ | ✅ |
| **Related products by rules / Rule-based product relations** | ❌ | ✅ | ✅ |
| **Content Staging & Preview** | ❌ | ✅ | ✅ |
| **Page Builder** | ✅ (now in OS too) | ✅ | ✅ |
| **B2B features** (company accounts, shared catalogs, quotes, requisition lists) | ❌ | ✅ | ✅ |
| **Gift Cards, Gift Registry, Reward Points, Store Credit** | ❌ | ✅ | ✅ |
| **RMA (Return Merchandise Authorization)** | ❌ | ✅ | ✅ |
| **Customer/Order Attributes (custom)** | ❌ | ✅ | ✅ |
| **Advanced/Elasticsearch reporting, Admin Action Logs** | Limited | ✅ | ✅ |
| **Visual Merchandiser & Rule-based category** | ❌ | ✅ | ✅ |
| **Commerce Intelligence (MBI)** | ❌ | ✅ (included tier) | ✅ |
| **Fastly CDN + WAF, managed hosting, cloud CLI** | ❌ | ❌ (self-host) | ✅ |
| Adobe support & SLA | Community | ✅ | ✅ |

> **⚠️ Exam Trap:** **Commerce on Cloud** is not a separate feature set — it's Adobe Commerce plus managed infrastructure, so never pick "Cloud" as the answer to a *feature* (edition) question.

### Traps

- **Page Builder** is now available in Open Source too — don't assume it's Commerce-only on a modern version.
- **B2B, Content Staging, Gift Cards, RMA, Reward Points, Store Credit, Gift Registry, rule-based related products** = **Adobe Commerce** (not Open Source). These are the classic "which edition?" answers.
- **Commerce on Cloud** is **not a different product** — it's Adobe Commerce + managed cloud infra (Fastly, New Relic, cloud Git-based deploys, 3 environments: Integration/Staging/Production on Pro).
- "Cloud" differences are about **infrastructure/hosting/deployment**, not commerce feature functionality.

---

## 5.4 CMS and Personalization Features

### CMS building blocks

| Feature | What it does | Edition |
|---------|--------------|---------|
| **CMS Pages** | Standalone content pages (About Us, etc.), scoped by store view | Both |
| **CMS Blocks** | Reusable content chunks, placed via widgets/layout/XML | Both |
| **Widgets** | Insert dynamic content (product lists, CMS block, links) into pages/blocks/layout | Both |
| **Page Builder** | Drag-and-drop visual content editor (rows, columns, banners, sliders, HTML) | Both (modern) |
| **Design/Theme config** | Store-view-scoped theme, header/footer, logo | Both |

### Personalization & targeting (mostly Adobe Commerce)

| Feature | What it does | Edition |
|---------|--------------|---------|
| **Customer Segments** | Dynamically group customers by attributes/behavior (order history, cart, address, etc.) | **Commerce** |
| **Related Products Rules / Rule-based relations** | Auto-suggest up-sell/cross-sell by rules | **Commerce** |
| **Dynamic Blocks** (formerly "Banners") | Content shown to specific **customer segments** or by rules | **Commerce** |
| **Content Staging & Preview** | Schedule content/price changes with a timeline & preview future state | **Commerce** |
| **Price Rules — Catalog Price Rule** | Adjust product prices by conditions (can target segments in Commerce) | Both (segment targeting = Commerce) |
| **Price Rules — Cart Price Rule (Coupons)** | Discounts at cart by conditions/coupon; can target **customer segments** | Both (segment targeting = Commerce) |
| **Visual Merchandiser** | Rule-based auto-sorting of products in categories | **Commerce** |

### Key concepts to distinguish

- **CMS Block vs Widget vs Dynamic Block**:
  - *CMS Block* = static reusable content.
  - *Widget* = mechanism to place content/functionality into a location.
  - *Dynamic Block* = **segment-targeted** content (personalization) — **Commerce only**.

> **⚠️ Exam Trap:** A **CMS Block** shows the same content to everyone (both editions); only a **Dynamic Block** targets a Customer Segment — so "show only to VIPs" is never a plain CMS Block.

- **Customer Segment** drives personalization: used by Dynamic Blocks, Cart Price Rules, Catalog Price Rules, and reports.
- **Content Staging**: create a **Campaign/Update** with start/end dates; use **Preview** to see the site at a future date. Applies to CMS pages, blocks, categories, products, price rules.

### Scenario cues

- "Show a banner **only to VIP customers**" → **Dynamic Block + Customer Segment** (Commerce).
- "Schedule a homepage change for **Black Friday, auto-revert after**" → **Content Staging** (Commerce).
- "Reusable footer content across pages" → **CMS Block + Widget** (Both).
- "Drag-and-drop landing page" → **Page Builder** (Both, modern).

---

## 5.5 Reports, Import/Export, and Commerce Services

### Reports (Reports menu)

| Report group | Examples | Notes |
|--------------|----------|-------|
| **Marketing** | Products in Cart, Search Terms, Abandoned Carts, Newsletter, Cart Price Rules (coupon usage) | Search Terms help catalog/SEO decisions |
| **Reviews** | By Products, By Customers | |
| **Sales** | Orders, Tax, Invoiced, Shipping, Refunds, Coupons | Many need **statistics refresh** |
| **Customers** | Order Total, Order Count, New, Now Online, **Wish Lists**, **Segments** | Segments report = **Commerce** |
| **Products** | Ordered, Viewed (Bestsellers, Most Viewed), Low Stock, Downloads | |
| **Statistics** | **Refresh Statistics** (lifetime vs by day) | Must refresh for Sales reports to be current |
| **Business Intelligence** | Advanced Reporting (free tier) & **Commerce Intelligence / MBI** | Cloud-based dashboards |

**Traps**
- Many **Sales reports rely on refreshed statistics** — if numbers look stale, run **Reports → Statistics → Refresh Statistics**.
- **Advanced Reporting** is a free, cloud-based dashboard (needs a Base URL + the reporting service enabled); **Commerce Intelligence (MBI)** is the deeper paid analytics platform.
- **Customer Segment reports** are **Adobe Commerce only**.

> **⚠️ Exam Trap:** Stale Sales report totals are fixed by **Reports → Statistics → Refresh Statistics** — reindexing or flushing the cache will *not* update report aggregates.

### Import / Export (System → Data Transfer)

| Aspect | Detail |
|--------|--------|
| **Format** | **CSV** (import & export) |
| **Entity types** | Products, Customers (and addresses), Customer Finance (Commerce), Advanced Pricing, Stock Sources, etc. |
| **Import behaviors** | **Add/Update**, **Replace**, **Delete** |
| **Validation** | Import validates the file first and reports errors before executing |
| **Images** | Product images imported from a media folder path referenced in the CSV |
| **Export** | Choose entity + attribute filters → generates CSV |

**Traps**
- Import file **must be CSV**; large imports typically use the **Import** UI or CLI/API.
- **Add/Update** merges; **Replace** removes existing entities and re-adds; **Delete** removes matched rows — know the difference.
- The **first row = column headers**; required columns (e.g. `sku` for products) must be present.

### Commerce Services (SaaS services layer)

| Service | Purpose |
|---------|---------|
| **Adobe Commerce Intelligence (MBI)** | BI dashboards & analytics |
| **Live Search** (Adobe Sensei-powered) | AI search & merchandising (replaces basic search; SaaS) |
| **Product Recommendations** | Sensei-powered recs (e.g. "customers also viewed") |
| **Catalog Service / Commerce Services** | GraphQL storefront catalog data, SaaS-backed |
| **Payment Services** | Adobe-native payment processing |
| **Advanced Reporting** | Free cloud reporting dashboard |

**Traps**
- **Live Search** and **Product Recommendations** are **Sensei/SaaS services** — connected via **Commerce Services / API keys** and available to Adobe Commerce (and, for some, Open Source with an account). They require SaaS data export (Data Ingestion / SaaS catalog sync).
- These are **not** the same as the on-prem Elasticsearch/OpenSearch that powers default catalog search.

> **⚠️ Exam Trap:** **Live Search** and **Product Recommendations** are Sensei-powered **SaaS** services needing Commerce Services connection and SaaS data sync — they are not the on-prem search engine and don't replace your database.

---

## 5.6 Indexing, Cache & Cron (Why Changes Don't Appear)

A recurring theme across the whole exam: a merchant makes a change in Admin but the storefront doesn't reflect it. The cause is almost always **indexing, cache, or cron**.

**Indexing**
- Commerce precomputes complex data (prices, catalog rules, product/category relations, stock, search) into **index tables** for fast storefront reads.
- **Index Mode**: **Update on Save** (reindex immediately when data changes) vs **Update on Schedule** (reindex in batches via cron — recommended for larger catalogs).
- Managed under **System > Index Management** (or `bin/magento indexer:reindex`).
- Classic symptom: a **Catalog Price Rule** or price change not showing until the relevant indexer runs.

**Cache**
- Full Page Cache (FPC) and other cache types store rendered output/config for speed.
- After certain changes, affected caches must be **flushed/refreshed** (System > Cache Management) before the storefront updates.
- On Adobe Commerce Cloud, **Fastly** provides the CDN/FPC layer.

**Cron**
- The **cron scheduler** drives background jobs: scheduled reindexing, **transactional email sending**, Content Staging updates going live/reverting, report statistics, catalog price rule application, and more.
- A stalled cron is the root cause of many "nothing is happening on schedule" symptoms — emails not sent, staged updates not applying, scheduled reindex not running.

> **⚠️ Exam Trap:** "I changed it but the storefront still shows the old value" → think **reindex** (for catalog/price/search data) or **cache flush** (for rendered pages/config), not a data-entry error. "Scheduled or background action didn't happen" (emails, staged content, scheduled reindex) → think **cron**. Refreshing report totals is **Refresh Statistics**, a separate action from reindexing.

---

## Quick-Reference "Edition" Cheat Sheet (highest-yield)

**Adobe Commerce–only (NOT Open Source):**
Customer Segments · Dynamic Blocks · Content Staging & Preview · Rule-based Related Products · Visual Merchandiser · B2B (Company accounts, Shared Catalogs, Quotes, Requisition Lists) · Gift Cards · Gift Registry · Reward Points · Store Credit · RMA · Customer/Order custom attributes · Admin Action Logs · Segment reports.

**Both editions:**
CMS Pages/Blocks · Widgets · Page Builder (modern) · Catalog & Cart Price Rules (basic) · Import/Export CSV · Standard reports · Multi-website/store/store-view.

---

# MCQ — Readiness Check

> Each answer is hidden right below its question. Try to answer first, then expand **Answer**.

**Q1.** A merchant sells the **same products at the same prices** but needs the store in **English and Spanish**. What is the correct structure?
- A) Two websites
- B) One website, two stores
- C) One website, one store, two store views
- D) Two stores, one store view each

<details><summary>Answer & Explanation</summary>

**Answer:** C — One website, one store, two store views

**Explanation:** Same catalog and prices with only the language differing means you only need additional store views; language/locale, product translations, and CMS content are all store-view-scoped.

**Exam Trap:** A different language does not require a new website or store — choosing "two websites" over-engineers the setup and wrongly duplicates customers, base currency, and catalog.

</details>

**Q2.** Which setting must change so a customer can log in with the **same account across two different websites**?
- A) Catalog Price Scope → Website
- B) Customer Account Sharing → Global
- C) Base Currency → Global
- D) Share Catalog → Enabled

<details><summary>Answer & Explanation</summary>

**Answer:** B — Customer Account Sharing → Global

**Explanation:** Customer accounts are website-scoped by default, so the same email exists independently per website; setting Customer Account Sharing to Global lets one account log in across all websites.

**Exam Trap:** Catalog Price Scope and Base Currency have nothing to do with login sharing — only the Customer Account Sharing option controls cross-website accounts.

</details>

**Q3.** Base **currency** is configured at which scope?
- A) Store View
- B) Store (Group)
- C) Website
- D) Global only

<details><summary>Answer & Explanation</summary>

**Answer:** C — Website

**Explanation:** Base currency is a website-scoped setting, which is exactly why regions that need different base currencies must be split into separate websites.

**Exam Trap:** Store views control the *displayed/allowed* currencies for presentation, but the base currency itself is fixed at the website level, not the store view.

</details>

**Q4.** The **root category** is defined at which level?
- A) Website
- B) Store (Group)
- C) Store View
- D) Global

<details><summary>Answer & Explanation</summary>

**Answer:** B — Store (Group)

**Explanation:** Each store (store group) is assigned exactly one root category, which defines the catalog/category tree available to that store.

**Exam Trap:** Store views share their parent store's single root category — you cannot give two store views under the same store different root categories.

</details>

**Q5.** Which capability is available in **Adobe Commerce but NOT Magento Open Source**?
- A) CMS Blocks
- B) Cart Price Rules
- C) Content Staging & Preview
- D) Import/Export via CSV

<details><summary>Answer & Explanation</summary>

**Answer:** C — Content Staging & Preview

**Explanation:** Content Staging & Preview is an Adobe Commerce–only capability for scheduling and previewing timed content and price changes; CMS Blocks, Cart Price Rules, and CSV Import/Export all exist in Magento Open Source.

**Exam Trap:** Basic price rules (cart and catalog) do exist in Open Source — only the scheduling/preview layer (Content Staging) and segment targeting are Commerce-only.

</details>

**Q6.** A marketer should manage **only the "Europe" website** in the admin. Which feature enforces this?
- A) Custom Resource Access only
- B) Role Scopes (restrict role to a website)
- C) Two-Factor Authentication
- D) Admin Session Lifetime

<details><summary>Answer & Explanation</summary>

**Answer:** B — Role Scopes (restrict role to a website)

**Explanation:** Role Scopes restrict an admin role to specific websites/store views (an Adobe Commerce feature), which is what limits a marketer to only the Europe website.

**Exam Trap:** Custom Resource Access limits *which features* a role can touch, not *which websites* — it cannot by itself confine an admin to one website's scope.

</details>

**Q7.** You need to show a **promotional banner only to customers in the "VIP" segment**. What do you use?
- A) CMS Block + Widget
- B) Dynamic Block + Customer Segment
- C) Catalog Price Rule
- D) Page Builder row

<details><summary>Answer & Explanation</summary>

**Answer:** B — Dynamic Block + Customer Segment

**Explanation:** Showing content to only a specific audience is segment-based personalization, delivered by a Dynamic Block targeted at a Customer Segment (Adobe Commerce).

**Exam Trap:** A plain CMS Block + Widget shows the same content to everyone; without a Customer Segment there is no VIP-only targeting.

</details>

**Q8.** A homepage banner must go live **at midnight on Black Friday and automatically revert** three days later. Best tool?
- A) Manually edit the CMS page twice
- B) Content Staging (scheduled update)
- C) A cron job
- D) Widget with conditions

<details><summary>Answer & Explanation</summary>

**Answer:** B — Content Staging (scheduled update)

**Explanation:** Content Staging schedules an update with start and end dates so the banner goes live and automatically reverts, with Preview to verify the future state before it publishes.

**Exam Trap:** Manually editing the page twice or building a custom cron job is error-prone and not the intended tool — Content Staging handles both the go-live and the automatic revert natively.

</details>

**Q9.** Which statement about **Adobe Commerce on Cloud** is correct?
- A) It has different commerce features than self-hosted Adobe Commerce
- B) It is Magento Open Source hosted by Adobe
- C) It is Adobe Commerce delivered on managed cloud infrastructure (Fastly, cloud deploys)
- D) It removes B2B features

<details><summary>Answer & Explanation</summary>

**Answer:** C — It is Adobe Commerce delivered on managed cloud infrastructure (Fastly, cloud deploys)

**Explanation:** Adobe Commerce on Cloud is the same Adobe Commerce product delivered on managed cloud infrastructure (Fastly CDN, New Relic, Git-based cloud deploys, and Integration/Staging/Production environments).

**Exam Trap:** Cloud does not add or remove commerce features — it is a hosting/deployment model, so any answer implying different features or "Open Source hosted by Adobe" is wrong.

</details>

**Q10.** Sales report totals look **out of date**. What's the most likely fix?
- A) Reindex the catalog
- B) Refresh Statistics (Reports → Statistics)
- C) Flush the cache
- D) Re-run cron for emails

<details><summary>Answer & Explanation</summary>

**Answer:** B — Refresh Statistics (Reports → Statistics)

**Explanation:** Sales reports read from aggregated statistics tables, so stale totals are corrected by running Reports → Statistics → Refresh Statistics.

**Exam Trap:** Reindexing the catalog or flushing the cache does not update report aggregates — only refreshing statistics fixes stale Sales report numbers.

</details>

**Q11.** During product import, you want to **remove existing products and load a fresh set**. Which import behavior?
- A) Add/Update
- B) Replace
- C) Delete
- D) Append

<details><summary>Answer & Explanation</summary>

**Answer:** B — Replace

**Explanation:** The Replace behavior removes the existing matched entities and re-imports them fresh, effectively wiping and reloading that product set from the CSV.

**Exam Trap:** Delete only removes matched rows and Add/Update merges into existing data — neither gives you the clean "remove then reload" that Replace does.

</details>

**Q12.** Which pair are **Adobe Sensei-powered SaaS Commerce Services**?
- A) Elasticsearch + Cron
- B) Live Search + Product Recommendations
- C) Page Builder + Widgets
- D) RMA + Store Credit

<details><summary>Answer & Explanation</summary>

**Answer:** B — Live Search + Product Recommendations

**Explanation:** Live Search and Product Recommendations are Adobe Sensei–powered SaaS Commerce Services, connected via API keys and fed by SaaS catalog data sync.

**Exam Trap:** These SaaS services are not the on-prem Elasticsearch/OpenSearch engine that powers default catalog search — don't conflate the SaaS layer with the local search engine.

</details>

**Q13.** A business runs **two distinct brands** with **separate customer bases and different base currencies**. Best structure?
- A) One website, two store views
- B) One website, two stores
- C) Two websites
- D) One store, two store views

<details><summary>Answer & Explanation</summary>

**Answer:** C — Two websites

**Explanation:** Separate customer bases and different base currencies are both website-scoped concerns, so each brand requires its own website.

**Exam Trap:** Stores and store views share the parent website's customers and base currency, so they cannot isolate customer bases or assign a different base currency per brand.

</details>

**Q14.** Which is **included in Magento Open Source**?
- A) Gift Cards
- B) Reward Points
- C) Page Builder
- D) Rule-based related products

<details><summary>Answer & Explanation</summary>

**Answer:** C — Page Builder

**Explanation:** Page Builder is now included in Magento Open Source, while Gift Cards, Reward Points, and rule-based related products remain Adobe Commerce–only.

**Exam Trap:** Page Builder used to be Commerce-only, so a dated assumption that it is exclusive to Adobe Commerce is a classic trap on modern versions.

</details>

**Q15.** Import/export files in Adobe Commerce use which format?
- A) XML
- B) JSON
- C) CSV
- D) XLSX

<details><summary>Answer & Explanation</summary>

**Answer:** C — CSV

**Explanation:** Adobe Commerce import and export both use CSV files, with the first row serving as the column headers and required columns (e.g., `sku`) present.

**Exam Trap:** XML and JSON appear elsewhere in the platform, but the Data Transfer import/export feature specifically requires CSV.

</details>

**Q16.** Which report helps you understand **what shoppers search for** so you can improve catalog/SEO?
- A) Low Stock
- B) Search Terms
- C) Order Count
- D) Tax

<details><summary>Answer & Explanation</summary>

**Answer:** B — Search Terms

**Explanation:** The Search Terms report reveals what shoppers actually type, informing catalog structure, synonyms, and SEO decisions.

**Exam Trap:** Low Stock, Order Count, and Tax reports say nothing about search behavior — only Search Terms captures shopper search intent.

</details>

**Q17.** A "Catalog Manager" admin should see **only Catalog and Products**, nothing else. How?
- A) Assign the Administrator role
- B) Create a role with Resource Access = Custom, tick only Catalog resources
- C) Enable 2FA
- D) Use Role Scopes

<details><summary>Answer & Explanation</summary>

**Answer:** B — Create a role with Resource Access = Custom, tick only Catalog resources

**Explanation:** A least-privilege role uses Resource Access = Custom with only the Catalog/Products ACL resources selected, so the admin sees nothing else.

**Exam Trap:** Role Scopes restricts websites, not features, and the Administrator role grants everything — neither produces a catalog-only admin.

</details>

**Q18.** **Visual Merchandiser** (rule-based product sorting in categories) is available in:
- A) Open Source only
- B) Adobe Commerce
- C) Cloud infrastructure only
- D) Neither

<details><summary>Answer & Explanation</summary>

**Answer:** B — Adobe Commerce

**Explanation:** Visual Merchandiser (rule-based automatic product sorting within categories) is an Adobe Commerce edition feature.

**Exam Trap:** It is not tied specifically to Cloud infrastructure — it's an edition (Commerce) feature available whether self-hosted or on Cloud.

</details>

---

# MCQ — Gap Coverage (Scope Depth, Services & Reporting)

**Q19.** A merchant needs the **same catalog and currency** but a **different root category** (different top-level menu) for its outlet section vs its main section. Best structure?
- A) Two websites
- B) One website with two **stores (store groups)**, each with its own root category
- C) One store with two store views
- D) Two store views under one store

<details><summary>Answer & Explanation</summary>

**Answer:** B — One website with two stores (store groups), each with its own root category

**Explanation:** The root category is store-(group)-scoped, so different top-level category trees under the same website and currency are achieved with multiple stores, not store views or separate websites.

**Exam Trap:** Store views can't have different root categories, and separate websites would needlessly split customers and currency — the answer is multiple stores under one website.

</details>

**Q20.** Which statement correctly distinguishes **Live Search** from the default catalog search?
- A) Live Search is the on-prem Elasticsearch/OpenSearch engine
- B) Live Search is a **Sensei-powered SaaS Commerce Service** connected via API keys and SaaS catalog sync, separate from the on-prem Elasticsearch/OpenSearch that powers default search
- C) Live Search only works for Open Source
- D) Live Search replaces the need for a database

<details><summary>Answer & Explanation</summary>

**Answer:** B — Live Search is a Sensei-powered SaaS Commerce Service connected via API keys and SaaS catalog sync, separate from the on-prem Elasticsearch/OpenSearch that powers default search

**Explanation:** Live Search runs as a Sensei/SaaS Commerce Service that requires a Commerce Services connection and SaaS data export, distinct from the on-prem Elasticsearch/OpenSearch engine behind default catalog search.

**Exam Trap:** Live Search does not replace your database or become the local search engine, and it isn't limited to Open Source — it's an external SaaS layer that sits alongside the platform.

</details>

**Q21.** A merchant wants free, cloud-based dashboards showing basic order/product trends without buying a deeper analytics platform. Which service fits, and how does it differ from Commerce Intelligence?
- A) Commerce Intelligence (MBI) — it is the free tier
- B) **Advanced Reporting** — a free cloud dashboard (needs Base URL + reporting service enabled); **Commerce Intelligence (MBI)** is the deeper paid analytics platform
- C) Live Search analytics
- D) Product Recommendations

<details><summary>Answer & Explanation</summary>

**Answer:** B — Advanced Reporting — a free cloud dashboard (needs Base URL + reporting service enabled); Commerce Intelligence (MBI) is the deeper paid analytics platform

**Explanation:** Advanced Reporting is the free, cloud-based dashboard (requiring a Base URL and the reporting service enabled), while Commerce Intelligence (MBI) is the deeper paid BI platform.

**Exam Trap:** The two are frequently swapped — MBI is not the "free tier," and Advanced Reporting is not the deep paid analytics product.

</details>

**Q22.** Which entities can **Content Staging** schedule updates for?
- A) Only CMS pages
- B) CMS pages, blocks, categories, products, and price rules
- C) Only price rules
- D) Only products and prices

<details><summary>Answer & Explanation</summary>

**Answer:** B — CMS pages, blocks, categories, products, and price rules

**Explanation:** Content Staging schedules timed Campaigns/Updates (with Preview) across CMS pages, blocks, categories, products, and price rules, and is Adobe Commerce–only.

**Exam Trap:** It is not limited to CMS pages or to prices alone — its scope spans catalog and content entities plus price rules.

</details>

**Q23.** Two brands run under separate websites but the business wants **one product to have different prices per website**. Which setting enables this?
- A) Customer Account Sharing = Global
- B) **Catalog Price Scope = Website** (Stores → Config → Catalog → Catalog → Price)
- C) Base Currency = Global
- D) Role Scopes

<details><summary>Answer & Explanation</summary>

**Answer:** B — Catalog Price Scope = Website (Stores → Config → Catalog → Catalog → Price)

**Explanation:** Setting Catalog Price Scope to Website lets the same SKU carry different prices per website; Global scope forces one price everywhere.

**Exam Trap:** Customer Account Sharing and Base Currency don't govern per-website pricing — only Catalog Price Scope does.

</details>

**Q24.** An admin changes the **resources of their own role**. What does Commerce require before saving, and why?
- A) Nothing special
- B) **Password re-entry/confirmation**, as a guard against accidentally locking oneself out of needed permissions
- C) A second admin to approve
- D) Disabling 2FA first

<details><summary>Answer & Explanation</summary>

**Answer:** B — Password re-entry/confirmation, as a guard against accidentally locking oneself out of needed permissions

**Explanation:** Editing your own role's resources requires re-entering your password, a safeguard against self-lockout or accidental removal of permissions you still need.

**Exam Trap:** No second approver or 2FA toggle is involved — the specific guard is password confirmation when changing your own role.

</details>

**Q25.** A merchant wants automated "customers who viewed this also viewed" suggestions powered by Adobe Sensei. Which service, and what is required to feed it data?
- A) Visual Merchandiser; no data feed needed
- B) **Product Recommendations** (Sensei SaaS), connected via Commerce Services and requiring **SaaS catalog data sync / data ingestion**
- C) Related Products rules only, no SaaS
- D) Advanced Reporting

<details><summary>Answer & Explanation</summary>

**Answer:** B — Product Recommendations (Sensei SaaS), connected via Commerce Services and requiring SaaS catalog data sync / data ingestion

**Explanation:** "Customers who viewed this also viewed" is powered by Product Recommendations, a Sensei SaaS service that connects through Commerce Services and needs SaaS catalog data export/ingestion to function.

**Exam Trap:** Rule-based related products are manual and require no SaaS feed — they are not the same as Sensei-powered Product Recommendations.

</details>

**Q26.** A single admin user needs to manage catalog on the "US" website but must be blocked entirely from the "EU" website. Which two role-building controls combine to achieve least-privilege AND scope restriction?
- A) 2FA + Admin Session Lifetime
- B) **Resource Access = Custom** (limit to Catalog features) **+ Role Scopes** (restrict to the US website)
- C) Two separate user accounts only
- D) Customer Segments + Dynamic Blocks

<details><summary>Answer & Explanation</summary>

**Answer:** B — Resource Access = Custom (limit to Catalog features) + Role Scopes (restrict to the US website)

**Explanation:** Resource Access = Custom restricts which features the role can use, while Role Scopes restricts which websites/store views it applies to; combined, they enforce "catalog-only, US-only."

**Exam Trap:** Resource Access controls *features* and Role Scopes controls *scope* — using only one leaves either the wrong websites open or too many features enabled.

</details>

**Q27.** A merchant saved a Catalog Price Rule and a product price change, but the storefront still shows the old prices. Index Mode is set to "Update on Schedule." What is the most likely reason?
- A) The cache must be disabled permanently
- B) The relevant indexer hasn't reindexed yet (on-schedule mode waits for cron); running the reindex/cron updates the storefront
- C) The change requires a new website
- D) Catalog Price Rules never appear on the storefront

<details><summary>Answer & Explanation</summary>

**Answer:** B — The indexer hasn't reindexed yet

**Explanation:** With "Update on Schedule," reindexing happens in batches via cron rather than immediately on save. Until the indexer runs, the storefront serves stale index data. Reindexing (or the next cron pass) resolves it.

**Exam Trap:** "Update on Save" reindexes immediately; "Update on Schedule" defers to cron — a saved change not appearing is an indexing/cron timing issue, not a data-entry error.

</details>

**Q28.** Customers aren't receiving order emails and a scheduled Content Staging update never went live, though everything is configured correctly. What single underlying cause explains both?
- A) The Full Page Cache is too large
- B) Cron is not running — it drives email sending, scheduled staging, and scheduled reindexing
- C) The store view is disabled
- D) The catalog needs a new root category

<details><summary>Answer & Explanation</summary>

**Answer:** B — Cron is not running

**Explanation:** Cron drives background/scheduled jobs including transactional email dispatch and Content Staging updates applying/reverting. If cron is stopped, these time-based actions silently don't happen even when configured correctly.

**Exam Trap:** When multiple *scheduled/background* things fail at once (emails, staged content, scheduled reindex), suspect cron — not each feature individually.

</details>

**Q29.** After changing a CMS block's content, the update isn't visible on the storefront even though the data is saved and indexers are current. What is the most likely fix?
- A) Reindex the catalog
- B) Flush/refresh the cache (e.g., Full Page Cache) in Cache Management
- C) Refresh Statistics
- D) Re-run the SaaS data sync

<details><summary>Answer & Explanation</summary>

**Answer:** B — Flush/refresh the cache

**Explanation:** Rendered content like CMS blocks is served from Full Page Cache. When indexers are already current, a stale rendered page points to cache — flushing/refreshing the cache makes the new content appear.

**Exam Trap:** Reindexing fixes stale *catalog/price/search data*; cache flushing fixes stale *rendered pages/config*. Match the symptom to the right mechanism — and neither is "Refresh Statistics," which only updates report totals.

</details>

---

# Common Exam Traps (memorize these)

1. **Scope confusion (the #1 trap).**
   - Language/locale → **store view**. Root category → **store**. Currency/customers/prices → **website**.
   - "Different currency" or "separate customers" almost always means **separate websites**, not store views.

2. **Store view ≠ new catalog.** Store views share the store's **one root category**; they change *presentation/language*, not the product set.

3. **Edition mix-ups.** Content Staging, Customer Segments, Dynamic Blocks, B2B, Gift Cards, Reward Points, Store Credit, RMA, Gift Registry, Visual Merchandiser, rule-based related products = **Adobe Commerce only**. Page Builder is now in **both**.

4. **"Cloud" is infrastructure, not features.** Commerce on Cloud = same commerce functionality + managed hosting (Fastly, cloud deploys, Integration/Staging/Production). Don't pick "Cloud" as the answer for a *feature* question.

5. **One user = one role.** Admin permissions come from a single assigned role; build least-privilege with **Custom Resource Access**. **Role Scopes** (website restriction) is the *scope* control, not resource control.

6. **Reports look stale → Refresh Statistics.** Sales reports use aggregated data; reindex/cache flush won't fix stale report totals.

7. **Import behaviors.** Add/Update = merge; **Replace** = wipe & reload matched entities; **Delete** = remove. Files are **CSV**, header row required (e.g. `sku`).

8. **Dynamic Block vs CMS Block.** CMS Block = static reusable content (both editions). **Dynamic Block** = **segment-personalized** content (Commerce only).

9. **Live Search / Product Recommendations are SaaS Commerce Services** (Sensei), connected via API keys / SaaS data sync — not the same as on-prem Elasticsearch/OpenSearch catalog search.

10. **Advanced Reporting vs Commerce Intelligence.** Advanced Reporting = free cloud dashboard; **Commerce Intelligence (MBI)** = deeper paid analytics.

11. **Customer Account Sharing default = Per Website.** To share logins across sites you must switch it to **Global**.

12. **Admin Action Logs** (audit trail of admin activity) = **Adobe Commerce only**.

13. **"Changed it but storefront shows old value" → reindex or cache flush.** Reindex fixes stale catalog/price/search data (Update on Save = immediate; Update on Schedule = via cron); cache flush fixes stale rendered pages/config. Neither is "Refresh Statistics."

14. **Scheduled/background actions not happening (emails, staged content, scheduled reindex) → cron.** A stalled cron silently breaks all time-based jobs at once.
- **Base URLs**: can be set per website/store view to route domains to the right scope.

---

## 5.2 Admin Roles and Permissions

### Where it lives
**System → Permissions → User Roles** and **System → Permissions → All Users**.

### The two building blocks

1. **User Role** — a named set of permissions (Access + Resources + optional scope restriction).
2. **User** — an admin account **assigned to exactly one role**.

### Creating a role — the parts

| Tab / field | Purpose |
|-------------|---------|
| **Role Info** | Name the role; re-enter *your* password to confirm changes. |
| **Role Resources → Resource Access** | `All` (everything) or `Custom` (tick specific ACL resources in the tree). |
| **Role Scopes** | Restrict role to specific **websites/store views** (Adobe Commerce feature for scoped admins). |

### Important rules & traps

- A user has **one role only**; permissions are the union defined by that role.
- **Resource Access = Custom** lets you build least-privilege roles (e.g. a "Catalog Manager" who only sees Catalog + Products).
- **Role Scopes** (limiting a role to certain websites) is an **Adobe Commerce** capability — useful for multi-brand teams where a marketer manages only one website.
- Changing your own role's resources requires **password confirmation** (a guard against lockout/mistakes).
- **Action Logs / Admin Actions Log** (Reports → find under Admin Actions) is **Adobe Commerce only** — tracks what each admin user did. Good for auditing.
- **Two-Factor Authentication (2FA)** is enforced by default for admin in modern versions (both Open Source and Commerce).
- **Admin Session Lifetime** and **password lifetime/lockout** are set in Stores → Config → Advanced → Admin → Security.

---

## 5.3 Magento Open Source vs Adobe Commerce vs Adobe Commerce Cloud

### One-line definitions

- **Magento Open Source**: free, self-hosted, community-supported e-commerce platform (core features).
- **Adobe Commerce**: licensed (paid) — Open Source **plus** advanced B2B, marketing, targeting, and support. Can be self-hosted (on-prem) **or** cloud.
- **Adobe Commerce Cloud (Cloud infrastructure / PaaS)**: Adobe Commerce **delivered on a managed cloud platform** (hosting, CDN via Fastly, deployment pipeline, cloud tooling).

### Feature comparison (high-yield table)

| Feature | Open Source | Adobe Commerce | Commerce on Cloud |
|---------|:-----------:|:--------------:|:-----------------:|
| Cost | Free | Licensed | Licensed + hosting |
| Catalog & basic cart/checkout | ✅ | ✅ | ✅ |
| Customer segmentation & targeting | ❌ | ✅ | ✅ |
| **Price rules — Cart & Catalog** (basic) | ✅ | ✅ | ✅ |
| **Related products by rules / Rule-based product relations** | ❌ | ✅ | ✅ |
| **Content Staging & Preview** | ❌ | ✅ | ✅ |
| **Page Builder** | ✅ (now in OS too) | ✅ | ✅ |
| **B2B features** (company accounts, shared catalogs, quotes, requisition lists) | ❌ | ✅ | ✅ |
| **Gift Cards, Gift Registry, Reward Points, Store Credit** | ❌ | ✅ | ✅ |
| **RMA (Return Merchandise Authorization)** | ❌ | ✅ | ✅ |
| **Customer/Order Attributes (custom)** | ❌ | ✅ | ✅ |
| **Advanced/Elasticsearch reporting, Admin Action Logs** | Limited | ✅ | ✅ |
| **Visual Merchandiser & Rule-based category** | ❌ | ✅ | ✅ |
| **Commerce Intelligence (MBI)** | ❌ | ✅ (included tier) | ✅ |
| **Fastly CDN + WAF, managed hosting, cloud CLI** | ❌ | ❌ (self-host) | ✅ |
| Adobe support & SLA | Community | ✅ | ✅ |

### Traps

- **Page Builder** is now available in Open Source too — don't assume it's Commerce-only on a modern version.
- **B2B, Content Staging, Gift Cards, RMA, Reward Points, Store Credit, Gift Registry, rule-based related products** = **Adobe Commerce** (not Open Source). These are the classic "which edition?" answers.
- **Commerce on Cloud** is **not a different product** — it's Adobe Commerce + managed cloud infra (Fastly, New Relic, cloud Git-based deploys, 3 environments: Integration/Staging/Production on Pro).
- "Cloud" differences are about **infrastructure/hosting/deployment**, not commerce feature functionality.

---

## 5.4 CMS and Personalization Features

### CMS building blocks

| Feature | What it does | Edition |
|---------|--------------|---------|
| **CMS Pages** | Standalone content pages (About Us, etc.), scoped by store view | Both |
| **CMS Blocks** | Reusable content chunks, placed via widgets/layout/XML | Both |
| **Widgets** | Insert dynamic content (product lists, CMS block, links) into pages/blocks/layout | Both |
| **Page Builder** | Drag-and-drop visual content editor (rows, columns, banners, sliders, HTML) | Both (modern) |
| **Design/Theme config** | Store-view-scoped theme, header/footer, logo | Both |

### Personalization & targeting (mostly Adobe Commerce)

| Feature | What it does | Edition |
|---------|--------------|---------|
| **Customer Segments** | Dynamically group customers by attributes/behavior (order history, cart, address, etc.) | **Commerce** |
| **Related Products Rules / Rule-based relations** | Auto-suggest up-sell/cross-sell by rules | **Commerce** |
| **Dynamic Blocks** (formerly "Banners") | Content shown to specific **customer segments** or by rules | **Commerce** |
| **Content Staging & Preview** | Schedule content/price changes with a timeline & preview future state | **Commerce** |
| **Price Rules — Catalog Price Rule** | Adjust product prices by conditions (can target segments in Commerce) | Both (segment targeting = Commerce) |
| **Price Rules — Cart Price Rule (Coupons)** | Discounts at cart by conditions/coupon; can target **customer segments** | Both (segment targeting = Commerce) |
| **Visual Merchandiser** | Rule-based auto-sorting of products in categories | **Commerce** |

### Key concepts to distinguish

- **CMS Block vs Widget vs Dynamic Block**:
  - *CMS Block* = static reusable content.
  - *Widget* = mechanism to place content/functionality into a location.
  - *Dynamic Block* = **segment-targeted** content (personalization) — **Commerce only**.
- **Customer Segment** drives personalization: used by Dynamic Blocks, Cart Price Rules, Catalog Price Rules, and reports.
- **Content Staging**: create a **Campaign/Update** with start/end dates; use **Preview** to see the site at a future date. Applies to CMS pages, blocks, categories, products, price rules.

### Scenario cues

- "Show a banner **only to VIP customers**" → **Dynamic Block + Customer Segment** (Commerce).
- "Schedule a homepage change for **Black Friday, auto-revert after**" → **Content Staging** (Commerce).
- "Reusable footer content across pages" → **CMS Block + Widget** (Both).
- "Drag-and-drop landing page" → **Page Builder** (Both, modern).

---

## 5.5 Reports, Import/Export, and Commerce Services

### Reports (Reports menu)

| Report group | Examples | Notes |
|--------------|----------|-------|
| **Marketing** | Products in Cart, Search Terms, Abandoned Carts, Newsletter, Cart Price Rules (coupon usage) | Search Terms help catalog/SEO decisions |
| **Reviews** | By Products, By Customers | |
| **Sales** | Orders, Tax, Invoiced, Shipping, Refunds, Coupons | Many need **statistics refresh** |
| **Customers** | Order Total, Order Count, New, Now Online, **Wish Lists**, **Segments** | Segments report = **Commerce** |
| **Products** | Ordered, Viewed (Bestsellers, Most Viewed), Low Stock, Downloads | |
| **Statistics** | **Refresh Statistics** (lifetime vs by day) | Must refresh for Sales reports to be current |
| **Business Intelligence** | Advanced Reporting (free tier) & **Commerce Intelligence / MBI** | Cloud-based dashboards |

**Traps**
- Many **Sales reports rely on refreshed statistics** — if numbers look stale, run **Reports → Statistics → Refresh Statistics**.
- **Advanced Reporting** is a free, cloud-based dashboard (needs a Base URL + the reporting service enabled); **Commerce Intelligence (MBI)** is the deeper paid analytics platform.
- **Customer Segment reports** are **Adobe Commerce only**.

### Import / Export (System → Data Transfer)

| Aspect | Detail |
|--------|--------|
| **Format** | **CSV** (import & export) |
| **Entity types** | Products, Customers (and addresses), Customer Finance (Commerce), Advanced Pricing, Stock Sources, etc. |
| **Import behaviors** | **Add/Update**, **Replace**, **Delete** |
| **Validation** | Import validates the file first and reports errors before executing |
| **Images** | Product images imported from a media folder path referenced in the CSV |
| **Export** | Choose entity + attribute filters → generates CSV |

**Traps**
- Import file **must be CSV**; large imports typically use the **Import** UI or CLI/API.
- **Add/Update** merges; **Replace** removes existing entities and re-adds; **Delete** removes matched rows — know the difference.
- The **first row = column headers**; required columns (e.g. `sku` for products) must be present.

### Commerce Services (SaaS services layer)

| Service | Purpose |
|---------|---------|
| **Adobe Commerce Intelligence (MBI)** | BI dashboards & analytics |
| **Live Search** (Adobe Sensei-powered) | AI search & merchandising (replaces basic search; SaaS) |
| **Product Recommendations** | Sensei-powered recs (e.g. "customers also viewed") |
| **Catalog Service / Commerce Services** | GraphQL storefront catalog data, SaaS-backed |
| **Payment Services** | Adobe-native payment processing |
| **Advanced Reporting** | Free cloud reporting dashboard |

**Traps**
- **Live Search** and **Product Recommendations** are **Sensei/SaaS services** — connected via **Commerce Services / API keys** and available to Adobe Commerce (and, for some, Open Source with an account). They require SaaS data export (Data Ingestion / SaaS catalog sync).
- These are **not** the same as the on-prem Elasticsearch/OpenSearch that powers default catalog search.

---

## Quick-Reference "Edition" Cheat Sheet (highest-yield)

**Adobe Commerce–only (NOT Open Source):**
Customer Segments · Dynamic Blocks · Content Staging & Preview · Rule-based Related Products · Visual Merchandiser · B2B (Company accounts, Shared Catalogs, Quotes, Requisition Lists) · Gift Cards · Gift Registry · Reward Points · Store Credit · RMA · Customer/Order custom attributes · Admin Action Logs · Segment reports.

**Both editions:**
CMS Pages/Blocks · Widgets · Page Builder (modern) · Catalog & Cart Price Rules (basic) · Import/Export CSV · Standard reports · Multi-website/store/store-view.

---

# MCQ — Readiness Check

> Each answer is hidden right below its question. Try to answer first, then expand **Answer**.

**Q1.** A merchant sells the **same products at the same prices** but needs the store in **English and Spanish**. What is the correct structure?
- A) Two websites
- B) One website, two stores
- C) One website, one store, two store views
- D) Two stores, one store view each

<details><summary>Answer</summary>

**C** — Same catalog/prices, only language differs → store **views** under one store/website. Language/locale is store-view-scoped.
</details>

**Q2.** Which setting must change so a customer can log in with the **same account across two different websites**?
- A) Catalog Price Scope → Website
- B) Customer Account Sharing → Global
- C) Base Currency → Global
- D) Share Catalog → Enabled

<details><summary>Answer</summary>

**B** — Customer accounts are website-scoped by default; **Customer Account Sharing = Global** shares login across websites.
</details>

**Q3.** Base **currency** is configured at which scope?
- A) Store View
- B) Store (Group)
- C) Website
- D) Global only

<details><summary>Answer</summary>

**C** — Base currency is a **website**-level setting (why different-currency regions need separate websites).
</details>

**Q4.** The **root category** is defined at which level?
- A) Website
- B) Store (Group)
- C) Store View
- D) Global

<details><summary>Answer</summary>

**B** — Each **store (group)** has exactly one **root category**, defining its catalog tree.
</details>

**Q5.** Which capability is available in **Adobe Commerce but NOT Magento Open Source**?
- A) CMS Blocks
- B) Cart Price Rules
- C) Content Staging & Preview
- D) Import/Export via CSV

<details><summary>Answer</summary>

**C** — **Content Staging & Preview** is Adobe Commerce only. A, B, D exist in Open Source.
</details>

**Q6.** A marketer should manage **only the "Europe" website** in the admin. Which feature enforces this?
- A) Custom Resource Access only
- B) Role Scopes (restrict role to a website)
- C) Two-Factor Authentication
- D) Admin Session Lifetime

<details><summary>Answer</summary>

**B** — **Role Scopes** restrict a role to specific websites/store views (Adobe Commerce). Custom Resource Access limits *features*, not *scope*.
</details>

**Q7.** You need to show a **promotional banner only to customers in the "VIP" segment**. What do you use?
- A) CMS Block + Widget
- B) Dynamic Block + Customer Segment
- C) Catalog Price Rule
- D) Page Builder row

<details><summary>Answer</summary>

**B** — Segment-targeted content = **Dynamic Block + Customer Segment** (Commerce).
</details>

**Q8.** A homepage banner must go live **at midnight on Black Friday and automatically revert** three days later. Best tool?
- A) Manually edit the CMS page twice
- B) Content Staging (scheduled update)
- C) A cron job
- D) Widget with conditions

<details><summary>Answer</summary>

**B** — **Content Staging** schedules a start/end update with auto-revert and preview.
</details>

**Q9.** Which statement about **Adobe Commerce on Cloud** is correct?
- A) It has different commerce features than self-hosted Adobe Commerce
- B) It is Magento Open Source hosted by Adobe
- C) It is Adobe Commerce delivered on managed cloud infrastructure (Fastly, cloud deploys)
- D) It removes B2B features

<details><summary>Answer</summary>

**C** — Cloud = Adobe Commerce on **managed cloud infrastructure**; same commerce features, different hosting/deploy model.
</details>

**Q10.** Sales report totals look **out of date**. What's the most likely fix?
- A) Reindex the catalog
- B) Refresh Statistics (Reports → Statistics)
- C) Flush the cache
- D) Re-run cron for emails

<details><summary>Answer</summary>

**B** — Sales reports depend on aggregated stats → **Refresh Statistics**.
</details>

**Q11.** During product import, you want to **remove existing products and load a fresh set**. Which import behavior?
- A) Add/Update
- B) Replace
- C) Delete
- D) Append

<details><summary>Answer</summary>

**B** — **Replace** removes existing entities and re-imports; **Delete** only removes; **Add/Update** merges.
</details>

**Q12.** Which pair are **Adobe Sensei-powered SaaS Commerce Services**?
- A) Elasticsearch + Cron
- B) Live Search + Product Recommendations
- C) Page Builder + Widgets
- D) RMA + Store Credit

<details><summary>Answer</summary>

**B** — **Live Search** and **Product Recommendations** are Sensei-powered SaaS Commerce Services.
</details>

**Q13.** A business runs **two distinct brands** with **separate customer bases and different base currencies**. Best structure?
- A) One website, two store views
- B) One website, two stores
- C) Two websites
- D) One store, two store views

<details><summary>Answer</summary>

**C** — Separate customer bases + different base currency → **separate websites** (both are website-scoped).
</details>

**Q14.** Which is **included in Magento Open Source**?
- A) Gift Cards
- B) Reward Points
- C) Page Builder
- D) Rule-based related products

<details><summary>Answer</summary>

**C** — **Page Builder** is now in Open Source. Gift Cards, Reward Points, rule-based related products are Commerce-only.
</details>

**Q15.** Import/export files in Adobe Commerce use which format?
- A) XML
- B) JSON
- C) CSV
- D) XLSX

<details><summary>Answer</summary>

**C** — **CSV** for both import and export.
</details>

**Q16.** Which report helps you understand **what shoppers search for** so you can improve catalog/SEO?
- A) Low Stock
- B) Search Terms
- C) Order Count
- D) Tax

<details><summary>Answer</summary>

**B** — **Search Terms** report reveals shopper search behavior for catalog/SEO tuning.
</details>

**Q17.** A "Catalog Manager" admin should see **only Catalog and Products**, nothing else. How?
- A) Assign the Administrator role
- B) Create a role with Resource Access = Custom, tick only Catalog resources
- C) Enable 2FA
- D) Use Role Scopes

<details><summary>Answer</summary>

**B** — Least-privilege role: **Custom Resource Access** with only Catalog/Products ticked.
</details>

**Q18.** **Visual Merchandiser** (rule-based product sorting in categories) is available in:
- A) Open Source only
- B) Adobe Commerce
- C) Cloud infrastructure only
- D) Neither

<details><summary>Answer</summary>

**B** — **Visual Merchandiser** is an **Adobe Commerce** feature.
</details>

---

# Common Exam Traps (memorize these)

1. **Scope confusion (the #1 trap).**
   - Language/locale → **store view**. Root category → **store**. Currency/customers/prices → **website**.
   - "Different currency" or "separate customers" almost always means **separate websites**, not store views.

2. **Store view ≠ new catalog.** Store views share the store's **one root category**; they change *presentation/language*, not the product set.

3. **Edition mix-ups.** Content Staging, Customer Segments, Dynamic Blocks, B2B, Gift Cards, Reward Points, Store Credit, RMA, Gift Registry, Visual Merchandiser, rule-based related products = **Adobe Commerce only**. Page Builder is now in **both**.

4. **"Cloud" is infrastructure, not features.** Commerce on Cloud = same commerce functionality + managed hosting (Fastly, cloud deploys, Integration/Staging/Production). Don't pick "Cloud" as the answer for a *feature* question.

5. **One user = one role.** Admin permissions come from a single assigned role; build least-privilege with **Custom Resource Access**. **Role Scopes** (website restriction) is the *scope* control, not resource control.

6. **Reports look stale → Refresh Statistics.** Sales reports use aggregated data; reindex/cache flush won't fix stale report totals.

7. **Import behaviors.** Add/Update = merge; **Replace** = wipe & reload matched entities; **Delete** = remove. Files are **CSV**, header row required (e.g. `sku`).

8. **Dynamic Block vs CMS Block.** CMS Block = static reusable content (both editions). **Dynamic Block** = **segment-personalized** content (Commerce only).

9. **Live Search / Product Recommendations are SaaS Commerce Services** (Sensei), connected via API keys / SaaS data sync — not the same as on-prem Elasticsearch/OpenSearch catalog search.

10. **Advanced Reporting vs Commerce Intelligence.** Advanced Reporting = free cloud dashboard; **Commerce Intelligence (MBI)** = deeper paid analytics.

11. **Customer Account Sharing default = Per Website.** To share logins across sites you must switch it to **Global**.

12. **Admin Action Logs** (audit trail of admin activity) = **Adobe Commerce only**.
