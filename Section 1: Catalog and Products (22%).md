# Adobe Commerce Business Practitioner Professional (AD0-E723)
## Section 1: Catalog and Products (22%) — Notes & Practice MCQs

---

## PART A: STUDY NOTES

### 1.1 Product Types — When to Use What

| Product Type | Use When | Key Trait |
|---|---|---|
| **Simple Product** | Single SKU, no variations | Sold as-is |
| **Configurable Product** | Same item in multiple variations (size/color) needing separate SKUs/inventory | Parent-child; each child is a simple product |
| **Grouped Product** | Selling related standalone products together (e.g., dinnerware set) | No combined price; each child priced/purchased independently |
| **Bundle Product** | Customer builds a custom product from options (e.g., build-a-PC) | Dynamic or Fixed pricing; options can be required/optional |
| **Virtual Product** | No physical shipping (service, warranty, membership) | No weight, no shipping |
| **Downloadable Product** | Digital files delivered post-purchase (ebook, software) | Linked file/sample delivery |

**Exam tip:** "Customer selects size and color" → Configurable. "Sold as a set but items also sold separately" → Grouped. "Customer customizes with add-ons" → Bundle.

#### Bundle vs. Grouped — The Core Distinction

| | Grouped Product | Bundle Product |
|---|---|---|
| What it is | Links existing, independent simple products | Combines selectable options/components into one unit |
| Pricing | No combined price — each child keeps its own | One combined price (Dynamic or Fixed) |
| Cart behavior | Each child = separate cart line item | Whole bundle = ONE cart line item |
| Customer interaction | Picks quantities of already-defined items | Picks FROM options (required/optional) |
| SKU on order | Multiple SKUs | One bundle SKU |

**Two quick tests:**
1. Does the customer pick *from* options, or just quantities of already-defined items? → Options = Bundle; Quantities = Grouped
2. Does it show as ONE cart line or MULTIPLE? → One = Bundle; Multiple = Grouped

**Admin config difference:** Grouped products have **no Price field** on the parent at all. Bundle products always show a **Price Type (Dynamic/Fixed)** dropdown.

- **Fixed pricing**: bundle has one set price regardless of selections
- **Dynamic pricing**: price = sum of selected options
- **Fixed Product Set (Radio Buttons)**: a specific Bundle Items input type worth remembering by name

**MSI cross-tie:**
- **Ship Bundle Items Together**: bundle treated as one unit for sourcing/SSA — assumes single-source fulfillment
- **Ship Bundle Items Separately**: each component's stock deducted independently, potentially from different Sources/Stocks
- **Grouped products have no parent-level inventory** — each child has fully independent stock; the parent holds no stock itself
- Bundle (Ship Together) creates one coordinated reservation tied to a single cart line; Grouped creates separate, independent reservations per child line item

> **⚠️ Exam Trap:** Customer picks *from* options (one cart line, has a Price Type) = Bundle; picks *quantities* of already-defined products (multiple cart lines, no parent price field) = Grouped.

---

### 1.2 Storefront Behavior Based on Configuration

**Configurable Products**
- Swatches vs. Dropdown controlled by attribute's swatch type setting (Visual/Text/Dropdown), configured under Stores > Attributes > Product
- If "Use Product Image for Swatch" enabled, selecting a swatch swaps the main PDP image
- **Price display before selection**: shows the lowest-priced available/in-stock child variant (often "As low as $X")
- **Out-of-stock child behavior** depends on:
  - Catalog > Inventory > Product Stock Options > "Display Out of Stock Products" (Yes/No, global)
  - If Yes: shown disabled/greyed; if No: omitted entirely from options
- If all children are out of stock, the parent configurable itself shows out of stock

**Bundle Products**
- Dynamic pricing shows a price range ("$50.00 - $120.00") until selections are made
- Required options must be selected before "Add to Cart" is enabled; Optional can be skipped (often with a "None" choice)
- Multiple Select/Checkbox options allow selecting more than one product within a single option, multiplying into the Dynamic total

**Grouped Products**
- Shared "Add to Cart" button adds all children with qty > 0 as separate cart lines
- Out-of-stock children hidden/disabled per the same global "Display Out of Stock Products" setting

**Custom Options vs. Configurable Attributes**
- **Custom Options**: live on a Simple product; add a price modifier to that single SKU; no separate stock/SKU tracked
- **Configurable Attributes**: create genuinely separate child SKUs, each independently trackable, each with potential price overrides

**Visibility Settings & Storefront Effect**

| Visibility Value | Category Listing? | Search? | Direct URL? |
|---|---|---|---|
| Catalog, Search | Yes | Yes | Yes |
| Catalog | Yes | No | Yes |
| Search | No | Yes | Yes |
| Not Visible Individually | No | No | Yes (used for configurable/bundle children) |

---

### 1.3 Pricing Configuration

**Full Pricing Toolkit**

| Tool | Scope | Requires Coupon? | Applies Automatically? |
|---|---|---|---|
| Special Price | Single product, date range | No | Yes (strikethrough + special price) |
| Tier Price | Single product, qty breaks, per customer group | No | Yes, once qty threshold met |
| Catalog Price Rule | Multiple products via conditions | No | Yes — modifies displayed price directly |
| Cart Price Rule | Cart-level, conditions + actions | Optional | Only at cart/checkout |
| Advanced Pricing | Special + Tier + Cost + MAP combined | Varies | Yes |

**Catalog Price Rule Mechanics**
- Conditions: Category, Website, Customer Group, Attribute Set, product attributes
- Actions: % discount, Fixed amount discount, Fixed price
- "Discard subsequent rules" — stops lower-priority rules from also applying
- Rules process by **priority number** (lower = higher priority)
- Requires **Catalog Rule Price indexer** to reindex before showing on storefront

**Cart Price Rule Mechanics**
- Coupon options: No Coupon (auto-applies), Specific Coupon, Auto-generated (bulk unique codes)
- Conditions: cart subtotal, items in cart, customer group, shipping country, etc.
- Actions: % discount, Fixed amount off cart, Fixed amount off each item, Buy X Get Y Free, Free Shipping
- Priority + "Discard subsequent rules" works the same as Catalog rules but at cart level

**MAP (Minimum Advertised Price)**
- Configured per-product under Advanced Pricing
- Display Actual Price options:
  - **In Cart** — hidden everywhere except cart
  - **Before Order Confirmation** — hidden until checkout
  - **On Gesture** — hidden but revealed via "Click for price" popup
  - **Use config** — falls back to store default

**Price Calculation Order**
1. Base Price (or Special Price if active and lower)
2. Tier Price if qty threshold met (wins if lower than Special Price)
3. Catalog Price Rules applied on top, in priority order → produces **displayed/PDP price**
4. Cart Price Rules applied at cart level, in priority order, on top of the PDP price

> **⚠️ Exam Trap:** Tier Price wins only if lower than the active Special Price once the qty threshold is met; Catalog Price Rules set the PDP price, but Cart Price Rules discount only at the cart — they never change the displayed catalog price.

**Customer Group Pricing**
- Achieved via Tier Pricing scoped to a specific group, OR Catalog Price Rules conditioned on Customer Group
- Tier Price = product-specific group pricing; Catalog Price Rule = broad/rule-based group pricing across many products

**Price Scope (Store Config)**
- Catalog > General > Price > "Catalog Price Scope" = Global or Website
- Website scope allows entirely independent prices per website for the same SKU (multi-brand/multi-currency)

---

### 1.4 Product Attributes & Catalog Navigation

**Attribute Fundamentals**

| Property | Controls |
|---|---|
| Attribute Code | Internal identifier (code/API use) |
| Scope | Global / Website / Store View |
| Catalog Input Type | Text, Text Area, Dropdown, Multi-Select, Date, Yes/No, Price, Media Image, Fixed Product Set |
| Values Required | Mandatory at product creation |
| Unique Value | Prevents duplicate values (SKU-like fields) |

