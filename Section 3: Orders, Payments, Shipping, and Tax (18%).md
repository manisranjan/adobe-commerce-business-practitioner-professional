# Adobe Commerce Business Practitioner Professional (AD0-E723)
## Section 3: Orders, Payments, Shipping, and Tax (18%) — Notes & Practice MCQs

---

## PART A: STUDY NOTES

### 3.1 Order State/Status and Allowed Actions

**State vs. Status — the core distinction**
- **Order State** = a fixed, system-level value (cannot be renamed or removed): `new`, `pending_payment`, `processing`, `complete`, `closed`, `canceled`, `holded`, `payment_review`, `fraud`
- **Order Status** = a customizable, human-readable label mapped to a State. Multiple Statuses can map to the same State, but only one Status is the **default** for each State
- Admin path: **Stores > Settings > Order Status** — create custom statuses, then assign them to a State

**Typical state progression**
1. **New** — order placed, nothing invoiced/shipped yet
2. **Pending Payment** — used when payment confirmation is outstanding (e.g., bank transfer awaiting funds, PayPal awaiting IPN callback)
3. **Processing** — at least one invoice OR one shipment has been created, but not both are fully complete
4. **Complete** — fully invoiced AND fully shipped
5. **Closed** — fully refunded via credit memo
6. **Canceled** — order canceled before any invoice existed
7. **Holded** — temporarily frozen; reverts to its prior state on Unhold
8. **Payment Review** / **Fraud** — used by certain payment gateways pending manual fraud review

**Allowed Actions by Context (exam-critical)**

| Action | When Available |
|---|---|
| **Cancel** | Only if the order has **NOT yet been invoiced**. Once an invoice exists, the Cancel button disappears — a Credit Memo must be used instead to reverse the order |
| **Hold** | Available as long as the order isn't already Complete, Closed, or Canceled. Holding freezes the order (blocks invoice/shipment actions) until released |
| **Unhold** | Reverts the order back to its state prior to being placed on hold |
| **Invoice** | Available once payment has been authorized/received (for online methods) or manually for offline methods; can be partial (creates partial invoice, order stays Processing) or full |
| **Ship** | Available once the order is invoiced (for most payment flows) or, for methods like Cash on Delivery, may be shippable before invoicing since payment is collected at delivery |
| **Credit Memo (Refund)** | Only available after an invoice exists — you cannot refund an order that was never invoiced |
| **Reorder** | Creates a new cart pre-filled with the same items; available depending on store configuration, regardless of the original order's current state in most cases |

**Exam trap:** A holded order shows no Invoice/Ship/Cancel buttons until it's unheld — Hold acts as a full freeze on order-modifying actions.

---

### 3.2 Online vs. Offline Payment Methods

**Offline Payment Methods** (Check/Money Order, Bank Transfer, Cash on Delivery, Purchase Order)
- No real-time authorization or capture — Commerce has no visibility into whether payment was actually received
- Order typically lands in **Pending** status (state: `new`)
- **Invoice must be created manually** by an admin once payment is confirmed to have arrived outside the system
- **Refunds are processed offline** — the admin manually refunds the customer outside Commerce, then creates an **Offline Credit Memo** in Admin purely to record the reversal (no gateway call is made)
- No Void action — there's nothing to void since no online authorization occurred

**Online Payment Methods** (Credit Card via gateway, PayPal, Braintree, Adyen, Authorize.net, etc.)
- Controlled by the **Payment Action** setting (Stores > Configuration > Sales > Payment Methods > [method] > Payment Action):
  - **Authorize Only** — places a hold on funds at order time; order sits in Processing (or Pending Payment, depending on gateway) until the merchant manually **Captures** the payment (which then creates the invoice)
  - **Authorize and Capture** — funds are captured immediately at order placement; an invoice is generated automatically, order moves to Processing right away
