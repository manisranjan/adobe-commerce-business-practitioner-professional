# Adobe Commerce Business Practitioner Professional (AD0-E723)
## Section 4: Customers (14%) — Notes & Practice MCQs

---

## PART A: STUDY NOTES

### 4.1 Customer Groups & Segments — Targeting Pricing and Content

#### Customer Groups
- **Purpose:** Customer Groups determine (1) which **discounts/price rules** are available to a customer and (2) the **Tax Class** associated with that customer
- **Default groups:** General, NOT LOGGED IN, Wholesale (some editions/setups also ship a Retailer group)
- Customer Groups are **static** — a customer belongs to exactly one group at a time, either assigned automatically at registration (default group) or manually changed by an admin, or programmatically via B2B Company assignment
- Used as a **condition** in:
  - Catalog Price Rules (discount visibility by group)
  - Cart Price Rules (coupon/promotion eligibility by group)
  - Tier Pricing (different price breaks per group)
  - Tax Rules (Customer Tax Class is tied to Customer Group)
- **NOT LOGGED IN** must be explicitly included in any rule that should apply to guest shoppers — a very common oversight

#### Customer Segments
- **Purpose:** Dynamically group customers based on **behavioral and property-based conditions** — not a fixed assignment like Groups
- Example segment conditions: customer address, order history, shopping cart contents, total spent, last login date, specific products purchased
- **Dynamically refreshed** — a customer can move in and out of a segment automatically as their behavior/data changes (e.g., someone who abandons a cart today may qualify for a "Cart Abandoners" segment tomorrow, and drop out once they purchase)
- Segments can be used to:
  - Target **Cart Price Rules** (segment-based promotions, more granular than Customer Group targeting)
  - Drive **Dynamic Blocks** (personalized on-page content)
  - Generate **reports** and **export the list of targeted customers** for marketing use

#### Customer Groups vs. Customer Segments — Key Distinction

| | Customer Group | Customer Segment |
|---|---|---|
| Assignment | Static — one group per customer at a time | Dynamic — determined by rule conditions, re-evaluated continuously |
| Drives Tax Class? | Yes | No |
| Used in Catalog Price Rules? | Yes | No — segments aren't a Catalog Price Rule condition |
| Used in Cart Price Rules? | Yes | Yes |
| Used in Dynamic Blocks? | No (not directly) | Yes |
| Granularity | Broad (Retail, Wholesale, Guest) | Fine-grained, behavior/property-based |

**Exam tip:** If a scenario mentions targeting based on **order history, cart contents, or dynamically changing behavior**, that's a **Segment**. If it's about **tax class or a fixed price-rule tier**, that's a **Group**.

> **⚠️ Exam Trap:** Only Customer Groups drive Tax Class and can be used as a Catalog Price Rule condition — Segments cannot do either.

#### Dynamic Blocks
- Rich, interactive storefront content driven by logic from **Price Rules** and **Customer Segments**
- Built and inserted directly into **Page Builder** as a content block
- Enables scenarios like: "Show a banner promoting Product X only to customers in the 'High-Value Repeat Buyers' segment"

> **⚠️ Exam Trap:** Dynamic Blocks are driven by both Price Rules and Customer Segments, and are placed via Page Builder — they are not backend-only or email-only.

---

### 4.2 Configuring Customer Accounts and Attributes

#### Customer Account Scope
- Customers access account management via the **My Account** link in the storefront header
- From My Account, a customer can manage: personal info, **Address Book** (multiple shipping/billing addresses), **billing and shipping preferences**, **newsletter subscriptions**, **wishlist**, order history, stored payment methods (if supported), product reviews
- The storefront header invites shoppers to **Log in or Register** — registered accounts unlock benefits like saved addresses, faster checkout, order history, and wishlist persistence that guests don't get

#### Customer Attributes
- Provide the information needed to support order, fulfillment, and customer management processes
- Beyond the default system fields, merchants can add **custom customer attributes**
- Custom attributes can be added to three distinct sections:
  1. **Account Information** — core profile fields (e.g., "Preferred Contact Method")
  2. **Address Book** — per-address custom fields (e.g., "Delivery Instructions")
  3. **Billing Information** — fields specific to billing (e.g., "Tax ID Number")
- **Address attributes carry over** to the **Billing Information section during checkout**, and also appear when a **guest registers for an account** — meaning a custom address attribute isn't isolated to the Address Book alone; it surfaces wherever address data is collected

> **⚠️ Exam Trap:** An address attribute automatically surfaces in Billing Information at checkout and guest registration — never duplicate it across sections to achieve that.

#### Import Data (Customer-Related)
- Import supports many data types, including: products, advanced pricing, **customer data**, **customer address data**, and product images
- Supported operations for any import job:
  - **Add/Update** — inserts new records, updates existing ones by matching key
  - **Replace** — fully replaces matching records with the imported data
  - **Delete** — removes matching records
- Useful for bulk-onboarding customer lists (e.g., migrating a legacy customer database) or bulk-updating address records

#### Admin Roles and Permissions (Controlling Who Manages Accounts)
- To give an Admin user **restricted access**, the process is:
  1. **Create a Role** first, defining the exact resources/permissions that role can access (e.g., only Customer management, no Catalog or System access)
  2. **Save the Role**
  3. **Create the Admin User** and assign the restricted Role to them