**Scope rule of thumb**
- **Global** — SKU, weight, configurable-defining attributes (e.g., Color used for configurables MUST be Global — needed for consistent parent-child variation logic across the whole catalog)
- **Website** — price, sometimes stock status
- **Store View** — name, description, meta fields, URL key (localization)

**Storefront Properties Block**

| Setting | Effect |
|---|---|
| Use in Search | Includes value in quick search index |
| Visible in Advanced Search | Adds as filter field on Advanced Search page |
| Comparable on Storefront | Shows row on Compare Products page |
| Use in Layered Navigation | No / Filterable (with results) / Filterable (no results) |
| Use in Search Results Layered Navigation | Separate toggle for filtering within search results |
| Position | Sort order of filter in layered nav block |

**Key distinction:** "Filterable (no results)" still shows the filter option even at 0 matching products. "Filterable (with results)" hides options that would return zero products.

**Layered Navigation Requirements (ALL must be true)**
1. Input Type must be Dropdown, Multiple Select, or Price (text fields can't be filtered — no filtering support at all regardless of the Layered Navigation setting)
2. Attribute must be indexed (reindex after changes)
3. "Use in Layered Navigation" ≠ No
4. The category must have **Is Anchor = Yes** to inherit products/filters from subcategories

> **⚠️ Exam Trap:** A Text Field input type can never be filtered no matter how "Use in Layered Navigation" is set — and forgetting category Is Anchor = Yes silently hides subcategory products and their filters.

**Anchor vs. Non-Anchor Categories**
- **Anchor**: shows products + aggregates filters from all subcategories
- **Non-anchor**: shows only directly assigned products; no filter inheritance from children
- Top-level landing pages often non-anchor; deep leaf categories typically anchor

**Attribute Sets vs. Attribute Groups**
- **Attribute Set** = the "cabinet" — the whole collection of attributes assigned to a product type; determines what fields *exist*
- **Attribute Group** = the "drawers inside the cabinet" — purely organizes fields within the Admin edit UI; **zero effect on storefront, layered nav, search, or indexing**
- One Attribute Set can contain multiple Attribute Groups; a Group only exists within its parent Set (not reusable across multiple Sets)
- Adding a genuinely new attribute to a product requires adding it to the **Attribute Set**, not a Group
- Reordering/renaming Groups only affects the Admin product-edit screen layout — nothing storefront-facing changes
- System attributes (SKU, price, etc.) cannot be removed from a set

**One-line test:** Does this change what fields exist on the product, or just how they're visually organized for the admin editor? → Existence = Set; Organization = Group

**URL Rewrites**
- URL Key attribute is Store View scope (allows different slugs per store view/locale, e.g., /blue-shirt vs. /chemise-bleue)
- "Use Categories Path for Product URLs" setting affects URL structure
- Category Permanent Redirect setting affects SEO when URL keys change

---

### 1.5 Product Inventory Management — Single & Multi-Location

**Single Source Mode (Legacy)**
- One quantity field, one stock status per product — no per-location tracking
- Configured under Stores > Configuration > Catalog > Inventory
- Key settings: Manage Stock (Yes/No), Backorders (No / Allow Qty Below 0 / Allow + Notify), Min/Max Qty in Cart, Qty Uses Decimals, Notify for Quantity Below, Stock Status (independent Yes/No flag — can force Out of Stock even with qty > 0)

**Multi-Source Inventory (MSI) — Core Concepts**

| Concept | Definition |
|---|---|
| Source | A real physical (or virtual) location holding stock |
| Stock | A sellable pool combining one or more Sources |
| Sales Channel | Typically a Website; each website assigned to exactly ONE Stock |

**Relationship:** Multiple Sources → assigned to one or more Stocks → each Stock assigned to one or more Sales Channels (Websites). A single Source CAN be assigned to multiple Stocks simultaneously (this is fully supported — e.g., shared inventory across two brands/websites from one warehouse).

> **⚠️ Exam Trap:** A Website (Sales Channel) maps to exactly ONE Stock, but a single Source can belong to multiple Stocks — reversing that direction is a classic distractor.

**Admin Setup Flow**
1. Stores > Inventory > Sources — create each physical location
2. Stores > Inventory > Stocks — create a Stock, assign Sources with priority order
3. Assign Stock to Sales Channel(s)/Website(s)
4. On each product, assign to specific Sources with quantity at each

**Source Selection Algorithm (SSA)**
- **Priority (default)**: fulfills from highest-priority Source with enough stock; may split across sources
- **Distance-based**: available as advanced/extension option, selects nearest Source to shipping address
- **Critical scope rule**: SSA only evaluates Sources assigned to the relevant Stock — it will NEVER select a Source outside that Stock, regardless of that Source's own priority or available quantity

> **⚠️ Exam Trap:** Default SSA is Priority-based, not distance-based — and a high-priority Source outside the order's Stock is never chosen. Nearest-warehouse fulfillment requires switching to Distance-based SSA.

**Reservations**
- MSI writes reservation records (signed deltas) instead of directly decrementing quantity — avoids DB row-locking under high concurrency, keeping checkout performant
- **Salable Quantity = Source Quantity − Sum of Reservations**
- A product can show 0 Salable Quantity even if raw Source Quantity appears non-zero, because reservations have consumed it (reservations aren't always reflected as a physical deduction to the source record itself)

> **⚠️ Exam Trap:** Salable Quantity = Source Quantity − Reservations, so a non-zero Source Qty can still show 0 salable — it is not a bug or a manual Out-of-Stock flag.

**Multi-Source Scenario Patterns**

| Scenario | Configuration |
|---|---|
| Nearest-warehouse fulfillment | Both Sources in one Stock; consider Distance-based SSA |
| Fully separate regional inventory pools (zero overlap) | Two separate Stocks, each with its own Source(s), each assigned to its own website |
| Dropship as fallback only | Lower-priority Source in the same Stock as main warehouse |
| Shared inventory across brands/websites | One Source assigned to multiple Stocks (supported) |

**Fundamental limitation of Single Source mode:** no concept of separate physical locations at all — moving to per-warehouse tracking requires MSI.

---

### 1.6 Product Relationships — Related, Up-sells, Cross-sells

Adobe Commerce supports three merchandising relationships, configured under a product's **Related Products, Up-Sells, and Cross-Sells** section. They differ by *where* they appear and *why* they are shown.

| Relationship | Where it displays | Purpose | Mental model |
|---|---|---|---|
| **Related Products** | Product Detail Page (PDP), usually a sidebar/"you may also like" block | Encourage buying the item *in addition* to the current one (complementary) | "Buy this too" |
| **Up-Sells** | PDP, shown as alternatives to the item being viewed | Steer the shopper to a *better/pricier* version (higher margin, upgrade) | "Buy this instead — it's better" |
| **Cross-Sells** | Shopping Cart page (near checkout) | Last-minute impulse add-ons (accessories, batteries) | "Grab this before you check out" |

- All three are **manual product-to-product assignments** in Open Source.
- **Adobe Commerce adds rule-based relations** (Related Products Rules) that auto-populate these blocks from conditions (e.g., "same category, price within 20%"), and can target Customer Segments — see Section 5. Open Source is manual-only.
- These relationships do **not** change price or bundle anything — they are purely merchandising suggestions, independent of Grouped/Bundle product types.

> **⚠️ Exam Trap:** Location is the tell — **Up-Sell/Related = PDP**, **Cross-Sell = cart page**. Up-Sell = a *better alternative* to the viewed item; Related = a *complementary companion*. Don't confuse these merchandising blocks with Grouped/Bundle product types, which actually combine SKUs into a purchase.

---

### 1.7 Category Management & Storefront Display

Categories are managed under **Catalog > Categories**, as a tree under each store's single **root category**.

**Key category settings**

| Setting | Controls |
|---|---|
| **Enable Category** | Whether the category (and its page) is active on the storefront |
| **Include in Menu** | Whether the category appears in the top navigation menu — *visibility in nav only*, unrelated to product aggregation |
| **Is Anchor** | Whether the category aggregates subcategory products and activates layered navigation (see 1.4) |
| **Display Mode** | **Products only** / **Static Block only** / **Static Block and Products** — governs what renders on the category page |
| **Add CMS Block** | Selects a CMS/Static Block to show when Display Mode includes a block |
| **Automatic Sorting / Visual Merchandiser** | Rule-based auto-assignment and sort of products (Adobe Commerce feature) |

**Display Mode options**
- **Products only** — standard product grid/list
- **Static Block only** — shows a CMS block *instead of* products (landing pages with no listing)
- **Static Block and Products** — CMS block above the product grid (common for SEO/marketing intros)

**Product assignment**
- **Manual**: drag/assign specific products into the category
- **Dynamic/Automatic (Visual Merchandiser, Commerce only)**: products auto-populate and auto-sort by rule conditions (e.g., "new in last 30 days," "color = red")

> **⚠️ Exam Trap:** **Include in Menu** only toggles top-nav visibility — it does NOT control whether subcategory products roll up (that's **Is Anchor**) and it does not hide the category page itself (that's **Enable Category**). "Show a marketing banner instead of products" = **Display Mode: Static Block only**; "banner above products" = **Static Block and Products**.

---

### 1.8 Catalog Search

**Default on-prem search engine**
- Adobe Commerce requires **Elasticsearch or OpenSearch** as the catalog search engine (the legacy MySQL search was removed in modern versions).
- **Quick Search** — the storefront search box; matches attributes flagged **Use in Search**.
- **Advanced Search** — a dedicated multi-field form; uses attributes flagged **Visible in Advanced Search**.

**Tuning search relevance**
- **Search Weight** (1–10) per attribute — higher weight makes matches on that attribute rank higher.
- **Search Synonyms** (Marketing > SEO & Search > Search Synonyms) — treat terms as equivalent (e.g., "sofa" ↔ "couch").
- **Search Terms** report / redirects — view what shoppers type; redirect specific terms to a landing page and see suggested terms.

**Live Search (Adobe Commerce SaaS, Sensei-powered)**
- An optional **SaaS** service that replaces the default storefront search experience with AI-driven relevance, faceting, and merchandising rules.
- Connected via **Commerce Services / API keys** and requires **SaaS catalog data sync** — it is *not* the same as the on-prem Elasticsearch/OpenSearch that still powers the Admin/default search. See Section 5.

> **⚠️ Exam Trap:** "Use in Search" (Quick Search index) and "Visible in Advanced Search" are separate attribute toggles. Elasticsearch/OpenSearch is the *required on-prem engine*; **Live Search is an optional Sensei SaaS layer** on top — don't equate the two.

---

### 1.9 Product Media & Reviews

**Product media (Images and Videos)**
- Each product has a gallery; images are assigned **roles**: **Base** (main PDP image), **Small** (category/listing thumbnail), **Thumbnail** (cart/mini views), and **Swatch** (color swatch preview).
- One image can hold multiple roles simultaneously.
- **Video** can be added via a linked URL (YouTube/Vimeo) with API credentials configured.
- **Media/Image attribute** input type lets merchants add additional image attributes beyond the defaults.
- Images can be **imported in bulk** via CSV referencing a media folder path (see Section 5 Import).

**Product Reviews & Ratings**
- Customer reviews are submitted on the PDP and **moderated in Admin** (Marketing > User Content > Reviews) — Pending until approved.
- Configurable to allow **guests** to submit reviews or **registered customers only** (Stores > Config > Catalog > Catalog > Product Reviews).
- **Rating attributes** (e.g., Quality, Value, Price) are defined in Admin and scoped per store view.
- Reviews are **per store view**, supporting localized moderation.

> **⚠️ Exam Trap:** Image **roles** (Base/Small/Thumbnail/Swatch) decide *where each image appears* — a common question hinges on which role drives the listing thumbnail (**Small**) vs the main PDP image (**Base**). Reviews are **moderated** and can be limited to registered customers via config.

---

## PART B: PRACTICE MCQs

### Set 1 — Mixed Foundations (Product Types, Storefront, Pricing, Attributes, Inventory)

**Q1.** A camera can be purchased with an optional lens, optional case, and optional extended warranty, where the customer chooses which items to include and the total price changes based on selections. Which product type?
- A) Configurable Product
- B) Grouped Product
- C) Bundle Product
- D) Simple Product with Custom Options

