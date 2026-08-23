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

#### Catalog Price Rule — Actions Tab Detail
- **Apply** field options: **Percent of Product's Price** / **Fixed Amount Discount** / **Fixed Price**
- Note the naming difference from Cart Price Rules — Catalog rules don't have a "Buy X Get Y" or "Free Shipping" action; those are cart-only mechanics
- **Discard subsequent rules** works identically to Cart Price Rules — evaluated in priority order

#### One Coupon Code Per Cart (Default Behavior)
- Out of the box, Adobe Commerce only allows **one coupon code active in the cart at a time** — applying a second coupon replaces the first rather than stacking
- This is separate from the fact that multiple **no-coupon** Cart Price Rules and Catalog Price Rules can still all apply simultaneously alongside the one coupon-based rule

---

### 2.2 Explaining the Final Price

**Calculation order (full chain)**
1. Base Price (or Special Price if active and lower)
2. Tier Price if qty threshold met (wins if lower than Special Price)
3. Catalog Price Rules applied on top, in priority order → produces **displayed/PDP price**
4. Cart Price Rules applied at cart level, in priority order, on top of the PDP price

**Key traps**
- Catalog Price Rules and Cart Price Rules are **not mutually exclusive** — both can apply to the same order unless a rule specifically discards subsequent processing
- When multiple Cart Price Rules apply, each processes against the **current** running subtotal, not the original — so rule order changes the outcome (10% then $5 off ≠ $5 off then 10%)
- **Multiple Catalog Price Rules also stack multiplicatively in priority order** when neither discards subsequent rules — e.g., a 10% rule followed by a further 10% rule results in 19% total off (0.9 × 0.9 = 0.81), not a flat 20%
- **Tax and discount interaction**: whether tax is calculated on the pre-discount or post-discount amount depends on the store's Tax configuration (Sales > Tax > Calculation Settings > "Apply Discount on Prices" — Including Tax or Excluding Tax, plus "Apply Tax After Discount" setting). This affects the final total a customer sees, so a "final price" scenario question may hinge on this setting rather than the promotion rule itself
- **Free Shipping action nuance**: "For matching items only" grants free shipping just for the specific line items that matched the rule's conditions; "For shipment with matching items" grants free shipping for the ENTIRE shipment once any matching item is present — these produce different totals when the cart has a mix of qualifying and non-qualifying items

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

---

## PART B: PRACTICE MCQs

**Q1.** A merchant wants customers who buy a specific skincare set to automatically receive a free travel-size sample added to their cart. What should be configured?
- A) Catalog Price Rule with a Fixed Price action
- B) Cart Price Rule with "Buy X Get Y Free" action
- C) Special Price on the free item's product page
- D) Tier Pricing set to qty 1 at $0

**Answer: B.** Gift-with-purchase = Cart Price Rule, Buy X Get Y Free action targeting the specific SKU.

**Q2.** A product has a Base Price of $100, an active Special Price of $90, and a Catalog Price Rule giving 10% off all products in its category. What is the resulting displayed price?
- A) $100
- B) $70
- C) $81
- D) $90

**Answer: C — $81.** Special Price ($90) applies first, then the 10% Catalog Price Rule applies on top of that resulting price ($90 × 0.9 = $81).

**Q3.** A merchant creates and saves a new Catalog Price Rule, but the storefront still shows the old price hours later. What is the most likely cause?
- A) Coupon hasn't been distributed
- B) Catalog Rule Price indexer hasn't run since the rule was saved
- C) Catalog Price Rules only apply at checkout
- D) Rule needs "Discard subsequent rules" enabled to take effect

**Answer: B.**

**Q4.** A Cart Price Rule with a valid, active coupon code works for logged-in customers but returns "coupon invalid" for guest checkouts. Most likely misconfiguration?
- A) Coupon must be regenerated for guests
- B) Guest checkout never compatible with Cart Price Rules
- C) The rule's Customer Groups field likely doesn't include "NOT LOGGED IN"
- D) Websites field is missing the guest's website

**Answer: C.**

**Q5.** Two Cart Price Rules both match a customer's cart. Rule A has Priority = 1 and "Discard subsequent rules" = Yes. Rule B has Priority = 2. What happens?
- A) Both rules always apply together
- B) Only Rule A applies; Rule B is skipped
- C) Rule A never applies itself
- D) Whichever rule gives the larger discount applies

**Answer: B.** "Discard subsequent rules" on the higher-priority rule stops evaluation of lower-priority rules once it successfully applies.

