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

**Admin Setup Flow**
1. Stores > Inventory > Sources — create each physical location
2. Stores > Inventory > Stocks — create a Stock, assign Sources with priority order
3. Assign Stock to Sales Channel(s)/Website(s)
4. On each product, assign to specific Sources with quantity at each

**Source Selection Algorithm (SSA)**
- **Priority (default)**: fulfills from highest-priority Source with enough stock; may split across sources
- **Distance-based**: available as advanced/extension option, selects nearest Source to shipping address
- **Critical scope rule**: SSA only evaluates Sources assigned to the relevant Stock — it will NEVER select a Source outside that Stock, regardless of that Source's own priority or available quantity

**Reservations**
- MSI writes reservation records (signed deltas) instead of directly decrementing quantity — avoids DB row-locking under high concurrency, keeping checkout performant
- **Salable Quantity = Source Quantity − Sum of Reservations**
- A product can show 0 Salable Quantity even if raw Source Quantity appears non-zero, because reservations have consumed it (reservations aren't always reflected as a physical deduction to the source record itself)

**Multi-Source Scenario Patterns**

| Scenario | Configuration |
|---|---|
| Nearest-warehouse fulfillment | Both Sources in one Stock; consider Distance-based SSA |
| Fully separate regional inventory pools (zero overlap) | Two separate Stocks, each with its own Source(s), each assigned to its own website |
| Dropship as fallback only | Lower-priority Source in the same Stock as main warehouse |
| Shared inventory across brands/websites | One Source assigned to multiple Stocks (supported) |

**Fundamental limitation of Single Source mode:** no concept of separate physical locations at all — moving to per-warehouse tracking requires MSI.

---

## PART B: PRACTICE MCQs

### Set 1 — Mixed Foundations (Product Types, Storefront, Pricing, Attributes, Inventory)

**Q1.** A camera can be purchased with an optional lens, optional case, and optional extended warranty, where the customer chooses which items to include and the total price changes based on selections. Which product type?
- A) Configurable Product
- B) Grouped Product
- C) Bundle Product
- D) Simple Product with Custom Options
**Answer: C — Bundle Product.** Bundle products let customers select optional/required components with a dynamically calculated total.

**Q2.** A dining set (table + 4 chairs) where each piece is also sold individually at its own price, and the customer chooses quantities of each. Which product type?
- A) Bundle Product
- B) Grouped Product
- C) Configurable Product
- D) Virtual Product
**Answer: B — Grouped Product.** Independently priced, independently purchasable items displayed together.

**Q3.** A configurable product has a child variant (Small) out of stock while other sizes remain in stock, with "Display Out of Stock Products" disabled. Storefront effect?
- A) Entire configurable hidden
- B) Small option not selectable/shown
- C) Small shows with a warning
- D) Price auto-increases
**Answer: B.** Only the specific out-of-stock variant is excluded; the parent remains visible if other variants are in stock.

**Q4.** A merchant wants 15% off for the "Wholesale" group on all "Outdoor Gear" category products, no coupon, reflected directly in displayed price. What's configured?
- A) Cart Price Rule with coupon
- B) Catalog Price Rule scoped to group + category
- C) Special Price per product
- D) Tier Pricing per product
**Answer: B — Catalog Price Rule.** Applies automatically, no code, modifies displayed price.

**Q5.** Customers buying 10+ units should automatically get a lower per-unit price, visible on the PDP before adding to cart. Which feature?
- A) Cart Price Rule
- B) Catalog Price Rule
- C) Tier Pricing
- D) Special Price
**Answer: C — Tier Pricing.** Quantity-based price breaks configured directly on the product.

**Q6.** For a dropdown attribute to appear as a layered nav filter, which must ALL be true?
- A) Global scope + required at creation
- B) "Use in Layered Navigation" enabled + category set as Anchor
- C) Used in Product Listing + Comparable
- D) Assigned to default attribute set only
**Answer: B.** Attribute-level filterable setting AND category Is Anchor = Yes must both be true.