<details><summary>Answer & Explanation</summary>

**Answer:** C — Bundle Product

**Explanation:** Bundle products let customers select optional/required components with a dynamically calculated total.

**Exam Trap:** A Simple product with Custom Options can add priced add-ons, but it can't track separate inventory per component — a Bundle is required when the selectable items are real, independently-stocked products.

</details>

**Q2.** A dining set (table + 4 chairs) where each piece is also sold individually at its own price, and the customer chooses quantities of each. Which product type?
- A) Bundle Product
- B) Grouped Product
- C) Configurable Product
- D) Virtual Product

<details><summary>Answer & Explanation</summary>

**Answer:** B — Grouped Product

**Explanation:** Independently priced, independently purchasable items displayed together.

**Exam Trap:** A Bundle would collapse the pieces into one cart line at a combined price; Grouped keeps each piece as its own line item at its own price.

</details>

**Q3.** A configurable product has a child variant (Small) out of stock while other sizes remain in stock, with "Display Out of Stock Products" disabled. Storefront effect?
- A) Entire configurable hidden
- B) Small option not selectable/shown
- C) Small shows with a warning
- D) Price auto-increases

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Only the specific out-of-stock variant is excluded; the parent remains visible if other variants are in stock.

**Exam Trap:** The whole configurable only goes out of stock when ALL children are sold out — a single unavailable variant never hides the parent product.

</details>

**Q4.** A merchant wants 15% off for the "Wholesale" group on all "Outdoor Gear" category products, no coupon, reflected directly in displayed price. What's configured?
- A) Cart Price Rule with coupon
- B) Catalog Price Rule scoped to group + category
- C) Special Price per product
- D) Tier Pricing per product

<details><summary>Answer & Explanation</summary>

**Answer:** B — Catalog Price Rule

**Explanation:** Applies automatically, no code, modifies displayed price.

**Exam Trap:** A Cart Price Rule also needs no coupon, but it only discounts inside the cart — it never changes the displayed catalog/PDP price the way a Catalog Price Rule does.

</details>

**Q5.** Customers buying 10+ units should automatically get a lower per-unit price, visible on the PDP before adding to cart. Which feature?
- A) Cart Price Rule
- B) Catalog Price Rule
- C) Tier Pricing
- D) Special Price

<details><summary>Answer & Explanation</summary>

**Answer:** C — Tier Pricing

**Explanation:** Quantity-based price breaks configured directly on the product.

**Exam Trap:** Special Price is a single date-ranged reduction, not quantity-driven; only Tier Pricing keys the discount to the number of units purchased.

</details>

**Q6.** For a dropdown attribute to appear as a layered nav filter, which must ALL be true?
- A) Global scope + required at creation
- B) "Use in Layered Navigation" enabled + category set as Anchor
- C) Used in Product Listing + Comparable
- D) Assigned to default attribute set only

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Attribute-level filterable setting AND category Is Anchor = Yes must both be true.

**Exam Trap:** Global scope and "required at creation" have nothing to do with layered nav; the category's Is Anchor = Yes flag is the requirement test-takers most often forget.

</details>