- This is the mechanism for limiting which internal staff can view or edit customer accounts, process refunds, or access sensitive data — critical for both operational security and privacy compliance
- Roles are resource-based (tree of checkboxes covering every Admin menu area) — a role can be scoped as narrowly as "view customer grid only, no edit" if configured that way

> **⚠️ Exam Trap:** You must create and save the Role first, then create the Admin User and assign it — the user cannot be restricted without a pre-existing role.

---

### 4.3 Assisting a Shopper While Respecting Privacy

#### Checkout Process Security
- Once checkout begins, the session moves to a **secure, encrypted channel (HTTPS)**
- Visible indicators: a **padlock icon** in the browser address bar, and the URL scheme changes from `http://` to `https://`
- Relevant when a scenario asks how to reassure a shopper their payment/personal data is protected during checkout, or when troubleshooting a "not secure" browser warning (usually indicates a misconfigured SSL/TLS certificate or mixed content on the checkout page)

#### Login as Customer
- Lets an authorized Admin user **log into the storefront as a specific customer** to see exactly what they see — used for troubleshooting a shopper's reported issue (e.g., "I can't complete checkout")
- Configuration path: **Stores > Settings > Configuration** in the Admin sidebar, where the feature's availability and behavior are controlled
- This is a **privacy-sensitive feature** — it should be scoped via Admin Roles so only appropriate support staff can use it, and its use should respect customer consent/notification expectations
- Directly relevant to 4.3: this is the primary tool for "assisting a shopper" in a hands-on way, but it must be governed carefully because it grants access to the customer's session/data

> **⚠️ Exam Trap:** Login as Customer is privacy-sensitive and must be permission-scoped via Admin Roles — it works for registered accounts and exposes the shopper's live session/data.

#### Adobe Support Customer Data Access and Privacy
- Adobe technical support may need access to a merchant's Commerce data to investigate a support case
- **Only the primary Adobe Commerce account holder** can authorize this access, via **Adobe Commerce account privacy settings**
- **Important distinction:** the **"Project Owner"** of an Adobe Commerce Cloud project is **not necessarily the same person** as the primary Adobe Commerce account holder — authorization must come from the correct role, not just whoever manages the Cloud project day-to-day
- Granting this access **before** a support ticket is created can speed up investigation and resolution

> **⚠️ Exam Trap:** Only the primary Adobe Commerce account holder can authorize Adobe Support data access — the Cloud "Project Owner" is not automatically that person.

#### GDPR (General Data Protection Regulation)
- EU regulation giving EU citizens greater control over their personal data
- **Scope:** applies to any organization operating within the EU, **and** to organizations outside the EU that offer goods/services to EU customers or businesses — extraterritorial reach is a key point
- Common data-subject requests Commerce must be able to support:
  - A shopper requesting a **copy of their stored data** (data portability/access request)
  - A shopper requesting **deletion of their information** (right to erasure)
- Adobe provides **dataflow diagrams and database information** (the "Personal Information Reference") so system integrators can build scripts/tools to fulfill these requests against the specific database structure
- **Exam framing:** GDPR compliance in Commerce is enabled through documentation/tooling support (Personal Information Reference) rather than a single built-in "GDPR button" — the merchant/integrator is responsible for building the actual fulfillment process

---

### 4.4 Customer-Facing Storefront Features

Beyond profile management, registered customers interact with several account-linked features. Know which are B2C, which are B2B, and which edition each requires.

| Feature | What it does | Edition |
|---|---|---|
| **Wish List** | Save products to buy later, persisted to the account; can be shared | Both |
| **Compare Products** | Temporary side-by-side comparison of attributes; not saved long-term to the account | Both |
| **Gift Registry** | A shareable event list (wedding, baby) others can purchase from | **Adobe Commerce** |
| **Reward Points** | Earn/redeem points across actions (purchases, reviews); balance tied to the account | **Adobe Commerce** |
| **Store Credit** | A prepaid balance (from refunds or admin issuance) usable at checkout | **Adobe Commerce** |
| **Order History / Reorder** | View past orders and quickly reorder | Both |

> **⚠️ Exam Trap:** **Wish List** (saved to the account for later) is distinct from **Compare Products** (transient, not persisted). **Gift Registry, Reward Points, and Store Credit are Adobe Commerce–only** customer features — a frequent "which edition?" distractor.

### 4.5 B2B Company Accounts (Adobe Commerce)

For B2B, customers are organized into **Company Accounts** rather than acting as lone shoppers.

- A **Company** groups multiple buyers under one organization with a **Company Administrator** who manages sub-users.
- **Company User Roles & Permissions** control what each sub-user can do (view prices, place orders, manage the company profile) — this is **intra-company access control**, distinct from storewide Customer Groups.
- A **Company Structure/hierarchy** models teams and reporting lines; orders can require **approval rules** based on thresholds.
- Related B2B capabilities: **Shared Catalogs** (per-company pricing/visibility), **Quotes** (negotiated pricing), **Requisition Lists** (saved reusable order lists), and **Quick Order** (add by SKU/upload).
- B2B is an **Adobe Commerce** feature set; it must be enabled in configuration.