**Q7.** Warehouses in NY and LA, both in the same Stock assigned to the US website. Order comes from California. With default SSA, how is the fulfilling source determined?
- A) Geographic proximity
- B) Priority ranking of sources within the Stock
- C) Random selection
- D) Always split evenly
**Answer: B.** Default SSA is Priority-based, not distance-based.

**Q8.** A product shows quantity 50 but the merchant wants it to appear unavailable immediately, without changing quantity. What to configure?
- A) Set Stock Status to Out of Stock
- B) Set quantity to 0
- C) Disable the product
- D) Set Backorders to No
**Answer: A.** Stock Status is independent from the quantity field.

**Q9.** SKU must stay identical across US and Canadian (French) store views, but Description should differ. Correct scope pairing?
- A) SKU: Store View, Description: Global
- B) SKU: Global, Description: Store View
- C) Both: Website
- D) Both: Global
**Answer: B.** SKU is always Global; Description can be Store View scope for localization.

**Q10.** A bundle is configured with Fixed pricing. What does this mean for the storefront price?
- A) Base price plus/minus selected option prices
- B) Price stays the same regardless of selections
- C) Price = highest-priced option only
- D) Fixed pricing disables option selection
**Answer: B.** Fixed pricing = one set price no matter what's selected.

---

### Set 2 — Bundle vs. Grouped Focused Drill

**Q1.** A gift basket lets customers pick any 4 of 10 possible snacks; total price changes depending on selection. Product type?
- A) Grouped Product
- B) Bundle Product (Dynamic)
- C) Bundle Product (Fixed)
- D) Configurable Product
**Answer: B.** Picking a subset FROM options with variable pricing = Bundle, Dynamic.

**Q2.** A cleanser, toner, and moisturizer — each already individually listed with own prices — shown together on one page, each keeping its own price and becoming separate cart line items.
- A) Bundle (Fixed)
- B) Grouped Product
- C) Bundle (Dynamic)
- D) Simple with Custom Options
**Answer: B — Grouped Product.** Existing standalone products displayed together; separate cart lines.

**Q3.** The merchant wants the assembled product to appear as ONE cart line item regardless of how many components were selected. Which type must this be?
- A) Grouped Product
- B) Bundle Product
- C) Either, depending on settings
- D) Configurable Product only
**Answer: B.** Bundle always collapses into a single cart line; Grouped always creates separate lines.

**Q4.** A bundle with Fixed pricing at $99. Customer selects a premium option that would normally add $20 in a dynamic bundle. Final price at checkout?
- A) $99, unaffected
- B) $119, Fixed still adds surcharges
- C) $79
- D) Cannot be determined
**Answer: A — $99.** Fixed pricing means total doesn't change with selections.

**Q5.** Which statement correctly distinguishes the KEY structural difference between Bundle and Grouped?
- A) Bundle only virtual, Grouped only physical
- B) Grouped links existing independent products for display; Bundle defines selectable options/components combining into one purchasable unit
- C) Bundle never has a price
- D) Grouped always requires a coupon
**Answer: B.**

---

### Set 3 — Bundle & Grouped × MSI Inventory (Cross-Topic)

**Q1.** A bundle set to "Ship Bundle Items Separately" contains a laptop (Component A) and bag (Component B), mapped to different sources under MSI. What happens to inventory when ordered?
- A) All deducted from a single source tied to bundle SKU
- B) Each component's stock deducted independently, potentially from different sources
- C) Only the first component's stock is deducted
- D) Stock deduction disabled entirely
**Answer: B.**

**Q2.** How does MSI inventory tracking work for a Grouped product vs. its children?
- A) Grouped parent has combined stock pool shared by children
- B) Ship Bundle Items setting determines Grouped deduction
- C) Each child has fully independent inventory; the Grouped parent holds no stock itself
- D) Grouped requires a single shared source across children
**Answer: C.**

**Q3.** A bundle set to "Ship Bundle Items Together," but the two components are only stocked at two different, non-overlapping warehouses. What issue does this create?
- A) "Together" requires more config steps
- B) "Together" assumes fulfillment from a single source, which breaks down if components only exist at different locations
- C) "Together" only works with Fixed pricing
- D) "Together" auto-enables backorders
**Answer: B.**

