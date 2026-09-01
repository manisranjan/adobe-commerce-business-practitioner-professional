# Adobe Commerce Business Practitioner Professional
## Section 6: eCommerce General Knowledge (12%)

> Weighting: **12%** of the exam. Expect scenario questions on privacy law, whether a payment flow is PCI-compliant, and which Adobe Commerce features affect SEO. You must apply concepts, not just recall definitions.

---

## 6.1 Data Privacy, Accessibility, and Compliance

### Data privacy regulations (know what each is + jurisdiction)

| Regulation | Region | Applies to | Core requirements |
|-----------|--------|-----------|-------------------|
| **GDPR** (General Data Protection Regulation) | European Union / EEA | Any business handling EU residents' personal data | Consent, right to access, right to be forgotten (erasure), data portability, breach notification (72h), lawful basis |
| **CCPA / CPRA** | California, USA | Businesses meeting thresholds serving CA residents | Right to know, delete, opt-out of **sale/sharing** of personal data, non-discrimination |
| **PIPEDA** | Canada | Private-sector orgs | Consent, access, accountability |
| **LGPD** | Brazil | Brazilian residents' data | Similar to GDPR |
| **PCI DSS** | Global (card industry) | Anyone storing/processing/transmitting card data | Secure cardholder data (covered in 6.2) |

### Key privacy concepts (high-yield)

- **Personal data / PII**: any info identifying a person (name, email, address, IP, order history).
- **Consent**: must be **explicit, informed, and opt-in** for GDPR (pre-ticked boxes are not valid consent).
- **Right to be forgotten / erasure**: customer can request deletion of their personal data.
- **Data portability**: provide the customer's data in a portable format.
- **Data minimization**: collect only what you need.
- **Breach notification**: GDPR requires notifying authorities within **72 hours**.

### How Adobe Commerce supports privacy

- **Privacy / GDPR tooling**: admin ability to **delete or anonymize customer data** and export a customer's personal information on request.
- **Cookie Restriction Mode**: when enabled, the storefront asks for **cookie consent** before setting non-essential cookies (Stores → Config → General → Web → Default Cookie Settings → Cookie Restriction Mode = Yes).
- **Cookie lifetime / domain** configuration for compliance.
- **Customer data export** (via Import/Export or GDPR extensions) for data-portability/right-to-know requests.
- **Admin Action Logs** (Adobe Commerce) help demonstrate accountability/auditing.

### Accessibility (a11y)

| Concept | Detail |
|---------|--------|
| **WCAG** (Web Content Accessibility Guidelines) | The standard; levels **A, AA, AAA**. **AA** is the common legal/target benchmark. |
| **POUR principles** | **P**erceivable, **O**perable, **U**nderstandable, **R**obust |
| **ADA / Section 508** (US) | Legal drivers requiring accessible digital experiences |
| **EN 301 549** (EU) | European accessibility standard |
| Practical features | Alt text on images, keyboard navigation, sufficient color contrast, form labels, semantic HTML, ARIA where needed |

**Accessibility cues in scenarios**: adding **alt text**, ensuring **keyboard-only navigation**, **color contrast**, **screen-reader** support → all WCAG/accessibility answers. Adobe Commerce themes (e.g., Luma/Blank) provide a baseline, but merchants are responsible for content-level accessibility (alt text, headings, contrast).

### Traps

- **GDPR = EU, CCPA/CPRA = California.** Don't swap them.
- Consent must be **opt-in** for GDPR; **opt-out of sale** is the CCPA model.
- **Cookie Restriction Mode** is the built-in Commerce setting for cookie consent.
- **WCAG AA** is the practical accessibility target, not AAA.
- Accessibility is **shared responsibility** — the platform gives tools, the merchant must maintain accessible content.

---

## 6.2 PCI Compliance & Payment Flows

### What PCI DSS is
**Payment Card Industry Data Security Standard** — mandatory rules for any organization that **stores, processes, or transmits cardholder data**. Goal: protect card data and reduce fraud.

### The golden rule for the exam
**The less card data touches your server, the lower your PCI scope.** Flows that keep the merchant server from ever handling raw card data are the most compliant and lowest-burden.

### Payment integration types (know which is most compliant)

| Integration type | How it works | Card data touches merchant server? | PCI burden |
|------------------|-------------|:----------------------------------:|-----------|
| **Hosted / Redirect** (e.g., PayPal redirect) | Customer is sent to the gateway's site to pay | ❌ No | Lowest (**SAQ A**) |
| **Hosted fields / iframe / Direct Post / tokenization** (e.g., Braintree Hosted Fields, Stripe Elements) | Card fields are served by the gateway inside the merchant page; card data goes straight to gateway, merchant gets a **token** | ❌ No (data bypasses server) | Low (**SAQ A / A-EP**) |
| **Direct API / on-server capture** | Merchant page collects card data and posts it through its **own server** to the gateway | ✅ Yes | Highest (**SAQ D**) — full PCI DSS |

