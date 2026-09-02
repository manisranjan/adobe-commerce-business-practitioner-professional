# Adobe Commerce Business Practitioner Professional (AD0-E723)
## Section 2: Promotions (16%) — Notes & Practice MCQs

---

## PART A: STUDY NOTES

### 2.1 Configuring the Appropriate Promotion Type

| Scenario Signal | Correct Tool |
|---|---|
| Discount shown directly on category/PDP, no code | Catalog Price Rule |
| Discount only at cart/checkout | Cart Price Rule |
| "Buy 2, get 1 free" | Cart Price Rule — Buy X Get Y Free |
| Free shipping reward | Cart Price Rule — Free Shipping action |
| Coupon shared via email blast | Cart Price Rule with Specific or Auto-Generated coupon |
| Discount for customer segment across many SKUs, always on | Catalog Price Rule scoped by customer group |
| Gift with purchase (specific free item) | Cart Price Rule — Buy X Get Y Free targeting specific SKU |
| Tiered discount by spend threshold | Multiple Cart Price Rules layered by priority |

> **⚠️ Exam Trap:** "On the product page with no code" signals a Catalog Price Rule, while anything triggered at the cart/checkout (coupons, Buy X Get Y, free shipping) is always a Cart Price Rule.

**Key setup fields**
- Websites / Customer Groups — scope of applicability
- Coupon — None / Specific Coupon / Auto Generate
- Uses per Coupon / Uses per Customer — redemption caps
- From/To dates — active window
- Priority — lower number evaluates first
- Discard subsequent rules — stops lower-priority rules once matched

#### Cart Price Rule — Full Tab Breakdown

**Rule Information tab**
- Rule Name, Description (internal only)
- Active (Yes/No)
- Websites — multi-select; rule only fires on selected websites
- Customer Groups — multi-select; must explicitly include "NOT LOGGED IN" if guests should qualify
- Coupon — No Coupon / Specific Coupon / Auto Generate
- Uses per Coupon — total redemptions allowed for that code across all customers
- Uses per Customer — redemptions allowed for one logged-in customer (does not apply to guests)
- From/To dates

**Conditions tab**
- Builds a nested AND/OR condition tree
- Common condition types: Subtotal, Total Items Quantity, Weight, Category (matches if ANY item in cart is in that category), Customer Group, Shipping Country/Region/Postcode, Payment Method
- Conditions can be combined ("Cart Subtotal is greater than X AND Category is Y") to create precise targeting
- Leaving Conditions completely empty makes the rule apply to ALL carts matching Websites/Customer Groups/dates

**Actions tab**
- **Apply** — Percent of product price discount / Fixed amount discount for whole cart / Fixed amount discount for each item / Buy X Get Y Free
- **Discount Amount** — the % or $ value
- **Discount Qty Step** (for Buy X Get Y) — defines the "X" trigger quantity and how many free "Y" units are granted per step
- **Maximum Qty Discount is Applied To** — caps how many units in the cart receive the discount
- **Discard subsequent rules**
- **Free Shipping** — No / For matching items only / For shipment with matching items

**Labels tab**
- **Store Labels** — the text shown to the customer in the cart/order summary describing the discount (can differ per store view for localization); if left blank, the Rule Name displays instead

> **⚠️ Exam Trap:** Guests only qualify when "NOT LOGGED IN" is explicitly added to Customer Groups, and "Uses per Customer" caps are ignored for guests since they can't be tracked.

#### Catalog Price Rule — Actions Tab Detail
- **Apply** field options: **Percent of Product's Price** / **Fixed Amount Discount** / **Fixed Price**
- Note the naming difference from Cart Price Rules — Catalog rules don't have a "Buy X Get Y" or "Free Shipping" action; those are cart-only mechanics
- **Discard subsequent rules** works identically to Cart Price Rules — evaluated in priority order