> **⚠️ Exam Trap:** Company **roles/permissions** govern access *within a single company* (who can see prices/order) — do not confuse them with **Customer Groups** (storewide pricing/tax) or **Segments** (dynamic marketing). All B2B company features are Adobe Commerce–only.

---

## PART B: PRACTICE MCQs

**Q1.** A merchant wants to give a 10% permanent tier discount to all Wholesale customers and ensure they're taxed under a separate B2B tax rule. What should be used?

- A) Customer Segment
- B) Customer Group
- C) Dynamic Block
- D) Customer Attribute

<details><summary>Answer & Explanation</summary>

**Answer:** B — Customer Group

**Explanation:** Groups drive both price rule/tier eligibility AND Tax Class association; this is a static, group-level need.

**Exam Trap:** A Customer Segment could not satisfy the tax requirement — Segments never determine Tax Class, so a scenario combining tier pricing *and* a tax rule must use a Group.

</details>

**Q2.** A merchant wants to show a personalized homepage banner only to customers who abandoned a cart in the last 7 days. Which feature is most appropriate?

- A) Customer Group
- B) Customer Segment
- C) Import Data
- D) Admin Role

<details><summary>Answer & Explanation</summary>

**Answer:** B — Customer Segment

**Explanation:** Behavior-based, dynamically changing criteria (cart abandonment recency) is a Segment use case, not a static Group.

**Exam Trap:** "Personalized banner" implies a Dynamic Block for delivery, but the *targeting logic* that identifies cart abandoners is the Customer Segment — don't confuse the display mechanism with the audience rule.

</details>

**Q3.** A Cart Price Rule needs to apply to guest shoppers as well as logged-in customers. What must be explicitly configured?

- A) A Customer Segment must be created for guests
- B) The "NOT LOGGED IN" group must be included in the rule's Customer Groups field
- C) Guests are automatically included in all Cart Price Rules by default
- D) A separate rule must be created exclusively for guests

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** NOT LOGGED IN must be explicitly selected — it's easy to forget and a common troubleshooting root cause.

**Exam Trap:** Guests are NOT included by default; omitting NOT LOGGED IN silently excludes every guest shopper even though the rule otherwise looks correct.

</details>

**Q4.** Which of the following is TRUE about Customer Segments compared to Customer Groups?

- A) Segments determine the customer's Tax Class; Groups do not
- B) Segments are static and assigned once; Groups are dynamic and continuously refreshed
- C) Segments are dynamically refreshed and can be used with Dynamic Blocks; Groups are static and used for tax class and pricing tiers
- D) Segments and Groups are functionally identical

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Segments are dynamically refreshed and can drive Dynamic Blocks and Cart Price Rules; Groups are static and used for Tax Class and pricing tiers.

**Exam Trap:** Options A and B invert the two core facts (tax ownership and static-vs-dynamic) — read direction carefully, since the distractors are the true statements reversed.

</details>

**Q5.** A merchant wants to add a custom field called "Preferred Delivery Window" that should appear both in the customer's Address Book AND during checkout when entering billing information. Where should this attribute be added?

- A) Account Information only
- B) A custom attribute added to the Address section — address attributes automatically surface in Billing Information at checkout and guest registration
- C) It must be manually duplicated as two separate attributes
- D) This is not supported in Commerce

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Address attributes carry over to the Billing Information section during checkout and guest registration without needing duplication.

**Exam Trap:** Adding the field to Account Information would NOT make it appear in the Address Book or checkout — only the Address section propagates to Billing Information.

</details>

**Q6.** A merchant needs to bulk-update existing customer address records from a legacy system export, without creating duplicates for addresses that already exist. Which import operation should be used?

- A) Add/Update
- B) Replace
- C) Delete
- D) Import does not support customer address data

<details><summary>Answer & Explanation</summary>

**Answer:** A — Add/Update

**Explanation:** Inserts new records and updates existing ones matched by key, avoiding duplication.

**Exam Trap:** Replace would overwrite matched records wholesale (wiping fields not in the file), and Delete would remove them — only Add/Update merges safely without duplicates.

</details>

**Q7.** A merchant wants a support team member to be able to view and edit customer records, but have NO access to Catalog, System configuration, or Sales data. How should this be configured?

- A) Assign them the default Administrator role with a warning not to touch other areas
- B) Create a restricted Role scoped only to Customer-related resources, save it, then create the Admin User and assign that Role
- C) This level of restriction is not possible in Commerce
- D) Create a Customer Segment for admin staff

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Create a restricted Role scoped only to Customer-related resources, save it, then create the Admin User and assign that Role.

**Exam Trap:** The Role must exist and be saved *before* the user can be assigned to it — you cannot restrict permissions directly on the user account.

</details>

**Q8.** A shopper reports they cannot complete checkout and describes an error. What built-in Admin feature lets support staff see exactly what the shopper is experiencing on the storefront?

- A) Customer Segment preview
- B) Login as Customer
- C) Dynamic Block editor
- D) Import Data preview

<details><summary>Answer & Explanation</summary>

**Answer:** B — Login as Customer

