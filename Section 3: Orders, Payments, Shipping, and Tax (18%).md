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

> **⚠️ Exam Trap:** Cancel is available only before an invoice exists; the instant an order is invoiced, the Cancel button disappears and a Credit Memo becomes the only way to reverse it.

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

> **⚠️ Exam Trap:** With Payment Action = Authorize Only no invoice is created at placement — invoicing happens only on manual Capture; Authorize and Capture invoices automatically up front.

**Quick comparison table**

| | Offline Methods | Online Methods |
|---|---|---|
| Real-time authorization? | No | Yes (via gateway) |
| Invoice creation | Manual, admin-triggered | Automatic (if Authorize and Capture) or manual Capture (if Authorize Only) |
| Refund mechanism | Offline Credit Memo (no gateway call) | Online Credit Memo (triggers gateway refund API) |
| Void available? | No | Yes, if payment was only authorized (not yet captured) |
| Common order state after placement | Pending / New | Processing (if auto-captured) or Pending Payment (if awaiting confirmation) |

> **⚠️ Exam Trap:** Only online methods support Void, and only offline methods refund without a gateway call — swapping the refund mechanism (offline = no API call, online = gateway refund API) is a classic distractor.

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

> **⚠️ Exam Trap:** "Tax Calculation Based On" only selects which address drives the rate lookup — it is not the Shipping Origin ship-from setting and does not itself define any tax rate.

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

> **⚠️ Exam Trap:** "Apply Discount on Prices" and "Apply Tax After Discount" are two independent settings — one sets the base the discount is calculated on, the other sets whether tax is figured on the pre- or post-discount amount.

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

> **⚠️ Exam Trap:** Tax exemption is structural — a customer is exempt only because no Tax Rule links their Customer Tax Class to the rate, not because of an "exempt" checkbox; and a single Tax Rule can sum multiple stacked rates.

#### Catalog Prices: Including vs. Excluding Tax — Why This Matters

- **"Catalog Prices" = Excluding Tax** (most common for B2C in the US where price tags exclude sales tax): the price entered on the product (e.g., $100) is treated as the pre-tax price; tax is calculated and added on top at checkout.
- **"Catalog Prices" = Including Tax** (common in the EU/UK where displayed prices are tax-inclusive by law): the price entered on the product (e.g., $100) is treated as ALREADY containing tax; Commerce backs the tax amount out for reporting/tax remittance purposes rather than adding it on top.
- This setting fundamentally changes what a merchant should type into the product's Price field — getting it backwards either double-charges tax or undercharges it.

> **⚠️ Exam Trap:** Catalog Prices = Including Tax means the entered price already contains tax (Commerce backs it out); Excluding Tax means tax is added on top at checkout — reversing them silently over- or under-charges.

#### Display Settings Are Independent of Calculation Settings

A common point of confusion: **how tax is calculated** (Tax Rules, Tax Calculation Based On) is entirely separate from **how the price is displayed** to the shopper. You can calculate tax correctly but still configure the storefront to display:
- Price **Excluding Tax** only
- Price **Including Tax** only
- **Both** ("$100 (Excl. Tax: $92.59)")

These display settings exist independently for: **Product Prices** (catalog/PDP), **Shopping Cart**, and **Sales/Order documents** (invoices, credit memos) — a merchant could show tax-excluded prices on the catalog but tax-included totals in the cart, for example.

---

### 3.4 Sales Documents, Order Emails & Gift Options

#### Sales Documents (the paper trail)
An order generates distinct documents, each a separate record:

| Document | Created when | Effect |
|---|---|---|
| **Order** | Checkout completes | The master record; not itself a financial document |
| **Invoice** | Payment is captured/recorded | Records the charge; can be partial or full |
| **Shipment** | Items are dispatched | Records fulfillment; supports tracking numbers; can be partial |
| **Credit Memo** | A refund is issued | Reverses charge (online = gateway refund; offline = record only); can restock items |

- Invoices, Shipments, and Credit Memos can each be **partial**, which is why an order can sit in **Processing** with mixed progress.
- A Credit Memo can be created **per invoice** and optionally **return items to stock** (restock checkbox).