#### One Coupon Code Per Cart (Default Behavior)
- Out of the box, Adobe Commerce only allows **one coupon code active in the cart at a time** — applying a second coupon replaces the first rather than stacking
- This is separate from the fact that multiple **no-coupon** Cart Price Rules and Catalog Price Rules can still all apply simultaneously alongside the one coupon-based rule

> **⚠️ Exam Trap:** The one-coupon limit applies only to *coupon-based* rules — a second coupon silently replaces the first rather than being rejected, while no-coupon and catalog rules keep stacking underneath.

---

### 2.2 Explaining the Final Price

**Calculation order (full chain)**
1. Base Price (or Special Price if active and lower)
2. Tier Price if qty threshold met (wins if lower than Special Price)
3. Catalog Price Rules applied on top, in priority order → produces **displayed/PDP price**
4. Cart Price Rules applied at cart level, in priority order, on top of the PDP price

> **⚠️ Exam Trap:** Catalog Price Rules resolve into the displayed PDP price *before* the cart is even touched — Cart Price Rules then discount on top of that already-reduced price, so the two never compete, they compound.

**Key traps**
- Catalog Price Rules and Cart Price Rules are **not mutually exclusive** — both can apply to the same order unless a rule specifically discards subsequent processing
- When multiple Cart Price Rules apply, each processes against the **current** running subtotal, not the original — so rule order changes the outcome (10% then $5 off ≠ $5 off then 10%)
- **Multiple Catalog Price Rules also stack multiplicatively in priority order** when neither discards subsequent rules — e.g., a 10% rule followed by a further 10% rule results in 19% total off (0.9 × 0.9 = 0.81), not a flat 20%
- **Tax and discount interaction**: whether tax is calculated on the pre-discount or post-discount amount depends on the store's Tax configuration (Sales > Tax > Calculation Settings > "Apply Discount on Prices" — Including Tax or Excluding Tax, plus "Apply Tax After Discount" setting). This affects the final total a customer sees, so a "final price" scenario question may hinge on this setting rather than the promotion rule itself
- **Free Shipping action nuance**: "For matching items only" grants free shipping just for the specific line items that matched the rule's conditions; "For shipment with matching items" grants free shipping for the ENTIRE shipment once any matching item is present — these produce different totals when the cart has a mix of qualifying and non-qualifying items

> **⚠️ Exam Trap:** Because each Cart Price Rule recalculates against the *running* subtotal, reordering "10% off" and "$5 off" changes the final total — rule priority is a pricing decision, not just housekeeping.

---

### 2.3 Troubleshooting Promotion Issues

| Symptom | Likely Cause |
|---|---|
| Catalog Price Rule discount not showing | Catalog Rule Price indexer hasn't run — needs reindex |
| Coupon code "invalid" | From/To date window issue; Websites/Customer Groups mismatch; Uses per Customer limit reached |
| Cart rule not applying to guests | "NOT LOGGED IN" not included in Customer Groups field |
| Two promotions, only one applies | Priority + "Discard subsequent rules" flag on higher-priority rule |
| Free shipping not applying | Free Shipping action scope mismatch, or shipping method doesn't support promotional override |
| Discount total doesn't match expected | Rule condition evaluated against different subtotal than expected (e.g., Discount Qty Step logic in Buy X Get Y) |
| Rule works on one store, not another | Rule's Websites field doesn't include that store's website |
| Customer applied a second coupon and lost the first discount | Adobe Commerce allows only ONE coupon code active in the cart at a time by default — the second replaces the first, it doesn't stack |
| "Buy 2 Get 1 Free" only discounted 1 unit total instead of expected multiples | **Discount Qty Step** wasn't set correctly, or **Maximum Qty Discount is Applied To** capped the total units eligible |
| Rule condition using Category never matches | Category condition checks whether a matching category ID is directly assigned to the product — if the rule references a parent (anchor) category ID but products are only assigned to a child category, the condition can fail depending on how the condition attribute is configured |
| Free shipping applied to the whole order when it should have applied only to the discounted item | "Free Shipping" action was set to "For shipment with matching items" instead of "For matching items only" |
| Discount shows a different amount than expected after tax | Store's Tax > Calculation Settings ("Apply Discount on Prices"/"Apply Tax After Discount") don't match the assumption used when calculating the expected discount |
| Rule label shown to customer looks wrong or blank | Store Labels field on the Labels tab wasn't filled in for that store view — falls back to internal Rule Name |
| Minimum order amount blocks checkout despite a valid coupon | Sales > Sales > Minimum Order Amount setting is a separate, unrelated restriction from the Cart Price Rule and can block checkout independently of any active promotion |