**Explanation:** Login as Customer lets an authorized Admin user log into the storefront as that specific shopper to see exactly what they experience.

**Exam Trap:** It is configured under Stores > Settings > Configuration and must be permission-scoped — it is not a read-only "preview," it enters the customer's actual session.

</details>

**Q9.** Which of the following is a privacy consideration specifically relevant to the "Login as Customer" feature?

- A) It requires the customer's Tax Class to be set to Exempt
- B) It should be scoped via Admin Roles to appropriate staff only, since it grants access to a customer's session/data
- C) It automatically deletes the customer's data after use
- D) It only works for guest checkouts, never registered accounts

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** It should be scoped via Admin Roles to appropriate staff only, since it grants access to a customer's session/data.

**Exam Trap:** Login as Customer targets registered accounts (guests have no persistent account to log into) — option D reverses how the feature actually works.

</details>

**Q10.** A customer notices the checkout page URL has changed from `http://` to `https://` with a padlock icon. What does this indicate?

- A) The order has been successfully placed
- B) The session has moved to a secure, encrypted channel for checkout
- C) The customer has been logged out
- D) A promotional discount has been applied

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** The session has moved to a secure, encrypted channel (HTTPS) for checkout, indicated by the padlock and the `https://` scheme.

**Exam Trap:** HTTPS/padlock signals transport security only — it does not confirm the order was placed, applied a discount, or logged the user out.

</details>

**Q11.** Adobe technical support needs to access a merchant's Commerce data to investigate a support case. Who is authorized to grant this access?

- A) Any Admin user with the Administrator role
- B) The Project Owner of the Adobe Commerce Cloud project
- C) Only the primary Adobe Commerce account holder, via account privacy settings
- D) Adobe support can access data without merchant authorization

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Only the primary Adobe Commerce account holder can grant this access, via account privacy settings.

**Exam Trap:** The Cloud "Project Owner" is not necessarily the primary account holder — day-to-day project management does not confer authority to approve Adobe Support data access.

</details>

**Q12.** A shopper based in the EU requests a copy of all personal data a merchant has stored about them. What regulation governs this request, and what does Commerce provide to help fulfill it?

- A) CCPA; Commerce provides an automatic one-click export button
- B) GDPR; Commerce provides dataflow diagrams and database information (Personal Information Reference) for building a fulfillment process
- C) PCI-DSS; Commerce automatically anonymizes the data
- D) GDPR does not apply unless the merchant is physically located in the EU

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** GDPR applies even to non-EU organizations that offer goods/services to EU customers — Commerce supports compliance via the Personal Information Reference (dataflow diagrams and database documentation), not a single built-in button.

**Exam Trap:** There is no one-click GDPR export in Commerce; the merchant/integrator must build the fulfillment process using the provided documentation.

</details>

**Q13.** A US-based merchant with no physical presence in the EU sells products online and ships to EU customers. Does GDPR apply to them?

- A) No, because they have no physical presence in the EU
- B) Yes, GDPR applies to any organization offering goods/services to customers or businesses in the EU, regardless of physical location
- C) Only if the merchant has over 500 EU customers
- D) Only if the merchant explicitly opts in to GDPR compliance

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** GDPR applies to any organization offering goods/services to customers or businesses in the EU, regardless of physical location.

**Exam Trap:** GDPR's extraterritorial reach is triggered by *serving EU customers*, not by physical presence, customer count thresholds, or opting in — all of which are distractors here.

</details>

**Q14.** A Dynamic Block is configured to show different promotional content depending on which Customer Segment a shopper currently belongs to. Where is this Dynamic Block ultimately placed on the storefront?

- A) It cannot be placed anywhere; it's backend-only
- B) It's added directly to the Page Builder stage as a content block
- C) It replaces the entire storefront theme
- D) It can only be used in email campaigns, never on-page

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** A Dynamic Block is added directly to the Page Builder stage as a content block.

**Exam Trap:** Dynamic Blocks are on-page storefront content via Page Builder — they are neither backend-only nor restricted to email campaigns.

</details>

**Q15.** A shopper requests that their personal information be deleted from a merchant's store, citing their rights under GDPR. What is Commerce's role in fulfilling this request?

- A) Commerce automatically deletes all data with no merchant involvement
- B) Commerce provides the Personal Information Reference (dataflow/database documentation) so the merchant or integrator can build a process to fulfill the deletion request
- C) This request can be legally ignored if the customer made a purchase
- D) Only Adobe Support can process deletion requests

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Commerce provides the Personal Information Reference (dataflow/database documentation) so the merchant or integrator can build a process to fulfill the deletion request.

**Exam Trap:** Deletion is not automatic and cannot be legally ignored after a purchase — the merchant owns building the erasure workflow, using Adobe's reference documentation.

</details>

---

### Set 2 — Gap Coverage: Group Assignment, Segment Limits, Import Behaviors & Attributes