#### Order/Transactional Emails
- Adobe Commerce sends **transactional (sales) emails** automatically at lifecycle events: **Order Confirmation, Invoice, Shipment, Credit Memo**, plus order comment/update emails.
- Configured under **Stores > Configuration > Sales > Sales Emails** — each email type can be enabled/disabled, assigned a template, and given sender identity and CC/BCC.
- Templates are managed in **Marketing > Communications > Email Templates**, and are **scoped per store view** for localization.
- Emails are dispatched via the **cron-driven queue**, not synchronously — a stalled cron is a common "customers aren't getting order emails" root cause.

#### Gift Options
- **Gift Messages**: allow a shopper to attach a message at the **order level and/or per item**, enabled under Stores > Config > Sales > Sales > Gift Options.
- **Gift Wrapping** and **Printed Card / Gift Receipt** are **Adobe Commerce** features (with optional per-order fees), configurable globally and overridable per product.
- Gift options can be allowed or disallowed at the product level, overriding the store default.

> **⚠️ Exam Trap:** Order, Invoice, Shipment, and Credit Memo are **separate documents** — creating an Invoice does not ship anything, and a Shipment does not capture payment. Transactional emails are **cron-queued**, so "no emails sent" usually means cron isn't running, not a template problem. **Gift Wrapping/Gift Receipt are Adobe Commerce–only**, but basic **Gift Messages** exist in both editions.

#### Ship to Multiple Addresses (Multishipping)
- Adobe Commerce supports **"Ship to Multiple Addresses"** checkout, letting one cart be split so different items go to different shipping addresses.
- Enabled under **Stores > Configuration > Sales > Multishipping Settings**.
- Mechanically, multishipping **splits the cart into multiple separate orders** (one per address), each with its own shipping cost and shipment — it is not one order with many addresses.
- **Virtual/downloadable-only carts** can't use multishipping (nothing to ship to multiple places).

> **⚠️ Exam Trap:** Multishipping produces **multiple orders**, not a single order shipped to several addresses. It must be explicitly enabled in configuration and doesn't apply to non-shippable (virtual/downloadable) carts.

---

## PART B: PRACTICE MCQs

**Q1.** An order has had a partial shipment created but no invoice has been generated yet. What order state is it in?

- A) Complete
- B) Processing
- C) New
- D) Closed

<details><summary>Answer & Explanation</summary>

**Answer:** B — Processing

**Explanation:** Processing is used whenever at least one invoice OR one shipment exists, but the order isn't yet both fully invoiced and fully shipped.

**Exam Trap:** A shipment alone is enough to reach Processing — an invoice is not required, so Processing does not imply payment has been captured.

</details>

**Q2.** A merchant wants to cancel an order, but the Cancel button is missing from the order view. What is the most likely reason?

- A) The order has already been invoiced
- B) The order is using an offline payment method
- C) The customer is a guest
- D) The order total is above a configured threshold

<details><summary>Answer & Explanation</summary>

**Answer:** A — The order has already been invoiced

**Explanation:** Once an invoice exists, the Cancel button disappears; an invoiced order can only be reversed with a Credit Memo.

**Exam Trap:** Guest checkout, offline payment methods, and order totals never hide Cancel — only the existence of an invoice removes it.

</details>

**Q3.** An order is placed on Hold. Which actions are available while it remains on hold?

- A) Invoice and Ship remain fully available
- B) Only Cancel remains available
- C) All order-modifying actions (Invoice, Ship, Cancel) are frozen until Unhold is used
- D) Only Credit Memo remains available

<details><summary>Answer & Explanation</summary>

**Answer:** C — All order-modifying actions (Invoice, Ship, Cancel) are frozen until Unhold is used

**Explanation:** Hold is a complete freeze; Invoice, Ship, and Cancel are all unavailable until the order is released via Unhold, which restores it to its prior state.

**Exam Trap:** Hold is not a partial freeze — any option claiming "only X remains available" is wrong because nothing order-modifying stays available while held.

</details>

**Q4.** A customer pays via Bank Transfer (an offline method). What must happen before an invoice can be created for this order?

- A) The payment gateway automatically captures funds
- B) An admin manually creates the invoice after confirming payment was received outside the system
- C) The order auto-invoices after 24 hours
- D) The customer must confirm receipt via email link

<details><summary>Answer & Explanation</summary>

**Answer:** B — An admin manually creates the invoice after confirming payment was received

**Explanation:** Offline methods have no gateway visibility, so invoicing is always a manual admin action once payment is confirmed to have arrived outside the system.