**Q4.** Within a Grouped product, one of three linked children has 0 saleable quantity (Out of Stock) while the other two remain In Stock. Storefront effect?
- A) Entire Grouped page unavailable
- B) Out-of-stock child's price hidden but purchasable
- C) Out-of-stock child shown as unavailable/hidden; others remain purchasable
- D) All children auto-backordered
**Answer: C.**

**Q5.** Considering MSI reservations: how does the reservation record differ between a Bundle (Ship Together) and a Grouped product when ordered?
- A) Identical reservation records for both
- B) Bundle (Together) creates one coordinated reservation tied to a single cart line; Grouped creates separate, independent reservations per child line item
- C) Neither uses MSI reservations
- D) Only Grouped triggers reservations
**Answer: B.**

---

### Set 4 — Product Attributes & Catalog Navigation

**Q1.** Difference between "Filterable (with results)" and "Filterable (no results)"?
- A) They behave identically
- B) "With results" hides options that would return 0 products; "no results" still shows them
- C) "No results" disables the filter entirely
- D) "With results" only works on Price attributes
**Answer: B.**

**Q2.** "Use in Layered Navigation" is set to "Filterable (with results)" but the attribute's Input Type is "Text Field," and it still won't filter. What's the issue?
- A) Dropdown
- B) Multiple Select
- C) Text Field
- D) Price
**Answer: C — Text Field.** Layered nav filtering requires Dropdown, Multiple Select, or Price input types.

**Q3.** Attribute is correctly Dropdown, indexed, and Filterable — but subcategory products/filters aren't appearing on the parent category page. Likely misconfiguration?
- A) Attribute scope must be Global
- B) Parent category's "Is Anchor" must be Yes
- C) Attribute needs a new Attribute Set
- D) Reindexing has no effect
**Answer: B.**

**Q4.** URL Key needs to differ between English (/blue-shirt) and French (/chemise-bleue) store views. Required scope?
- A) Global
- B) Website
- C) Store View
- D) No configurable scope
**Answer: C.**

**Q5.** Functional difference between Attribute Set and Attribute Group?
- A) Set determines available attributes; Group is a cosmetic admin-UI subdivision with no storefront effect
- B) Group determines available attributes; Set is cosmetic
- C) Both directly affect layered nav
- D) Same concept, two names
**Answer: A.**

**Q6.** Configurable Product uses "Color" as the defining attribute for child variations. Required scope?
- A) Global — needed for consistent parent-child relationships across the catalog
- B) Store View — so each store defines variations independently
- C) Website — to align with per-website pricing
- D) Scope doesn't matter
**Answer: A.**

---

### Set 5 — Attribute Set vs. Attribute Group (Isolated Drill)

**Q1.** A merchant wants to add a brand-new attribute "Fabric Weight" that doesn't currently exist on T-Shirt products. What must they do?
- A) Create a new Attribute Group and add it there
- B) Add "Fabric Weight" to the product's Attribute Set
- C) Groups automatically inherit new attributes from other sets
- D) Just reindex the catalog
**Answer: B.** New fields require the Attribute Set, not a Group.

**Q2.** A merchant reorders/renames Attribute Groups purely for Admin UI convenience. Effect on storefront?
- A) Layered nav filter order changes
- B) Product URL structure affected
- C) Nothing changes on storefront — only the Admin edit screen layout is affected
- D) Attribute scope resets
**Answer: C.**

**Q3.** Containment relationship between Attribute Sets and Attribute Groups?
- A) One Set can contain multiple Groups; a Group exists only within its parent Set
- B) One Group can contain multiple Sets
- C) No containment relationship
- D) Each Set can only have one Group
**Answer: A.**

---

### Set 6 — Storefront Behavior & Pricing (Deep Practice)

**Q1.** A configurable product has three color variants priced $40, $45, $50. Before selection, what price displays on the PDP by default?
- A) Average price
- B) Price of the first-created child
- C) Lowest price among available/in-stock variants
- D) Highest price
**Answer: C.**

**Q2.** A Catalog Price Rule for 20% off "Sale" category is saved, but the storefront discount isn't showing yet. Most likely explanation?
- A) Coupon code not shared yet
- B) Catalog Rule Price indexer hasn't run since the rule was saved
- C) Catalog rules never affect displayed price
- D) Rule needs "Discard subsequent rules" to activate
**Answer: B.**