**Q16.** A merchant wants to target a promotion using **order-history-based** criteria in a **Catalog Price Rule**. Why won't this work as intended?
- A) Catalog Price Rules can only target a single product
- B) Customer Segments are not available as a condition in Catalog Price Rules — segment targeting works in Cart Price Rules and Dynamic Blocks, not Catalog Price Rules
- C) Order history is never a valid segment condition
- D) Catalog Price Rules require a coupon code

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Customer Segments (which is what evaluates behavioral data like order history) can drive Cart Price Rules and Dynamic Blocks, but Catalog Price Rules target by Customer Group, not Segment. A behavior-based catalog-price scenario is a mismatch trap.

**Exam Trap:** Catalog Price Rules also never use coupon codes (option D) — coupons belong to Cart Price Rules; the real issue is that Segments simply aren't a Catalog Price Rule condition.

</details>

**Q17.** When a new customer registers on the storefront without any special assignment, which Customer Group are they placed into?
- A) NOT LOGGED IN
- B) The default customer group configured in Stores > Configuration > Customers > Customer Configuration (typically "General")
- C) Wholesale
- D) They have no group until an admin assigns one

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** New registrants are assigned the configured **default group** (out of the box, "General"). NOT LOGGED IN is reserved for guests/unauthenticated sessions, not registered customers.

**Exam Trap:** NOT LOGGED IN applies only while a session is unauthenticated — the moment a shopper registers/logs in they move to the default (General) group, never staying in NOT LOGGED IN.

</details>

**Q18.** A merchant needs to remove a specific batch of obsolete customer records via import, matching them by key, without touching other customers. Which import behavior is correct?
- A) Add/Update
- B) Replace
- C) Delete
- D) Append

<details><summary>Answer & Explanation</summary>

**Answer:** C — Delete

**Explanation:** The Delete behavior removes only the records matched by key in the import file. Add/Update would insert/merge; Replace would overwrite matched records with new data rather than removing them.

**Exam Trap:** There is no "Append" import behavior — the three valid operations are Add/Update, Replace, and Delete; only Delete removes records.

</details>

**Q19.** A merchant adds a custom attribute "Preferred Contact Method" that should appear only in the customer's core profile, not in any address form. Which section should it be added to?
- A) Address Book
- B) Billing Information
- C) Account Information
- D) It must be added to all three sections

<details><summary>Answer & Explanation</summary>

**Answer:** C — Account Information

**Explanation:** Account Information holds core profile fields. Address attributes (Address Book) automatically surface in Billing Information at checkout and guest registration, so adding it there would spread it beyond the profile — the opposite of the requirement.

**Exam Trap:** Choosing Address Book would leak the field into Billing Information/checkout automatically — to confine an attribute to the profile only, it must live in Account Information.

</details>

**Q20.** Besides Customer Segments, what else can drive the content logic of a **Dynamic Block**?
- A) Only Customer Groups
- B) Price Rules (Cart/Catalog rule logic) in addition to Customer Segments
- C) Tax Rules
- D) Import jobs

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Dynamic Blocks are driven by logic from both **Price Rules** and **Customer Segments**, enabling personalized on-page content tied to promotions or segment membership.

**Exam Trap:** Customer Groups and Tax Rules do not drive Dynamic Block content — the two valid logic sources are Price Rules and Customer Segments.

</details>

**Q21.** A registered customer wants to store several shipping addresses (home, office, gift recipient) for faster checkout. Where is this managed, and is it available to guests?
- A) In the Address Book under My Account; not available to guests, since guests have no persistent account
- B) In Customer Segments; available to everyone
- C) Only an admin can add multiple addresses
- D) Guests get the same Address Book as registered users

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** The Address Book in My Account lets registered customers save multiple addresses. Guests can't persist addresses because they have no account — saved addresses are one of the registration benefits over guest checkout.

**Exam Trap:** A persistent Address Book is a registration benefit — guests may enter an address for a single order but cannot store multiple addresses for reuse.

</details>

**Q22.** A support agent needs to reproduce a checkout problem exactly as a specific registered customer experiences it. They use **Login as Customer**. Which governance control is most important here, and why?
- A) Catalog Price Scope, to fix the price
- B) Admin Roles scoping, because Login as Customer grants access to the customer's session and personal data and should be limited to appropriate staff
- C) Cookie Restriction Mode, to hide the session
- D) Customer Group reassignment, to elevate the agent

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Login as Customer exposes the customer's session/data, so it must be permission-scoped via Admin Roles to only appropriate support staff, respecting privacy and consent expectations.

**Exam Trap:** The governance control is the Admin Role permission scope on the *agent* — not the customer's group, price scope, or cookie settings, which are unrelated distractors.

</details>

---

### Set 3 — Gap Coverage: Storefront Features & B2B Company Accounts

**Q23.** A shopper wants to save products to their account to purchase on a future visit. Which feature persists the list to their account?
- A) Compare Products
- B) Wish List
- C) Requisition List
- D) Gift Registry

<details><summary>Answer & Explanation</summary>

**Answer:** B — Wish List

**Explanation:** Wish List saves products to the customer's account for later and is available in both editions. Compare Products is transient, Requisition List is a B2B reusable order list, and Gift Registry is an event list for others to buy from.

**Exam Trap:** Compare Products is temporary and not persisted; Wish List is the account-saved "buy later" feature. Don't confuse either with the B2B Requisition List.

</details>

