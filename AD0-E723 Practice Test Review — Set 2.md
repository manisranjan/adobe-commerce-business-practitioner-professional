# AD0-E723 Practice Test Review — Set 2
### Adobe Commerce Business Practitioner Professional — Questions, Explanations & Exam Traps

---

### Q1. A merchant sells software licenses that require no shipping and have no weight, but must still be added to the cart and paid for online. Which product type should be used?
- Simple product with "Ship Separately" enabled
- Virtual product
- Downloadable product

<details><summary>Answer & Explanation</summary>

**Answer:** Virtual product

**Explanation:** A Virtual product represents a non-tangible item (service, membership, license) that has no weight and requires no shipping, yet is still purchasable. Downloadable is specifically for items the customer downloads (files); if there's no actual file delivered through Commerce, Virtual is the correct fit.

**Exam Trap:** Virtual vs. Downloadable — both are intangible and skip shipping, but Downloadable attaches actual downloadable files with link/sample management. Pure "service/license, no file delivered" = Virtual.

</details>

---

### Q2. A store admin needs product prices to differ between the US website and the EU website, while keeping the same product catalog. Which configuration enables per-website pricing?
- Set Catalog Price Scope to Website
- Set Catalog Price Scope to Global
- Create separate root categories per website

<details><summary>Answer & Explanation</summary>

**Answer:** Set Catalog Price Scope to Website

**Explanation:** Catalog Price Scope (Stores > Configuration > Catalog > Catalog > Price) controls whether prices are shared globally or can be set per website. Setting it to "Website" unlocks a price field per website, allowing the US and EU sites to carry different prices for the same products.

**Exam Trap:** Global scope forces one shared price across all websites. Root categories affect catalog structure, not pricing — they're unrelated to price scope.

</details>

---

### Q3. A merchandiser wants products from subcategories to appear on a parent category's listing page and enable layered navigation filters. Which category setting accomplishes this?
- Set "Include in Menu" to Yes
- Set "Is Anchor" to Yes
- Set "Display Mode" to Static Block Only

<details><summary>Answer & Explanation</summary>

**Answer:** Set "Is Anchor" to Yes

**Explanation:** The "Is Anchor" setting makes a category aggregate products from its subcategories and activates layered navigation on that category page. "Include in Menu" only controls navigation menu visibility, and "Static Block Only" display mode would hide the product listing entirely.

**Exam Trap:** "Include in Menu" and "Is Anchor" are commonly confused. Menu visibility ≠ product aggregation/filtering. Anchor is the switch for subcategory product inclusion + layered nav.

</details>

---

### Q4. A catalog manager wants to schedule a temporary price reduction on a product from Nov 24 to Nov 27, after which the price automatically reverts. Which pricing field should be used?
- Tier price
- Special price with From/To dates
- Group price

<details><summary>Answer & Explanation</summary>

**Answer:** Special price with From/To dates

**Explanation:** Special Price supports a From/To date range, automatically applying the discounted price during the window and reverting afterward — ideal for time-bound sales. Tier price is quantity-based; Group price is customer-group-based, neither with a native date window.

**Exam Trap:** Special Price is the only per-product native price field with built-in date scheduling. Don't reach for a catalog price rule when a single-product, time-bound discount is described (though a rule also works, Special Price is the direct product-level answer).

</details>

---

### Q5. In MSI, a merchant operates two warehouses and one retail store, all fulfilling a single website. How should these be modeled?
- Three Stocks, one Source, mapped to the website
- Three Sources aggregated into one Stock, mapped to the website's sales channel
- One Source and three Stocks mapped to store views

<details><summary>Answer & Explanation</summary>

**Answer:** Three Sources aggregated into one Stock, mapped to the website's sales channel

**Explanation:** Each physical location (2 warehouses + 1 store) is a Source holding real quantity. A single Stock aggregates all three Sources and is assigned to the website (sales channel), giving one combined salable quantity to shoppers.

**Exam Trap:** Sources = physical locations (many). Stock = virtual aggregation (usually one per sales channel). Reversing them — "one Source, many Stocks" — is the classic MSI modeling error.