**Q7.** Warehouses in NY and LA, both in the same Stock assigned to the US website. Order comes from California. With default SSA, how is the fulfilling source determined?
- A) Geographic proximity
- B) Priority ranking of sources within the Stock
- C) Random selection
- D) Always split evenly

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Default SSA is Priority-based, not distance-based.

**Exam Trap:** Geographic proximity only drives sourcing if Distance-based SSA is explicitly configured — the out-of-the-box Priority algorithm ignores the customer's location.

</details>

**Q8.** A product shows quantity 50 but the merchant wants it to appear unavailable immediately, without changing quantity. What to configure?
- A) Set Stock Status to Out of Stock
- B) Set quantity to 0
- C) Disable the product
- D) Set Backorders to No

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** Stock Status is independent from the quantity field.

**Exam Trap:** Setting quantity to 0 also removes availability, but the scenario requires keeping qty at 50 — only the Stock Status flag forces Out of Stock without touching quantity.

</details>

**Q9.** SKU must stay identical across US and Canadian (French) store views, but Description should differ. Correct scope pairing?
- A) SKU: Store View, Description: Global
- B) SKU: Global, Description: Store View
- C) Both: Website
- D) Both: Global

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** SKU is always Global; Description can be Store View scope for localization.

**Exam Trap:** SKU cannot be scoped below Global — any option that scopes SKU to Website or Store View is automatically wrong.

</details>

**Q10.** A bundle is configured with Fixed pricing. What does this mean for the storefront price?
- A) Base price plus/minus selected option prices
- B) Price stays the same regardless of selections
- C) Price = highest-priced option only
- D) Fixed pricing disables option selection

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Fixed pricing = one set price no matter what's selected.

**Exam Trap:** Fixed pricing does NOT disable option selection — customers still choose options; only the total price stays constant regardless of what they pick.

</details>

---

### Set 2 — Bundle vs. Grouped Focused Drill

**Q1.** A gift basket lets customers pick any 4 of 10 possible snacks; total price changes depending on selection. Product type?
- A) Grouped Product
- B) Bundle Product (Dynamic)
- C) Bundle Product (Fixed)
- D) Configurable Product

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Picking a subset FROM options with variable pricing = Bundle, Dynamic.

**Exam Trap:** A Fixed-price Bundle would charge the same total regardless of which 4 snacks are chosen; because the price changes with selection, it must be Dynamic.

</details>

**Q2.** A cleanser, toner, and moisturizer — each already individually listed with own prices — shown together on one page, each keeping its own price and becoming separate cart line items.
- A) Bundle (Fixed)
- B) Grouped Product
- C) Bundle (Dynamic)
- D) Simple with Custom Options

<details><summary>Answer & Explanation</summary>

**Answer:** B — Grouped Product

**Explanation:** Existing standalone products displayed together; separate cart lines.

**Exam Trap:** A Bundle would merge the three items into one cart line at a combined price — Grouped keeps each as its own line item at its own price.

</details>

**Q3.** The merchant wants the assembled product to appear as ONE cart line item regardless of how many components were selected. Which type must this be?
- A) Grouped Product
- B) Bundle Product
- C) Either, depending on settings
- D) Configurable Product only

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Bundle always collapses into a single cart line; Grouped always creates separate lines.

**Exam Trap:** Cart-line behavior is fixed by product type, not a toggle — you can't make a Grouped product show as one line or a Bundle show as many.

</details>

**Q4.** A bundle with Fixed pricing at $99. Customer selects a premium option that would normally add $20 in a dynamic bundle. Final price at checkout?
- A) $99, unaffected
- B) $119, Fixed still adds surcharges
- C) $79
- D) Cannot be determined

<details><summary>Answer & Explanation</summary>

**Answer:** A — $99

**Explanation:** Fixed pricing means total doesn't change with selections.

**Exam Trap:** The $20 "premium option" surcharge only exists under Dynamic pricing; a Fixed-price bundle ignores individual option prices entirely.

</details>

**Q5.** Which statement correctly distinguishes the KEY structural difference between Bundle and Grouped?
- A) Bundle only virtual, Grouped only physical
- B) Grouped links existing independent products for display; Bundle defines selectable options/components combining into one purchasable unit
- C) Bundle never has a price
- D) Grouped always requires a coupon

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Grouped links pre-existing, independent products for display, while a Bundle defines selectable options/components that combine into one purchasable unit.

**Exam Trap:** Both types can hold physical items and neither requires a coupon — the real distinction is structural (linking existing products vs. defining combinable options), not virtual/physical or pricing.

</details>

---

### Set 3 — Bundle & Grouped × MSI Inventory (Cross-Topic)

**Q1.** A bundle set to "Ship Bundle Items Separately" contains a laptop (Component A) and bag (Component B), mapped to different sources under MSI. What happens to inventory when ordered?
- A) All deducted from a single source tied to bundle SKU
- B) Each component's stock deducted independently, potentially from different sources
- C) Only the first component's stock is deducted
- D) Stock deduction disabled entirely

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** With "Ship Bundle Items Separately," each component is sourced and deducted on its own, so components can come from different MSI Sources.

**Exam Trap:** "Ship Separately" is exactly what enables multi-source deduction; "Ship Together" would instead force all components from a single source.

</details>

**Q2.** How does MSI inventory tracking work for a Grouped product vs. its children?
- A) Grouped parent has combined stock pool shared by children
- B) Ship Bundle Items setting determines Grouped deduction
- C) Each child has fully independent inventory; the Grouped parent holds no stock itself
- D) Grouped requires a single shared source across children

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** A Grouped product's parent holds no inventory; each linked child tracks its own stock independently under MSI.

**Exam Trap:** There is no combined/shared stock pool on a Grouped parent — deduction always happens at the individual child level, and the "Ship Bundle Items" setting is a Bundle-only concept.

</details>

**Q3.** A bundle set to "Ship Bundle Items Together," but the two components are only stocked at two different, non-overlapping warehouses. What issue does this create?
- A) "Together" requires more config steps
- B) "Together" assumes fulfillment from a single source, which breaks down if components only exist at different locations
- C) "Together" only works with Fixed pricing
- D) "Together" auto-enables backorders

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** "Ship Together" assumes a single source can fulfill the whole bundle; if components only exist at different, non-overlapping warehouses, no single source can satisfy the order.

**Exam Trap:** The problem is a sourcing/fulfillment conflict, not a pricing restriction or an automatic backorder side effect.

</details>

**Q4.** Within a Grouped product, one of three linked children has 0 saleable quantity (Out of Stock) while the other two remain In Stock. Storefront effect?
- A) Entire Grouped page unavailable
- B) Out-of-stock child's price hidden but purchasable
- C) Out-of-stock child shown as unavailable/hidden; others remain purchasable
- D) All children auto-backordered

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** The out-of-stock child is shown as unavailable/hidden while the in-stock children remain purchasable.

**Exam Trap:** One sold-out child never makes the whole Grouped page unavailable, and it isn't silently purchasable with just a hidden price.

</details>

**Q5.** Considering MSI reservations: how does the reservation record differ between a Bundle (Ship Together) and a Grouped product when ordered?
- A) Identical reservation records for both
- B) Bundle (Together) creates one coordinated reservation tied to a single cart line; Grouped creates separate, independent reservations per child line item
- C) Neither uses MSI reservations
- D) Only Grouped triggers reservations

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** A Bundle shipped together creates one coordinated reservation on its single cart line, while a Grouped product creates a separate reservation per child line item.

**Exam Trap:** Both product types use MSI reservations — the difference is one coordinated reservation vs. independent per-child reservations, not whether reservations occur at all.

</details>

---

### Set 4 — Product Attributes & Catalog Navigation