**Q3.** A Cart Price Rule offers 10% off orders over $100, no coupon required. Effect on the PDP/category listing price?
- A) Shows strikethrough discounted price on PDP
- B) Catalog/PDP price remains unchanged; discount only applies once in the cart
- C) Category listing auto-updates to reflect cart rule
- D) Requires the Catalog Price Rule indexer
**Answer: B.**

**Q4.** A merchant wants to send 5,000 customers a unique, single-use discount code each via email. Which Cart Price Rule coupon setting?
- A) No Coupon
- B) Specific Coupon
- C) Auto-generated (Coupon Qty)
- D) Cannot generate multiple codes
**Answer: C.**

**Q5.** MAP policy: price hidden on PDP, but customer can click "See price" to reveal it in a popup without adding to cart. Which MAP Display Actual Price setting?
- A) In Cart
- B) On Gesture
- C) Before Order Confirmation
- D) Use config
**Answer: B.**

**Q6.** Two websites (USD and EUR) share one catalog but need entirely independent prices per website for the same SKUs. What must be configured?
- A) Catalog Price Scope (Catalog > General > Price) = Website
- B) Separate Attribute Set per website
- C) Tier Pricing scoped per group per website
- D) Price is always Global and cannot vary
**Answer: A.**

**Q7.** A bundle has one Required option and one Optional option. Effect on Add to Cart button?
- A) Both must be selected before enabling
- B) Neither affects the button
- C) Required must be selected before Add to Cart is enabled; Optional can be skipped
- D) Optional must be selected first
**Answer: C.**

---

### Set 7 — Product Inventory Management (Single & Multi-Location)

**Q1.** Correct relationship between Sources, Stocks, and Sales Channels in MSI?
- A) Stock assigned to Source, Source assigned to Sales Channel
- B) Sales Channel assigned to multiple Stocks, each Stock has one Source
- C) One or more Sources assigned to a Stock; each Sales Channel (Website) assigned to one Stock
- D) All independent, no assignment relationship
**Answer: C.**

**Q2.** Why does MSI use reservations instead of directly decrementing quantity at order time?
- A) Allows unlimited overselling
- B) Avoids DB row-locking on the quantity column during high-concurrency checkouts
- C) Only used for virtual products
- D) Replaces the need for a Source entirely
**Answer: B.**

**Q3.** A product shows Source Quantity of 20 at a warehouse, but Salable Quantity displays as 0 on the storefront. Explanation?
- A) System error
- B) Reservations against the product have consumed the available quantity
- C) Stock Status was manually set Out of Stock
- D) Only occurs when Manage Stock is disabled
**Answer: B.**

**Q4.** East Coast website must draw ONLY from the East Coast warehouse, West Coast website ONLY from the West Coast warehouse, zero overlap. Configuration?
- A) One Stock with both Sources, assigned to both websites
- B) One Source per website but shared Stock
- C) Two separate Stocks, each with its respective Source, each assigned to its corresponding website
- D) Not supported
**Answer: C.**

**Q5.** Two brands share one warehouse and want both websites to draw from the SAME physical inventory pool. Is assigning one Source to two different Stocks supported?
- A) Not supported, one Stock per Source only
- B) Fully supported — a Source can be assigned to multiple Stocks simultaneously
- C) Requires duplicating the Source
- D) Requires reverting to Single Source mode
**Answer: B.**

**Q6.** A high-priority Source is NOT assigned to the Stock used by the order's website. Will SSA ever select it?
- A) Never — SSA only considers Sources assigned to the relevant Stock
- B) Always selected first regardless of Stock assignment
- C) Only if all Sources in the assigned Stock are out of stock
- D) SSA auto-reassigns the Source
**Answer: A.**

**Q7.** A merchant on legacy Single Source mode wants to track stock separately across two warehouses. Fundamental limitation to address?
- A) Cannot track backorders at all
- B) Single Source has one combined quantity/status with no concept of separate physical locations; MSI is needed
- C) Only available for virtual products
- D) Requires a separate installation
**Answer: B.**