</details>

---

### Q6. A shopper adds the last unit of a product to their cart but does not check out. Another shopper cannot add that unit. What MSI mechanism causes this?
- The Source quantity was decremented immediately
- A reservation reduced the salable quantity
- The product was set to Out of Stock

<details><summary>Answer & Explanation</summary>

**Answer:** A reservation reduced the salable quantity

**Explanation:** Placing an order (not merely adding to cart) creates a reservation; however, salable quantity reflects reservations against confirmed orders. In MSI, salable quantity = source quantity − reservations, so committed units aren't available to others while held.

**Exam Trap:** MSI never decrements the physical Source quantity at order placement — it uses reservations. Source quantity only changes at shipment.

</details>

---

### Q7. A B2B buyer needs to place large recurring orders quickly by uploading a list of SKUs and quantities. Which Adobe Commerce B2B feature supports this?
- Requisition Lists
- Quick Order (Order by SKU)
- Shared Catalogs

<details><summary>Answer & Explanation</summary>

**Answer:** Quick Order (Order by SKU)

**Explanation:** Quick Order lets buyers add products directly by entering or uploading SKUs and quantities, bypassing catalog browsing — ideal for fast, bulk, repeat ordering. Requisition Lists save reusable lists; Shared Catalogs control pricing/visibility per company.

**Exam Trap:** Quick Order (fast SKU entry/upload for immediate add-to-cart) vs. Requisition List (saved reusable list for repeat ordering). "Upload a list to order now" = Quick Order.

</details>

---

### Q8. A company account admin in B2B wants to control which sub-users can see prices and place orders. Which capability handles this?
- Company user roles and permissions
- Customer groups
- Customer segments

<details><summary>Answer & Explanation</summary>

**Answer:** Company user roles and permissions

**Explanation:** B2B Company accounts include role-based permissions that let a company administrator define what each sub-user can do (view prices, place orders, manage the company profile, etc.). Customer groups/segments don't provide intra-company user permissioning.

**Exam Trap:** Company roles/permissions are a B2B-specific, per-company access control — distinct from storewide customer groups (pricing/tax) and dynamic segments (marketing).

</details>

---

### Q9. A promotions manager wants "Buy 2, Get 1 Free" on a specific product. Which cart price rule action supports this?
- Percent of product price discount
- Buy X get Y free (Buy X get Y)
- Fixed amount discount for whole cart

<details><summary>Answer & Explanation</summary>

**Answer:** Buy X get Y free (Buy X get Y)

**Explanation:** The cart price rule action "Buy X get Y free" is purpose-built for BOGO-style promotions, letting you specify the quantity to buy (X) and the quantity discounted free (Y).

**Exam Trap:** Don't force a percentage or fixed discount for BOGO — Commerce has a dedicated "Buy X get Y free" action for exactly this scenario.

</details>

---

### Q10. A cart price rule and a catalog price rule both apply to the same product. In what order are they calculated?
- Cart price rule first, then catalog price rule
- Catalog price rule first (adjusts base price), then cart price rule on the subtotal
- They apply simultaneously to the original price

<details><summary>Answer & Explanation</summary>

**Answer:** Catalog price rule first (adjusts base price), then cart price rule on the subtotal

**Explanation:** Catalog price rules modify the displayed/base product price before the item reaches the cart. Cart price rules then act on the resulting subtotal at checkout. The sequence is always catalog → cart.

**Exam Trap:** A common miscalculation applies both discounts to the original price independently. They stack sequentially: catalog first, cart second.

</details>

---

### Q11. A merchant wants a coupon that can be generated as many unique codes for a single campaign, so each customer gets their own code. Which coupon setting is required?
- Specific Coupon → No Coupon
- Specific Coupon → Specific Coupon with "Use Auto Generation"
- No Coupon required

<details><summary>Answer & Explanation</summary>

**Answer:** Specific Coupon → Specific Coupon with "Use Auto Generation"