**Q1.** Difference between "Filterable (with results)" and "Filterable (no results)"?
- A) They behave identically
- B) "With results" hides options that would return 0 products; "no results" still shows them
- C) "No results" disables the filter entirely
- D) "With results" only works on Price attributes

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** "Filterable (with results)" hides filter options that would return zero products; "Filterable (no results)" still lists them even at 0 matches.

**Exam Trap:** Both settings enable filtering and both work on any filterable attribute type — the only difference is whether zero-result options are displayed.

</details>

**Q2.** "Use in Layered Navigation" is set to "Filterable (with results)" but the attribute's Input Type is "Text Field," and it still won't filter. What's the issue?
- A) Dropdown
- B) Multiple Select
- C) Text Field
- D) Price

<details><summary>Answer & Explanation</summary>

**Answer:** C — Text Field

**Explanation:** Layered nav filtering requires Dropdown, Multiple Select, or Price input types.

**Exam Trap:** No layered-nav setting can rescue a Text Field — the input type itself is unsupported for filtering, regardless of how "Use in Layered Navigation" is configured.

</details>

**Q3.** Attribute is correctly Dropdown, indexed, and Filterable — but subcategory products/filters aren't appearing on the parent category page. Likely misconfiguration?
- A) Attribute scope must be Global
- B) Parent category's "Is Anchor" must be Yes
- C) Attribute needs a new Attribute Set
- D) Reindexing has no effect

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Even a correctly configured filterable attribute won't aggregate subcategory products unless the parent category has Is Anchor = Yes.

**Exam Trap:** The attribute is fine here — the miss is the category-level Is Anchor flag, not attribute scope or a new Attribute Set.

</details>

**Q4.** URL Key needs to differ between English (/blue-shirt) and French (/chemise-bleue) store views. Required scope?
- A) Global
- B) Website
- C) Store View
- D) No configurable scope

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** URL Key is Store View scope, so each locale/store view can carry its own slug.

**Exam Trap:** Global or Website scope would force one shared URL key across all store views, making per-locale slugs impossible.

</details>

**Q5.** Functional difference between Attribute Set and Attribute Group?
- A) Set determines available attributes; Group is a cosmetic admin-UI subdivision with no storefront effect
- B) Group determines available attributes; Set is cosmetic
- C) Both directly affect layered nav
- D) Same concept, two names

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** The Attribute Set defines which attributes exist on a product; Attribute Groups only organize those fields within the Admin edit screen, with no storefront, search, or indexing impact.

**Exam Trap:** Reordering or renaming Groups changes nothing on the storefront — only adding to the Set changes which fields actually exist on the product.

</details>

**Q6.** Configurable Product uses "Color" as the defining attribute for child variations. Required scope?
- A) Global — needed for consistent parent-child relationships across the catalog
- B) Store View — so each store defines variations independently
- C) Website — to align with per-website pricing
- D) Scope doesn't matter

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** A configurable's defining attribute (Color) must be Global so parent-child variation logic stays consistent across the entire catalog.

**Exam Trap:** Store View or Website scope on a defining attribute breaks the variation matrix — it is not a valid choice for configurable-defining attributes.

</details>

---

### Set 5 — Attribute Set vs. Attribute Group (Isolated Drill)

**Q1.** A merchant wants to add a brand-new attribute "Fabric Weight" that doesn't currently exist on T-Shirt products. What must they do?
- A) Create a new Attribute Group and add it there
- B) Add "Fabric Weight" to the product's Attribute Set
- C) Groups automatically inherit new attributes from other sets
- D) Just reindex the catalog

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** New fields require the Attribute Set, not a Group.

**Exam Trap:** Attribute Groups only organize existing fields; they can't introduce a brand-new attribute, and Groups never auto-inherit attributes from other Sets.

</details>

**Q2.** A merchant reorders/renames Attribute Groups purely for Admin UI convenience. Effect on storefront?
- A) Layered nav filter order changes
- B) Product URL structure affected
- C) Nothing changes on storefront — only the Admin edit screen layout is affected
- D) Attribute scope resets

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Renaming or reordering Attribute Groups only changes the Admin product-edit layout; nothing storefront-facing (layered nav, URLs, scope) is affected.

**Exam Trap:** Group order has zero influence on layered-nav filter order or URL structure — those are unrelated to admin-UI grouping.

</details>

**Q3.** Containment relationship between Attribute Sets and Attribute Groups?
- A) One Set can contain multiple Groups; a Group exists only within its parent Set
- B) One Group can contain multiple Sets
- C) No containment relationship
- D) Each Set can only have one Group

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** One Attribute Set can contain many Attribute Groups, and each Group exists only inside its parent Set (not reusable across Sets).

**Exam Trap:** The containment is one-directional — a Group never contains Sets, and a Set is not limited to a single Group.

</details>

---

### Set 6 — Storefront Behavior & Pricing (Deep Practice)

**Q1.** A configurable product has three color variants priced $40, $45, $50. Before selection, what price displays on the PDP by default?
- A) Average price
- B) Price of the first-created child
- C) Lowest price among available/in-stock variants
- D) Highest price

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Before any selection, the configurable PDP shows the lowest price among available/in-stock child variants (the "As low as" price).

**Exam Trap:** It is not an average or the first-created child's price — it's specifically the lowest in-stock variant price.

</details>

**Q2.** A Catalog Price Rule for 20% off "Sale" category is saved, but the storefront discount isn't showing yet. Most likely explanation?
- A) Coupon code not shared yet
- B) Catalog Rule Price indexer hasn't run since the rule was saved
- C) Catalog rules never affect displayed price
- D) Rule needs "Discard subsequent rules" to activate

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Catalog Price Rules only affect the storefront after the Catalog Rule Price indexer reindexes; until then the discount won't appear.

**Exam Trap:** Catalog Price Rules need no coupon, and "Discard subsequent rules" only controls rule stacking — neither explains a missing discount.

</details>

**Q3.** A Cart Price Rule offers 10% off orders over $100, no coupon required. Effect on the PDP/category listing price?
- A) Shows strikethrough discounted price on PDP
- B) Catalog/PDP price remains unchanged; discount only applies once in the cart
- C) Category listing auto-updates to reflect cart rule
- D) Requires the Catalog Price Rule indexer

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** A Cart Price Rule discounts only within the cart/checkout; the PDP and category listing prices stay unchanged.

**Exam Trap:** Cart rules never alter the displayed catalog price and don't use the Catalog Rule Price indexer — that indexer serves Catalog Price Rules only.

</details>

**Q4.** A merchant wants to send 5,000 customers a unique, single-use discount code each via email. Which Cart Price Rule coupon setting?
- A) No Coupon
- B) Specific Coupon
- C) Auto-generated (Coupon Qty)
- D) Cannot generate multiple codes

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** The Auto-generated (Coupon Qty) option produces bulk, unique, single-use codes suitable for emailing individually.

**Exam Trap:** "Specific Coupon" creates one shared code for everyone; only Auto-generated yields thousands of distinct per-customer codes.

</details>

**Q5.** MAP policy: price hidden on PDP, but customer can click "See price" to reveal it in a popup without adding to cart. Which MAP Display Actual Price setting?
- A) In Cart
- B) On Gesture
- C) Before Order Confirmation
- D) Use config

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** "On Gesture" hides the price but reveals it via a click/popup without adding to cart.

**Exam Trap:** "In Cart" and "Before Order Confirmation" reveal the price at a later checkout step, not via an on-page click — only On Gesture uses the popup reveal.

</details> but need entirely independent prices per website for the same SKUs. What must be configured?
- A) Catalog Price Scope (Catalog > General > Price) = Website
- B) Separate Attribute Set per website
- C) Tier Pricing scoped per group per website
- D) Price is always Global and cannot vary

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** Setting Catalog Price Scope to Website lets the same SKU carry entirely independent prices per website.

**Exam Trap:** Price scope is a store-config setting, not something solved by separate Attribute Sets or per-group tier pricing.

</details>