- **Void** action is available only on an **authorized-but-not-yet-captured** online transaction — it releases the hold without charging the customer
- **Refunds are processed online** — creating a Credit Memo in Admin triggers an actual API refund call to the payment gateway, returning funds to the customer automatically

**Quick comparison table**

| | Offline Methods | Online Methods |
|---|---|---|
| Real-time authorization? | No | Yes (via gateway) |
| Invoice creation | Manual, admin-triggered | Automatic (if Authorize and Capture) or manual Capture (if Authorize Only) |
| Refund mechanism | Offline Credit Memo (no gateway call) | Online Credit Memo (triggers gateway refund API) |
| Void available? | No | Yes, if payment was only authorized (not yet captured) |
| Common order state after placement | Pending / New | Processing (if auto-captured) or Pending Payment (if awaiting confirmation) |

**Exam trap:** A scenario describing "the admin manually enters that payment was received before creating an invoice" is always an **offline** method — no online gateway is involved.

---

### 3.3 Configuring Shipping and Tax Behavior

#### Shipping

**Built-in Shipping Methods**
- **Flat Rate** — one fixed rate per item or per order, configured directly in Admin
- **Table Rates** — CSV-imported rate table based on weight, quantity, order subtotal, or destination
- **Free Shipping** — a dedicated method with a configurable minimum order amount threshold, separate from Free Shipping granted via a Cart Price Rule action
- **Carrier integrations** (UPS, FedEx, USPS, DHL) — real-time rate quotes via carrier APIs; require account credentials configured in Admin
- **In-Store Pickup / Click & Collect** — available via MSI-integrated pickup locations tied to Sources

**Key configuration points**
- **Shipping Origin** (Stores > Configuration > Sales > Shipping Settings > Origin) — the "ship from" address used for rate calculations (especially relevant for carrier and tax nexus calculations)
- Per-website/store enabling — each shipping method can be enabled/disabled and rate-configured independently per website
- **Allowed Countries** — shipping methods can be restricted to specific destination countries
- With **MSI**, an order can be split into multiple shipments if line items are fulfilled from different Sources — each shipment can have its own tracking number

**Free Shipping — Two Distinct Paths**
1. The dedicated **Free Shipping** shipping method (a carrier option a customer selects, gated by a minimum subtotal)
2. **Free Shipping as a Cart Price Rule action** — overrides the cost of the shipping method actually selected, either for matching items only or the whole shipment (see Section 2 notes)

#### Tax

**Tax Classes**
- **Product Tax Class** — assigned per product (e.g., Taxable Goods, Digital Goods, Shipping)
- **Customer Tax Class** — assigned per customer group (e.g., Retail Customer, Tax Exempt)

**Tax Rules**
- Combine a **Tax Rate** (defined by Tax Zone: country/state/zip range) + **Product Tax Class** + **Customer Tax Class**
- Multiple Tax Rules can apply to the same combination (e.g., stacking a state tax + a county tax rule) — Commerce sums applicable rates

**Tax Calculation Based On** (Stores > Configuration > Sales > Tax > Calculation Settings)
- Options: **Shipping Origin**, **Shipping Address**, **Billing Address**, or **Customer's Default Address**
- This determines WHICH address's zone/rate is used to look up the applicable Tax Rule

**Catalog & Display Price Settings**
- **Catalog Prices**: Excluding Tax or Including Tax — determines how prices are *stored/entered* in the catalog
- Separate **Display** settings exist for: Product Prices (catalog pages), Shopping Cart, Sales/Order documents — each can independently show price Excluding Tax, Including Tax, or Both
- **Apply Customer Tax** — resolves which Customer Tax Class rate applies based on the shopper's assigned customer group

**Fixed Product Tax (FPT) / Weee**
- Used for flat per-unit tax/fee amounts unrelated to percentage-based tax rules (e.g., environmental fees, battery deposit fees)
- Configured on the product itself as a fixed additional charge, can be taxable or non-taxable itself depending on settings