> **⚠️ Exam Trap:** For Buy X Get Y, the reward only repeats when Discount Qty Step sets the ratio *and* "Maximum Qty Discount is Applied To" is raised — leaving the max at its default silently limits the giveaway to a single unit.

---

### 2.4 Advanced Cart Rule Mechanics & Loyalty Tools

#### Two condition sets on a Cart Price Rule
A Cart Price Rule actually has **two** places to define conditions, which are easy to confuse:

| Location | Question it answers | Effect |
|---|---|---|
| **Conditions tab** | *Does this cart qualify for the rule at all?* | Gatekeeper — if the whole cart doesn't match, the rule never fires |
| **Actions tab → "Apply the rule only to cart items matching the following conditions"** | *Which specific line items receive the discount?* | Narrows the discount to a subset of items even after the cart qualifies |

- Example: Conditions = "Subtotal > $100" (whole cart must exceed $100), Actions item-conditions = "Category = Clearance" (only clearance items are discounted). Both must be read separately in a scenario.

#### "Apply to Shipping Amount"
- On a **Percent of product price** action, the **"Apply to Shipping Amount"** checkbox extends the percentage discount to include the shipping charge, not just the merchandise — a subtle way a discount total can differ from the merchandise-only expectation.

#### Loyalty & stored-value tools (Adobe Commerce)
These are separate from price rules but are commonly grouped under "promotions" in scenarios:

| Tool | What it is | Edition |
|---|---|---|
| **Gift Cards** | A purchasable stored-value product (virtual/physical/combined) redeemable at checkout | **Adobe Commerce** |
| **Reward Points** | Earn/redeem loyalty points across actions (purchase, register, review); merchant sets earn/spend rates | **Adobe Commerce** |
| **Store Credit** | Prepaid balance (from refunds or admin) spendable at checkout | **Adobe Commerce** |

- Gift Cards are a **product type** the shopper buys and someone redeems; Store Credit is a **balance** the merchant grants. Don't conflate them.

> **⚠️ Exam Trap:** The **Conditions tab** decides whether the cart qualifies; the **Actions-tab item conditions** decide which items are actually discounted — a rule can qualify yet discount only a subset. Also, **Gift Cards, Reward Points, and Store Credit are Adobe Commerce–only** and are distinct from Cart/Catalog Price Rules (a Gift Card is a purchasable product; Store Credit is a granted balance).

---

## PART B: PRACTICE MCQs

**Q1.** A merchant wants customers who buy a specific skincare set to automatically receive a free travel-size sample added to their cart. What should be configured?
- A) Catalog Price Rule with a Fixed Price action
- B) Cart Price Rule with "Buy X Get Y Free" action
- C) Special Price on the free item's product page
- D) Tier Pricing set to qty 1 at $0

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Gift-with-purchase = Cart Price Rule, Buy X Get Y Free action targeting the specific SKU.

**Exam Trap:** A Catalog Price Rule can only reduce an existing product's price — it can never *add* a free item to the cart, so it cannot deliver a gift-with-purchase.

</details>

**Q2.** A product has a Base Price of $100, an active Special Price of $90, and a Catalog Price Rule giving 10% off all products in its category. What is the resulting displayed price?
- A) $100
- B) $70
- C) $81
- D) $90

<details><summary>Answer & Explanation</summary>

