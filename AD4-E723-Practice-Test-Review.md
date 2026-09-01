# (AD0-E723) Practice Test Review
### Adobe Commerce Business Practitioner Professional — Questions, Explanations & Exam Traps

---

### Q1. A B2B sales lead needs to offer lower prices per unit when a buyer orders 50 or more units of an item, with a further reduced unit price at 100 units and above. The discount should be visible on the product page for qualifying quantities. Which pricing configuration fits this requirement?
- Tier price
- Group price
- Special price

<details><summary>Answer & Explanation</summary>

**Answer:** Tier price

**Explanation:** Tier price is a per-product pricing structure keyed on quantity breaks — exactly what "50 units → price A, 100 units → price B" describes. It displays directly on the product page as a price-break table. Group price is a single flat discount per customer group with no quantity ladder; Special price is a flat, time-bound discount unrelated to quantity.

**Exam Trap:** Don't confuse Tier Price with Group Price. Group price only sets *one* discounted price per customer group — no quantity-break logic. Tier price is the only native option with built-in quantity breaks.

</details>
---

### Q2. A product data specialist wants "Color" and "Size" to be usable as variation options so that each combination corresponds to a separate underlying product. Which attribute setting must be enabled on these fields to support this behavior?
- Enable "Use in Layered Navigation."
- Set the attribute scope to Global.
- The "Use for Configurable Product" setting on the attributes.

<details><summary>Answer & Explanation</summary>

**Answer:** The "Use for Configurable Product" setting on the attributes.

**Explanation:** To use an attribute as a configurable "super attribute," it must be a select (dropdown) type with "Use for Configurable Products = Yes" enabled. Global scope is a related requirement for configurables (values must be consistent site-wide) but it is not the setting that activates the attribute for variation building.

**Exam Trap:** The exam baits you with "Global scope" because it's a real, necessary requirement for configurable attributes — but the setting that specifically *enables* configurable use is "Use for Configurable Products," not the scope field.

</details>
---

### Q3. A digital merchandiser wants a "Material" filter in layered navigation for category and search results pages. The attribute should be selectable as a facet but not affect search relevance. Which key storefront properties should be enabled on the Material attribute?
- Disable the attribute as a filter on category pages and search results pages, and enable keyword search for it.
- Enable the attribute as a filter on category pages and search results pages, and disable keyword search for it.
- Enable the attribute as a filter on category pages, keep it unavailable as a filter on search results pages, and disable keyword search for it.

<details><summary>Answer & Explanation</summary>

**Answer:** Enable the attribute as a filter on category pages and search results pages, and disable keyword search for it.

**Explanation:** "Filterable in layered navigation" and "Use in Search" (keyword/relevance) are independent toggles. You want Filterable = Yes (both category and search results pages) and Use in Search = No, so it acts as a facet without influencing full-text ranking.

**Exam Trap:** Watch for the distinction between "filterable" (facet/navigation) and "Use in Search" (full-text relevance/indexing) — they are not the same setting, and the exam often tests whether you know they're independent.

</details>
---

### Q4. In Inventory Management, which entity represents a virtual, aggregated inventory of products mapped to sales channels such as websites?
- Stock
- Reservation
- Source

<details><summary>Answer & Explanation</summary>

**Answer:** Stock

**Explanation:** In MSI: Sources = physical locations holding real quantity. Stock = a virtual aggregation of one or more Sources, assigned to a Sales Channel (website). Reservations are the mechanism for temporarily decrementing salable quantity without touching actual source quantity.

**Exam Trap:** MSI hierarchy — Source (physical) → Stock (virtual aggregation) → Sales Channel (website). Reservation is a transactional concept for quantity holds, not a structural inventory entity.

</details>
---

### Q5. A product is enabled and correctly assigned to a website and category. Its stock quantity is set to 0, and the global configuration "Display Out of Stock Products" remains set to "No." How will this product appear in search results and category listings?
- The product will appear in search results and will be hidden on category listing pages.
- The product will be hidden from category listings and will appear in search results.
- The product will be hidden from search results and category listings.

<details><summary>Answer & Explanation</summary>

**Answer:** The product will be hidden from search results and category listings.

**Explanation:** When "Display Out of Stock Products" = No, out-of-stock products (qty = 0, not backorderable) are excluded from both category listing pages and search results (and layered navigation). This is a global setting applied uniformly across storefront listing surfaces.

**Exam Trap:** Don't assume search results are more "permissive" than category pages. "Display Out of Stock Products = No" suppresses the product everywhere it would normally be listed — category grids, layered nav, and search.

</details>
---

### Q6. How does Adobe Commerce hold and release inventory between order placement and shipment when Inventory Management is enabled?
- By updating inventory only on invoice creation, without using reservations.
- By deducting source quantities on order and restoring them only if the order is canceled.
- By creating stock-level reservations on order and clearing them on shipment, cancellation, or refund.

<details><summary>Answer & Explanation</summary>

**Answer:** By creating stock-level reservations on order and clearing them on shipment, cancellation, or refund.

**Explanation:** With MSI enabled, placing an order doesn't immediately decrement the physical Source quantity. A negative reservation is created against the Stock to reduce salable quantity immediately. The actual Source quantity is deducted only when the order ships; reservations clear on cancel/refund/shipment.