**Discount and Tax Interaction**
- **Apply Discount on Prices**: Including Tax or Excluding Tax — determines the base amount a percentage discount is calculated against
- **Apply Tax After Discount**: Yes/No — determines whether tax is computed before or after the promotional discount is applied to the price (directly affects "final price" calculation scenarios)

**Cross-Border / VAT Considerations**
- **VAT ID Validation** — for B2B EU scenarios, validates a customer's VAT number against the VIES database; can be configured to grant VAT-exempt pricing to validated intra-EU B2B customers
- Tax Zones can be defined narrowly (by ZIP/postcode range) or broadly (whole country) depending on jurisdictional needs

---

#### Tax — Worked Example (How the Pieces Actually Combine)

Think of tax calculation as a **lookup chain**: Commerce takes the order's relevant address, finds which Tax Zone it falls into, then finds a Tax Rule that matches that Zone + the product's Tax Class + the customer's Tax Class, and applies the resulting rate.

**Step-by-step:**

1. **Determine the relevant address.** Controlled by "Tax Calculation Based On" — say it's set to **Shipping Address**, and the customer ships to California.
2. **Match the address to a Tax Zone.** A Tax Zone/Rate record exists for "US – CA," carrying an 8.25% rate.
3. **Match the Product Tax Class.** The product being purchased is assigned the "Taxable Goods" Product Tax Class.
4. **Match the Customer Tax Class.** The customer belongs to the "Retail Customer" group, mapped to the "Retail Customer" Customer Tax Class.
5. **Find the Tax Rule** that ties these three together: Tax Rate (US-CA, 8.25%) + Product Tax Class (Taxable Goods) + Customer Tax Class (Retail Customer) → this Rule fires and applies 8.25% tax to that line item.

**If the customer instead belongs to a "Wholesale – Tax Exempt" group** mapped to a different Customer Tax Class, and NO Tax Rule exists connecting that Customer Tax Class to the CA rate, the item is simply not taxed for that customer — tax exemption is achieved by the *absence* of a matching rule, not a special "exempt" flag.

**Stacking rates (e.g., state + county tax):** Some jurisdictions require two rates on the same transaction (e.g., a 6% state rate plus a 1.5% county rate). This is handled by creating **two separate Tax Rates** for the same Zone and attaching **both to the same Tax Rule** (a Tax Rule can reference multiple Tax Rates) — Commerce sums them, so the customer sees one combined 7.5% line, or a separate line per rate depending on "Display Full Tax Summary" settings.

#### Catalog Prices: Including vs. Excluding Tax — Why This Matters

- **"Catalog Prices" = Excluding Tax** (most common for B2C in the US where price tags exclude sales tax): the price entered on the product (e.g., $100) is treated as the pre-tax price; tax is calculated and added on top at checkout.
- **"Catalog Prices" = Including Tax** (common in the EU/UK where displayed prices are tax-inclusive by law): the price entered on the product (e.g., $100) is treated as ALREADY containing tax; Commerce backs the tax amount out for reporting/tax remittance purposes rather than adding it on top.
- This setting fundamentally changes what a merchant should type into the product's Price field — getting it backwards either double-charges tax or undercharges it.

#### Display Settings Are Independent of Calculation Settings

A common point of confusion: **how tax is calculated** (Tax Rules, Tax Calculation Based On) is entirely separate from **how the price is displayed** to the shopper. You can calculate tax correctly but still configure the storefront to display:
- Price **Excluding Tax** only
- Price **Including Tax** only
- **Both** ("$100 (Excl. Tax: $92.59)")

These display settings exist independently for: **Product Prices** (catalog/PDP), **Shopping Cart**, and **Sales/Order documents** (invoices, credit memos) — a merchant could show tax-excluded prices on the catalog but tax-included totals in the cart, for example.

---

## PART B: PRACTICE MCQs

**Q1.** An order has had a partial shipment created but no invoice has been generated yet. What order state is it in?