**Answer:** C — $81

**Explanation:** Special Price ($90) applies first, then the 10% Catalog Price Rule applies on top of that resulting price ($90 × 0.9 = $81).

**Exam Trap:** Don't add the discounts to the base price ($100 − 10% = $90) — the Catalog Price Rule stacks on the already-lowered Special Price, not on the original $100.

</details>

**Q3.** A merchant creates and saves a new Catalog Price Rule, but the storefront still shows the old price hours later. What is the most likely cause?
- A) Coupon hasn't been distributed
- B) Catalog Rule Price indexer hasn't run since the rule was saved
- C) Catalog Price Rules only apply at checkout
- D) Rule needs "Discard subsequent rules" enabled to take effect

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Saving a Catalog Price Rule doesn't update the storefront until the Catalog Rule Price indexer runs; the old price persists until reindex completes.

**Exam Trap:** Catalog Price Rules are indexed, not calculated live at request time — this is the opposite of Cart Price Rules, which are evaluated dynamically in the cart and need no reindex.

</details>

**Q4.** A Cart Price Rule with a valid, active coupon code works for logged-in customers but returns "coupon invalid" for guest checkouts. Most likely misconfiguration?
- A) Coupon must be regenerated for guests
- B) Guest checkout never compatible with Cart Price Rules
- C) The rule's Customer Groups field likely doesn't include "NOT LOGGED IN"
- D) Websites field is missing the guest's website

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Guests belong to the "NOT LOGGED IN" customer group; if that group isn't selected in the rule's Customer Groups field, the coupon is rejected for guest carts even though it's otherwise valid.

**Exam Trap:** The coupon itself is never "invalid" — eligibility is gated by Customer Groups, so a working code can still fail purely because a group is missing from the rule.

</details>

**Q5.** Two Cart Price Rules both match a customer's cart. Rule A has Priority = 1 and "Discard subsequent rules" = Yes. Rule B has Priority = 2. What happens?
- A) Both rules always apply together
- B) Only Rule A applies; Rule B is skipped
- C) Rule A never applies itself
- D) Whichever rule gives the larger discount applies

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** "Discard subsequent rules" on the higher-priority rule stops evaluation of lower-priority rules once it successfully applies.

**Exam Trap:** "Discard subsequent" does not skip the rule that carries the flag — Rule A still applies itself; it only blocks the rules that would be evaluated *after* it.

</details>

**Q6.** A product has an active Catalog Price Rule (10% off) AND the customer has a valid coupon for a Cart Price Rule (further $5 off the cart). What happens at checkout?
- A) Only the Catalog Price Rule applies
- B) Both can apply — Catalog Price Rule reduces the displayed price first, then the Cart Price Rule discounts further at checkout
- C) Only the Cart Price Rule applies, overriding the Catalog rule
- D) Commerce blocks having both active simultaneously

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Catalog and Cart Price Rules are not mutually exclusive; they stack unless one is set to discard subsequent processing.

**Exam Trap:** "Discard subsequent rules" only affects other rules of the *same type* in priority order — it never makes a Catalog Price Rule suppress a Cart Price Rule, so the two rule families always coexist.

</details>

**Q7.** A promotion works correctly on Store A but doesn't apply at all on Store B, even though both stores share the same customer groups. What is most likely misconfigured?
- A) Customer Groups missing a group from Store B
- B) Catalog Rule Price indexer needs to run separately for Store B
- C) The rule's Websites field doesn't include Store B's website
- D) Coupon codes must be regenerated per store

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** A rule only fires on the websites selected in its Websites field; if Store B's website isn't included, the rule never evaluates there regardless of matching customer groups.

**Exam Trap:** Websites scope rule eligibility, while Customer Groups scope who qualifies — matching customer groups can't rescue a rule that isn't assigned to the store's website.

</details>

---

### Set 2 — Advanced Configuration, Calculation & Troubleshooting Depth