**Explanation:** Enabling "Use Auto Generation" on a Specific Coupon cart price rule lets you generate many unique coupon codes from the Manage Coupon Codes section — one per customer for tracking and single-use control.

**Exam Trap:** "Specific Coupon" alone gives one shared code. To issue many unique codes, you must enable "Use Auto Generation" and generate the batch.

</details>

---

### Q12. A store owner reports that a catalog price rule was created and saved but discounted prices are not showing on the storefront. What is the most likely missing step?
- The rule's coupon code was not entered
- The catalog price rules were not applied/the price index was not updated
- The cart price rule priority is too low

<details><summary>Answer & Explanation</summary>

**Answer:** The catalog price rules were not applied/the price index was not updated

**Explanation:** Catalog price rules require applying the rule (and reindexing prices) to take effect on the storefront. Until the rule is applied and the price index refreshes, discounted prices won't appear.

**Exam Trap:** Catalog price rules never use coupon codes — that distractor is invalid. The real gotcha is that catalog rules must be applied/indexed, unlike cart rules which apply live.

</details>

---

### Q13. A regular price is $200. A catalog price rule applies "to fixed amount $150." A cart price rule then applies 10% off. What is the final price?
- $135
- $150
- $180

<details><summary>Answer & Explanation</summary>

**Answer:** $135

**Explanation:** The catalog price rule sets the price to a fixed $150 first. The cart price rule then applies 10% off: $150 × 0.90 = $135.

**Exam Trap:** "To fixed amount" replaces the price entirely (not a percentage off the original). Then the cart rule discounts the new $150 base, not the original $200.

</details>

---

### Q14. A marketer wants free shipping automatically when the cart subtotal exceeds $75, without a coupon. Which approach is correct?
- Enable the Free Shipping carrier with a Minimum Order Amount of $75
- Create a catalog price rule with a free shipping action
- Add a coupon-only cart price rule

<details><summary>Answer & Explanation</summary>

**Answer:** Enable the Free Shipping carrier with a Minimum Order Amount of $75

**Explanation:** The native Free Shipping shipping method has a "Minimum Order Amount" setting that grants free shipping automatically once the subtotal threshold is met — no coupon needed.

**Exam Trap:** Catalog price rules can't grant free shipping (they affect product price only). For a no-coupon subtotal threshold, the Free Shipping carrier's Minimum Order Amount is the simplest native answer.

</details>

---

### Q15. An order was placed and invoiced but not yet shipped. The customer wants to cancel. What is the correct action?
- Cancel the order directly
- Create a credit memo (refund) since it's already invoiced
- Delete the order record

<details><summary>Answer & Explanation</summary>

**Answer:** Create a credit memo (refund) since it's already invoiced

**Explanation:** Once an invoice exists, the order can no longer be simply "canceled" — payment has been captured/recorded. The proper path to reverse it is a credit memo (refund), which also restocks inventory if configured.

**Exam Trap:** Cancel is only available before invoicing. After an invoice, you must use a credit memo. Orders are never "deleted" as a business action.

</details>

---

### Q16. A payment method authorizes funds at order placement but only captures when the merchant creates an invoice. Which payment action is configured?
- Authorize and Capture
- Authorize Only
- Offline

<details><summary>Answer & Explanation</summary>

**Answer:** Authorize Only

**Explanation:** "Authorize Only" reserves (authorizes) the funds at checkout but defers the actual capture until the merchant generates an invoice. "Authorize and Capture" takes funds immediately at order placement.

**Exam Trap:** Authorize Only = hold now, capture at invoice. Authorize and Capture = charge immediately. The invoice step is where capture happens for Authorize Only.

</details>

---

### Q17. A merchant needs shipping rates that vary by destination and order weight, imported from a spreadsheet. Which shipping method fits?
- Flat Rate
- Table Rates
- Free Shipping

<details><summary>Answer & Explanation</summary>

**Answer:** Table Rates

**Explanation:** Table Rates lets you import a CSV defining rates by conditions such as weight vs. destination, price vs. destination, or number of items vs. destination — perfect for spreadsheet-driven, variable rates.