**Q24.** Which set of customer-facing features requires Adobe Commerce (not Magento Open Source)?
- A) Wish List and Compare Products
- B) Gift Registry, Reward Points, and Store Credit
- C) Address Book and Order History
- D) Newsletter subscription and Reorder

<details><summary>Answer & Explanation</summary>

**Answer:** B — Gift Registry, Reward Points, and Store Credit

**Explanation:** Gift Registry, Reward Points, and Store Credit are Adobe Commerce customer features. Wish List, Compare, Address Book, Order History, Reorder, and Newsletter exist in both editions.

**Exam Trap:** Wish List and Compare are the common distractors — they are in BOTH editions, whereas Gift Registry/Reward Points/Store Credit are Commerce-only.

</details>

**Q25.** A B2B company administrator needs to control which of their sub-users can see prices and place orders. Which capability handles this?
- A) Customer Groups
- B) Customer Segments
- C) Company User Roles and Permissions
- D) Admin User Roles

<details><summary>Answer & Explanation</summary>

**Answer:** C — Company User Roles and Permissions

**Explanation:** B2B Company accounts include role-based permissions that let a company administrator define what each sub-user can do (view prices, place orders, manage the company). Customer Groups/Segments are storewide/marketing constructs, and Admin User Roles govern back-office staff, not company buyers.

**Exam Trap:** Company roles control access *within one company*; they are not Customer Groups (storewide pricing/tax), Segments (dynamic marketing), or Admin Roles (internal staff).

</details>

**Q26.** A merchant issues a refund as a prepaid balance the customer can spend on a future order rather than returning money to their card. Which Adobe Commerce feature is this?
- A) Reward Points
- B) Store Credit
- C) Gift Registry
- D) Cart Price Rule

<details><summary>Answer & Explanation</summary>

**Answer:** B — Store Credit

**Explanation:** Store Credit is a prepaid account balance (from refunds or admin issuance) redeemable at checkout. Reward Points are earned/redeemed loyalty points, Gift Registry is an event list, and a Cart Price Rule is a promotion.

**Exam Trap:** Store Credit (a spendable balance) differs from Reward Points (earned loyalty points); both are Adobe Commerce–only but serve different purposes.

</details>

---

## Quick-Reference Exam Traps — Section 4

1. **Groups are static, Segments are dynamic** — if the scenario describes behavior that changes over time (cart contents, order history, recency), it's a Segment, not a Group.
2. **Only Customer Groups drive Tax Class** — Segments have no tax relationship.
3. **NOT LOGGED IN must be explicitly included** in any rule meant to reach guest shoppers — a very common oversight in both Catalog and Cart Price Rules.
4. **Address attributes automatically surface in Billing Information at checkout and guest registration** — no need to duplicate the attribute across sections.
5. **Admin Roles must be created BEFORE the Admin User** — the role defines the permission boundary; the user is then assigned to it.
6. **Login as Customer is privacy-sensitive** — it should be permission-scoped and used thoughtfully, since it exposes a customer's session/data to staff.
7. **The Adobe Commerce Cloud "Project Owner" is NOT automatically the primary Adobe Commerce account holder** — only the latter can authorize Adobe Support's data access.
8. **GDPR has extraterritorial reach** — it applies to any organization (regardless of physical location) that offers goods/services to EU customers or businesses.
9. **GDPR compliance is enabled via documentation/tooling (Personal Information Reference)**, not a single automated built-in feature — the merchant/integrator must build the actual fulfillment workflow.
10. **Import Data's three operations (Add/Update, Replace, Delete) apply broadly** — including to customer and customer address data, not just products.
11. **Wish List (saved to account) vs. Compare Products (transient)** — only the Wish List persists; both exist in both editions.
12. **Gift Registry, Reward Points, and Store Credit are Adobe Commerce–only** customer-facing features; Store Credit (spendable balance) is not the same as Reward Points (loyalty points).
13. **B2B Company User Roles/Permissions control access within a single company** — distinct from Customer Groups (storewide pricing/tax), Segments (dynamic marketing), and Admin Roles (internal staff). All B2B company features are Adobe Commerce–only.
  2. **Address Book** — per-address custom fields (e.g., "Delivery Instructions")
  3. **Billing Information** — fields specific to billing (e.g., "Tax ID Number")
- **Address attributes carry over** to the **Billing Information section during checkout**, and also appear when a **guest registers for an account** — meaning a custom address attribute isn't isolated to the Address Book alone; it surfaces wherever address data is collected

#### Import Data (Customer-Related)
- Import supports many data types, including: products, advanced pricing, **customer data**, **customer address data**, and product images
- Supported operations for any import job:
  - **Add/Update** — inserts new records, updates existing ones by matching key
  - **Replace** — fully replaces matching records with the imported data
  - **Delete** — removes matching records
- Useful for bulk-onboarding customer lists (e.g., migrating a legacy customer database) or bulk-updating address records

#### Admin Roles and Permissions (Controlling Who Manages Accounts)
- To give an Admin user **restricted access**, the process is:
  1. **Create a Role** first, defining the exact resources/permissions that role can access (e.g., only Customer management, no Catalog or System access)
  2. **Save the Role**
  3. **Create the Admin User** and assign the restricted Role to them
