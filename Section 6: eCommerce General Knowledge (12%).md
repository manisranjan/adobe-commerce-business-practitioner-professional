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

> **⚠️ Exam Trap:** GDPR = EU/EEA (opt-in consent), CCPA/CPRA = California (opt-out of sale) — a scenario that names a European country points to GDPR, never CCPA.

### Key privacy concepts (high-yield)

- **Personal data / PII**: any info identifying a person (name, email, address, IP, order history).
- **Consent**: must be **explicit, informed, and opt-in** for GDPR (pre-ticked boxes are not valid consent).
- **Right to be forgotten / erasure**: customer can request deletion of their personal data.
- **Data portability**: provide the customer's data in a portable format.
- **Data minimization**: collect only what you need.
- **Breach notification**: GDPR requires notifying authorities within **72 hours**.

> **⚠️ Exam Trap:** Under GDPR consent must be an active opt-in — pre-ticked boxes or "continued browsing" are not valid consent, even though they feel convenient.

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

> **⚠️ Exam Trap:** The practical/legal accessibility benchmark is WCAG **AA**, not the stricter AAA — pick AA when a question asks for the standard target level.

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

> **⚠️ Exam Trap:** Lower PCI scope is about whether card data touches your server, not whether a third party is involved — a PayPal redirect is *more* compliant (SAQ A), not less.

### Key PCI concepts

- **Tokenization**: replace card number with a non-sensitive **token**; merchant stores the token, not the PAN. Reduces scope.
- **PAN** (Primary Account Number): the card number — the most sensitive data.
- **Never store**: full magnetic stripe, **CVV/CVC**, or PIN after authorization. (CVV must never be stored.)

> **⚠️ Exam Trap:** Storing the CVV is never compliant — encryption or tokenization does not make it acceptable, so any answer that stores CVV is automatically wrong.
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

> **⚠️ Exam Trap:** Duplicate-content problems are solved with **canonical tags**, while a changed URL key needs a **301 redirect** — don't reach for a 301 to fix duplicates or a canonical to preserve a moved page.

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

## 6.4 Site Performance, Mobile & Structured Data

Modern SEO and eCommerce UX go beyond keywords — search engines reward fast, mobile-friendly, well-structured pages.

### Core Web Vitals & performance
- **Core Web Vitals** are Google's page-experience metrics and a ranking signal:
  - **LCP** (Largest Contentful Paint) — loading speed of the main content
  - **INP** (Interaction to Next Paint, which replaced FID) — responsiveness to input
  - **CLS** (Cumulative Layout Shift) — visual stability (elements not jumping around)
- **HTTPS is itself a ranking signal** — secure sites are favored (ties back to PCI/checkout security).
- Adobe Commerce performance levers: **Full Page Cache** (Fastly/Varnish), **image optimization / lazy loading**, **CDN**, **flat catalog / proper indexing**, and minimizing third-party scripts.

### Mobile
- **Google uses mobile-first indexing** — the mobile version of a page is what's primarily crawled and ranked.
- **Responsive design** (the storefront adapts to screen size) is the expected baseline; Adobe Commerce's Luma theme is responsive.

### Structured data (Schema.org / rich results)
- **Structured data** (Schema.org markup, usually JSON-LD) describes page content to search engines, enabling **rich results** (star ratings, price, availability, breadcrumbs) in the SERP.
- Adobe Commerce's Luma theme includes basic product schema; deeper/rich structured data often needs theme work or an extension.
- Rich results can improve click-through even without changing ranking directly.

### Cookie categories (privacy overlap)
- **First-party vs third-party cookies** — first-party are set by your domain; third-party by external domains (ads/analytics) and are increasingly restricted by browsers.
- **Essential vs non-essential cookies** — essential (cart/session) may be exempt from consent; non-essential (marketing/analytics) require consent under GDPR. **Cookie Restriction Mode** gates the non-essential ones.

> **⚠️ Exam Trap:** Core Web Vitals (LCP, **INP** — not the old FID — and CLS), mobile-first indexing, and HTTPS are ranking/experience factors distinct from on-page tags like title/meta. **Structured data drives rich results (enhanced SERP snippets)**, which is different from canonical tags (duplicate content) or sitemaps (discovery). Only **non-essential** cookies require consent — essential cart/session cookies generally don't.

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

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** **GDPR** governs the personal data of EU/EEA residents, so a store selling to customers in Germany and France falls under it regardless of where the merchant is based.

**Exam Trap:** GDPR applies based on where the *data subjects* are, not where the company is headquartered — a US-based merchant selling to EU shoppers is still bound by GDPR.

</details>