**Exam Trap:** Flat Rate is a single fixed charge; only Table Rates supports condition-based, importable rate tables.

</details>

---

### Q18. A tax rule must apply a reduced rate to food products but standard rate to electronics. Which tax entity differentiates this?
- Customer Tax Class
- Product Tax Class
- Tax Zone only

<details><summary>Answer & Explanation</summary>

**Answer:** Product Tax Class

**Explanation:** When tax treatment depends on the type of product (food vs. electronics), you assign different Product Tax Classes and scope the tax rules accordingly. Customer Tax Class differentiates by who is buying, not what is sold.

**Exam Trap:** Product type → Product Tax Class. Customer type (retail/wholesale/B2B) → Customer Tax Class. Match the differentiator in the scenario to the correct class.

</details>

---

### Q19. A merchant wants the storefront to show prices including tax to EU shoppers but excluding tax to US shoppers. Which setting controls this display?
- Catalog Prices (Excluding/Including Tax) in tax configuration
- Customer Tax Class
- Currency setup

<details><summary>Answer & Explanation</summary>

**Answer:** Catalog Prices (Excluding/Including Tax) in tax configuration

**Explanation:** The "Catalog Prices" display setting (Stores > Configuration > Sales > Tax > Calculation Settings / Price Display) governs whether catalog prices render including or excluding tax, and this can be scoped per website/store view for regional differences.

**Exam Trap:** Tax display settings are separate from tax calculation. The include/exclude display is a presentation setting configurable per scope, not a function of currency or customer class.

</details>

---

### Q20. A merchant wants to run two brands with completely separate catalogs, customer bases, and currencies in one Commerce installation. What structure is required?
- Two store views under one store
- Two stores under one website
- Two websites

<details><summary>Answer & Explanation</summary>

**Answer:** Two websites

**Explanation:** Separate customer bases and separate base currencies both require the Website level. Two websites give each brand its own catalog assortment, customer accounts (by default), payment/shipping, and base currency.

**Exam Trap:** Store View = language/presentation; Store = root catalog; Website = customers, currency, payments. "Separate customers + currency" always means Website.

</details>

---

### Q21. Which admin scope level controls the base currency for pricing?
- Store View
- Website
- Global default only

<details><summary>Answer & Explanation</summary>

**Answer:** Website

**Explanation:** Base currency is configured at the Website scope, which is why different-currency regions require separate websites. Display currencies can be added per store view, but the base currency is website-level.

**Exam Trap:** Don't confuse base currency (Website scope) with allowed/display currencies (can be set per store view). The base is always website-scoped.

</details>

---

### Q22. A retailer wants shoppers on all its brand sites to log in with a single account. Which setting enables this?
- Share Customer Accounts = Per Website
- Share Customer Accounts = Global
- Customer Group = Shared

<details><summary>Answer & Explanation</summary>

**Answer:** Share Customer Accounts = Global

**Explanation:** "Share Customer Accounts = Global" makes one customer record set span all websites in the installation, so a shopper logs in with the same credentials everywhere. Per Website isolates accounts to each website.

**Exam Trap:** The default is Per Website (isolated). You must explicitly set Global for cross-site single login.

</details>

---

### Q23. A back-office user should manage only products and categories, with no access to sales or configuration. How is this achieved?
- Assign the Administrator role
- Create a User Role with Custom Resource Access limited to Catalog resources
- Restrict via Customer Group

<details><summary>Answer & Explanation</summary>

**Answer:** Create a User Role with Custom Resource Access limited to Catalog resources

**Explanation:** Admin least-privilege is enforced through User Roles: set Resource Access to Custom and enable only the Catalog-related resources, then assign the user to that role.

**Exam Trap:** Customer Groups are shopper-side, unrelated to admin permissions. Admin access is always defined via User Roles → Role Resources.

</details>

---

### Q24. A marketing team should manage promotions on only the "Europe" website in the admin. Which capability enforces this website restriction?
- Custom Resource Access alone
- Role Scopes limiting the role to the Europe website
- Two-Factor Authentication