**Q8.** A customer already has one coupon code applied to their cart and enters a second, different valid coupon code. What happens?
- A) Both discounts stack automatically
- B) The second coupon is rejected outright and cannot be entered
- C) The second coupon replaces the first — Commerce only allows one active coupon per cart by default
- D) The system picks whichever coupon gives the smaller discount

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Adobe Commerce allows only one coupon-based Cart Price Rule active in the cart at a time out of the box; a new valid coupon replaces the previous one rather than stacking.

**Exam Trap:** The second coupon is *not* rejected — it's accepted and silently overwrites the first, so the customer may unknowingly lose a better discount.

</details>

**Q9.** A merchant sets up a "Buy 3 Get 1 Free" Cart Price Rule but customers report only getting 1 free item no matter how many sets of 3 they buy (e.g., buying 9 should yield 3 free). What setting most likely needs adjustment?
- A) Priority number
- B) Discount Qty Step and/or Maximum Qty Discount is Applied To
- C) Uses per Coupon
- D) Websites field

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** The Discount Qty Step defines how the "X get Y" ratio repeats, and Maximum Qty Discount is Applied To caps how many total units can receive the discount — both need to be configured to allow multiple repetitions.

**Exam Trap:** Leaving "Maximum Qty Discount is Applied To" at its default (often 0/1) silently caps the reward, so the buy-X-get-Y ratio never repeats no matter how many sets are purchased.

</details>

**Q10.** Two Catalog Price Rules apply to the same product, neither has "Discard subsequent rules" checked: Rule 1 (priority 1) gives 10% off, Rule 2 (priority 2) gives another 10% off. What is the resulting total discount?
- A) Exactly 20% off
- B) 19% off (0.9 × 0.9 = 0.81)
- C) Only the higher-priority rule applies, so 10% off
- D) The rules cancel each other out

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Catalog Price Rules stack multiplicatively in priority order when neither discards subsequent rules — 10% then another 10% compounds to 19% total off, not a flat 20%.

**Exam Trap:** Stacked percentage rules compound against the running price, so two 10% rules never equal a flat 20% — the second percentage is taken on the already-reduced amount.

</details>

**Q11.** A Cart Price Rule sets Free Shipping to "For matching items only." The cart contains one item that matches the rule's conditions and one item that does not. What is the shipping outcome?
- A) The entire shipment ships free
- B) Only the matching item's portion is free; the non-matching item is still charged shipping (per the applicable shipping calculation)
- C) Neither item ships free
- D) The rule automatically upgrades to "For shipment with matching items"

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** "For matching items only" scopes the free shipping benefit to just the qualifying line items, unlike "For shipment with matching items," which frees the whole shipment.

**Exam Trap:** The two Free Shipping scopes produce different totals on a mixed cart — choosing "For shipment with matching items" would have made the entire order ship free once a single qualifying item was present.

</details>

**Q12.** A merchant configures a Store Label on a Cart Price Rule for the English store view, but leaves the German store view's Store Label field blank. What displays to German-store customers when the rule applies?
- A) Nothing displays; the discount is silently applied with no label
- B) The internal Rule Name displays as a fallback
- C) The English store's label displays instead
- D) An error appears in the cart

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** If a Store Label is left blank for a given store view, Commerce falls back to displaying the rule's internal Rule Name.

**Exam Trap:** The fallback is the internal Rule Name, not the label from another store view — so an internal-only name meant for admins can accidentally appear to customers.

</details>

**Q13.** A Cart Price Rule Condition is set to "Category is X," but the rule never triggers even though products from category X's *subcategory* are in the cart. What's the likely issue?
- A) The condition only matches products directly assigned to category X itself — subcategory-only products won't satisfy it unless the condition or category assignment accounts for that
- B) Category conditions are not supported in Cart Price Rules
- C) The rule needs "Discard subsequent rules" enabled
- D) Category conditions only work with Catalog Price Rules, never Cart Price Rules

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** The Category condition checks direct category assignment on the product, not automatic inheritance from anchor/subcategory relationships — a common scenario-based trap.