- This is the mechanism for limiting which internal staff can view or edit customer accounts, process refunds, or access sensitive data — critical for both operational security and privacy compliance
- Roles are resource-based (tree of checkboxes covering every Admin menu area) — a role can be scoped as narrowly as "view customer grid only, no edit" if configured that way

---

### 4.3 Assisting a Shopper While Respecting Privacy

#### Checkout Process Security
- Once checkout begins, the session moves to a **secure, encrypted channel (HTTPS)**
- Visible indicators: a **padlock icon** in the browser address bar, and the URL scheme changes from `http://` to `https://`
- Relevant when a scenario asks how to reassure a shopper their payment/personal data is protected during checkout, or when troubleshooting a "not secure" browser warning (usually indicates a misconfigured SSL/TLS certificate or mixed content on the checkout page)

#### Login as Customer
- Lets an authorized Admin user **log into the storefront as a specific customer** to see exactly what they see — used for troubleshooting a shopper's reported issue (e.g., "I can't complete checkout")
- Configuration path: **Stores > Settings > Configuration** in the Admin sidebar, where the feature's availability and behavior are controlled
- This is a **privacy-sensitive feature** — it should be scoped via Admin Roles so only appropriate support staff can use it, and its use should respect customer consent/notification expectations
- Directly relevant to 4.3: this is the primary tool for "assisting a shopper" in a hands-on way, but it must be governed carefully because it grants access to the customer's session/data

#### Adobe Support Customer Data Access and Privacy
- Adobe technical support may need access to a merchant's Commerce data to investigate a support case
- **Only the primary Adobe Commerce account holder** can authorize this access, via **Adobe Commerce account privacy settings**
- **Important distinction:** the **"Project Owner"** of an Adobe Commerce Cloud project is **not necessarily the same person** as the primary Adobe Commerce account holder — authorization must come from the correct role, not just whoever manages the Cloud project day-to-day
- Granting this access **before** a support ticket is created can speed up investigation and resolution

#### GDPR (General Data Protection Regulation)
- EU regulation giving EU citizens greater control over their personal data
- **Scope:** applies to any organization operating within the EU, **and** to organizations outside the EU that offer goods/services to EU customers or businesses — extraterritorial reach is a key point
- Common data-subject requests Commerce must be able to support:
  - A shopper requesting a **copy of their stored data** (data portability/access request)
  - A shopper requesting **deletion of their information** (right to erasure)
- Adobe provides **dataflow diagrams and database information** (the "Personal Information Reference") so system integrators can build scripts/tools to fulfill these requests against the specific database structure
- **Exam framing:** GDPR compliance in Commerce is enabled through documentation/tooling support (Personal Information Reference) rather than a single built-in "GDPR button" — the merchant/integrator is responsible for building the actual fulfillment process

---

## PART B: PRACTICE MCQs

**Q1.** A merchant wants to give a 10% permanent tier discount to all Wholesale customers and ensure they're taxed under a separate B2B tax rule. What should be used?

- A) Customer Segment
- B) Customer Group
- C) Dynamic Block
- D) Customer Attribute

**Answer: B — Customer Group.** Groups drive both price rule/tier eligibility AND Tax Class association; this is a static, group-level need.

**Q2.** A merchant wants to show a personalized homepage banner only to customers who abandoned a cart in the last 7 days. Which feature is most appropriate?

- A) Customer Group
- B) Customer Segment
- C) Import Data
- D) Admin Role

**Answer: B — Customer Segment.** Behavior-based, dynamically changing criteria (cart abandonment recency) is a Segment use case, not a static Group.

**Q3.** A Cart Price Rule needs to apply to guest shoppers as well as logged-in customers. What must be explicitly configured?

- A) A Customer Segment must be created for guests
- B) The "NOT LOGGED IN" group must be included in the rule's Customer Groups field
- C) Guests are automatically included in all Cart Price Rules by default
- D) A separate rule must be created exclusively for guests

**Answer: B.** NOT LOGGED IN must be explicitly selected — it's easy to forget and a common troubleshooting root cause.

**Q4.** Which of the following is TRUE about Customer Segments compared to Customer Groups?

- A) Segments determine the customer's Tax Class; Groups do not
- B) Segments are static and assigned once; Groups are dynamic and continuously refreshed
- C) Segments are dynamically refreshed and can be used with Dynamic Blocks; Groups are static and used for tax class and pricing tiers
- D) Segments and Groups are functionally identical

**Answer: C.**

**Q5.** A merchant wants to add a custom field called "Preferred Delivery Window" that should appear both in the customer's Address Book AND during checkout when entering billing information. Where should this attribute be added?

- A) Account Information only
- B) A custom attribute added to the Address section — address attributes automatically surface in Billing Information at checkout and guest registration
- C) It must be manually duplicated as two separate attributes
- D) This is not supported in Commerce

**Answer: B.** Address attributes carry over to the Billing Information section during checkout and guest registration without needing duplication.

**Q6.** A merchant needs to bulk-update existing customer address records from a legacy system export, without creating duplicates for addresses that already exist. Which import operation should be used?

- A) Add/Update
- B) Replace
- C) Delete
- D) Import does not support customer address data

**Answer: A — Add/Update.** Inserts new records and updates existing ones matched by key, avoiding duplication.