**Exam Trap:** Offline methods never auto-invoice and have no time-based or customer-confirmation trigger — the admin is always the one who creates the invoice.

</details>

**Q5.** A credit card payment method is configured with Payment Action = "Authorize Only." What happens at the moment the order is placed?

- A) Funds are immediately captured and an invoice is auto-created
- B) A hold is placed on funds, but no invoice is created until the merchant manually captures the payment
- C) No authorization occurs at all until shipment
- D) The order is automatically canceled if not captured within 1 hour

<details><summary>Answer & Explanation</summary>

**Answer:** B — A hold is placed on funds, but no invoice is created until the merchant manually captures

**Explanation:** Authorize Only authorizes (holds) the funds at order time but does not capture them; the invoice is created only when the merchant manually captures the payment.

**Exam Trap:** Authorize Only never auto-invoices and never auto-cancels — capture is a deliberate manual step, unlike Authorize and Capture which invoices immediately at placement.

</details>

**Q6.** Which action is only available on an online payment method's authorization, and never applies to offline payment methods?

- A) Credit Memo
- B) Invoice
- C) Void
- D) Reorder

<details><summary>Answer & Explanation</summary>

**Answer:** C — Void

**Explanation:** Void releases an online authorization hold before capture; offline methods have no online authorization, so there is nothing to void.

**Exam Trap:** Void is only valid on an authorized-but-not-yet-captured online transaction — once capture occurs, the reversal path becomes a Credit Memo (refund), not Void.

</details>

**Q7.** A merchant issues a refund for an order paid via a credit card processed through an online gateway. What happens when the Credit Memo is created in Admin?

- A) Nothing — the merchant must separately log into the gateway to issue the refund
- B) An API call is automatically triggered to the payment gateway, returning funds to the customer's card
- C) The refund is only recorded internally with no actual money movement
- D) The order automatically cancels

<details><summary>Answer & Explanation</summary>

**Answer:** B — An API call is automatically triggered to the gateway, returning funds to the card

**Explanation:** For online methods, creating the Credit Memo in Admin triggers a real refund API call to the payment gateway, so funds are returned to the customer automatically.

**Exam Trap:** Online refunds do NOT require logging into the gateway separately — that manual, no-gateway-call behavior describes an offline Credit Memo instead.

</details>

**Q8.** A scenario describes an admin manually confirming payment was received via check before creating an invoice. Which type of payment method does this describe?

- A) Online payment method
- B) Offline payment method
- C) A method using Authorize and Capture
- D) A method requiring Void first

<details><summary>Answer & Explanation</summary>

**Answer:** B — Offline payment method

**Explanation:** No gateway involvement, manual confirmation of payment, and manual invoicing are the hallmarks of an offline method.

**Exam Trap:** Any scenario where the admin manually verifies payment arrived before invoicing is offline — Authorize and Capture and Void are online-only concepts and are distractors here.

</details>

**Q9.** A merchant needs shipping rates that vary based on order weight and destination, imported via a spreadsheet. Which shipping method fits?

- A) Flat Rate
- B) Free Shipping
- C) Table Rates
- D) In-Store Pickup

<details><summary>Answer & Explanation</summary>

**Answer:** C — Table Rates

**Explanation:** Table Rates uses a CSV-imported rate table keyed on weight, quantity, order subtotal, or destination — exactly the weight-and-destination requirement described.

**Exam Trap:** Flat Rate cannot vary by weight or destination, and carrier integrations pull live quotes via API rather than a merchant-supplied spreadsheet — only Table Rates is CSV-driven.

</details>

**Q10.** Which setting determines WHICH address (billing, shipping, origin, or customer default) is used to look up the applicable tax rate for an order?

- A) Catalog Price Scope
- B) Tax Calculation Based On
- C) Apply Discount on Prices
- D) Shipping Origin (Sales settings)

<details><summary>Answer & Explanation</summary>

**Answer:** B — Tax Calculation Based On

**Explanation:** "Tax Calculation Based On" selects which address (billing, shipping, origin, or customer default) is used to look up the applicable tax rate for the order.

**Exam Trap:** Shipping Origin only defines the merchant's ship-from address — it is one possible choice for this setting, not the setting that decides which address drives the lookup.

</details>

**Q11.** A merchant wants a flat, non-percentage environmental fee applied to certain electronics products regardless of the standard tax rate. What feature should be used?