**Exam Trap:** Don't describe this as simple "deduct on order, restore on cancel" — that's the legacy pre-MSI behavior. MSI specifically works through the Reservation system.

</details>
---

### Q7. A merchandiser wants to present a "Complete Desk Setup" page where several existing simple SKUs (desk, chair, lamp) appear together, but shoppers must be able to choose quantities for each item independently and do not have to buy the full set. Which product type should be used?
- Grouped product
- Bundle product
- Configurable product

<details><summary>Answer & Explanation</summary>

**Answer:** Grouped product

**Explanation:** Grouped products display multiple existing simple products together on one page, each with its own independent quantity box, with no obligation to buy all of them. Bundle produces one sellable "kit" unit with required/optional selections. Configurable represents variations of one product, not distinct products.

**Exam Trap:** Bundle vs. Grouped is a classic confusion point. Grouped = independent, separately purchasable products, no obligation to buy together. Bundle = a single configurable kit product with minimum-selection rules.

</details>
---

### Q8. A merchandiser wants an attribute to appear as a filter in layered navigation and Live Search facets, but not as a text-searchable field. How should the attribute be configured?
- Make the attribute not filterable in layered navigation/search results and set Use in Search = No.
- Make the attribute filterable in layered navigation/search results and set Use in Search = Yes.
- Make the attribute filterable in layered navigation/search results and set Use in Search = No.

<details><summary>Answer & Explanation</summary>

**Answer:** Make the attribute filterable in layered navigation/search results and set Use in Search = No.

**Explanation:** Same principle as Q3: filterable/facet visibility and full-text "Use in Search" are independent. To be a facet but not a searchable keyword field: Filterable = Yes, Use in Search = No.

**Exam Trap:** Same trap pattern as Q3, restated for Live Search — expect the "filterable ≠ searchable" distinction tested multiple times with different phrasing.

</details>
---

### Q9. A category is set to Anchor = Yes, and several attributes are configured for use in layered navigation. What changes for shoppers on that category page?
- Filters do not appear, leaving shoppers unable to narrow the category listing by those attributes.
- Filters appear, letting shoppers narrow the category listing by those attributes.
- Filters appear on the category product listing page, but selecting them does not change which products are shown.

<details><summary>Answer & Explanation</summary>

**Answer:** Filters appear, letting shoppers narrow the category listing by those attributes.

**Explanation:** Anchor = Yes on a category means the page includes products from subcategories AND activates layered navigation filtering for that category using any attribute marked "Use in Layered Navigation." Anchor = No suppresses layered nav filters on that page even if attributes are configured.

**Exam Trap:** Anchor isn't just "include subcategory products" — it's also the switch that turns ON layered navigation display for that category page.

</details>
---

### Q10. A merchandising strategist needs to configure product-level pricing that offers better rates to members of a loyalty customer group without requiring them to purchase multiple units. The lower price should always apply when those customers are logged in. Which pricing option should be used to target the loyalty customer group?
- Catalog price rule
- Cart price rule
- Group price rule.

<details><summary>Answer & Explanation</summary>

**Answer:** Group price rule.

**Explanation:** The requirement — product-level pricing, no quantity requirement, always applies when logged in — describes native Group Price (a per-customer-group price field on the product). In the answer key here it's labeled "Group price rule."

**Exam Trap:** Group Price (product-level, per customer group, no quantity/condition needed) vs. Catalog Price Rule (broader, condition-based, applies across many products with more flexible triggers) vs. Cart Price Rule (applies at cart/checkout, often via coupon codes) are commonly confused. "Product-level" + "no quantity requirement" + "always applies" points to Group Price.

</details>
---

### Q11. A catalog manager must offer a T-shirt where shoppers select size and color, and each size–color combination needs its own SKU and stock level. Shoppers see one product detail page that presents selectable options. Which product type fits this requirement?
- Grouped product
- Virtual product
- Configurable product

<details><summary>Answer & Explanation</summary>

**Answer:** Configurable product

**Explanation:** Configurable = parent product presenting selectable options (super attributes), with each combination mapped to its own simple product (own SKU, own stock).

**Exam Trap:** Don't confuse with Grouped (independent products, no single PDP with selectable variations) or Bundle (one sellable unit with selectable components, not separate underlying simple products with their own inventory).

</details>
---

### Q12. A promotions specialist sets up a cart price rule with a specific coupon code and configures "Uses per Coupon" to 100 while leaving "Uses per Customer" blank. After a successful email campaign, the 101st customer who tries the code receives a message that the coupon is invalid, even though the rule is still active and cart conditions are met. What is the cause of this issue?
- The coupon's expiration date has passed.
- The "Uses per Customer" limit has been reached.
- The "Uses per Coupon" limit has been reached.

<details><summary>Answer & Explanation</summary>

**Answer:** The "Uses per Coupon" limit has been reached.

**Explanation:** "Uses per Coupon" caps total redemptions of that code across all customers combined. "Uses per Customer" caps redemptions per individual. A blank "Uses per Customer" means unlimited per customer, but the code still stops working globally once the overall 100-use cap is hit.