<details><summary>Answer & Explanation</summary>

**Answer:** Role Scopes limiting the role to the Europe website

**Explanation:** Role Scopes (an Adobe Commerce feature) restrict a role's effective reach to specific websites/store views, so the team only manages Europe. Resource Access limits which features, while Scopes limit which websites.

**Exam Trap:** Resource Access = which features. Role Scopes = which websites/store views. The scenario about a specific website needs Scopes, not just Resource Access.

</details>

---

### Q25. Which edition provides Content Staging, Customer Segments, and B2B out of the box?
- Magento Open Source
- Adobe Commerce
- Both editions equally

<details><summary>Answer & Explanation</summary>

**Answer:** Adobe Commerce

**Explanation:** Content Staging & Preview, Customer Segments, Dynamic Blocks, and B2B (Company accounts, Shared Catalogs, Quotes, Requisition Lists) are Adobe Commerce features, not present in Magento Open Source.

**Exam Trap:** Page Builder is now in both editions, but Staging, Segments, Dynamic Blocks, RMA, Gift Cards, Reward Points, and B2B remain Adobe Commerce-only.

</details>

---

### Q26. A merchant wants to schedule a homepage content change to go live at midnight and revert automatically after a promotion. Which feature is designed for this?
- Content Staging (scheduled update)
- CMS block versioning
- Cron-based script

<details><summary>Answer & Explanation</summary>

**Answer:** Content Staging (scheduled update)

**Explanation:** Content Staging lets you create a scheduled update with start/end dates and preview the future state, automatically applying and reverting content changes — exactly this campaign scenario.

**Exam Trap:** Content Staging is Adobe Commerce-only. It applies to CMS pages, blocks, categories, products, and price rules with a timeline and preview.

</details>

---

### Q27. Which feature shows different banner content to shoppers based on customer segment membership?
- CMS Block
- Dynamic Block
- Widget

<details><summary>Answer & Explanation</summary>

**Answer:** Dynamic Block

**Explanation:** Dynamic Blocks (formerly Banners) render segment-targeted content, showing different banners to different customer segments or conditions. CMS Blocks are static and shown identically to everyone.

**Exam Trap:** CMS Block = static, same for all. Dynamic Block = segment-personalized. Widgets place content but don't natively target by segment.

</details>

---

### Q28. A non-technical marketer must build a rich landing page with hero images and columns without writing code. Which tool is used?
- Page Builder
- Layout XML
- Custom PHTML template

<details><summary>Answer & Explanation</summary>

**Answer:** Page Builder

**Explanation:** Page Builder is the drag-and-drop visual content editor (rows, columns, banners, sliders, buttons, HTML) enabling non-technical users to build rich layouts on CMS pages, blocks, and product descriptions.

**Exam Trap:** Layout XML and PHTML are developer methods. Page Builder is the no-code authoring answer.

</details>

---

### Q29. Which report requires refreshing statistics before it shows current totals?
- Products Ordered / Sales reports
- URL Rewrites list
- Customer Segments list

<details><summary>Answer & Explanation</summary>

**Answer:** Products Ordered / Sales reports

**Explanation:** Sales-related reports rely on aggregated statistics. If figures look stale, run Reports > Statistics > Refresh Statistics to recalculate. Configuration lists like URL rewrites aren't statistics-based.

**Exam Trap:** Stale sales report totals → Refresh Statistics, not reindex or cache flush.

</details>

---

### Q30. A merchant needs to bulk-update 5,000 product prices from a spreadsheet. Which capability should be used?
- System > Import with the Products entity (CSV)
- Manual editing in the product grid
- GraphQL mutation

<details><summary>Answer & Explanation</summary>

**Answer:** System > Import with the Products entity (CSV)

**Explanation:** The Import tool (System > Data Transfer > Import) handles bulk product data via CSV, using Add/Update behavior to modify thousands of records at once.

**Exam Trap:** Import files are always CSV. Add/Update merges changes; Replace wipes and reloads; Delete removes. For price updates, use Add/Update.