- A) Tax Rule with a Fixed Zone
- B) Fixed Product Tax (FPT/Weee)
- C) Customer Tax Class override
- D) Catalog Price Rule

<details><summary>Answer & Explanation</summary>

**Answer:** B — Fixed Product Tax (FPT/Weee)

**Explanation:** FPT applies a flat, per-unit charge (e.g., an environmental fee) configured directly on the product, independent of percentage-based Tax Rules.

**Exam Trap:** FPT is a fixed monetary amount, not a percentage — it lives outside the Tax Rate/Tax Rule engine and is not achieved with a Tax Rule or Customer Tax Class.

</details>

**Q12.** A store has "Apply Tax After Discount" = Yes. How does this affect the calculated tax compared to applying tax before the discount?

- A) No difference in the final tax amount
- B) Tax is calculated on the already-discounted price, generally resulting in a lower tax amount
- C) Tax is calculated twice, once before and once after
- D) This setting only affects shipping tax, not product tax

<details><summary>Answer & Explanation</summary>

**Answer:** B — Tax is calculated on the already-discounted price, generally producing a lower tax amount

**Explanation:** With Apply Tax After Discount = Yes, the discount reduces the taxable base first, so tax is computed on the lower, post-discount price.

**Exam Trap:** This setting changes the actual tax charged (not just its display), and it affects product tax broadly — it is not limited to shipping tax.

</details>

**Q13.** A B2B merchant serving EU customers wants validated intra-EU business customers to be exempt from VAT automatically. What feature supports this?

- A) Fixed Product Tax
- B) VAT ID Validation (via VIES)
- C) Customer Tax Class manually set per customer
- D) Tax Zones restricted by ZIP code

<details><summary>Answer & Explanation</summary>

**Answer:** B — VAT ID Validation (via VIES)

**Explanation:** VAT ID Validation checks a customer's VAT number against the EU VIES database and can automatically grant VAT-exempt pricing to validated intra-EU B2B customers.

**Exam Trap:** Manually assigning a Customer Tax Class works but is not automatic; only VAT ID Validation performs the external VIES check that drives automatic exemption.

</details>

**Q14.** A retailer's inventory is split across two MSI Sources. A customer orders two items, each fulfilled from a different Source. What can happen to the shipment(s) for this order?

- A) The order can only ever be one shipment regardless of Source split
- B) The order can be split into multiple shipments, one per Source, each with its own tracking number
- C) MSI blocks orders from being placed if items span multiple Sources
- D) The second item is automatically backordered

<details><summary>Answer & Explanation</summary>

**Answer:** B — The order can be split into multiple shipments, one per Source, each with its own tracking number

**Explanation:** With MSI, line items fulfilled from different Sources can be shipped separately, so a single order can produce multiple shipments each carrying its own tracking number.

**Exam Trap:** MSI does not block multi-Source orders or force a single shipment — splitting fulfillment across Sources is exactly what it is designed to support.

</details>

---

### Set 2 — Tax Deep Dive

**Q15.** A Tax Rule connects a Tax Rate for "US–CA" with the Product Tax Class "Taxable Goods" and Customer Tax Class "Retail Customer." A customer in the "Wholesale – Tax Exempt" group (mapped to a different Customer Tax Class) buys the same product. No Tax Rule exists linking that Customer Tax Class to the CA rate. What happens?

- A) The system throws a configuration error and blocks checkout
- B) The item is taxed anyway using the default Tax Rule
- C) The item is simply not taxed for this customer — exemption happens through the absence of a matching rule, not a special flag
- D) The system automatically creates a new Tax Rule on the fly

<details><summary>Answer & Explanation</summary>

**Answer:** C — The item is simply not taxed for this customer

**Explanation:** Tax exemption in Commerce is structural — when no Tax Rule links the customer's Tax Class to the applicable Tax Rate/Product Tax Class combination, no tax is applied.

**Exam Trap:** There is no explicit "exempt" toggle and no default fallback rule — exemption is the absence of a matching Tax Rule, not a setting or an error.

</details>

**Q16.** A jurisdiction requires both a 6% state tax and a 1.5% county tax to appear on the same order. How is this configured?

- A) Two separate Tax Rules must be created and applied to different products
- B) One Tax Rule can reference multiple Tax Rates (e.g., both the state and county rate), and Commerce sums them
- C) This scenario is not supported; only one rate can apply per Tax Rule
- D) The county tax must be manually added as a Fixed Product Tax