### Key PCI concepts

- **Tokenization**: replace card number with a non-sensitive **token**; merchant stores the token, not the PAN. Reduces scope.
- **PAN** (Primary Account Number): the card number — the most sensitive data.
- **Never store**: full magnetic stripe, **CVV/CVC**, or PIN after authorization. (CVV must never be stored.)
- **Encryption in transit**: TLS/HTTPS everywhere for checkout.
- **SAQ** (Self-Assessment Questionnaire): the level (A, A-EP, D…) depends on how much card data you handle.
- **Vault / stored cards**: modern gateways store the card **vaulted at the gateway** and give the merchant a token for repeat billing — keeps PCI scope low.

### How Adobe Commerce helps

- Ships integrations that use **tokenization / hosted fields / redirect** (e.g., PayPal, Braintree, **Adobe Payment Services**, Stripe) so card data **bypasses the merchant server**.
- **Adobe Commerce / Magento core does not store raw card numbers** by default — it relies on the gateway/vault.
- Enforces **HTTPS** for checkout and admin.

### Determining if a flow is PCI-compliant (scenario checklist)

Ask: **Does raw card data ever hit the merchant's server or database?**
- If **no** (redirect, hosted fields, tokenization) → compliant, low scope. ✅
- If the merchant **stores the PAN or CVV** on its own server → non-compliant / highest scope. ❌
- If checkout is **not over HTTPS** → non-compliant. ❌

### Traps

- **Storing CVV = automatic non-compliance.** CVV must never be stored, ever.
- **Redirect and hosted-fields/tokenized flows are lowest PCI scope** because card data bypasses the merchant server.
- **Direct API capture on your own server = highest burden (SAQ D)** — only pick this as "most compliant" if the question is about *control*, not compliance ease.
- PCI applies whenever you **store, process, OR transmit** card data — not only when you store it.
- **Tokenization ≠ encryption**; tokenization replaces the value, reducing what's in scope.

---

## 6.3 SEO-Relevant Features in Adobe Commerce

### Built-in SEO features (memorize the list)

| Feature | Where / What | SEO benefit |
|---------|--------------|-------------|
| **URL Rewrites / URL Keys** | Per product/category/CMS page; **Marketing → SEO & Search → URL Rewrites** | Clean, keyword-rich, readable URLs |
| **Search Engine Optimized URLs** (`web/seo/use_rewrites`) | Removes `index.php` from URLs | Cleaner URLs |
| **Meta title / description / keywords** | Product, category, CMS page fields | Control SERP snippets |
| **Canonical tags** | Config: Catalog → Catalog → Search Engine Optimization (canonical for products & categories) | Prevents duplicate-content penalties |
| **XML Sitemap** | **Marketing → SEO & Search → Site Map** (auto-generated, submit to search engines) | Helps crawlers index all pages |
| **robots.txt / Meta Robots** | Design config → Search Engine Robots (INDEX/FOLLOW, custom robots.txt) | Control crawling/indexing |
| **Rich snippets / Structured data** | Luma theme includes basic schema.org markup for products | Enhanced SERP results |
| **Redirects (301)** | Auto-create permanent redirect when URL key changes ("Create Permanent Redirect for old URL") | Preserve link equity, avoid 404s |
| **Friendly image alt text** | Product image alt attribute | Image SEO + accessibility overlap |
| **Layered navigation / crawlable categories** | Category structure | Better site architecture for crawling |
| **Hreflang** (multi-store views) | For multi-language stores | Signals language/region variants |

### Key SEO concepts

- **Canonical URL**: tells search engines the "master" version of a page to avoid duplicate content (important for products in multiple categories, filtered URLs).
- **301 redirect**: permanent redirect; Adobe Commerce can auto-create one when a URL key changes so old links and rankings are preserved.
- **XML Sitemap**: can be auto-generated and configured to update on a schedule; submit URL to Google Search Console.
- **Meta Robots default** (e.g., `INDEX, FOLLOW`) set globally; can be overridden per page.
- **URL suffix** (e.g., `.html`) is configurable for products/categories.

### Scenario cues

- "Products appear under multiple categories causing **duplicate content**" → **Canonical tags**.
- "Changed a product's URL key, avoid **404s and keep rankings**" → **301 permanent redirect** (auto-create option).
- "Help search engines **discover all pages**" → **XML Sitemap**.
- "Improve the **snippet shown in Google**" → **Meta title & description**.
- "Remove `index.php` from URLs" → enable **Web Server Rewrites / SEO-friendly URLs**.
- "Stop a page from being indexed" → **Meta Robots = NOINDEX** / robots.txt.

### Traps