**Exam Trap:** "Uses per Coupon" = global ceiling on the code; "Uses per Customer" = ceiling on repeat use by one shopper. A blank "Uses per Customer" doesn't override the coupon-wide limit.

</details>
---

### Q13. A product has a regular price of $120 and no special price. A catalog price rule with 25% off (priority 1) is active. A cart price rule with an additional 10% off the cart subtotal also applies. What is the effective final price of the product after both rules?
- $81
- $108
- $90

<details><summary>Answer & Explanation</summary>

**Answer:** $81

**Explanation:** Catalog price rule applies first (product-level): $120 × 0.75 = $90. Cart price rule then applies 10% off the already-discounted subtotal: $90 × 0.90 = $81.

**Exam Trap:** Catalog price rules always apply before cart price rules and modify the base/displayed price first; cart price rules then act on that discounted subtotal. A common wrong approach applies both percentages independently to the original $120.

</details>
---

### Q14. A business practitioner needs a promotion "10% off only for new customers placing their first order," applied at checkout. The promotion should not affect catalog pricing but must use customer order history. Which feature should be combined with a cart price rule?
- Cart price rule conditioned on a customer segment such as "Total Number of Orders less than 1"
- Catalog price rule conditioned on a customer segment such as "Total Number of Orders less than 1"
- Coupon-based promotion conditioned on a customer segment such as "Total Number of Orders less than 1"

<details><summary>Answer & Explanation</summary>

**Answer:** Cart price rule conditioned on a customer segment such as "Total Number of Orders less than 1"

**Explanation:** Customer segments can be built on order history/behavior attributes and referenced as a condition in a cart price rule. Catalog price rules can't reference checkout-context order-history conditions, and this doesn't require a coupon code.

**Exam Trap:** Catalog price rules don't have access to conditions like order count/history; they operate on product + customer group/website conditions, not cart-level behavior segments.

</details>
---

### Q15. A cart price rule "VIP 20% Off" (priority 1) has "Discard Subsequent Rules" set to Yes. A second cart price rule "Free Shipping over $100" (priority 2) is also valid for the same cart. The VIP rule applies first. How does this configuration affect the free-shipping rule?
- The free-shipping rule applies and the VIP discount is not applied to the order.
- The VIP rule applies and prevents the free-shipping rule from being applied.
- The VIP rule applies and the free-shipping rule is also applied to the order.

<details><summary>Answer & Explanation</summary>

**Answer:** The VIP rule applies and prevents the free-shipping rule from being applied.

**Explanation:** Priority 1 rule (VIP) with "Discard Subsequent Rules" = Yes stops the rule engine from evaluating/applying any lower-priority rules on that cart, so Free Shipping (priority 2) never applies, even though it's independently valid.

**Exam Trap:** "Discard Subsequent Rules" is rule-level and cuts off all lower-priority rules regardless of whether they target different things (discount vs. shipping).

</details>
---

### Q16. A merchant wants to permanently show a fixed $10 discount on a specific product SKU whenever a logged-in wholesale buyer views it in the catalog, regardless of how many units are purchased. The reduced price must display on the product and category pages. Which promotion type should be used?
- Catalog price rule with a customer group condition
- Cart price rule with a customer group condition.
- Customer group price with a wholesale customer group selection.

<details><summary>Answer & Explanation</summary>

**Answer:** Catalog price rule with a customer group condition

**Explanation:** This is a permanent, catalog-level price reduction, visible in product/category listings, targeted by customer group — the textbook catalog price rule scenario, scalable across many SKUs.

**Exam Trap:** Group Price is set per-product manually; Catalog Price Rule is more scalable (works across many SKUs/categories via one rule). The exam tests which mechanism fits a broad, rule-based scenario vs. a single-product manual setting.

</details>
---

### Q17. A marketing analyst is designing a promotion: "Enter SAVE10 to get 10% off all items in your cart, regardless of category." The discount should apply only when a valid code is entered at checkout. Which configuration should be used?
- Cart price rule with "Specific Coupon" and "Percent of product price discount" action.
- Sales rule with "Specific Coupon" and "Fixed amount discount for whole cart" action.
- Catalog price rule with a coupon code and a percentage discount.

<details><summary>Answer & Explanation</summary>

**Answer:** Cart price rule with "Specific Coupon" and "Percent of product price discount" action.

**Explanation:** Cart price rules are the only rule type tied to specific coupon codes entered at checkout. "Specific Coupon" makes the code required; "Percent of product price discount" applies the percentage across cart items.

**Exam Trap:** "Catalog price rule with a coupon code" is a nonsensical distractor — catalog price rules never use coupon codes; they apply automatically based on conditions.

</details>
---

### Q18. A marketing manager reports that a "SPRING20" coupon should give 20% off orders over $200 but shoppers see "The coupon code isn't valid" at checkout. The administrator confirms the cart price rule is active, the subtotal condition is met, and the code is typed correctly. During review, the administrator notices the rule is assigned only to the "Outlet" website, while customers are testing on the main website. What is the reason the coupon is not working?
- The cart price rule is configured for the wrong website.
- The cart price rule is configured with the wrong coupon code.
- The cart price rule is configured for the wrong customer group.