</details>

---

### Q31. During product import, which behavior updates existing products and adds new ones without deleting anything?
- Replace
- Add/Update
- Delete

<details><summary>Answer & Explanation</summary>

**Answer:** Add/Update

**Explanation:** Add/Update imports new records and updates matching existing ones by SKU, leaving unmatched existing products untouched — the safest bulk-edit behavior.

**Exam Trap:** Replace removes existing entities and re-imports (data loss risk); Delete removes matched rows. Add/Update is the non-destructive merge.

</details>

---

### Q32. Which Adobe Sensei-powered SaaS service provides AI-driven search with facets and merchandising rules, replacing default catalog search?
- Live Search
- Catalog Service
- Product Recommendations

<details><summary>Answer & Explanation</summary>

**Answer:** Live Search

**Explanation:** Live Search is the Sensei-powered SaaS search service delivering intelligent search, autocomplete, facets, and merchandising — connected via Commerce Services and replacing the default on-prem search experience.

**Exam Trap:** Live Search = search/merchandising. Catalog Service = product data API. Product Recommendations = recs. All are SaaS Commerce Services but serve different purposes.

</details>

---

### Q33. Which SaaS service surfaces "Customers who viewed this also viewed" style suggestions powered by Adobe Sensei?
- Live Search
- Product Recommendations
- Payment Services

<details><summary>Answer & Explanation</summary>

**Answer:** Product Recommendations

**Explanation:** Product Recommendations uses Adobe Sensei to generate recommendation units (viewed-this/viewed-that, bought-together, trending, etc.) rendered on storefront pages, connected via Commerce Services.

**Exam Trap:** Recommendations ≠ Live Search. Recommendations suggests related products; Live Search powers the search box and facets.

</details>

---

### Q34. A shopper exercises their GDPR "right to be forgotten." Which action best reflects Commerce's role?
- Delete/anonymize the customer's personal data in Commerce and coordinate with compliance
- Ignore the request since Commerce can't delete data
- Only disable the account login

<details><summary>Answer & Explanation</summary>

**Answer:** Delete/anonymize the customer's personal data in Commerce and coordinate with compliance

**Explanation:** The right to erasure requires removing or anonymizing the customer's stored personal data. Commerce provides tooling to delete/anonymize the account and related data, typically coordinated with the privacy/compliance team for cross-system completeness.

**Exam Trap:** Right to erasure = delete/anonymize, not just disable login. Commerce plays an active role in removing the data it holds.

</details>

---

### Q35. Which regulation is primarily concerned with California consumers' right to opt out of the sale of their personal data?
- GDPR
- CCPA/CPRA
- PIPEDA

<details><summary>Answer & Explanation</summary>

**Answer:** CCPA/CPRA

**Explanation:** CCPA/CPRA governs California residents, centering on rights to know, delete, and opt out of the sale/sharing of personal data, plus non-discrimination for exercising those rights.

**Exam Trap:** GDPR = EU, opt-in consent, right to erasure. CCPA/CPRA = California, opt-out of sale. Don't swap jurisdictions or consent models.

</details>

---

### Q36. A checkout redirects shoppers to the gateway's hosted page for card entry, returning only a token. How does this affect PCI scope?
- Increases scope because Commerce can't validate entry
- Reduces PCI scope because card data never touches the Commerce server
- No effect on PCI scope

<details><summary>Answer & Explanation</summary>

**Answer:** Reduces PCI scope because card data never touches the Commerce server

**Explanation:** Hosted/redirect flows keep cardholder data entirely on the provider's environment, so Commerce never stores, processes, or transmits raw card data — significantly reducing PCI scope (often SAQ A).

**Exam Trap:** The golden rule: the less card data touches your server, the lower your PCI scope. Redirect and hosted-fields/tokenized flows are lowest scope.

</details>

---

### Q37. A merchant stores full card numbers (even encrypted) in the Commerce database for manual processing. What is the PCI implication?
- No impact if encrypted
- Full PCI scope (SAQ D); generally not permitted without full controls
- Reduced scope due to tokenization