<details><summary>Answer & Explanation</summary>

**Answer:** B — One Tax Rule can reference multiple Tax Rates, and Commerce sums them

**Explanation:** Both the 6% state rate and the 1.5% county rate are attached to the same Tax Rule for the same Zone; Commerce adds them together on the transaction.

**Exam Trap:** Stacked jurisdictional taxes do not require separate rules per product or an FPT — a single Tax Rule holding multiple Tax Rates handles the summing.

</details>

**Q17.** "Tax Calculation Based On" is set to Shipping Address. A customer's billing address is in Texas, but they are shipping the order to a friend in New York. Which state's tax zone/rate is used?

- A) Texas, because billing address always takes priority
- B) New York, because the setting is explicitly configured to use the shipping address
- C) Whichever state has the higher tax rate
- D) The Shipping Origin state, since that overrides both billing and shipping

<details><summary>Answer & Explanation</summary>

**Answer:** B — New York, because the setting is configured to use the shipping address

**Explanation:** With Tax Calculation Based On = Shipping Address, the ship-to address (New York) determines the Tax Zone and rate, regardless of the Texas billing address.

**Exam Trap:** Billing address does not automatically win, and Shipping Origin does not override the selected address — the configured setting alone decides which address is used.

</details>

**Q18.** A merchant based in the EU sets "Catalog Prices" = Including Tax, and enters $100 as a product's price. What does Commerce treat this $100 as?

- A) A pre-tax price, with tax added on top at checkout, resulting in a higher total
- B) A tax-inclusive price — the $100 already contains the tax amount, which Commerce backs out for reporting/remittance
- C) An error, since Including Tax requires a $0 price entry
- D) The price is automatically doubled to account for tax

<details><summary>Answer & Explanation</summary>

**Answer:** B — A tax-inclusive price; the $100 already contains the tax, which Commerce backs out

**Explanation:** With Catalog Prices = Including Tax, the entered $100 is treated as already containing tax; Commerce extracts the tax portion for reporting/remittance rather than adding it on top.

**Exam Trap:** Including Tax does not add tax on top or require a $0 entry — it reinterprets the entered price as tax-inclusive, the opposite of Excluding Tax.

</details>

**Q19.** A merchant wants the Product Detail Page to show prices excluding tax, but the Shopping Cart to show the total including tax. Is this supported, and how?

- A) Not supported — display settings are always uniform across the whole storefront
- B) Supported — Display settings for Product Prices, Shopping Cart, and Sales documents are independently configurable
- C) Supported, but only by creating two separate storefronts
- D) Supported, but only if Catalog Prices = Including Tax

<details><summary>Answer & Explanation</summary>

**Answer:** B — Supported; display settings for Product Prices, Shopping Cart, and Sales documents are independently configurable

**Explanation:** Display settings are separate from calculation settings and can be set per area, so the PDP can show excluding-tax prices while the cart shows including-tax totals.

**Exam Trap:** This does not require separate storefronts or a particular Catalog Prices setting — display behavior is independent of how tax is calculated.

</details>

**Q20.** A store has "Apply Discount on Prices" = Excluding Tax, and "Apply Tax After Discount" = No. A $100 product (pre-tax) has a 10% Cart Price Rule discount and an 8% tax rate. What is the calculation order?

- A) Tax is calculated on $100 first ($108), then the 10% discount is applied to $108 ($97.20)
- B) The 10% discount is applied to the $100 pre-tax price first ($90), then 8% tax is calculated on the original $100, not the discounted amount, per "Apply Tax After Discount" = No
- C) Discount and tax are applied simultaneously with no defined order
- D) Only one of the two settings can be active at a time

<details><summary>Answer & Explanation</summary>

**Answer:** B — The discount applies to the $100 pre-tax price ($90), then 8% tax is calculated on the original $100 because Apply Tax After Discount = No

**Explanation:** "Apply Discount on Prices = Excluding Tax" sets the discount base to the pre-tax price, while "Apply Tax After Discount = No" means tax is still computed on the pre-discount amount — two distinct axes acting together.

**Exam Trap:** These two settings are independent and both active; don't assume applying the discount to the excluding-tax base also means tax follows the discount — that is a separate toggle.

</details>

