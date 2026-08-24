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

#### Dynamic Blocks
- Rich, interactive storefront content driven by logic from **Price Rules** and **Customer Segments**
- Built and inserted directly into **Page Builder** as a content block
- Enables scenarios like: "Show a banner promoting Product X only to customers in the 'High-Value Repeat Buyers' segment"

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