<details><summary>Answer & Explanation</summary>

**Answer:** Full PCI scope (SAQ D); generally not permitted without full controls

**Explanation:** Storing cardholder data on the merchant's own server — encrypted or not — places the environment in full PCI DSS scope with extensive controls required. This pattern is strongly discouraged.

**Exam Trap:** "Encrypted" does not exempt you from PCI scope. Storing the PAN yourself = highest burden. Never store CVV at all.

</details>

---

### Q38. Which SEO feature designates the master version of a page to prevent duplicate-content issues when a product appears in multiple categories?
- XML Sitemap
- Canonical tags
- robots.txt

<details><summary>Answer & Explanation</summary>

**Answer:** Canonical tags

**Explanation:** Canonical meta tags tell search engines the authoritative URL for content reachable via multiple paths (e.g., a product in several categories), consolidating ranking signals and preventing duplicate-content penalties.

**Exam Trap:** Canonical fixes duplicate content (same content, multiple URLs). Sitemap aids discovery; robots.txt controls crawl access. Match the tool to the goal.

</details>

---

### Q39. A merchant changes a product's URL key and wants old links to keep working and preserve rankings. What should happen?
- A 301 permanent redirect is auto-created
- A 302 temporary redirect
- The old URL is deleted from the sitemap

<details><summary>Answer & Explanation</summary>

**Answer:** A 301 permanent redirect is auto-created

**Explanation:** Adobe Commerce can automatically create a 301 permanent redirect when a URL key changes ("Create Permanent Redirect for old URL"), preserving link equity and avoiding 404s.

**Exam Trap:** Use 301 (permanent) for moved/changed URLs, not 302 (temporary). The permanent redirect passes ranking signals.

</details>

---

### Q40. Which file, editable from the Admin, tells search engine crawlers which paths they may or may not crawl?
- sitemap.xml
- robots.txt
- .htaccess

<details><summary>Answer & Explanation</summary>

**Answer:** robots.txt

**Explanation:** robots.txt allows/disallows crawler access to specified paths and is configurable from the Admin (Content > Design > Configuration > Search Engine Robots).

**Exam Trap:** robots.txt controls crawler access; sitemap.xml lists URLs to help discovery. Don't conflate "restrict crawling" with "help find pages."

</details>

---

### Q41. Which low-contrast text problem falls under which compliance area?
- GDPR privacy
- Accessibility / WCAG contrast requirements
- PCI DSS

<details><summary>Answer & Explanation</summary>

**Answer:** Accessibility / WCAG contrast requirements

**Explanation:** Minimum color-contrast ratios are a WCAG success criterion tied to accessibility (ADA/Section 508), ensuring readability for low-vision users — not a privacy or payment matter.

**Exam Trap:** Compliance distractors blend GDPR, cookies, and accessibility. A visual readability symptom always points to WCAG/accessibility.

</details>

---

### Q42. Which WCAG conformance level is the common legal/target benchmark for e-commerce sites?
- Level A
- Level AA
- Level AAA

<details><summary>Answer & Explanation</summary>

**Answer:** Level AA

**Explanation:** WCAG 2.x Level AA is the widely adopted legal and practical accessibility target, balancing broad coverage with feasibility. AAA is stricter and rarely required site-wide.

**Exam Trap:** AA (not AAA) is the standard benchmark. Remember the POUR principles: Perceivable, Operable, Understandable, Robust.

</details>

---

### Q43. A merchant enables Cookie Restriction Mode. What is the effect?
- Blocks all cookies permanently
- Prompts shoppers for cookie consent before setting non-essential cookies
- Deletes existing customer data

<details><summary>Answer & Explanation</summary>

**Answer:** Prompts shoppers for cookie consent before setting non-essential cookies

**Explanation:** Cookie Restriction Mode (Stores > Configuration > General > Web > Default Cookie Settings) displays a consent notice and withholds non-essential cookies until the shopper consents — supporting privacy compliance.

**Exam Trap:** It's a consent gate, not a total cookie block or a data-deletion tool.