<details><summary>Answer & Explanation</summary>

**Answer:** The cart price rule is configured for the wrong website.

**Explanation:** Cart price rules are scoped to specific websites. If the rule is assigned only to "Outlet" and the customer is on the main website, the rule doesn't apply there — Commerce reports the coupon as invalid.

**Exam Trap:** Always check the Websites field on cart/catalog price rules when a promotion "isn't showing up" or "says invalid" — website scope mismatches are a classic root-cause scenario.

</details>
---

### Q19. A merchandising planner wants to show a "Summer Sale – % off selected categories" banner, and the discounted prices must be visible directly in product listings for only those categories during the campaign period. Which promotion type aligns with this requirement?
- Catalog price rule scoped to specific categories with a percentage discount and scheduled dates.
- Cart price rule with a category condition and a percentage discount applied at checkout.
- Coupon-based promotion applied at checkout to specific categories with a percentage discount and scheduled dates.

<details><summary>Answer & Explanation</summary>

**Answer:** Catalog price rule scoped to specific categories with a percentage discount and scheduled dates.

**Explanation:** Catalog price rules can target categories directly as a condition, apply a percentage discount, use scheduling for the campaign window, and display the reduced price directly in product listings without any code entry or cart action.

**Exam Trap:** Cart price rules and coupon-based promotions only ever apply at cart/checkout — they never change the displayed price on category or product listing pages. "Discounted price must show directly in listings" always signals a catalog price rule.

</details>
---

### Q20. A support lead is reviewing a credit memo for a completed order that was paid using a payment service where the funds have already been captured. The team also needs to handle refunds for orders paid by Check/Money Order, where no communication with a payment service is required and only the store's records must be updated. How should the type of refund be selected for each payment method?
- Use offline refunds for captured gateway payments; use online refunds for Check/Money Order.
- Use online refunds for captured gateway payments; use offline refunds for Check/Money Order.
- Select no refund in Adobe Commerce for either the captured payment method or the Check/Money Order payment.

<details><summary>Answer & Explanation</summary>

**Answer:** Use online refunds for captured gateway payments; use offline refunds for Check/Money Order.

**Explanation:** Online refund = Commerce communicates with the gateway to actually return funds (needed when funds were captured via that gateway). Offline refund = Commerce updates its own records without contacting any payment service — appropriate for Check/Money Order.

**Exam Trap:** "Online" vs. "offline" refund is about whether Commerce talks to a payment gateway, not about e-commerce vs. in-person. Captured card payments need online refunds; Check/Money Order always needs offline.

</details>
---

### Q21. An implementation consultant must recommend payment options for a market where many customers lack credit cards, but use cash and local bank transfers. The merchant needs simple configuration and manual control over payment confirmation. Which approach fits this scenario?
- Configure offline payment methods for that store view.
- Configure automated payment methods for that store view.
- Configure online payment methods for that store view.

<details><summary>Answer & Explanation</summary>

**Answer:** Configure offline payment methods for that store view.

**Explanation:** Offline payment methods (Check/Money Order, Bank Transfer, Cash on Delivery, Purchase Order) require no gateway integration and rely on manual merchant confirmation — matching "simple configuration" + "manual control."

**Exam Trap:** Don't be pulled toward "online payment methods" just because bank transfer sounds electronic — Bank Transfer is categorized as an offline payment method in Commerce because it requires manual reconciliation.

</details>
---

### Q22. A promotions specialist wants to offer free shipping when a cart price rule is applied, even if the Free Shipping delivery method is not globally available for all orders. Which combination is required for this to work?
- Enable the Free Shipping delivery method and configure the cart price rule's Free Shipping action.
- Enable the Flat Rate shipping method and configure the cart price rule's percentage discount action.
- Enable the Table Rates shipping method and configure the cart price rule's fixed amount discount action.

<details><summary>Answer & Explanation</summary>

**Answer:** Enable the Free Shipping delivery method and configure the cart price rule's Free Shipping action.

**Explanation:** For a cart price rule's "Free Shipping" action to produce a free-shipping option at checkout, the Free Shipping shipping method must still be enabled in configuration (even if its own threshold isn't met) — the rule's action essentially unlocks it.

**Exam Trap:** The cart price rule's "Apply Free Shipping" checkbox depends on the Free Shipping carrier/method being enabled somewhere in shipping configuration — it doesn't create shipping availability out of thin air.

</details>
---

### Q23. A catalog manager needs a simple fixed shipping charge per order regardless of destination or order value, with no free shipping. Which shipping method fits this requirement?
- Flat Rate shipping configured as Per Order.
- Free Shipping configured as Enabled with Minimum Order Amount.
- Table Rates shipping configured as Weight vs Destination.

<details><summary>Answer & Explanation</summary>

**Answer:** Flat Rate shipping configured as Per Order.

**Explanation:** Flat Rate has two calculation modes: "Per Order" (one fixed charge regardless of items/destination) and "Per Item" (charge × quantity). "Per Order" matches a fixed charge regardless of destination or order value.

**Exam Trap:** Don't confuse Flat Rate's "Per Order" vs. "Per Item" — Per Item scales with quantity, which would not be "a simple fixed shipping charge."