**Q7.** A bundle has one Required option and one Optional option. Effect on Add to Cart button?
- A) Both must be selected before enabling
- B) Neither affects the button
- C) Required must be selected before Add to Cart is enabled; Optional can be skipped
- D) Optional must be selected first

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Only Required bundle options gate the Add to Cart button; Optional options can be left unselected.

**Exam Trap:** Optional options never block Add to Cart — requiring both selections misreads how required vs. optional options behave.

</details>

---

### Set 7 — Product Inventory Management (Single & Multi-Location)

**Q1.** Correct relationship between Sources, Stocks, and Sales Channels in MSI?
- A) Stock assigned to Source, Source assigned to Sales Channel
- B) Sales Channel assigned to multiple Stocks, each Stock has one Source
- C) One or more Sources assigned to a Stock; each Sales Channel (Website) assigned to one Stock
- D) All independent, no assignment relationship

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** One or more Sources are assigned to a Stock, and each Sales Channel (Website) is assigned to exactly one Stock.

**Exam Trap:** The direction matters — Sources roll up into Stocks, not the reverse, and a Website maps to a single Stock.

</details>

**Q2.** Why does MSI use reservations instead of directly decrementing quantity at order time?
- A) Allows unlimited overselling
- B) Avoids DB row-locking on the quantity column during high-concurrency checkouts
- C) Only used for virtual products
- D) Replaces the need for a Source entirely

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Reservations record signed deltas instead of decrementing the quantity column directly, avoiding row-locking during high-concurrency checkouts.

**Exam Trap:** Reservations don't enable overselling or replace Sources — they're a concurrency/performance mechanism.

</details>

**Q3.** A product shows Source Quantity of 20 at a warehouse, but Salable Quantity displays as 0 on the storefront. Explanation?
- A) System error
- B) Reservations against the product have consumed the available quantity
- C) Stock Status was manually set Out of Stock
- D) Only occurs when Manage Stock is disabled

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Salable Quantity = Source Quantity − Reservations, so pending reservations can drive salable to 0 while raw Source Quantity still reads 20.

**Exam Trap:** This isn't a system error or a manual Stock Status change — it's the normal reservation math at work.

</details>

**Q4.** East Coast website must draw ONLY from the East Coast warehouse, West Coast website ONLY from the West Coast warehouse, zero overlap. Configuration?
- A) One Stock with both Sources, assigned to both websites
- B) One Source per website but shared Stock
- C) Two separate Stocks, each with its respective Source, each assigned to its corresponding website
- D) Not supported

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Zero-overlap regional pools require two separate Stocks, each with its own Source, each assigned to its corresponding website.

**Exam Trap:** A single shared Stock (even holding both Sources) would let either website draw from either warehouse — the opposite of the isolation required.

</details>

**Q5.** Two brands share one warehouse and want both websites to draw from the SAME physical inventory pool. Is assigning one Source to two different Stocks supported?
- A) Not supported, one Stock per Source only
- B) Fully supported — a Source can be assigned to multiple Stocks simultaneously
- C) Requires duplicating the Source
- D) Requires reverting to Single Source mode

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** A single Source can be assigned to multiple Stocks simultaneously, so two brand websites can share one warehouse's inventory pool.

**Exam Trap:** Sharing a Source across Stocks is fully supported — you never duplicate the Source or fall back to Single Source mode.

</details>

**Q6.** A high-priority Source is NOT assigned to the Stock used by the order's website. Will SSA ever select it?
- A) Never — SSA only considers Sources assigned to the relevant Stock
- B) Always selected first regardless of Stock assignment
- C) Only if all Sources in the assigned Stock are out of stock
- D) SSA auto-reassigns the Source

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** SSA only evaluates Sources assigned to the Stock tied to the order's website; a Source outside that Stock is never selected.

**Exam Trap:** A Source's high priority is irrelevant if it isn't in the relevant Stock — priority only ranks Sources already within that Stock.

</details>

**Q7.** A merchant on legacy Single Source mode wants to track stock separately across two warehouses. Fundamental limitation to address?
- A) Cannot track backorders at all
- B) Single Source has one combined quantity/status with no concept of separate physical locations; MSI is needed
- C) Only available for virtual products
- D) Requires a separate installation

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Single Source mode has one combined quantity/status with no notion of separate locations, so per-warehouse tracking requires enabling MSI.

**Exam Trap:** MSI is built-in (no separate install) and works for all product types — the limitation is purely Single Source's single-location model.

</details>

---

### Set 8 — Gap Coverage: Virtual/Downloadable, Custom Options, Pricing Precedence & Inventory Edge Cases

**Q1.** A merchant sells a 2-year extended warranty that has no physical component and never ships. Which product type is most appropriate?
- A) Downloadable Product
- B) Virtual Product
- C) Simple Product with a $0 weight
- D) Bundle Product

<details><summary>Answer & Explanation</summary>

**Answer:** B — Virtual Product

**Explanation:** Virtual products represent non-shippable items (services, warranties, memberships) — they carry no weight and trigger no shipping step at checkout. A Downloadable product would be wrong because nothing is delivered as a file; a zero-weight Simple product still behaves as a shippable good in the flow.

**Exam Trap:** A zero-weight Simple product still enters the shipping flow at checkout — only a Virtual product removes the shipping step entirely.

</details>

**Q2.** A store sells an e-book that customers download after purchase, and wants to offer a free preview chapter. Which product type and feature combination fits?
- A) Virtual Product with a Custom Option
- B) Downloadable Product with a Sample file
- C) Grouped Product linking the preview and full book
- D) Simple Product with a MAP setting

<details><summary>Answer & Explanation</summary>

**Answer:** B — Downloadable Product with a Sample

**Explanation:** Downloadable products deliver linked files post-purchase and support a separately configurable **Sample** (the free preview) that can be accessed without buying. Virtual products deliver no file.

**Exam Trap:** Only a Downloadable product's Sample lets shoppers access a preview file without purchasing — a Virtual product delivers no downloadable content at all.

</details>

**Q3.** A single T-shirt SKU offers optional "Add gift wrap (+$5)" and "Add a personalized message (+$3)" that do not need separate inventory tracking. What is the correct way to model these add-ons?
- A) Configurable attributes so each combination is its own child SKU
- B) Custom Options on the Simple product, each with a price modifier
- C) A Bundle product with required options
- D) Tier Pricing keyed to the add-on quantity

<details><summary>Answer & Explanation</summary>

**Answer:** B — Custom Options

**Explanation:** Custom Options live on a single Simple SKU and add price modifiers without creating separately tracked SKUs or stock. Configurable attributes would wrongly spawn distinct child SKUs; a Bundle changes the product type and cart behavior unnecessarily.

**Exam Trap:** Configurable attributes create real child SKUs with their own inventory — overkill for gift wrap or a message that needs no stock tracking.

</details>

**Q4.** A product has Base Price $100, an active Special Price of $80, and a Tier Price of $75 when buying 5+ units. A customer adds 6 units. Which unit price applies, and why?
- A) $80 — Special Price always overrides tier breaks
- B) $75 — the Tier Price wins because it is lower once the qty threshold is met
- C) $100 — tier and special prices cannot both be active
- D) $77.50 — the two discounts are averaged

<details><summary>Answer & Explanation</summary>

**Answer:** B — $75

**Explanation:** When the quantity threshold is met, the Tier Price is compared and applies if it is lower than the (Special) price. Here $75 < $80, so the tier break wins for the 6-unit purchase.

**Exam Trap:** Special Price does not automatically override tier breaks — the system applies whichever is lower once the qty threshold is reached, and discounts are never averaged.

</details>

**Q5.** A retailer wants nearest-warehouse fulfillment so shipments leave the Source physically closest to the customer. Both warehouses are in one Stock. What must be configured?
- A) Priority-based SSA with warehouses ranked by size
- B) Distance-based SSA (Source Selection Algorithm)
- C) Two separate Stocks, one per warehouse
- D) Backorders enabled on both Sources