</details>

---

### Q44. A merchant wants two catalog trees (Retail and Outlet) under one brand, sharing customers and currency, single login. What structure fits?
- One website, two stores (each with its own root category)
- One website, two store views
- Two websites

<details><summary>Answer & Explanation</summary>

**Answer:** One website, two stores (each with its own root category)

**Explanation:** Two catalog trees require two root categories, which live at the Store level. Keeping both stores under one website preserves shared customers, currency, and single login.

**Exam Trap:** Root category is assigned per Store, not per Store View. Store views under one store share the same root category.

</details>

---

### Q45. Which native feature lets shoppers save products to purchase later, tied to their account?
- Wish List
- Compare Products
- Requisition List

<details><summary>Answer & Explanation</summary>

**Answer:** Wish List

**Explanation:** Wish List lets logged-in customers save products for later, viewable in their account. Compare Products is a temporary side-by-side comparison; Requisition List is a B2B reusable ordering list.

**Exam Trap:** Wish List (save for later, B2C) vs. Requisition List (B2B repeat ordering). Compare is transient and not saved to the account long-term.

</details>

---

### Q46. Which entity is static, manually assigned, and used to control pricing, tax, and catalog permissions?
- Customer Segment
- Customer Group
- Customer Attribute

<details><summary>Answer & Explanation</summary>

**Answer:** Customer Group

**Explanation:** Customer Groups are static, admin-assigned categories used for group pricing, tax classes, and B2B catalog permissions. Segments are dynamic and rule-based for marketing.

**Exam Trap:** Groups = static, pricing/tax/permissions. Segments = dynamic, marketing/content targeting (Adobe Commerce-only).

</details>

---

### Q47. A merchant wants a "kit" sold as a single sellable unit where the buyer picks one CPU, one RAM, and one drive from defined options. Which product type fits?
- Grouped product
- Bundle product
- Configurable product

<details><summary>Answer & Explanation</summary>

**Answer:** Bundle product

**Explanation:** A Bundle product is one sellable unit assembled from selectable options (with required/optional selections and quantities), ideal for "build your own" kits like a custom PC.

**Exam Trap:** Bundle = one configurable kit unit. Grouped = independent products bought separately. Configurable = variations of a single product (size/color).

</details>

---

### Q48. Which product type presents multiple existing simple products together, each with its own quantity box and no obligation to buy all?
- Bundle
- Grouped
- Configurable

<details><summary>Answer & Explanation</summary>

**Answer:** Grouped

**Explanation:** A Grouped product displays several existing simple products on one page, each independently purchasable with its own quantity input and no requirement to buy the whole set.

**Exam Trap:** Grouped = independent simple products together. Bundle = one kit unit with selection rules. Don't confuse the two.

</details>

---

### Q49. A merchant needs an attribute usable as a configurable "super attribute" for building variations. What type and setting are required?
- Text field, Global scope
- Dropdown (select) type with "Use to Create Configurable Product" enabled
- Multiselect with "Use in Search" enabled

<details><summary>Answer & Explanation</summary>

**Answer:** Dropdown (select) type with "Use to Create Configurable Product" enabled

**Explanation:** Configurable super attributes must be Dropdown (select) input type with "Use to Create Configurable Product = Yes" and Global scope, so each option maps to a distinct simple product variation.

**Exam Trap:** Only select-type attributes with the configurable setting enabled can build variations. Text/multiselect types cannot serve as super attributes.

</details>

---

### Q50. A support agent needs to find all orders for a shopper who provided only their email address. What is the fastest Admin action?
- Search the Orders grid by increment ID
- Search the Customers grid by email, then view the customer's orders
- Search by billing name

<details><summary>Answer & Explanation</summary>

**Answer:** Search the Customers grid by email, then view the customer's orders

**Explanation:** Email uniquely identifies a customer account. Searching the Customers grid by email opens the account, where the Orders tab lists all associated orders — the most direct path.

**Exam Trap:** Email is unique; name search returns ambiguous matches, and order-number search doesn't help when only an email is provided.

</details>