**Q6.** A product has an active Catalog Price Rule (10% off) AND the customer has a valid coupon for a Cart Price Rule (further $5 off the cart). What happens at checkout?
- A) Only the Catalog Price Rule applies
- B) Both can apply — Catalog Price Rule reduces the displayed price first, then the Cart Price Rule discounts further at checkout
- C) Only the Cart Price Rule applies, overriding the Catalog rule
- D) Commerce blocks having both active simultaneously

**Answer: B.** Catalog and Cart Price Rules are not mutually exclusive; they stack unless one is set to discard subsequent processing.

**Q7.** A promotion works correctly on Store A but doesn't apply at all on Store B, even though both stores share the same customer groups. What is most likely misconfigured?
- A) Customer Groups missing a group from Store B
- B) Catalog Rule Price indexer needs to run separately for Store B
- C) The rule's Websites field doesn't include Store B's website
- D) Coupon codes must be regenerated per store

**Answer: C.**

---

### Set 2 — Advanced Configuration, Calculation & Troubleshooting Depth

**Q8.** A customer already has one coupon code applied to their cart and enters a second, different valid coupon code. What happens?
- A) Both discounts stack automatically
- B) The second coupon is rejected outright and cannot be entered
- C) The second coupon replaces the first — Commerce only allows one active coupon per cart by default
- D) The system picks whichever coupon gives the smaller discount

**Answer: C.** Adobe Commerce allows only one coupon-based Cart Price Rule active in the cart at a time out of the box; a new valid coupon replaces the previous one rather than stacking.

**Q9.** A merchant sets up a "Buy 3 Get 1 Free" Cart Price Rule but customers report only getting 1 free item no matter how many sets of 3 they buy (e.g., buying 9 should yield 3 free). What setting most likely needs adjustment?
- A) Priority number
- B) Discount Qty Step and/or Maximum Qty Discount is Applied To
- C) Uses per Coupon
- D) Websites field

**Answer: B.** The Discount Qty Step defines how the "X get Y" ratio repeats, and Maximum Qty Discount is Applied To caps how many total units can receive the discount — both need to be configured to allow multiple repetitions.

**Q10.** Two Catalog Price Rules apply to the same product, neither has "Discard subsequent rules" checked: Rule 1 (priority 1) gives 10% off, Rule 2 (priority 2) gives another 10% off. What is the resulting total discount?
- A) Exactly 20% off
- B) 19% off (0.9 × 0.9 = 0.81)
- C) Only the higher-priority rule applies, so 10% off
- D) The rules cancel each other out

**Answer: B.** Catalog Price Rules stack multiplicatively in priority order when neither discards subsequent rules — 10% then another 10% compounds to 19% total off, not a flat 20%.

**Q11.** A Cart Price Rule sets Free Shipping to "For matching items only." The cart contains one item that matches the rule's conditions and one item that does not. What is the shipping outcome?
- A) The entire shipment ships free
- B) Only the matching item's portion is free; the non-matching item is still charged shipping (per the applicable shipping calculation)
- C) Neither item ships free
- D) The rule automatically upgrades to "For shipment with matching items"

**Answer: B.** "For matching items only" scopes the free shipping benefit to just the qualifying line items, unlike "For shipment with matching items," which frees the whole shipment.

**Q12.** A merchant configures a Store Label on a Cart Price Rule for the English store view, but leaves the German store view's Store Label field blank. What displays to German-store customers when the rule applies?
- A) Nothing displays; the discount is silently applied with no label
- B) The internal Rule Name displays as a fallback
- C) The English store's label displays instead
- D) An error appears in the cart

**Answer: B.** If a Store Label is left blank for a given store view, Commerce falls back to displaying the rule's internal Rule Name.

**Q13.** A Cart Price Rule Condition is set to "Category is X," but the rule never triggers even though products from category X's *subcategory* are in the cart. What's the likely issue?
- A) The condition only matches products directly assigned to category X itself — subcategory-only products won't satisfy it unless the condition or category assignment accounts for that
- B) Category conditions are not supported in Cart Price Rules
- C) The rule needs "Discard subsequent rules" enabled
- D) Category conditions only work with Catalog Price Rules, never Cart Price Rules

**Answer: A.** The Category condition checks direct category assignment on the product, not automatic inheritance from anchor/subcategory relationships — a common scenario-based trap.

**Q14.** A store has "Apply Tax After Discount" configured. How does this affect the final price a customer sees compared to applying tax before the discount?
- A) It has no effect on the final total, only on reporting
- B) Tax is calculated on the already-discounted amount, which generally results in a lower tax amount than taxing the pre-discount price
- C) It always increases the total the customer pays
- D) This setting only applies to Catalog Price Rules, not Cart Price Rules

**Answer: B.** When tax is applied after the discount, tax is computed on the reduced price, typically yielding a lower tax charge than if tax were calculated on the original, pre-discount amount.

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