</details>
---

### Q24. An order is paid by "Check / Money Order." The customer never sends the check, and the order remains in Pending Payment. Customer service decides to cancel the order without any interaction with a payment gateway. Which type of payment method is involved?
- Manual payment method
- Online payment method
- Offline payment method

<details><summary>Answer & Explanation</summary>

**Answer:** Offline payment method

**Explanation:** Check/Money Order is a classic offline payment method — no payment gateway is ever contacted for authorization, capture, or cancellation; cancellation is purely a Commerce-side status change.

**Exam Trap:** "No interaction with a payment gateway" is the defining test for classifying something as an offline payment method on this exam.

</details>
---

### Q25. A support agent reviews an order in Pending Payment state that was placed with a payment method configured as "Authorize and Capture" but where the payment gateway has not yet confirmed the charge. The customer asks to stop the order before any charge is completed. Which action is considered safe and supported for this order state?
- Confirm the charge with the payment gateway and then issue a refund to the customer.
- Refund the order amount to the customer's card number and change the order status to Complete.
- Cancel the order while it is still in Pending/Pending Payment state before payment is captured.

<details><summary>Answer & Explanation</summary>

**Answer:** Cancel the order while it is still in Pending/Pending Payment state before payment is captured.

**Explanation:** The safest, most supported action is to cancel before the charge is confirmed/captured — avoiding ever completing a transaction that then needs to be reversed.

**Exam Trap:** When a question asks for the "safe and supported" action and the order hasn't been captured yet, canceling early is preferred over "let it complete, then refund."

</details>
---

### Q26. A B2B wholesaler needs retail customers to pay standard VAT on products while approved wholesale customers should not be charged VAT. How should this difference be modeled in Commerce tax configuration?
- Set Retail and Wholesale customer tax classes; apply VAT rules only to Wholesale customers.
- Set Retail and Wholesale product tax classes; apply VAT rules only to Retail products.
- Set Retail and Wholesale customer tax classes; apply VAT rules only to Retail customers.

<details><summary>Answer & Explanation</summary>

**Answer:** Set Retail and Wholesale customer tax classes; apply VAT rules only to Retail customers.

**Explanation:** Tax rules combine a Customer Tax Class and Product Tax Class. Since the differentiation is about which customers pay VAT, the Customer Tax Class is the lever to pull, with the VAT tax rule scoped to apply only when the customer's tax class = Retail.

**Exam Trap:** When a scenario distinguishes tax treatment by customer type (retail vs. wholesale, B2B vs. B2C), use Customer Tax Class. When it's about product type, use Product Tax Class instead.

</details>
---

### Q27. An order is in state New and a merchant has Inventory Management (MSI) enabled. With "Decrease Stock When Order is Placed" set to Yes, what inventory-related effect occurs when this New order is submitted?
- The inventory department manager is notified to hold an item for the shipping department.
- A reservation is created that reduces the salable quantity for the ordered items.
- The item is shipped out immediately and is marked out of stock.

<details><summary>Answer & Explanation</summary>

**Answer:** A reservation is created that reduces the salable quantity for the ordered items.

**Explanation:** With MSI, placing an order creates a negative reservation reducing salable quantity immediately; the physical source quantity isn't touched until shipment.

**Exam Trap:** "Decrease Stock When Order Is Placed = Yes" sounds like it should immediately decrement the physical Source quantity — but under MSI, it triggers reservation creation instead.

</details>
---

### Q28. An order is in state Complete, with all items invoiced and fully shipped. A customer contacts support to cancel the order entirely. Which statement describes what an administrator can do at this point?
- The order cannot be canceled; instead, a credit memo can be created for a refund.
- Wait for the items to be returned then issue the customer a gift card.
- Change the order status to Cancelled and send the customer a return label.

<details><summary>Answer & Explanation</summary>

**Answer:** The order cannot be canceled; instead, a credit memo can be created for a refund.

**Explanation:** Once an order is fully invoiced and shipped (Complete state), "Cancel" is no longer available — cancellation only works before invoicing. The correct path for a Complete order is a credit memo (refund).

**Exam Trap:** Order "Cancel" only applies to unfulfilled orders (before invoicing/shipping). Once shipped/invoiced, the system forces a Credit Memo / refund flow.

</details>
---

### Q29. Which feature is used to define rules based on shopper attributes, order history, and browsing behavior so that promotions and content can target specific groups of shoppers dynamically?
- Customer groups
- Customer segments
- Customer management

<details><summary>Answer & Explanation</summary>

**Answer:** Customer segments

**Explanation:** Customer Segments dynamically group shoppers based on combined criteria — attributes, order history, browsing behavior — and can be referenced by cart price rules, dynamic blocks, etc. Customer Groups are static, manually-assigned categories.

**Exam Trap:** Customer Groups vs. Customer Segments is heavily tested. Groups = static, used for pricing/tax/catalog permissions. Segments = dynamic, rule-based, used for targeted marketing/content (an Adobe Commerce, not Open Source, feature).

</details>
---