<details><summary>Answer & Explanation</summary>

**Answer:** B — Distance-based SSA

**Explanation:** The default Priority algorithm fulfills by rank, not geography. Distance-based SSA selects the nearest Source to the shipping address. Both Sources must be in the same Stock for either algorithm to consider them.

**Exam Trap:** Ranking warehouses by size under Priority SSA still ignores the customer's location — only Distance-based SSA factors in geographic proximity.

</details>

**Q6.** "Display Out of Stock Products" is set to **No** globally. A configurable product's Medium variant sells out while Small and Large remain in stock. What does the shopper see?
- A) The whole configurable is hidden from the catalog
- B) The Medium option is omitted from the selectable variations; the product stays visible
- C) Medium appears greyed-out but selectable
- D) The product shows "As low as" using the Medium price

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** With the global setting = No, out-of-stock children are omitted entirely from the options. The parent remains visible because in-stock variants still exist. (If the setting were Yes, Medium would show disabled/greyed instead.)

**Exam Trap:** With the setting = No the sold-out variant is removed, not greyed out — greyed-but-visible behavior only happens when "Display Out of Stock Products" = Yes.

</details>

**Q7.** A merchant sets Backorders to "Allow Qty Below 0 and Notify Customer" on a product currently at 0 salable qty. What happens when a shopper orders it?
- A) The order is blocked until stock is replenished
- B) The order is accepted, quantity goes negative, and the customer is notified the item is backordered
- C) The product is automatically hidden until restocked
- D) Stock Status flips to Out of Stock and prevents purchase

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** This backorders setting lets the sale proceed with negative quantity and surfaces a backorder notice to the shopper. "Allow Qty Below 0" (without notify) would do the same but without the customer-facing message.

**Exam Trap:** The "and Notify Customer" variant behaves identically to plain "Allow Qty Below 0" for stock — the only difference is the shopper-facing backorder message, not whether the order is accepted.

</details>

**Q8.** A brand wants its advertised price hidden everywhere on the storefront and only revealed in the shopping cart. Which MAP "Display Actual Price" setting achieves this?
- A) On Gesture
- B) In Cart
- C) Before Order Confirmation
- D) Use config

<details><summary>Answer & Explanation</summary>

**Answer:** B — In Cart

**Explanation:** "In Cart" hides the actual price everywhere except the cart. "Before Order Confirmation" reveals it later (at checkout), and "On Gesture" reveals it via a click/popup — both would show it before the cart.

**Exam Trap:** "Before Order Confirmation" sounds later than the cart but reveals the price at checkout; only "In Cart" keeps it hidden everywhere up to the cart page itself.

</details>

**Q9.** A configurable product must use "Color" as a defining variation attribute across the entire catalog. A merchant mistakenly sets Color's scope to Store View. What problem does this cause?
- A) Nothing — Store View scope is valid for configurable attributes
- B) Parent-child variation logic breaks because defining attributes must be Global for consistent variations across the whole catalog
- C) The color swatches stop rendering
- D) Prices per website can no longer differ

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Attributes that define configurable variations must be **Global** scope so the parent-child relationship is consistent everywhere. Store View or Website scope would make the variation matrix inconsistent and is not permitted for configurable-defining attributes.

**Exam Trap:** Store View scope is valid for many attributes (like name), but never for one that defines configurable variations — that specific role forces Global scope.

</details>

**Q10.** A bundle is set to Fixed pricing at $150 AND "Ship Bundle Items Separately," with components stocked at two different Sources. Which statement is correct?
- A) Fixed pricing forces single-source shipping, overriding the separate-shipping setting
- B) The customer pays the fixed $150 regardless of selections, while each component's stock is deducted independently from its own Source
- C) Dynamic pricing is required whenever items ship separately
- D) The bundle cannot be ordered because pricing and shipping settings conflict

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Price type (Fixed vs Dynamic) and shipment handling (Together vs Separately) are independent settings. Fixed pricing keeps the total at $150 no matter what is selected, while "Ship Separately" lets each component deduct stock from its own Source under MSI.

**Exam Trap:** Pricing type and shipment handling are orthogonal — Fixed pricing never forces single-source shipping, and the two settings never conflict to block an order.

</details>

**Q11.** A parent (landing) category shows only its directly assigned products and none of the filters or products from its subcategories. Layered navigation filters are correctly configured on the attributes. What is the misconfiguration?
- A) The attributes need Global scope
- B) The parent category's "Is Anchor" is set to No
- C) The catalog needs a full reindex only
- D) The subcategory products are set to Not Visible Individually

<details><summary>Answer & Explanation</summary>

**Answer:** B — Is Anchor = No

**Explanation:** A non-anchor category shows only directly assigned products and does not aggregate products or filters from its children. Setting Is Anchor = Yes makes it inherit subcategory products and their layered-nav filters.

**Exam Trap:** Attribute scope and reindexing aren't the issue here — missing subcategory products/filters point straight to the category's Is Anchor being No.

</details>

**Q12.** A merchant needs the same SKU to keep an identical product name globally but a localized URL key per store view (/red-shoe vs /chaussure-rouge). Which scope settings are correct?
- A) Name: Store View, URL Key: Global
- B) Name: Global, URL Key: Store View
- C) Both Global
- D) Both Website

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** URL Key is Store View scope, allowing per-locale slugs. Keeping the Name Global means it stays identical across store views. (Often Name is Store View to allow translation, but the scenario explicitly requires an identical global name.)

**Exam Trap:** URL Key must be Store View for per-locale slugs; scoping it Global or Website would force one shared URL across all store views.

</details>

---

### Set 9 — Gap Coverage: Relationships, Categories, Search & Media

**Q1.** A merchant wants to suggest accessory items on the **shopping cart page** just before checkout. Which product relationship is this?
- A) Related Products
- B) Up-Sells
- C) Cross-Sells
- D) Grouped Product

<details><summary>Answer & Explanation</summary>

**Answer:** C — Cross-Sells

**Explanation:** Cross-Sells display on the shopping cart page as impulse add-ons shown right before checkout. Related Products and Up-Sells both appear on the PDP, and a Grouped product is a product type, not a merchandising suggestion.

**Exam Trap:** The display location is the giveaway — cart page = Cross-Sell; PDP = Related/Up-Sell. Cross-Sells are the only one tied to the cart.

</details>

**Q2.** A merchant wants the PDP to steer shoppers toward a higher-end model of the item they are viewing. Which relationship fits?
- A) Cross-Sells
- B) Up-Sells
- C) Related Products
- D) Bundle options

<details><summary>Answer & Explanation</summary>

**Answer:** B — Up-Sells

**Explanation:** Up-Sells present better/pricier alternatives to the currently viewed product on the PDP, encouraging an upgrade. Related Products are complementary companions, and Cross-Sells live on the cart page.

**Exam Trap:** Up-Sell = "buy this *instead*, it's better"; Related = "buy this *too*, it complements." Both are on the PDP, so the intent (alternative vs. companion) distinguishes them.

</details>

**Q3.** A merchant wants a category page to show a promotional banner (CMS block) **above** the normal product grid. Which Display Mode?
- A) Products only
- B) Static Block only
- C) Static Block and Products
- D) Anchor

<details><summary>Answer & Explanation</summary>

**Answer:** C — Static Block and Products

**Explanation:** "Static Block and Products" renders the selected CMS block above the product listing. "Static Block only" would replace the products entirely, and "Products only" shows no block. Anchor is a separate setting unrelated to Display Mode.

**Exam Trap:** "Static Block only" hides the product grid completely — pick "Static Block and Products" when the requirement is a banner *plus* the listing.

</details>