**Q2.** Under **GDPR**, a valid consent for marketing cookies must be:
- A) A pre-ticked opt-in box
- B) Implied by continuing to browse
- C) An explicit, informed opt-in action
- D) Assumed unless the user opts out

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** GDPR requires **explicit, informed opt-in** consent; the user must take a clear affirmative action before non-essential cookies are set.

**Exam Trap:** Pre-ticked boxes and "consent by continuing to browse" look like opt-in but are explicitly invalid under GDPR — the opt-out model belongs to CCPA, not GDPR.

</details>

**Q3.** Which Adobe Commerce setting prompts shoppers for **cookie consent** before setting non-essential cookies?
- A) Catalog Price Scope
- B) Cookie Restriction Mode
- C) Content Staging
- D) URL Rewrites

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** **Cookie Restriction Mode** (Stores → Config → General → Web → Default Cookie Settings) is the built-in Adobe Commerce feature that asks shoppers for cookie consent before setting non-essential cookies.

**Exam Trap:** The other options are real Commerce features but unrelated to privacy — Cookie Restriction Mode is the specific setting; there is no separate "GDPR mode" toggle to select instead.

</details>

**Q4.** What is the common legal **accessibility benchmark** for websites?
- A) WCAG Level A
- B) WCAG Level AA
- C) WCAG Level AAA
- D) PCI SAQ A

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** **WCAG 2.x Level AA** is the widely adopted legal and target benchmark for accessible websites, referenced by regulations like the ADA and EN 301 549.

**Exam Trap:** AAA is stricter but rarely required or fully achievable across a site — don't pick the highest level assuming "more is better"; AA is the correct practical target.

</details>

**Q5.** Which four principles underpin WCAG accessibility?
- A) Secure, Optimized, Usable, Reliable
- B) Perceivable, Operable, Understandable, Robust
- C) Private, Open, Universal, Responsive
- D) Portable, Observable, Unified, Reachable

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** The four WCAG principles are **POUR** — Perceivable, Operable, Understandable, and Robust.

**Exam Trap:** The distractors use plausible security/UX buzzwords (Secure, Optimized, Reliable) — POUR is a fixed accessibility acronym, so don't be lured by option words that sound technical but aren't part of WCAG.

</details>

**Q6.** A checkout **redirects the customer to PayPal's site** to enter card details, then returns them. Regarding PCI scope, this flow is:
- A) The highest PCI burden (SAQ D)
- B) The lowest PCI scope because card data never touches the merchant server
- C) Non-compliant because it uses a third party
- D) Only compliant if the merchant stores the CVV

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** A redirect/hosted flow sends the customer to PayPal's own site to enter card details, so raw card data never touches the merchant server — this yields the **lowest PCI scope (SAQ A)**.

**Exam Trap:** Using a third party does not make a flow non-compliant — it's the opposite; keeping card data off your server is exactly what *reduces* PCI burden.

</details>

**Q7.** A merchant's custom checkout **collects card numbers and stores the CVV** in its own database for future use. This is:
- A) Fully PCI-compliant with tokenization
- B) Acceptable if encrypted
- C) Non-compliant — CVV must never be stored
- D) Compliant under SAQ A

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Storing the **CVV** after authorization is never permitted under PCI DSS, so collecting card numbers and retaining the CVV makes this checkout non-compliant.

**Exam Trap:** Encryption or tokenization does not rescue this — the prohibition on storing CVV is absolute, so any answer that keeps the CVV "securely" is still wrong.

</details>

**Q8.** **Tokenization** in a payment flow means:
- A) Encrypting the entire database
- B) Replacing the card number with a non-sensitive token so the PAN isn't stored
- C) Redirecting to the bank
- D) Storing the CVV securely

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Tokenization replaces the **PAN with a non-sensitive token**; the merchant stores the token instead of the card number, which reduces PCI scope.

**Exam Trap:** Tokenization is not the same as encryption — encryption transforms data that can be reversed with a key, while a token is a meaningless surrogate that removes the real card number from scope entirely.

</details>

**Q9.** PCI DSS applies to any organization that does which of the following with cardholder data?
- A) Only stores it
- B) Only transmits it
- C) Stores, processes, or transmits it
- D) Only displays it

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** PCI DSS applies to any organization that **stores, processes, OR transmits** cardholder data — doing any one of the three brings you into scope.

**Exam Trap:** PCI obligations don't start only when you *store* card data — merely transmitting or processing it (e.g., passing it through your server) is enough to trigger compliance requirements.

</details>