**Q21.** A merchant sells a product with a $5 flat environmental fee that should apply regardless of the customer's percentage-based tax rate. Which tax feature fits, and how does it differ structurally from a standard Tax Rule?

- A) Tax Rule — it works identically to percentage-based tax
- B) Fixed Product Tax (FPT/Weee) — a flat per-unit charge, structurally separate from the percentage-based Tax Rule engine
- C) Customer Tax Class override — applied per customer group
- D) Catalog Price Rule — reduces price by a fixed amount

<details><summary>Answer & Explanation</summary>

**Answer:** B — Fixed Product Tax (FPT/Weee), a flat per-unit charge separate from the percentage-based Tax Rule engine

**Explanation:** FPT adds a fixed $5 per-unit amount to the product regardless of any percentage tax rate, and it is configured on the product outside the Tax Rate/Tax Rule structure.

**Exam Trap:** FPT is not a percentage and is not a Catalog Price Rule (which changes price, not tax) — it is a distinct fixed-fee mechanism layered on top of normal tax.

</details>

**Q22.** A B2B merchant wants intra-EU business customers with a validated VAT number to automatically be exempt from VAT charges. What must be configured, and against what external system is the number checked?

- A) VAT ID Validation, checked against the VIES database
- B) Fixed Product Tax, checked against local tax authority APIs
- C) A manual Customer Tax Class assignment with no external validation
- D) Tax Zones restricted by ZIP code, with no external validation

<details><summary>Answer & Explanation</summary>

**Answer:** A — VAT ID Validation, checked against the VIES database

**Explanation:** VAT ID Validation submits the customer's VAT number to the EU VIES service; validated intra-EU business customers can then be granted automatic VAT exemption.

**Exam Trap:** The external system is VIES specifically — not a local tax authority or FPT — and only VAT ID Validation provides automatic, validated exemption rather than manual class assignment.

</details>

---

### Set 3 — Gap Coverage: Order Lifecycle, Payment Actions & Shipping Nuances

**Q23.** What is the key difference between the **Canceled** and **Closed** order states?
- A) They are interchangeable labels for the same state
- B) Canceled means the order was voided before any invoice existed; Closed means the order was fully refunded via credit memo after invoicing
- C) Closed means the customer abandoned the cart; Canceled means the payment failed
- D) Canceled is used for online payments only; Closed for offline only

<details><summary>Answer & Explanation</summary>

**Answer:** B — Canceled means voided before any invoice existed; Closed means fully refunded via credit memo after invoicing

**Explanation:** The presence or absence of an invoice is the dividing line — orders stopped before invoicing are Canceled, while invoiced-then-fully-refunded orders become Closed.

**Exam Trap:** Neither state depends on the payment method being online or offline, and they are not interchangeable — the invoice history is what separates them.

</details>

**Q24.** A credit card method is set to Payment Action = "Authorize and Capture." What happens at the moment the order is placed?
- A) Only a hold is placed; the merchant must capture manually later
- B) Funds are captured immediately, an invoice is generated automatically, and the order moves to Processing
- C) The order sits in Pending Payment until a Void is issued
- D) The order is auto-shipped

<details><summary>Answer & Explanation</summary>

**Answer:** B — Funds are captured immediately, an invoice is generated automatically, and the order moves to Processing

**Explanation:** Authorize and Capture takes payment at order placement and auto-creates the invoice, so the order moves straight to Processing.

**Exam Trap:** Auto-invoicing is not auto-shipping — the order still needs a shipment; and unlike Authorize Only, there is no manual Capture step and no Pending Payment wait.

</details>

**Q25.** A merchant creates a **partial invoice** for 2 of the 5 items on an order. What state is the order in afterward?
- A) Complete, because an invoice exists
- B) Processing, because it is partially invoiced but not fully invoiced and shipped
- C) Closed
- D) Pending Payment

<details><summary>Answer & Explanation</summary>

**Answer:** B — Processing, because it is partially invoiced but not fully invoiced and shipped

**Explanation:** Creating an invoice for some line items moves the order to Processing; Complete requires the order to be fully invoiced AND fully shipped.

**Exam Trap:** The mere existence of any invoice does not make an order Complete — a partial invoice satisfies neither the full-invoice nor the full-shipment condition.

</details>