### Q30. A shopper contacts support with a GDPR/CCPA-style request to obtain a copy of the personal data stored about them. At a conceptual level, how can Commerce help address this request?
- Export the customer's account and order data from Commerce and escalate the request to the privacy/compliance team for complex handling.
- Export the customer's account and order data from Commerce and send it directly to the customer without involving the privacy/compliance team.
- Escalate the request to the privacy/compliance team without exporting any data from Commerce.

<details><summary>Answer & Explanation</summary>

**Answer:** Export the customer's account and order data from Commerce and escalate the request to the privacy/compliance team for complex handling.

**Explanation:** Fulfilling a data-subject access request means pulling the customer's stored account/order data from Commerce and looping in the compliance team, since a full response often spans multiple systems.

**Exam Trap:** Don't over-rotate to "just escalate to compliance" — the exam expects recognition of Commerce's practical role in exporting the data it holds as part of the response.

</details>
---

### Q31. Which feature is designed to display different storefront content to specific groups of shoppers based on defined conditions?
- Customer blocks with conditional product logic
- Static blocks with custom code
- Dynamic blocks configured with customer segment conditions

<details><summary>Answer & Explanation</summary>

**Answer:** Dynamic blocks configured with customer segment conditions

**Explanation:** Dynamic Blocks are purpose-built to show different content to different segments/conditions dynamically. Static blocks show the same content to everyone with no built-in targeting logic.

**Exam Trap:** Static Block ≠ Dynamic Block. Dynamic Blocks (leaning on Customer Segments) are the purpose-built targeting mechanism.

</details>
---

### Q32. A marketing manager wants to capture a "Preferred Store Location" value from new customers during registration and later use it to segment customers by store. Which Commerce feature should be used to add this field to the registration form?
- A custom admin attribute.
- A custom store attribute.
- A custom customer attribute.

<details><summary>Answer & Explanation</summary>

**Answer:** A custom customer attribute.

**Explanation:** Registration form fields map to Customer EAV attributes. To add a new field to registration (and later use its value in segmentation), you create a custom customer attribute and enable it for the registration form.

**Exam Trap:** "Store attribute" and "admin attribute" reference different EAV entity types — a registration form field is always a customer attribute.

</details>
---

### Q33. A segmentation specialist wants to build a customer segment based on a custom "Industry" field collected during registration. Which prerequisite is needed before "Industry" can be used in the segment rule?
- Define "Industry" as a customer attribute and enable it for use in customer segments.
- Define "Industry" as a store configuration setting and enable it for use in customer segments.
- Define "Industry" as a segment rule option and enable it for use in customer segments.

<details><summary>Answer & Explanation</summary>

**Answer:** Define "Industry" as a customer attribute and enable it for use in customer segments.

**Explanation:** The field must first exist as a customer attribute; then, since customer segments reference customer attributes, it must specifically be enabled for use in segment rule conditions.

**Exam Trap:** This tests a two-step dependency: (1) attribute must exist and be customer-scoped, and (2) it must specifically be enabled for use in segments — existing alone doesn't make it selectable everywhere.

</details>
---

### Q34. A multi-brand merchant wants all sites in a single Commerce instance to use the same set of customer records so that a shopper can log in with the same credentials on every site. Which customer scope configuration supports this requirement?
- Set "Share Customer Accounts" to Global.
- Set "Share Customer Accounts" to Store.
- Set "Share Customer Accounts" to Website.

<details><summary>Answer & Explanation</summary>

**Answer:** Set "Share Customer Accounts" to Global.

**Explanation:** "Share Customer Accounts" = Global means one shared customer table/login works across all websites in the installation — exactly the "single login, all sites" requirement.

**Exam Trap:** "Share Customer Accounts" really has two meaningful states: Global (shared across all websites) or Per Website (separate accounts per website). There's no meaningful "Store" scope for this setting.

</details>
---

### Q35. A customer service agent receives an email from a shopper asking for help locating their last order. The shopper provides their email address. Which Admin action should the agent use first to find the customer's orders?
- Search for the order by order number.
- Search for the customer by email.
- Search for the customer by name.

<details><summary>Answer & Explanation</summary>

**Answer:** Search for the customer by email.

**Explanation:** Email is the unique identifier for a customer account; searching the Customers grid by email is the fastest, most direct way to locate the account and view order history.

**Exam Trap:** Email search is preferred because name search can return multiple ambiguous matches, unlike the unique email identifier.

</details>
---

### Q36. Which feature displays performance metrics for saved audience definitions over time?
- Bestsellers reports
- Customer segment reports
- Invoiced sales reports

<details><summary>Answer & Explanation</summary>

**Answer:** Customer segment reports

**Explanation:** "Saved audience definitions" = customer segments; Commerce/Adobe provides segment-specific reporting to track performance of these dynamically-defined audiences over time.

**Exam Trap:** Bestsellers and Invoiced Sales reports are standard sales-analytics reports unrelated to audience/segment membership tracking.

</details>
---

### Q37. A company sells the same products worldwide but needs product descriptions and CMS content localized for Spanish and Portuguese in Latin America, with the same prices and catalog. What is the structural approach?
- One website, one store, and separate store views for Spanish and Portuguese.
- One website, two stores for Spanish and Portuguese, each with its own catalog and pricing rules.
- Two websites, one for Spanish and one for Portuguese, each with separate catalogs and prices for Latin America.