**Q10.** The same product appears under **three categories**, creating **duplicate URLs**. Which SEO feature addresses this?
- A) XML Sitemap
- B) Canonical tags
- C) 301 redirect
- D) Meta keywords

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** **Canonical tags** designate the single master URL for a product that appears under multiple categories, preventing duplicate-content penalties.

**Exam Trap:** A 301 redirect would break the legitimate multi-category access, and an XML sitemap only aids discovery — canonical tags are the right tool for duplicate URLs that must all keep working.

</details>

**Q11.** A merchant **changes a product's URL key** and wants to keep rankings and avoid 404s. What should happen?
- A) A 302 temporary redirect
- B) A 301 permanent redirect (auto-created)
- C) Delete the old URL from the sitemap
- D) Add a canonical tag only

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** A **301 permanent redirect** preserves rankings and avoids 404s when a URL key changes; Adobe Commerce can auto-create one via "Create Permanent Redirect for old URL."

**Exam Trap:** A 302 is temporary and does not pass link equity, and a canonical tag alone won't stop the old URL from 404ing — a permanent move needs a 301.

</details>

**Q12.** Which Adobe Commerce feature helps **search engines discover all store pages**?
- A) Cookie Restriction Mode
- B) XML Sitemap
- C) Layered navigation
- D) Cart Price Rules

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** The **XML Sitemap** lists all store URLs for crawlers and can be submitted to search engines so they discover every page.

**Exam Trap:** Layered navigation helps shoppers browse but doesn't guarantee crawler discovery of all pages — the XML sitemap is the feature purpose-built for search-engine discovery.

</details>

**Q13.** To improve the **title and description shown in Google search results** for a category page, you edit:
- A) The category's meta title and meta description
- B) The robots.txt
- C) The canonical tag
- D) The URL suffix

<details><summary>Answer & Explanation</summary>

**Answer:** A

**Explanation:** The category's **meta title and meta description** control the title and description shown in the Google search-results snippet.

**Exam Trap:** The canonical tag, robots.txt, and URL suffix influence indexing or URLs — none of them change the visible SERP snippet text, which is driven by the meta fields.

</details>

**Q14.** Which US privacy law gives consumers the right to **opt out of the sale of their personal data**?
- A) GDPR
- B) PIPEDA
- C) CCPA/CPRA
- D) PCI DSS

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** **CCPA/CPRA** (California) gives consumers the right to **opt out of the sale or sharing** of their personal data, along with rights to know and delete.

**Exam Trap:** GDPR's model is opt-in consent, not opt-out of sale — don't map the "opt-out of sale" right to GDPR; it's the signature CCPA/CPRA concept.

</details>

**Q15.** Which practice most improves **image accessibility and image SEO** at the same time?
- A) Larger image files
- B) Descriptive alt text on images
- C) More product images
- D) Watermarks

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** **Descriptive alt text** aids screen readers (accessibility) and gives search engines text to index for image search (SEO) at the same time.

**Exam Trap:** Bigger files, more images, or watermarks don't help either goal — alt text is the single practice that serves accessibility and image SEO simultaneously.

</details>

**Q16.** Under GDPR, a customer requests deletion of all their personal data. This is the right to:
- A) Data portability
- B) Erasure (right to be forgotten)
- C) Non-discrimination
- D) Rectification

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Deleting all of a customer's personal data on request is the GDPR **right to erasure**, also called the **right to be forgotten**.

**Exam Trap:** Don't confuse erasure with rectification (correcting data) or portability (exporting data) — only erasure means the data is deleted.

</details>

**Q17.** Which payment integration gives the merchant the **highest PCI burden (SAQ D)**?
- A) Redirect to a hosted payment page
- B) Gateway-hosted iframe fields with tokenization
- C) Collecting card data on the merchant page and posting it through the merchant's own server
- D) PayPal Express redirect

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** Collecting raw card data on the merchant page and posting it through the merchant's **own server** places the merchant in full PCI DSS scope (**SAQ D**).

**Exam Trap:** Redirects and gateway-hosted iframe/tokenized fields keep data off your server (low scope) — the highest burden comes specifically from the flow where card data transits your own infrastructure.

</details>

**Q18.** To remove `index.php` from storefront URLs for cleaner, SEO-friendly links, you:
- A) Add canonical tags
- B) Enable Web Server Rewrites (Use Web Server Rewrites = Yes)
- C) Regenerate the XML sitemap
- D) Set Meta Robots to NOINDEX

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Enabling **Web Server Rewrites** (Stores → Config → Web → Search Engine Optimization, Use Web Server Rewrites = Yes) removes `index.php` from storefront URLs for cleaner, SEO-friendly links.