- A) Complete
- B) Processing
- C) New
- D) Closed

**Answer: B — Processing.** Processing is used whenever at least one invoice OR shipment exists, but the order isn't fully invoiced and shipped yet.

**Q2.** A merchant wants to cancel an order, but the Cancel button is missing from the order view. What is the most likely reason?

- A) The order has already been invoiced
- B) The order is using an offline payment method
- C) The customer is a guest
- D) The order total is above a configured threshold

**Answer: A.** Once an invoice exists, Cancel is no longer available — a Credit Memo must be used instead to reverse the order.

**Q3.** An order is placed on Hold. Which actions are available while it remains on hold?

- A) Invoice and Ship remain fully available
- B) Only Cancel remains available
- C) All order-modifying actions (Invoice, Ship, Cancel) are frozen until Unhold is used
- D) Only Credit Memo remains available

**Answer: C.** Hold freezes order-modifying actions entirely until the order is released via Unhold.

**Q4.** A customer pays via Bank Transfer (an offline method). What must happen before an invoice can be created for this order?

- A) The payment gateway automatically captures funds
- B) An admin manually creates the invoice after confirming payment was received outside the system
- C) The order auto-invoices after 24 hours
- D) The customer must confirm receipt via email link

**Answer: B.** Offline methods have no gateway visibility — invoicing is always a manual admin action once payment is confirmed to have arrived.

**Q5.** A credit card payment method is configured with Payment Action = "Authorize Only." What happens at the moment the order is placed?

- A) Funds are immediately captured and an invoice is auto-created
- B) A hold is placed on funds, but no invoice is created until the merchant manually captures the payment
- C) No authorization occurs at all until shipment
- D) The order is automatically canceled if not captured within 1 hour

**Answer: B.**

**Q6.** Which action is only available on an online payment method's authorization, and never applies to offline payment methods?

- A) Credit Memo
- B) Invoice
- C) Void
- D) Reorder

**Answer: C — Void.** Void releases an online authorization hold before capture; offline methods have no online authorization to void.

**Q7.** A merchant issues a refund for an order paid via a credit card processed through an online gateway. What happens when the Credit Memo is created in Admin?

- A) Nothing — the merchant must separately log into the gateway to issue the refund
- B) An API call is automatically triggered to the payment gateway, returning funds to the customer's card
- C) The refund is only recorded internally with no actual money movement
- D) The order automatically cancels

**Answer: B.**

**Q8.** A scenario describes an admin manually confirming payment was received via check before creating an invoice. Which type of payment method does this describe?

- A) Online payment method
- B) Offline payment method
- C) A method using Authorize and Capture
- D) A method requiring Void first

**Answer: B — Offline.** No gateway involvement, manual confirmation and manual invoicing are hallmarks of offline methods.

**Q9.** A merchant needs shipping rates that vary based on order weight and destination, imported via a spreadsheet. Which shipping method fits?

- A) Flat Rate
- B) Free Shipping
- C) Table Rates
- D) In-Store Pickup

**Answer: C — Table Rates.** CSV-imported rate table based on weight/qty/subtotal/destination.

**Q10.** Which setting determines WHICH address (billing, shipping, origin, or customer default) is used to look up the applicable tax rate for an order?

- A) Catalog Price Scope
- B) Tax Calculation Based On
- C) Apply Discount on Prices
- D) Shipping Origin (Sales settings)

**Answer: B — Tax Calculation Based On.**

**Q11.** A merchant wants a flat, non-percentage environmental fee applied to certain electronics products regardless of the standard tax rate. What feature should be used?

- A) Tax Rule with a Fixed Zone
- B) Fixed Product Tax (FPT/Weee)
- C) Customer Tax Class override
- D) Catalog Price Rule

**Answer: B — Fixed Product Tax.**

**Q12.** A store has "Apply Tax After Discount" = Yes. How does this affect the calculated tax compared to applying tax before the discount?