**Q26.** A shopper wants to place the same order again. An Admin uses the **Reorder** action. What does it do?
- A) Duplicates the original order in its shipped state
- B) Creates a new cart/order pre-filled with the same items, available regardless of the original order's current state in most configurations
- C) Reopens the original order for editing
- D) Only works if the original order is Complete

<details><summary>Answer & Explanation</summary>

**Answer:** B — Creates a new cart/order pre-filled with the same items, available regardless of the original order's state in most configurations

**Explanation:** Reorder builds a fresh cart pre-populated with the original items (subject to store config and product availability) without touching the original order.

**Exam Trap:** Reorder does not reopen, edit, or duplicate the original order's fulfillment state, and it is not restricted to Complete orders.

</details>

**Q27.** A merchant uses Cash on Delivery. Compared to a standard online-captured order, what is notable about when shipment can occur?
- A) Shipment is blocked until an online capture completes
- B) Shipment may be allowed before invoicing, since payment is collected at delivery
- C) The order can never be shipped, only invoiced
- D) A Void must be issued before shipping

<details><summary>Answer & Explanation</summary>

**Answer:** B — Shipment may be allowed before invoicing, since payment is collected at delivery

**Explanation:** With Cash on Delivery, money changes hands physically at delivery, so the order can be shippable before an invoice is created — unlike online flows where capture/invoicing precedes shipment.

**Exam Trap:** COD has no online authorization, so Void never applies, and shipment is not blocked waiting on an online capture.

</details>

**Q28.** A merchant wants one fixed shipping charge applied to the entire order regardless of item count, configured directly in Admin without a rate table. Which method and setting?
- A) Table Rates based on subtotal
- B) Flat Rate with Type = Per Order
- C) Free Shipping with a minimum threshold
- D) Carrier integration with real-time quotes

<details><summary>Answer & Explanation</summary>

**Answer:** B — Flat Rate with Type = Per Order

**Explanation:** Flat Rate's Per Order type charges a single fixed amount for the whole order regardless of item count, configured directly in Admin with no rate table.

**Exam Trap:** Per Item would multiply the charge by quantity; Table Rates needs a CSV; carriers need credentials and return live quotes — only Flat Rate Per Order gives one fixed order-level charge.

</details>

**Q29.** In a tax scenario, what specific role does the **Shipping Origin** (Sales > Shipping Settings > Origin) play, distinct from "Tax Calculation Based On"?
- A) It always overrides Tax Calculation Based On and determines the tax rate
- B) It defines the merchant's "ship from" address used for carrier rate calculations and can matter for tax nexus/origin-based tax rules, whereas "Tax Calculation Based On" selects which address (billing/shipping/origin/default) drives the rate lookup
- C) It has no relationship to tax at all
- D) It sets the customer's default tax class

<details><summary>Answer & Explanation</summary>

**Answer:** B — Shipping Origin defines the merchant's ship-from address (used for carrier rating and origin-based tax nexus), while Tax Calculation Based On selects which address drives the rate lookup

**Explanation:** The two settings are related but distinct: Shipping Origin is the ship-from location; Tax Calculation Based On chooses which address (which could be the origin) is used to find the tax rate.

**Exam Trap:** Shipping Origin never overrides Tax Calculation Based On and does not set a customer's tax class — it only supplies a ship-from address.

</details>

**Q30.** A payment gateway places an order into the **Payment Review** state. What does this indicate, and what should the merchant do?
- A) The order is complete and can be archived
- B) The gateway has flagged the transaction for manual fraud/payment review; the merchant should Accept or Deny it before proceeding to invoice/ship
- C) The order was refunded and needs a credit memo
- D) The customer must re-enter payment details

<details><summary>Answer & Explanation</summary>

**Answer:** B — The gateway flagged the transaction for manual fraud/payment review; the merchant should Accept or Deny it before invoicing/shipping

**Explanation:** Payment Review (and the related Fraud handling) is used by certain gateways when a transaction needs manual review; the order cannot progress until the merchant accepts or denies the payment.

**Exam Trap:** Payment Review is not a completed, refunded, or re-entry state — it is a pending decision the merchant must resolve before the order can move forward.

</details>

---

### Set 4 — Gap Coverage: Sales Documents, Emails, Gift Options & Multishipping

**Q31.** A merchant issues a refund on an invoiced order and wants the returned units added back to inventory. What must they do when creating the Credit Memo?
- A) Nothing — refunds never affect stock
- B) Enable the "Return to Stock" option for the items on the Credit Memo
- C) Cancel the order instead
- D) Create a new Shipment