**Q4.** A category exists and is enabled but does not appear in the storefront top navigation menu. Which single setting most likely needs changing?
- A) Is Anchor
- B) Include in Menu
- C) Display Mode
- D) Enable Category

<details><summary>Answer & Explanation</summary>

**Answer:** B — Include in Menu

**Explanation:** "Include in Menu" controls whether an enabled category shows in the top navigation. Enable Category is already on (the category exists/works), Is Anchor governs product aggregation, and Display Mode governs page content.

**Exam Trap:** Include in Menu ≠ Enable Category ≠ Is Anchor. Menu visibility is purely navigational and independent of whether the category page works or aggregates subcategory products.

</details>

**Q5.** A shopper searches "couch" but products are tagged only as "sofa," returning no results. What is the correct fix?
- A) Increase the Search Weight on the name attribute
- B) Add a Search Synonym mapping "couch" to "sofa"
- C) Enable Advanced Search on the attribute
- D) Switch the search engine to MySQL

<details><summary>Answer & Explanation</summary>

**Answer:** B — Add a Search Synonym

**Explanation:** Search Synonyms (Marketing > SEO & Search > Search Synonyms) make terms equivalent so "couch" returns "sofa" products. Search Weight only affects ranking of already-matching results, and MySQL search no longer exists in modern versions.

**Exam Trap:** Synonyms fix *no-result vocabulary mismatches*; Search Weight only re-ranks results that already match. They solve different problems.

</details>

**Q6.** Which search engine is required to power catalog search in modern Adobe Commerce, and how does Live Search relate to it?
- A) MySQL fulltext; Live Search replaces the database
- B) Elasticsearch or OpenSearch is the required on-prem engine; Live Search is an optional Sensei-powered SaaS layer connected via Commerce Services
- C) Live Search is the required on-prem engine
- D) They are the same product under two names

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Modern Adobe Commerce requires Elasticsearch or OpenSearch as the on-prem catalog search engine. Live Search is a separate, optional SaaS service (Adobe Sensei) that requires SaaS catalog data sync and layers AI-driven search/merchandising on top.

**Exam Trap:** Elasticsearch/OpenSearch (required, on-prem) is not the same as Live Search (optional, SaaS). Don't pick MySQL — legacy MySQL search was removed.

</details>

**Q7.** Which image role determines the thumbnail shown on category/product-listing pages?
- A) Base
- B) Small
- C) Thumbnail
- D) Swatch

<details><summary>Answer & Explanation</summary>

**Answer:** B — Small

**Explanation:** The **Small** image role drives category and product-listing thumbnails. **Base** is the main PDP image, **Thumbnail** is used in cart/mini views, and **Swatch** previews a color swatch. One image can hold multiple roles.

**Exam Trap:** Don't assume the "Thumbnail" role controls listing thumbnails — the **Small** role does. Base = main PDP image; Thumbnail = cart/mini-cart.

</details>

**Q8.** A merchant wants only registered customers (not guests) to submit product reviews, with each review moderated before publishing. How is this achieved?
- A) Reviews are always guest-enabled and auto-published
- B) Set the review configuration to registered customers only; reviews land in a Pending state for Admin moderation
- C) Disable the Reviews module entirely
- D) Reviews can only be added via import

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Product Reviews configuration (Stores > Config > Catalog > Catalog > Product Reviews) can restrict submission to registered customers, and submitted reviews are held as Pending in Admin (Marketing > User Content > Reviews) until approved.

**Exam Trap:** Reviews are moderated (Pending → Approved), not auto-published, and the guest-vs-registered restriction is a configuration toggle — not a hard-coded behavior.

</details>

---

## Quick-Reference Exam Traps — Section 1

1. **Bundle vs. Grouped** — Bundle = customer picks *from* options, collapses to ONE cart line, has a Price Type (Fixed/Dynamic). Grouped = customer picks *quantities* of already-defined products, MULTIPLE cart lines, no parent price field.
2. **Fixed vs. Dynamic bundle pricing** — Fixed = one set price regardless of selections; Dynamic = sum of selected options. Fixed pricing never adds option surcharges.
3. **Custom Options vs. Configurable Attributes** — Custom Options add a price modifier on a single Simple SKU with no separate stock; Configurable Attributes create genuinely separate, independently tracked child SKUs.
4. **Virtual vs. Downloadable** — Virtual = non-shippable service/warranty/membership with no file delivery; Downloadable = digital file delivered post-purchase, supports a free Sample.
5. **Price precedence** — Base/Special first, then Tier Price wins if lower once the qty threshold is met, then Catalog Price Rules (produce the PDP price), then Cart Price Rules at checkout.
6. **Catalog Price Rules require the Catalog Rule Price indexer** to reindex before the discount shows on the storefront — the most common "why isn't my discount showing" cause.
7. **Stock Status is independent of quantity** — a product with qty 50 can be forced Out of Stock via the Stock Status flag alone.
8. **Salable Quantity = Source Quantity − Reservations** — a non-zero Source Qty can still show 0 salable because reservations consumed it. MSI uses reservations to avoid DB row-locking under high concurrency.
9. **SSA only considers Sources in the relevant Stock** — a high-priority Source outside that Stock is never selected, regardless of its own priority or quantity.
10. **Default SSA is Priority-based, not distance-based** — nearest-warehouse fulfillment requires switching to Distance-based SSA, with all candidate Sources in the same Stock.
11. **One Source can belong to multiple Stocks** (shared inventory across brands/websites) — fully supported. Separate, zero-overlap regional pools require separate Stocks.
12. **Configurable-defining attributes (e.g., Color) MUST be Global scope** — needed for consistent parent-child variation logic across the whole catalog.
13. **Layered navigation needs ALL of:** Dropdown/Multi-Select/Price input type + indexed + "Use in Layered Navigation" ≠ No + category Is Anchor = Yes. Text fields can never be filtered.
14. **"Filterable (with results)" hides zero-result options; "Filterable (no results)" still shows them.**
15. **Attribute Set vs. Attribute Group** — the Set determines which fields *exist* on a product; the Group is purely admin-UI organization with zero storefront/search/indexing effect. New attributes go on the Set, not a Group.
16. **URL Key is Store View scope** (per-locale slugs); SKU is always Global. Configurable-defining attributes are Global.
17. **"Display Out of Stock Products" is a global setting** — Yes shows sold-out variants greyed/disabled; No omits them entirely. If ALL children are out of stock, the parent configurable shows out of stock.
18. **MAP "Display Actual Price" variants** — In Cart (hidden until cart), Before Order Confirmation (hidden until checkout), On Gesture (revealed via click/popup), Use config (store default).
19. **Configurable PDP pre-selection price** shows the lowest-priced available/in-stock child ("As low as $X").
20. **Grouped products hold no parent-level inventory** — each child has fully independent stock; the parent never carries its own quantity.
21. **Product relationships by location** — Related & Up-Sell display on the PDP (Up-Sell = better alternative, Related = complementary companion); Cross-Sell displays on the cart page. Rule-based relations are Adobe Commerce only.
22. **Category settings are distinct** — Enable Category (page active), Include in Menu (top-nav visibility), Is Anchor (subcategory rollup + layered nav), Display Mode (Products / Static Block only / Static Block and Products) each control different things.
23. **Elasticsearch/OpenSearch is the required on-prem search engine** (MySQL search was removed); Live Search is an optional Sensei SaaS layer, not a replacement engine. "Use in Search" and "Visible in Advanced Search" are separate attribute toggles; Search Weight re-ranks, Synonyms fix vocabulary mismatches.
24. **Image roles** — Base = main PDP image, Small = listing/category thumbnail, Thumbnail = cart/mini-cart, Swatch = color preview. One image can hold multiple roles.
25. **Product Reviews are moderated** (Pending → Approved) and can be limited to registered customers via configuration.