- A) No difference in the final tax amount
- B) Tax is calculated on the already-discounted price, generally resulting in a lower tax amount
- C) Tax is calculated twice, once before and once after
- D) This setting only affects shipping tax, not product tax

**Answer: B.**

**Q13.** A B2B merchant serving EU customers wants validated intra-EU business customers to be exempt from VAT automatically. What feature supports this?

- A) Fixed Product Tax
- B) VAT ID Validation (via VIES)
- C) Customer Tax Class manually set per customer
- D) Tax Zones restricted by ZIP code

**Answer: B.**

**Q14.** A retailer's inventory is split across two MSI Sources. A customer orders two items, each fulfilled from a different Source. What can happen to the shipment(s) for this order?

- A) The order can only ever be one shipment regardless of Source split
- B) The order can be split into multiple shipments, one per Source, each with its own tracking number
- C) MSI blocks orders from being placed if items span multiple Sources
- D) The second item is automatically backordered

**Answer: B.**

---

### Set 2 — Tax Deep Dive

**Q15.** A Tax Rule connects a Tax Rate for "US–CA" with the Product Tax Class "Taxable Goods" and Customer Tax Class "Retail Customer." A customer in the "Wholesale – Tax Exempt" group (mapped to a different Customer Tax Class) buys the same product. No Tax Rule exists linking that Customer Tax Class to the CA rate. What happens?

- A) The system throws a configuration error and blocks checkout
- B) The item is taxed anyway using the default Tax Rule
- C) The item is simply not taxed for this customer — exemption happens through the absence of a matching rule, not a special flag
- D) The system automatically creates a new Tax Rule on the fly

**Answer: C.** Tax exemption in Commerce is achieved structurally — no matching Tax Rule means no tax is applied — not through an explicit "exempt" toggle on the customer.

**Q16.** A jurisdiction requires both a 6% state tax and a 1.5% county tax to appear on the same order. How is this configured?

- A) Two separate Tax Rules must be created and applied to different products
- B) One Tax Rule can reference multiple Tax Rates (e.g., both the state and county rate), and Commerce sums them
- C) This scenario is not supported; only one rate can apply per Tax Rule
- D) The county tax must be manually added as a Fixed Product Tax

**Answer: B.**

**Q17.** "Tax Calculation Based On" is set to Shipping Address. A customer's billing address is in Texas, but they are shipping the order to a friend in New York. Which state's tax zone/rate is used?

- A) Texas, because billing address always takes priority
- B) New York, because the setting is explicitly configured to use the shipping address
- C) Whichever state has the higher tax rate
- D) The Shipping Origin state, since that overrides both billing and shipping

**Answer: B.**

**Q18.** A merchant based in the EU sets "Catalog Prices" = Including Tax, and enters $100 as a product's price. What does Commerce treat this $100 as?

- A) A pre-tax price, with tax added on top at checkout, resulting in a higher total
- B) A tax-inclusive price — the $100 already contains the tax amount, which Commerce backs out for reporting/remittance
- C) An error, since Including Tax requires a $0 price entry
- D) The price is automatically doubled to account for tax

**Answer: B.**

**Q19.** A merchant wants the Product Detail Page to show prices excluding tax, but the Shopping Cart to show the total including tax. Is this supported, and how?

- A) Not supported — display settings are always uniform across the whole storefront
- B) Supported — Display settings for Product Prices, Shopping Cart, and Sales documents are independently configurable
- C) Supported, but only by creating two separate storefronts
- D) Supported, but only if Catalog Prices = Including Tax

**Answer: B.** Calculation settings and display settings are separate; display can be configured independently per area (catalog, cart, sales documents).

**Q20.** A store has "Apply Discount on Prices" = Excluding Tax, and "Apply Tax After Discount" = No. A $100 product (pre-tax) has a 10% Cart Price Rule discount and an 8% tax rate. What is the calculation order?