**Exam Trap:** Canonical tags, sitemaps, and Meta Robots don't alter URL structure — dropping `index.php` is specifically a Web Server Rewrites setting.

</details>

---

# MCQ — Gap Coverage (Jurisdictions, PCI Levels & SEO Depth)

**Q19.** A Canadian retailer handling Canadian residents' personal data in the private sector is primarily governed by which regulation?
- A) GDPR
- B) CCPA/CPRA
- C) PIPEDA
- D) LGPD

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** **PIPEDA** governs private-sector handling of personal data in **Canada**. GDPR = EU, CCPA/CPRA = California, LGPD = Brazil.

**Exam Trap:** Each regulation is jurisdiction-specific — match the country in the scenario to the right law; a Canadian retailer is PIPEDA, never GDPR or CCPA.

</details>

**Q20.** Under GDPR, within how long must a qualifying personal-data breach be reported to the supervisory authority?
- A) 24 hours
- B) 48 hours
- C) 72 hours
- D) 30 days

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** GDPR requires notifying the supervisory authority of a qualifying personal-data breach within **72 hours** of becoming aware of it.

**Exam Trap:** 24 or 48 hours sound stricter and plausible, but the GDPR figure is exactly 72 hours — memorize the number rather than guessing the tightest deadline.

</details>

**Q21.** A merchant uses a gateway-hosted **iframe / hosted fields** integration where card data goes straight to the gateway and the merchant receives only a token. Which PCI SAQ level typically applies, and why?
- A) SAQ D, because any card entry is full scope
- B) SAQ A / A-EP, because card data bypasses the merchant server (only a token is handled)
- C) No SAQ is needed at all
- D) SAQ D-Merchant, because tokens are cardholder data

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Hosted-fields/iframe/tokenized flows keep raw card data off the merchant server, placing the merchant in the **lowest scope (SAQ A / A-EP)**. Only on-server capture triggers full **SAQ D**.

**Exam Trap:** A token is not cardholder data, so handling only a token does not push you to SAQ D — the SAQ level tracks whether raw card data touches your server, not whether card entry happens on your page.

</details>

**Q22.** A merchant runs the **same catalog in English, French, and German** across store views and wants search engines to serve the right language version to the right region. Which SEO feature signals these language/region variants?
- A) Canonical tags
- B) 301 redirects
- C) Hreflang annotations
- D) robots.txt

<details><summary>Answer & Explanation</summary>

**Answer:** C

**Explanation:** **Hreflang** annotations signal language/region variants of the same content across multi-store-view setups, helping search engines serve the correct localized version to each region.

**Exam Trap:** Canonical tags would wrongly collapse the localized versions into one, and 301s would redirect users away — hreflang is the only signal that keeps all language versions live and correctly targeted.

</details>

**Q23.** A merchant temporarily routes an old URL to a new one during a short campaign and wants search engines to keep indexing the original afterward. Which redirect type is appropriate, and how does it differ from a 301?
- A) 301 — it passes link equity permanently
- B) 302 (temporary) — it signals the move is temporary so the original URL's ranking is retained, unlike a 301 which is permanent and transfers equity to the new URL
- C) Canonical tag — redirects are never temporary
- D) NOINDEX on the old URL

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** A **302** is a temporary redirect, so the original URL stays the indexed/ranking page. A **301** is permanent and transfers link equity to the new URL (Adobe Commerce's "Create Permanent Redirect" = 301).

**Exam Trap:** Using a 301 for a short-term move would permanently hand rankings to the temporary URL — match redirect type to intent: 302 for temporary, 301 for permanent.

</details>

**Q24.** A merchant wants a specific thank-you page to exist for customers but never appear in Google's index. Which is the most direct control?
- A) Add a canonical tag pointing elsewhere
- B) Set Meta Robots to NOINDEX (or block via robots.txt)
- C) Delete the page from the XML sitemap only
- D) Enable Cookie Restriction Mode

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** Setting **Meta Robots = NOINDEX** (or a robots.txt disallow) tells search engines not to index the page while it remains accessible to customers.

**Exam Trap:** Removing a page from the XML sitemap does not prevent indexing — if the page is otherwise discoverable (links, direct visits), only a NOINDEX directive reliably keeps it out of the index.

</details>

**Q25.** Which US privacy concept is central to **CCPA/CPRA** but is NOT the primary consent model under **GDPR**?
- A) Explicit opt-in before any data collection
- B) The right to **opt out of the sale/sharing** of personal data
- C) 72-hour breach notification
- D) Data portability

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** **CCPA/CPRA** centers on the **opt-out of sale/sharing** of personal data, whereas **GDPR** requires an explicit opt-in consent model.