<details><summary>Answer & Explanation</summary>

**Answer:** B — Enable "Return to Stock" on the Credit Memo

**Explanation:** A Credit Memo optionally restocks items via the Return to Stock checkbox per line item. Cancel is unavailable after invoicing, and refunds do not restock automatically unless the option is selected.

**Exam Trap:** Restocking is not automatic — the Credit Memo's Return to Stock option must be checked, and Cancel is no longer available once an invoice exists.

</details>

**Q32.** Customers report they are not receiving order confirmation emails, though orders are placed successfully and the email templates are correct. What is the most likely cause?
- A) The email template is missing
- B) The cron job that processes the email queue is not running
- C) Multishipping is disabled
- D) The order is in Payment Review

<details><summary>Answer & Explanation</summary>

**Answer:** B — Cron is not running

**Explanation:** Transactional/sales emails are dispatched through a cron-driven queue. If cron isn't running, orders still complete but emails never send. The templates being correct rules out a template issue.

**Exam Trap:** "No emails despite correct templates" points to cron/the email queue, not the template or SMTP config first — sales emails are asynchronous, not sent synchronously at checkout.

</details>

**Q33.** A shopper wants to send items from a single cart to two different recipients at two different addresses in one checkout. What capability is required, and what is the result?
- A) Gift Options; produces one order
- B) Ship to Multiple Addresses (Multishipping), which splits the cart into multiple separate orders, one per address
- C) A Grouped product
- D) Cross-Sells

<details><summary>Answer & Explanation</summary>

**Answer:** B — Multishipping, which splits into multiple orders

**Explanation:** Ship to Multiple Addresses (enabled in Multishipping Settings) lets one cart be split so different items ship to different addresses, creating a separate order per address. It doesn't apply to virtual/downloadable-only carts.

**Exam Trap:** Multishipping yields multiple orders, not one order with several addresses, and must be explicitly enabled in configuration.

</details>

**Q34.** A merchant wants to offer paid gift wrapping and a printed gift receipt at checkout. Which statement is correct?
- A) Both are standard in Magento Open Source
- B) Gift Wrapping and Gift Receipt are Adobe Commerce features; basic gift messages exist in both editions
- C) Gift options require a Bundle product
- D) Gift receipts are only available via Cart Price Rules

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Gift Wrapping and Printed Card/Gift Receipt (with optional fees) are Adobe Commerce features, while basic Gift Messages (order/item level) are available in both editions.

**Exam Trap:** Don't assume all gift options are Open Source — only basic gift messages are; paid Gift Wrapping/Gift Receipt are Adobe Commerce–only.

</details>

**Q35.** An order shows a partial Invoice and a partial Shipment but is neither fully invoiced nor fully shipped. What state is it in, and why aren't these the same document?
- A) Complete; invoice and shipment are the same action
- B) Processing; Invoice (captures payment) and Shipment (records fulfillment) are separate documents, and either being partial keeps the order in Processing
- C) Closed; a credit memo was issued
- D) Holded; documents are frozen

<details><summary>Answer & Explanation</summary>

**Answer:** B — Processing; Invoice and Shipment are separate documents

**Explanation:** Invoicing records the financial capture; shipping records fulfillment. They are independent documents that can each be partial, so an order with partial progress on either remains in Processing until both are complete.

**Exam Trap:** Creating an Invoice does not ship anything, and a Shipment does not capture payment — they are distinct documents, which is exactly why Processing spans so many mixed-progress scenarios.

</details>

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
16. **Order, Invoice, Shipment, and Credit Memo are separate documents** — an Invoice captures payment but ships nothing; a Shipment records fulfillment but captures no payment. Any partial document keeps the order in Processing.
17. **Credit Memos restock only when "Return to Stock" is checked** — refunds do not add inventory back automatically.
18. **Transactional (sales) emails are cron-queued**, not sent synchronously — "no order emails" usually means cron isn't running, not a broken template.
19. **Multishipping splits a cart into multiple separate orders** (one per address), must be explicitly enabled, and never applies to virtual/downloadable-only carts.
20. **Gift Wrapping and Gift Receipt are Adobe Commerce–only**; basic order/item Gift Messages exist in both editions.
Section 3: Orders, Payments, Shipping, and Tax (18%)