**Exam Trap:** Anchor categories aggregate products for *catalog browsing*, but a promotion's Category condition still matches only the category IDs directly assigned to the product — subcategory-only products won't satisfy a parent-category condition.

</details>

**Q14.** A store has "Apply Tax After Discount" configured. How does this affect the final price a customer sees compared to applying tax before the discount?
- A) It has no effect on the final total, only on reporting
- B) Tax is calculated on the already-discounted amount, which generally results in a lower tax amount than taxing the pre-discount price
- C) It always increases the total the customer pays
- D) This setting only applies to Catalog Price Rules, not Cart Price Rules

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** When tax is applied after the discount, tax is computed on the reduced price, typically yielding a lower tax charge than if tax were calculated on the original, pre-discount amount.

**Exam Trap:** Tax-after-discount is a store-level Tax setting (Sales > Tax), not a property of any promotion — a "final price" question can hinge on this config even when the promotion is set up correctly.

</details>

---

### Set 3 — Gap Coverage: Coupon Limits, Priority, Conditions & Action Types

**Q23.** A merchant wants a single shared coupon code (SAVE20) usable a maximum of 500 times total across all customers, but only once per logged-in customer. Which two fields enforce this?
- A) Uses per Coupon = 1 and Uses per Customer = 500
- B) Uses per Coupon = 500 and Uses per Customer = 1
- C) Priority = 500 and Uses per Customer = 1
- D) Coupon Qty = 500 and Uses per Coupon = 1

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** "Uses per Coupon" caps total redemptions of that code across everyone (500), while "Uses per Customer" caps redemptions per logged-in customer (1). Note: Uses per Customer does not apply to guests, since guests can't be reliably tracked.

**Exam Trap:** "Uses per Customer" is enforced only for logged-in accounts — guests aren't tracked, so the once-per-customer cap can be bypassed by checking out as a guest.

</details>

**Q24.** A Cart Price Rule is created with the Coupon field set to "No Coupon," Active = Yes, and no Conditions entered. Which carts does it apply to?
- A) No carts, because a coupon is required to trigger any rule
- B) Only carts where the customer manually opts in
- C) All carts matching the rule's Websites, Customer Groups, and date window — it applies automatically
- D) Only the first cart of each day

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** A No-Coupon rule with empty Conditions applies automatically to every cart that matches the Websites/Customer Groups/date scope. Leaving Conditions blank means "match all," so the discount auto-applies with no code needed.

**Exam Trap:** Empty Conditions means "apply to everything," not "apply to nothing" — a No-Coupon rule with no conditions silently discounts every qualifying cart, which can be an unintended giveaway.

</details>

**Q25.** Two Cart Price Rules both match a cart. Rule X has Priority = 20, Rule Y has Priority = 5. Neither discards subsequent rules. Which is evaluated first?
- A) Rule X, because a higher number means higher priority
- B) Rule Y, because a lower priority number is evaluated first
- C) Whichever was created most recently
- D) Both are evaluated simultaneously with no defined order

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Lower priority numbers are evaluated first. Rule Y (5) processes before Rule X (20). Since neither discards subsequent rules, both apply, but order matters because each rule works against the running subtotal.

**Exam Trap:** Lower number = higher priority (evaluated first) — the intuitive "higher number wins" assumption is backwards in Commerce rule priority.

</details>

**Q26.** A merchant configures a "Buy 2, Get 1 Free" rule and wants the free-item benefit to repeat, but never discount more than 6 units total in a single cart. Which combination of Actions-tab fields is required?
- A) Discount Amount = 6 and Discard subsequent rules = Yes
- B) Discount Qty Step defines the buy-X/get-Y ratio; Maximum Qty Discount is Applied To = 6 caps the discounted units
- C) Uses per Coupon = 6 only
- D) Free Shipping = For matching items only

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Discount Qty Step sets the repeating trigger/reward ratio (buy 2 get 1), and "Maximum Qty Discount is Applied To" caps the total units that can receive the discount at 6. Coupon usage limits are unrelated to per-unit discount caps.

