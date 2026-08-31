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

### What is scoped where (this is the #1 tested concept)

- **Website scope**: base currency, customer accounts (share option), payment methods, shipping, tax rules, product prices (in "Website" price scope), inventory (if not global).
- **Store (group) scope**: the **root category** — determines which catalog/category tree is available.
- **Store View scope**: **language/locale**, product name/description translations, CMS content translations, allowed currencies for display, product visibility per view.

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