<details><summary>Answer & Explanation</summary>

**Answer:** One website, one store, and separate store views for Spanish and Portuguese.

**Explanation:** Store Views are the correct scope for pure localization (language/content) when catalog and pricing stay identical. Website and Store level changes are reserved for scenarios needing separate catalogs, pricing, or customer bases.

**Exam Trap:** Website > Store > Store View hierarchy: Website = separate business unit/pricing/customer base; Store = separate root catalog; Store View = separate language/presentation, same catalog and pricing. "Same prices and catalog" signals Store View.

</details>
---

### Q38. In Adobe Commerce, which SaaS-based capability exposes high-performance APIs for retrieving product information in headless or multi-channel storefronts?
- Live Search
- App Builder
- Catalog Service

<details><summary>Answer & Explanation</summary>

**Answer:** Catalog Service

**Explanation:** Catalog Service is Adobe's SaaS product-data API layer built for high-performance product information retrieval across headless/multi-channel storefronts. Live Search is specifically about search/autocomplete/facets, a distinct SaaS service.

**Exam Trap:** Catalog Service (product data API) vs. Live Search (search/merchandising API) vs. App Builder (extensibility/serverless framework) are often confused. Match "retrieving product information" to Catalog Service, not Live Search.

</details>
---

### Q39. A merchandiser wants to show a special "VIP Sale" banner only to customers in a "High-Value Customers" segment when they visit the homepage. Other customers should not see this banner. Which feature is designed for this type of targeted content?
- Dynamic block
- CMS Block
- Widgets

<details><summary>Answer & Explanation</summary>

**Answer:** Dynamic block

**Explanation:** Same principle as Q31 — content targeted by customer segment conditions is the defining use case for Dynamic Blocks.

**Exam Trap:** Same trap family as Q31: CMS/Static Blocks and Widgets don't have native segment-based visibility logic; only Dynamic Blocks do.

</details>
---

### Q40. A merchandiser plans a campaign landing page that requires a rich layout with hero images, text columns, and product callouts, and should be easy for non-technical users to update. Which authoring tool within the CMS should be used to build this page?
- Page Builder content types on a CMS page.
- HTML code content on a CMS page.
- Third party content types on a CMS page.

<details><summary>Answer & Explanation</summary>

**Answer:** Page Builder content types on a CMS page.

**Explanation:** Page Builder is the drag-and-drop, non-technical-friendly authoring tool within CMS pages for building rich layouts without needing to write HTML/code.

**Exam Trap:** "HTML code content on a CMS page" is the legacy, developer-required authoring method — the opposite of "easy for non-technical users."

</details>
---

### Q41. A merchant's primary need is a no-cost platform to launch a first online store, with the option to later upgrade to a licensed edition without replatforming. Which product should a Commerce Consultant recommend initially?
- Magento Open Source
- Adobe Commerce on premises
- Adobe Commerce Optimizer

<details><summary>Answer & Explanation</summary>

**Answer:** Magento Open Source

**Explanation:** Magento Open Source is the free, self-hosted edition sharing the same core codebase/architecture as Adobe Commerce, allowing upgrade to a licensed edition later without a full replatform.

**Exam Trap:** "Adobe Commerce Optimizer" and "Adobe Commerce on premises" are both licensed/paid offerings — neither fits "no-cost to launch." The free starting point is Magento Open Source.

</details>
---

### Q42. In the Commerce Admin, which feature defines what areas of the Admin interface a back office user can access, based on selected resources and scope?
- User accounts with configured preferences and notifications.
- Admin Action Logging with configured Admin Permission Roles and Users.
- User Roles with configured Role Resources and scope.

<details><summary>Answer & Explanation</summary>

**Answer:** User Roles with configured Role Resources and scope.

**Explanation:** User Roles (Admin > System > Permissions > User Roles) define exactly which "Resources" (Admin menu sections/functions) a role can access, plus the applicable scope.

**Exam Trap:** "User accounts" is where you assign a role to a specific person, not where you define what the role can access — the actual permission definition lives in Role Resources.

</details>
---

### Q43. Which permission setting allows an admin role to see configuration sections and data, but prevents saving any changes?
- Read-only access
- Edit access
- Write-only access

<details><summary>Answer & Explanation</summary>

**Answer:** Read-only access

**Explanation:** Admin role resource permissions include a "Read Only" access level for a resource, allowing a role to view a configuration section's data without being able to submit/save changes.

**Exam Trap:** Don't confuse this with the resource being completely denied — "Read Only" is a distinct middle-ground access level, separate from full Edit access and from no access at all.

</details>
---

### Q44. A merchant has one brand and wants to run two different catalog trees: one for "Retail" products and another for "Outlet" products, under the same customer base and base currency. Shoppers should use a single login for both. Which structure should be used?
- One website with two stores, each using its own root category.
- One website with two store views, each using its own root category.
- One store with two store views, each using its own root category.

<details><summary>Answer & Explanation</summary>

**Answer:** One website with two stores, each using its own root category.

**Explanation:** Two different catalog trees = two different Root Categories = two different Stores (Store level determines root category/catalog assortment). Staying on one Website keeps the shared customer base, currency, and single login.