**Exam Trap:** "Maximum Qty Discount is Applied To" limits discounted *units within a cart*, whereas Uses per Coupon limits *redemptions of the code* — confusing the two leads to the wrong cap.

</details>

**Q27.** A merchant wants a Catalog Price Rule that sets a specific product's price to a flat $19.99 regardless of its normal price, rather than a percentage or subtractive discount. Which Apply action is correct?
- A) Percent of Product's Price
- B) Fixed Amount Discount
- C) Fixed Price
- D) Buy X Get Y Free

<details><summary>Answer & Explanation</summary>

**Answer:** C — Fixed Price

**Explanation:** "Fixed Price" overrides the product's price to the exact value entered ($19.99). "Fixed Amount Discount" would subtract $19.99 from the price instead. Buy X Get Y and Free Shipping are cart-only actions and don't exist on Catalog Price Rules.

**Exam Trap:** "Fixed Price" sets the price *to* the value; "Fixed Amount Discount" subtracts the value *from* the price — mixing them up turns a $19.99 target price into a $19.99 markdown.

</details>

**Q28.** A merchant needs to email 10,000 customers a unique, single-use code each. In the Cart Price Rule Coupon field, which option and follow-up step is correct?
- A) No Coupon, then export the cart
- B) Specific Coupon with one shared code
- C) Auto Generate, then use the Manage Coupon Codes / Generate section to create the batch of unique codes
- D) Auto Generate is only for Catalog Price Rules

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Setting Coupon = Auto Generate enables generating a batch of unique codes (Coupon Qty) that can be exported and emailed individually. A Specific Coupon is one shared code, which doesn't meet the "unique per customer" requirement.

**Exam Trap:** Catalog Price Rules have no Coupon field at all, so "Auto Generate" only exists on Cart Price Rules — unique-code campaigns are inherently a cart-rule mechanic.

</details>

**Q29.** A rule's Conditions use "Total Items Quantity is greater than 4," but a customer with exactly 4 items reports the discount doesn't apply. Why is this expected behavior?
- A) The condition is misconfigured and should never trigger
- B) "Greater than 4" excludes 4 itself — the customer needs 5+ items; "equals or greater than 4" would be needed to include 4
- C) Item quantity conditions only count distinct SKUs, not units
- D) The rule requires a coupon code the customer didn't enter

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** "Greater than 4" is strictly greater, so 4 items do not qualify — the customer needs 5 or more. To include exactly 4, the operator must be "equals or greater than." This is a classic off-by-one condition trap.

**Exam Trap:** "is greater than" is exclusive of the value itself; use "equals or greater than" whenever the threshold quantity should also qualify.

</details>

---

### Set 4 — Gap Coverage: Advanced Rule Mechanics & Loyalty Tools

**Q30.** A merchant wants a rule that only activates when the cart subtotal exceeds $100, but the percentage discount should apply **only to items in the Clearance category**. How is this configured on a single Cart Price Rule?
- A) Put both conditions on the Conditions tab
- B) Set the subtotal test on the Conditions tab, and add the Clearance category test under the Actions tab's "Apply the rule only to cart items matching the following conditions"
- C) It requires two separate rules
- D) Use the Labels tab

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** The Conditions tab gates whether the whole cart qualifies (subtotal > $100), while the Actions-tab item conditions narrow which line items actually receive the discount (Clearance category). A single rule handles both.

**Exam Trap:** A Cart Price Rule has TWO condition sets — the Conditions tab (does the cart qualify?) and the Actions-tab item conditions (which items get discounted?). Putting the item filter on the Conditions tab would wrongly require the *whole* cart to be Clearance.

</details>