- A) Tax is calculated on $100 first ($108), then the 10% discount is applied to $108 ($97.20)
- B) The 10% discount is applied to the $100 pre-tax price first ($90), then 8% tax is calculated on the original $100, not the discounted amount, per "Apply Tax After Discount" = No
- C) Discount and tax are applied simultaneously with no defined order
- D) Only one of the two settings can be active at a time

**Answer: B.** "Apply Tax After Discount" = No means tax is calculated on the pre-discount (original) price, even though the discount itself is separately applied to the excluding-tax base price — these are two distinct configuration axes that both need to be read carefully in a scenario.

**Q21.** A merchant sells a product with a $5 flat environmental fee that should apply regardless of the customer's percentage-based tax rate. Which tax feature fits, and how does it differ structurally from a standard Tax Rule?

- A) Tax Rule — it works identically to percentage-based tax
- B) Fixed Product Tax (FPT/Weee) — a flat per-unit charge, structurally separate from the percentage-based Tax Rule engine
- C) Customer Tax Class override — applied per customer group
- D) Catalog Price Rule — reduces price by a fixed amount

**Answer: B.**

**Q22.** A B2B merchant wants intra-EU business customers with a validated VAT number to automatically be exempt from VAT charges. What must be configured, and against what external system is the number checked?

- A) VAT ID Validation, checked against the VIES database
- B) Fixed Product Tax, checked against local tax authority APIs
- C) A manual Customer Tax Class assignment with no external validation
- D) Tax Zones restricted by ZIP code, with no external validation

**Answer: A.**

---

## Quick-Reference Exam Traps — Section 3

1. **Cancel disappears once an invoice exists** — a Credit Memo is the only path to reverse an invoiced order.
2. **Hold freezes ALL order-modifying actions**, not just one — Invoice, Ship, and Cancel are all blocked until Unhold.
3. **Offline methods always require manual invoicing** — there is no gateway confirming payment, so Commerce can't auto-invoice.
4. **Void only applies to online, authorized-but-not-captured transactions** — never available for offline methods, and never available after capture has already occurred.
5. **Refunds differ mechanically**: offline = manual/no gateway call; online = triggers an actual gateway refund API call.
6. **Payment Action (Authorize Only vs. Authorize and Capture)** determines whether invoicing happens automatically at order placement or requires a manual Capture step later.
7. **Tax Calculation Based On** controls which address's zone drives the tax rate lookup — this is a distinct setting from Shipping Origin.
8. **Apply Tax After Discount** changes the actual tax amount charged, not just display — critical for "final price" scenario math.
9. **Fixed Product Tax (FPT)** is a flat per-unit charge, structurally different from percentage-based Tax Rules.
10. **MSI can split a single order into multiple shipments** when line items are fulfilled from different Sources.
11. **Tax exemption is structural, not a flag** — a customer is "exempt" simply because no Tax Rule connects their Customer Tax Class to a given Tax Rate/Product Tax Class combination, not because of an explicit exempt switch.
12. **One Tax Rule can carry multiple Tax Rates** — this is how stacked jurisdictional taxes (state + county, etc.) are summed onto a single order line.
13. **Catalog Prices setting (Including vs. Excluding Tax) changes what the entered product price MEANS** — Including Tax treats the entered value as already tax-inclusive (tax is backed out), Excluding Tax treats it as pre-tax (tax is added on top). Mixing this up either double-charges or under-charges tax.
14. **Display settings are independent of calculation settings** — a store can calculate tax correctly while showing tax-excluded prices on the catalog and tax-included totals in the cart; these are separate configuration axes (Product Prices, Shopping Cart, Sales documents each configured on their own).
15. **"Apply Discount on Prices" and "Apply Tax After Discount" are two distinct settings** that both affect final price scenario math — don't assume one implies the other's behavior.
Section 3: Orders, Payments, Shipping, and Tax (18%)