- **Canonical tags** solve duplicate-content issues (not redirects).
- **301 (permanent)** vs **302 (temporary)** — for a permanent URL change, use **301** to pass link equity. Adobe Commerce's "Create Permanent Redirect" = 301.
- **Meta keywords** have little/no modern SEO value, but the field still exists in Adobe Commerce — don't assume it's absent.
- **XML sitemap ≠ HTML sitemap** — the XML sitemap is for crawlers.
- SEO features live under **Marketing → SEO & Search** and **Stores → Config → General → Web** and **Catalog** — know where.

---

## Quick-Reference Cheat Sheet (highest-yield)

- **GDPR = EU** (opt-in consent, right to erasure, 72h breach). **CCPA/CPRA = California** (opt-out of sale).
- **WCAG AA** = accessibility target; **POUR** principles; alt text, keyboard nav, contrast.
- **Cookie Restriction Mode** = Commerce's cookie-consent setting.
- **PCI lowest scope** = card data never touches your server (redirect / hosted fields / tokenization). **Never store CVV.**
- **PCI applies to store, process, OR transmit** card data.
- **SEO built-ins**: URL rewrites, canonical tags, meta title/description, XML sitemap, 301 redirects, robots, structured data.

---

# MCQ — Readiness Check

> Each answer is hidden right below its question. Try to answer first, then expand **Answer**.

**Q1.** A store sells to **customers in Germany and France**. Which regulation primarily governs its handling of their personal data?
- A) CCPA
- B) GDPR
- C) PIPEDA
- D) LGPD

<details><summary>Answer</summary>

**B** — **GDPR** governs EU/EEA residents' personal data.
</details>

**Q2.** Under **GDPR**, a valid consent for marketing cookies must be:
- A) A pre-ticked opt-in box
- B) Implied by continuing to browse
- C) An explicit, informed opt-in action
- D) Assumed unless the user opts out

<details><summary>Answer</summary>

**C** — GDPR requires **explicit, informed opt-in**; pre-ticked boxes and opt-out models are not valid consent.
</details>

**Q3.** Which Adobe Commerce setting prompts shoppers for **cookie consent** before setting non-essential cookies?
- A) Catalog Price Scope
- B) Cookie Restriction Mode
- C) Content Staging
- D) URL Rewrites

<details><summary>Answer</summary>

**B** — **Cookie Restriction Mode** (Stores → Config → Web → Default Cookie Settings).
</details>

**Q4.** What is the common legal **accessibility benchmark** for websites?
- A) WCAG Level A
- B) WCAG Level AA
- C) WCAG Level AAA
- D) PCI SAQ A

<details><summary>Answer</summary>

**B** — **WCAG 2.x Level AA** is the widely adopted legal/target standard.
</details>

**Q5.** Which four principles underpin WCAG accessibility?
- A) Secure, Optimized, Usable, Reliable
- B) Perceivable, Operable, Understandable, Robust
- C) Private, Open, Universal, Responsive
- D) Portable, Observable, Unified, Reachable

<details><summary>Answer</summary>

**B** — **POUR**: Perceivable, Operable, Understandable, Robust.
</details>

**Q6.** A checkout **redirects the customer to PayPal's site** to enter card details, then returns them. Regarding PCI scope, this flow is:
- A) The highest PCI burden (SAQ D)
- B) The lowest PCI scope because card data never touches the merchant server
- C) Non-compliant because it uses a third party
- D) Only compliant if the merchant stores the CVV

<details><summary>Answer</summary>

**B** — Redirect/hosted flows keep card data off the merchant server → **lowest PCI scope (SAQ A)**.
</details>

**Q7.** A merchant's custom checkout **collects card numbers and stores the CVV** in its own database for future use. This is:
- A) Fully PCI-compliant with tokenization
- B) Acceptable if encrypted
- C) Non-compliant — CVV must never be stored
- D) Compliant under SAQ A

<details><summary>Answer</summary>

**C** — **Storing CVV is never allowed** under PCI DSS; this is non-compliant.
</details>

**Q8.** **Tokenization** in a payment flow means:
- A) Encrypting the entire database
- B) Replacing the card number with a non-sensitive token so the PAN isn't stored
- C) Redirecting to the bank
- D) Storing the CVV securely

<details><summary>Answer</summary>

**B** — Tokenization swaps the **PAN for a token**, reducing PCI scope; the merchant stores the token, not the card number.
</details>

**Q9.** PCI DSS applies to any organization that does which of the following with cardholder data?
- A) Only stores it
- B) Only transmits it
- C) Stores, processes, or transmits it
- D) Only displays it

<details><summary>Answer</summary>

**C** — PCI applies to **store, process, OR transmit** — any of the three.
</details>

**Q10.** The same product appears under **three categories**, creating **duplicate URLs**. Which SEO feature addresses this?
- A) XML Sitemap
- B) Canonical tags
- C) 301 redirect
- D) Meta keywords