**Exam Trap:** Both frameworks include portability and breach concepts, so those don't distinguish them — the defining CCPA-not-GDPR concept is the opt-out of sale/sharing.

</details>

**Q26.** A privacy principle states a business should collect only the personal data actually needed for a stated purpose. What is this principle called?
- A) Data portability
- B) Data minimization
- C) Right to erasure
- D) Non-discrimination

<details><summary>Answer & Explanation</summary>

**Answer:** B

**Explanation:** **Data minimization** means collecting only the personal data actually needed for a stated purpose. Portability, erasure, and non-discrimination are separate data-subject rights/principles.

**Exam Trap:** "Collect only what you need" describes minimization, not the right to erasure — don't confuse a collection-limiting principle with a rights-based one triggered by a customer request.

</details>

---

# MCQ — Gap Coverage (Performance, Mobile & Structured Data)

**Q27.** Which set of metrics are Google's **Core Web Vitals**, used as a page-experience ranking signal?
- A) Bounce rate, session duration, pages per visit
- B) LCP (loading), INP (responsiveness), and CLS (visual stability)
- C) Title, meta description, and canonical
- D) Crawl budget, index coverage, and sitemap size

<details><summary>Answer & Explanation</summary>

**Answer:** B — LCP, INP, CLS

**Explanation:** Core Web Vitals are Largest Contentful Paint (loading), Interaction to Next Paint (responsiveness, which replaced FID), and Cumulative Layout Shift (visual stability). They are page-experience signals distinct from on-page meta tags.

**Exam Trap:** INP replaced FID as the responsiveness metric — an answer citing FID is outdated. Core Web Vitals are experience/performance signals, not the same as title/meta tags or crawl settings.

</details>

**Q28.** Google primarily crawls and ranks the mobile version of a site. What is this practice called, and what is the expected storefront design baseline?
- A) Desktop-first indexing; fixed-width design
- B) Mobile-first indexing; responsive design
- C) AMP-only indexing; separate mobile site
- D) Structured-data indexing; JSON-LD design

<details><summary>Answer & Explanation</summary>

**Answer:** B — Mobile-first indexing; responsive design

**Explanation:** Google uses mobile-first indexing, treating the mobile rendering as the primary version for crawling/ranking. Responsive design (adapting to screen size) is the expected baseline; Adobe Commerce's Luma theme is responsive.

**Exam Trap:** Mobile-first indexing means the *mobile* page content drives ranking — a rich desktop page with a stripped-down mobile version can hurt rankings.

</details>

**Q29.** A merchant wants star ratings, price, and availability to appear directly in Google search results. Which SEO technique enables these rich results?
- A) Canonical tags
- B) Structured data (Schema.org, typically JSON-LD)
- C) 301 redirects
- D) robots.txt directives

<details><summary>Answer & Explanation</summary>

**Answer:** B — Structured data (Schema.org)

**Explanation:** Structured data markup describes page content to search engines, enabling rich results (ratings, price, availability) in the SERP. Canonicals fix duplicate content, 301s handle moved URLs, and robots.txt controls crawling — none produce rich snippets.

**Exam Trap:** Rich results come from structured data, not from meta tags, canonicals, or sitemaps — match the enhanced-snippet goal to Schema.org markup.

</details>

**Q30.** Under GDPR/cookie-consent rules, which cookies generally require the shopper's consent before being set?
- A) Essential cart/session cookies
- B) Non-essential cookies such as marketing and analytics
- C) All cookies, including strictly necessary ones
- D) Only first-party cookies

<details><summary>Answer & Explanation</summary>

**Answer:** B — Non-essential (marketing/analytics) cookies

**Explanation:** Essential cookies (cart, session) are generally exempt from consent, while non-essential cookies (marketing, analytics) require consent. Cookie Restriction Mode gates the non-essential ones.

**Exam Trap:** Consent is required for non-essential cookies, not strictly-necessary ones — and the essential/non-essential split matters more than first-party vs third-party for the consent requirement.

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

13. **Core Web Vitals = LCP, INP, CLS** (INP replaced FID). They are page-experience/performance ranking signals, separate from title/meta tags. HTTPS is also a ranking signal.

14. **Mobile-first indexing** means the mobile version drives crawling/ranking; responsive design is the baseline.

15. **Structured data (Schema.org) produces rich results** (ratings/price/availability in the SERP) — different from canonical tags (duplicate content), 301s (moved URLs), and sitemaps (discovery).

16. **Only non-essential cookies (marketing/analytics) require consent**; essential cart/session cookies generally don't. The essential vs non-essential split drives the consent requirement.