**Q7.** A merchant wants a support team member to be able to view and edit customer records, but have NO access to Catalog, System configuration, or Sales data. How should this be configured?

- A) Assign them the default Administrator role with a warning not to touch other areas
- B) Create a restricted Role scoped only to Customer-related resources, save it, then create the Admin User and assign that Role
- C) This level of restriction is not possible in Commerce
- D) Create a Customer Segment for admin staff

**Answer: B.**

**Q8.** A shopper reports they cannot complete checkout and describes an error. What built-in Admin feature lets support staff see exactly what the shopper is experiencing on the storefront?

- A) Customer Segment preview
- B) Login as Customer
- C) Dynamic Block editor
- D) Import Data preview

**Answer: B — Login as Customer.**

**Q9.** Which of the following is a privacy consideration specifically relevant to the "Login as Customer" feature?

- A) It requires the customer's Tax Class to be set to Exempt
- B) It should be scoped via Admin Roles to appropriate staff only, since it grants access to a customer's session/data
- C) It automatically deletes the customer's data after use
- D) It only works for guest checkouts, never registered accounts

**Answer: B.**

**Q10.** A customer notices the checkout page URL has changed from `http://` to `https://` with a padlock icon. What does this indicate?

- A) The order has been successfully placed
- B) The session has moved to a secure, encrypted channel for checkout
- C) The customer has been logged out
- D) A promotional discount has been applied

**Answer: B.**

**Q11.** Adobe technical support needs to access a merchant's Commerce data to investigate a support case. Who is authorized to grant this access?

- A) Any Admin user with the Administrator role
- B) The Project Owner of the Adobe Commerce Cloud project
- C) Only the primary Adobe Commerce account holder, via account privacy settings
- D) Adobe support can access data without merchant authorization

**Answer: C.** Note: the Project Owner is not necessarily the same person as the primary account holder — a common exam trap.

**Q12.** A shopper based in the EU requests a copy of all personal data a merchant has stored about them. What regulation governs this request, and what does Commerce provide to help fulfill it?

- A) CCPA; Commerce provides an automatic one-click export button
- B) GDPR; Commerce provides dataflow diagrams and database information (Personal Information Reference) for building a fulfillment process
- C) PCI-DSS; Commerce automatically anonymizes the data
- D) GDPR does not apply unless the merchant is physically located in the EU

**Answer: B.** GDPR applies even to non-EU organizations that offer goods/services to EU customers — Commerce supports compliance via documentation/tooling, not a single built-in button.

**Q13.** A US-based merchant with no physical presence in the EU sells products online and ships to EU customers. Does GDPR apply to them?

- A) No, because they have no physical presence in the EU
- B) Yes, GDPR applies to any organization offering goods/services to customers or businesses in the EU, regardless of physical location
- C) Only if the merchant has over 500 EU customers
- D) Only if the merchant explicitly opts in to GDPR compliance

**Answer: B.**

**Q14.** A Dynamic Block is configured to show different promotional content depending on which Customer Segment a shopper currently belongs to. Where is this Dynamic Block ultimately placed on the storefront?

- A) It cannot be placed anywhere; it's backend-only
- B) It's added directly to the Page Builder stage as a content block
- C) It replaces the entire storefront theme
- D) It can only be used in email campaigns, never on-page

**Answer: B.**

**Q15.** A shopper requests that their personal information be deleted from a merchant's store, citing their rights under GDPR. What is Commerce's role in fulfilling this request?

- A) Commerce automatically deletes all data with no merchant involvement
- B) Commerce provides the Personal Information Reference (dataflow/database documentation) so the merchant or integrator can build a process to fulfill the deletion request
- C) This request can be legally ignored if the customer made a purchase
- D) Only Adobe Support can process deletion requests

**Answer: B.**

---

## Quick-Reference Exam Traps — Section 4

1. **Groups are static, Segments are dynamic** — if the scenario describes behavior that changes over time (cart contents, order history, recency), it's a Segment, not a Group.
2. **Only Customer Groups drive Tax Class** — Segments have no tax relationship.
3. **NOT LOGGED IN must be explicitly included** in any rule meant to reach guest shoppers — a very common oversight in both Catalog and Cart Price Rules.
4. **Address attributes automatically surface in Billing Information at checkout and guest registration** — no need to duplicate the attribute across sections.
5. **Admin Roles must be created BEFORE the Admin User** — the role defines the permission boundary; the user is then assigned to it.
6. **Login as Customer is privacy-sensitive** — it should be permission-scoped and used thoughtfully, since it exposes a customer's session/data to staff.
7. **The Adobe Commerce Cloud "Project Owner" is NOT automatically the primary Adobe Commerce account holder** — only the latter can authorize Adobe Support's data access.
8. **GDPR has extraterritorial reach** — it applies to any organization (regardless of physical location) that offers goods/services to EU customers or businesses.
9. **GDPR compliance is enabled via documentation/tooling (Personal Information Reference)**, not a single automated built-in feature — the merchant/integrator must build the actual fulfillment workflow.
10. **Import Data's three operations (Add/Update, Replace, Delete) apply broadly** — including to customer and customer address data, not just products.