<details><summary>Answer</summary>

**B** — **Canonical tags** designate the master URL and prevent duplicate-content penalties.
</details>

**Q11.** A merchant **changes a product's URL key** and wants to keep rankings and avoid 404s. What should happen?
- A) A 302 temporary redirect
- B) A 301 permanent redirect (auto-created)
- C) Delete the old URL from the sitemap
- D) Add a canonical tag only

<details><summary>Answer</summary>

**B** — Use a **301 permanent redirect**; Adobe Commerce can auto-create one ("Create Permanent Redirect for old URL").
</details>

**Q12.** Which Adobe Commerce feature helps **search engines discover all store pages**?
- A) Cookie Restriction Mode
- B) XML Sitemap
- C) Layered navigation
- D) Cart Price Rules

<details><summary>Answer</summary>

**B** — The **XML Sitemap** lists URLs for crawlers; submit it to search engines.
</details>

**Q13.** To improve the **title and description shown in Google search results** for a category page, you edit:
- A) The category's meta title and meta description
- B) The robots.txt
- C) The canonical tag
- D) The URL suffix

<details><summary>Answer</summary>

**A** — **Meta title/description** control the SERP snippet.
</details>

**Q14.** Which US privacy law gives consumers the right to **opt out of the sale of their personal data**?
- A) GDPR
- B) PIPEDA
- C) CCPA/CPRA
- D) PCI DSS

<details><summary>Answer</summary>

**C** — **CCPA/CPRA** (California) centers on the **opt-out of sale/sharing** model.
</details>

**Q15.** Which practice most improves **image accessibility and image SEO** at the same time?
- A) Larger image files
- B) Descriptive alt text on images
- C) More product images
- D) Watermarks

<details><summary>Answer</summary>

**B** — **Alt text** aids screen readers (accessibility) and image search (SEO).
</details>

**Q16.** Under GDPR, a customer requests deletion of all their personal data. This is the right to:
- A) Data portability
- B) Erasure (right to be forgotten)
- C) Non-discrimination
- D) Rectification

<details><summary>Answer</summary>

**B** — The **right to erasure / right to be forgotten**.
</details>

**Q17.** Which payment integration gives the merchant the **highest PCI burden (SAQ D)**?
- A) Redirect to a hosted payment page
- B) Gateway-hosted iframe fields with tokenization
- C) Collecting card data on the merchant page and posting it through the merchant's own server
- D) PayPal Express redirect

<details><summary>Answer</summary>

**C** — Handling raw card data on your **own server** = full PCI DSS / **SAQ D**.
</details>

**Q18.** To remove `index.php` from storefront URLs for cleaner, SEO-friendly links, you:
- A) Add canonical tags
- B) Enable Web Server Rewrites (Use Web Server Rewrites = Yes)
- C) Regenerate the XML sitemap
- D) Set Meta Robots to NOINDEX

<details><summary>Answer</summary>

**B** — Enable **Web Server Rewrites** (Stores → Config → Web → Search Engine Optimization) to drop `index.php`.
</details>

---

# Common Exam Traps (memorize these)

1. **GDPR vs CCPA.** GDPR = **EU, opt-in consent, right to erasure, 72h breach notice**. CCPA/CPRA = **California, opt-out of sale**. Don't swap jurisdictions or models.

2. **Consent model.** GDPR needs **explicit opt-in**; pre-ticked boxes and "continued browsing" are **not** valid consent.

3. **Cookie Restriction Mode** is the built-in Adobe Commerce cookie-consent setting — know its name and where it lives.

4. **WCAG AA**, not AAA, is the practical/legal accessibility target. Remember **POUR**. Accessibility is a **shared responsibility** (platform + merchant content).

5. **PCI golden rule.** Lowest scope = card data **never touches the merchant server** (redirect, hosted fields, tokenization). Highest = **on-server capture (SAQ D)**.

6. **Never store CVV** — this is an instant non-compliance answer. PCI applies to **store, process, OR transmit** (not storage alone).

7. **Tokenization ≠ encryption.** Tokenization replaces the PAN with a token to reduce scope.

8. **Canonical vs redirect.** **Canonical** fixes duplicate content (same content, multiple URLs). **301 redirect** handles a **changed/moved URL** to preserve rankings and avoid 404s.

9. **301 (permanent) vs 302 (temporary).** For permanent URL changes use **301**; Adobe Commerce's "Create Permanent Redirect" is a 301.

10. **XML sitemap** is for crawlers (discovery); **meta title/description** control the SERP snippet; **robots** control indexing. Match the tool to the goal.

11. **Meta keywords** field still exists in Adobe Commerce but has negligible SEO value — don't assume it's gone.

12. **SEO-friendly URLs / Web Server Rewrites** removes `index.php`; **URL Rewrites** manage individual page URLs — know the distinction.