**Exam Trap:** Root Category assignment happens at the Store level, not Store View — store views under the same store share the same root category by design. Hierarchy: Website → Store (root category/catalog) → Store View (language/presentation).

</details>
---

### Q45. A merchant uses a payment provider that redirects shoppers from the Commerce checkout to a secure hosted payment page on the provider's site. After payment, the shopper is redirected back with a token and status. Commerce never renders card fields. How does this flow impact PCI scope for Commerce?
- This increases PCI scope for Commerce because the Commerce server cannot verify that payment data was entered securely.
- The redirect/hosted payment page approach keeps card data off the Commerce server, reducing PCI scope for the Commerce application.
- This is a PCI infraction because payment data should not be entered outside of the Commerce server's purview.

<details><summary>Answer & Explanation</summary>

**Answer:** The redirect/hosted payment page approach keeps card data off the Commerce server, reducing PCI scope for the Commerce application.

**Explanation:** Since cardholder data is entered and processed entirely on the payment provider's hosted page, Commerce's PCI compliance burden is significantly reduced — a standard PCI scope-reduction pattern.

**Exam Trap:** This is the opposite of Q47's pattern — redirecting away from Commerce for card entry reduces PCI scope; storing card data directly in Commerce's own database increases PCI scope. Learn to distinguish these two patterns quickly.

</details>
---

### Q46. Which feature generates a structured sitemap file listing store URLs for search engines to crawl more efficiently?
- Products export
- Category hierarchy
- XML sitemap

<details><summary>Answer & Explanation</summary>

**Answer:** XML sitemap

**Explanation:** Adobe Commerce natively generates an XML Sitemap (Marketing > SEO & Search > Site Map) listing store URLs to help search engines crawl more completely.

**Exam Trap:** Don't confuse with robots.txt (Q50) — sitemap.xml lists URLs to help crawlers find pages; robots.txt restricts/permits crawling of certain paths.

</details>
---

### Q47. A merchant uses Magento Open Source with a custom "offline credit card" payment method that stores card numbers in encrypted form in the Commerce database and processes them manually later. What is the PCI implication?
- Commerce is in limited PCI scope if payments are processed within 24 hours and the data is then deleted.
- Commerce has no PCI impact if card numbers are stored in encrypted form.
- Commerce is in full PCI scope because it stores cardholder data; this pattern is generally not allowed without full PCI controls.

<details><summary>Answer & Explanation</summary>

**Answer:** Commerce is in full PCI scope because it stores cardholder data; this pattern is generally not allowed without full PCI controls.

**Explanation:** Storing cardholder data (even encrypted, if Commerce manages the encryption/keys) within the Commerce database puts the environment in full PCI DSS scope, requiring extensive compliance controls.

**Exam Trap:** "Encrypted" doesn't automatically mean "PCI-exempt" — if Commerce itself manages storage AND encryption (not delegating to a certified tokenization service), it's still in full PCI scope. Mirror-image trap to Q45.

</details>
---

### Q48. Which type of privacy right is exercised when a person asks an organization to remove their customer account and associated personal information?
- The right to data portability of personal data.
- The right to erasure/deletion of personal data.
- The right to access personal data.

<details><summary>Answer & Explanation</summary>

**Answer:** The right to erasure/deletion of personal data.

**Explanation:** This is the GDPR/CCPA-style "right to be forgotten" / right to erasure — deleting the account and associated personal data, distinct from portability (getting a copy) or access (viewing what's held).

**Exam Trap:** Right to Access, Right to Portability, and Right to Erasure are three distinct GDPR-style rights — match the specific action requested (deletion vs. copy vs. view) to the correct named right.

</details>
---

### Q49. Which compliance area is involved when text has very low color contrast against its background, making it difficult to read?
- Cookie consent and tracking disclosure requirements.
- GDPR data protection and privacy requirements.
- Accessibility/ADA and WCAG contrast requirements.

<details><summary>Answer & Explanation</summary>

**Answer:** Accessibility/ADA and WCAG contrast requirements.

**Explanation:** Color contrast ratio requirements are a core WCAG success criterion, directly tied to ADA/accessibility compliance — not a privacy (GDPR) or cookie-consent matter.

**Exam Trap:** Compliance-flavored questions often blend GDPR, cookie-consent, and accessibility as similar-sounding distractors. The specific symptom (visual readability/contrast) should always point to Accessibility/WCAG.

</details>
---

### Q50. Which file, configurable from within Adobe Commerce, is used to instruct search engine crawlers which areas of the site should be allowed or disallowed for crawling?
- sitemap.xml configuration
- robots.txt configuration
- readme.txt configuration

<details><summary>Answer & Explanation</summary>

**Answer:** robots.txt configuration

**Explanation:** robots.txt explicitly allows/disallows search engine crawler access to specific site paths; Adobe Commerce lets you edit this file's content directly from Admin.

**Exam Trap:** Same sitemap.xml vs. robots.txt distinction as Q46 — sitemap.xml lists pages to help crawling; robots.txt restricts what may be crawled. Don't conflate "help crawlers find pages" with "control crawler access."

</details>