**Q31.** A percentage Cart Price Rule discount is coming out larger than the merchant expected on the merchandise alone. Which setting most likely explains the difference?
- A) Discard subsequent rules
- B) "Apply to Shipping Amount" is enabled, so the percentage also discounts the shipping charge
- C) Uses per Coupon
- D) Priority number

<details><summary>Answer & Explanation</summary>

**Answer:** B — "Apply to Shipping Amount" is enabled

**Explanation:** With "Apply to Shipping Amount" checked on a percent-of-price action, the discount extends to the shipping charge as well as merchandise, producing a larger total discount than a merchandise-only calculation.

**Exam Trap:** A discount that seems "too big" often means it's also being applied to shipping via "Apply to Shipping Amount" — a setting separate from the discount percentage itself.

</details>

**Q32.** A merchant wants to sell a stored-value product that a customer buys and a recipient later redeems at checkout. Which feature is this, and which edition provides it?
- A) Store Credit; both editions
- B) Gift Cards; Adobe Commerce
- C) Cart Price Rule; both editions
- D) Reward Points; Magento Open Source

<details><summary>Answer & Explanation</summary>

**Answer:** B — Gift Cards; Adobe Commerce

**Explanation:** Gift Cards are a purchasable stored-value product type (virtual/physical/combined) redeemable at checkout, available in Adobe Commerce. Store Credit is a balance the merchant grants (not purchased), and both Gift Cards and Store Credit are Commerce-only.

**Exam Trap:** A Gift Card is a *product the shopper buys*; Store Credit is a *balance the merchant grants* (often from a refund). Both are Adobe Commerce–only — don't map either to Open Source.

</details>

---

## Quick-Reference Exam Traps — Section 2

1. **Special Price applies before Catalog Price Rules**, not instead of them — they stack (e.g., $100 → $90 Special → $81 after 10% Catalog Rule).
2. **Catalog Price Rules require reindexing** (Catalog Rule Price indexer) to reflect on the storefront — the single most common troubleshooting root cause.
3. **Customer Groups field controls WHO is eligible**, separate from whether a coupon code itself is valid — a common guest-checkout failure point is "NOT LOGGED IN" missing from that list.
4. **"Discard subsequent rules"** stops lower-priority rules from evaluating once a higher-priority rule successfully matches — it does not mean the flagged rule itself is skipped.
5. **Catalog Price Rules and Cart Price Rules are not mutually exclusive** — both can apply to the same order, stacking on top of each other.
6. **Cart Price Rules process against the running subtotal**, not the original — rule order changes the final outcome.
7. **Websites field scopes rule eligibility per storefront** — a rule missing a website simply won't evaluate there, even with correct customer groups.
8. **Only one coupon code can be active per cart by default** — applying a second replaces the first rather than stacking.
9. **Multiple Catalog Price Rules stack multiplicatively**, not additively, when neither discards subsequent rules (two 10% rules = 19% off total, not 20%).
10. **Free Shipping has two distinct scopes**: "For matching items only" vs. "For shipment with matching items" — these produce different totals when the cart is mixed.
11. **Category conditions check direct product assignment**, not automatic subcategory inheritance — a rule can silently fail to trigger if products only live in a subcategory.
12. **Tax-before-vs-after-discount** is a store-level Tax configuration setting, not a promotion setting, but it directly affects the "final price" a scenario question expects you to calculate.
13. **Store Labels are per-store-view** and fall back to the internal Rule Name if left blank for a given view.
14. **A Cart Price Rule has two condition sets** — the Conditions tab (does the whole cart qualify?) and the Actions-tab item conditions (which specific items get discounted?). They are read separately.
15. **"Apply to Shipping Amount"** extends a percentage discount to the shipping charge — a common reason a discount total looks larger than the merchandise-only expectation.
16. **Gift Cards, Reward Points, and Store Credit are Adobe Commerce–only** — a Gift Card is a purchasable stored-value product; Store Credit is a merchant-granted balance. Both are distinct from Cart/Catalog Price Rules.
