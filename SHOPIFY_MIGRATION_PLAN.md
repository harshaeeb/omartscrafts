# Shopify Migration Plan — OmartsCrafts
**From:** Ecwid  
**To:** Shopify  
**Site:** www.omartscrafts.com  
**Date:** June 2026

---

## Table of Contents

1. [Pre-Migration Checklist](#1-pre-migration-checklist)
2. [Shopify Account Setup](#2-shopify-account-setup)
3. [Product Migration](#3-product-migration)
4. [Customer & Order Data Migration](#4-customer--order-data-migration)
5. [Page Creation — Home](#5-page-creation--home)
6. [Page Creation — About Us](#6-page-creation--about-us)
7. [Page Creation — Contact Us](#7-page-creation--contact-us)
8. [Policy Pages](#8-policy-pages)
9. [Navigation & Menus](#9-navigation--menus)
10. [Theme & Branding](#10-theme--branding)
11. [Domain Migration](#11-domain-migration)
12. [SEO Preservation](#12-seo-preservation)
13. [Payment & Shipping Setup](#13-payment--shipping-setup)
14. [Testing Checklist](#14-testing-checklist)
15. [Go-Live Cutover](#15-go-live-cutover)
16. [Post-Launch Monitoring](#16-post-launch-monitoring)

---

## 1. Pre-Migration Checklist

Before touching anything, do these backups and audits.

### 1.1 Export Everything from Ecwid

| What | Where in Ecwid | Format |
|------|---------------|--------|
| Products (all fields) | My Sales → Catalog → Export | CSV |
| Product images | Download via FTP or bulk-export tool | ZIP |
| Customers | My Sales → Customers → Export | CSV |
| Orders | My Sales → Orders → Export | CSV |
| Discount codes | Marketing → Discount Coupons | CSV / manual list |
| Store pages (Home, About, etc.) | Copy HTML from page editor | Text / HTML |
| SEO metadata per product/page | Record manually or via Ecwid API | Spreadsheet |

### 1.2 Audit Current Site

- [ ] List all active product categories and sub-categories
- [ ] List all product variants (size, color, material, etc.)
- [ ] Note which products are digital vs. physical
- [ ] Record current domain DNS settings (A record, CNAME, MX records)
- [ ] Screenshot current Home, About, Contact, and policy pages for reference
- [ ] Capture Google Analytics baseline metrics (traffic, top pages, conversion rate)
- [ ] Export Google Search Console sitemap and top-ranking URLs

### 1.3 Gather Assets

- [ ] Logo files (SVG + PNG, multiple sizes)
- [ ] Brand color hex codes and fonts
- [ ] All product photography (high-res originals, not Ecwid-compressed versions)
- [ ] Lifestyle / banner images used on the homepage
- [ ] Any video content

---

## 2. Shopify Account Setup

### 2.1 Create Your Shopify Store

1. Go to shopify.com → Start free trial (14 days).
2. Choose a plan — **Basic Shopify** is sufficient to start; upgrade when needed.
3. Set store name to **OmartsCrafts** (you can change the display name later).
4. Complete the initial setup wizard (country, currency, time zone).

### 2.2 Essential Apps to Install First

Install these before importing products so settings are in place:

| App | Purpose | Cost |
|-----|---------|------|
| Matrixify (Excelify) | Bulk product/customer import via CSV | Free tier / ~$20/mo |
| Shopify Email | Transactional + marketing emails | Free (pay per send) |
| Judge.me / Loox | Product reviews | Free tier available |
| ReConvert | Post-purchase upsell | Free tier available |
| SEO Manager or Plug In SEO | SEO auditing & meta fields | ~$20/mo |
| Tidio or Gorgias | Live chat / customer support | Free tier available |

### 2.3 Store Settings

Navigate to **Settings** and configure:

- **General:** Store name, address, phone, email
- **Currencies:** Set primary currency (AUD, USD — whichever applies)
- **Units:** Weight units (kg vs. lb) — important for shipping
- **Time zone & date format**
- **Order ID format:** Set a prefix (e.g., `OMC-#1001`) to avoid overlap with old Ecwid order IDs

---

## 3. Product Migration

This is the most complex step. Do it methodically.

### 3.1 Clean Up the Ecwid Export CSV

Open the Ecwid product export CSV and map columns to Shopify's required format.

**Shopify product CSV column reference:**

```
Handle, Title, Body (HTML), Vendor, Type, Tags, Published,
Option1 Name, Option1 Value, Option2 Name, Option2 Value,
Variant SKU, Variant Price, Variant Compare At Price,
Variant Inventory Qty, Variant Requires Shipping,
Variant Weight, Variant Weight Unit,
Image Src, Image Alt Text,
SEO Title, SEO Description
```

**Mapping guide (Ecwid → Shopify):**

| Ecwid Field | Shopify Field | Notes |
|-------------|--------------|-------|
| Product Name | Title | Direct copy |
| Description | Body (HTML) | Paste HTML directly |
| Category | Type + Tags | Use category as Type; sub-cats as Tags |
| Price | Variant Price | |
| Compare At Price | Variant Compare At Price | For sale prices |
| SKU | Variant SKU | |
| Stock | Variant Inventory Qty | |
| Weight | Variant Weight | Check units match |
| Image URL | Image Src | Host images on Shopify CDN after upload |
| Options (Size, etc.) | Option1 Name / Value | Each variant = separate row |
| SEO Title | SEO Title | |
| Meta Description | SEO Description | |

### 3.2 Handle Product Variants

For each product with multiple variants (e.g., size S/M/L):

- Each variant becomes its **own row** in the Shopify CSV.
- The `Handle` column must be **identical** for all rows of the same product.
- The `Title` only appears in the **first row**; leave blank in subsequent rows.

**Example:**
```csv
Handle,       Title,        Option1 Name, Option1 Value, Variant Price
craft-bag-01, Craft Bag,    Size,         Small,          25.00
craft-bag-01, ,             Size,         Medium,         28.00
craft-bag-01, ,             Size,         Large,          30.00
```

### 3.3 Upload Product Images

Option A — Reference existing URLs (fast):
- If Ecwid images are still live, use their direct URLs in `Image Src`. Shopify will download and host them.

Option B — Upload manually (reliable):
1. Download all images from Ecwid.
2. Go to Shopify Admin → Content → Files → Upload.
3. Replace image URLs in the CSV with new Shopify CDN URLs.

### 3.4 Import Products via Matrixify

1. In Shopify Admin → Apps → Matrixify → Import.
2. Upload your cleaned CSV.
3. Run a **test import** on 5–10 products first.
4. Verify those products look correct in Shopify Admin.
5. Run the full import.

### 3.5 Post-Import Product Audit

- [ ] Spot-check 10% of products for correct prices, images, descriptions
- [ ] Verify all variant options imported correctly
- [ ] Check inventory counts match Ecwid
- [ ] Ensure all products are assigned to correct collections
- [ ] Set up **Collections** in Shopify (equivalent to Ecwid categories):
  - Go to Products → Collections → Create Collection
  - Use **Automated Collections** with tag rules for hands-off management

### 3.6 Create Collections (Categories)

Map your Ecwid categories to Shopify Collections:

| Ecwid Category | Shopify Collection | Type |
|---------------|-------------------|------|
| (your category 1) | (collection name) | Automated by tag |
| (your category 2) | (collection name) | Automated by tag |
| New Arrivals | New Arrivals | Automated by tag: `new-arrival` |
| Sale | Sale | Automated by compare-at price |
| All Products | All | Default |

---

## 4. Customer & Order Data Migration

### 4.1 Import Customers

1. Export customers from Ecwid as CSV.
2. Map fields to Shopify customer CSV format:

```
First Name, Last Name, Email, Phone, 
Address1, Address2, City, Province, Province Code,
Country, Country Code, Zip,
Note, Tags, Tax Exempt
```

3. Import via Matrixify or Shopify Admin → Customers → Import.
4. **Note:** Passwords cannot be migrated. Customers will need to reset passwords via "Forgot Password" on first login.
5. Send a welcome/migration email to customers explaining the new site.

### 4.2 Order History

- Orders from Ecwid **cannot be functionally migrated** to Shopify (different order management systems).
- Export Ecwid orders to CSV and store as an **offline archive** (Google Sheets or local file).
- Set Shopify order ID prefix to avoid confusion (see Section 2.3).
- For outstanding/open orders at migration time: **fulfill them in Ecwid** before going live on Shopify, or manually recreate them in Shopify as draft orders.

---

## 5. Page Creation — Home

### 5.1 Create the Home Page Theme Sections

In Shopify, the homepage is built using **theme sections**, not a static page. Go to **Online Store → Themes → Customize**.

**Recommended homepage sections (in order):**

#### Section 1: Hero Banner
- Full-width image or video of your best-selling/flagship product
- Headline: e.g., *"Handcrafted with Love"*
- Sub-headline: e.g., *"Unique handmade crafts, gifts, and accessories"*
- CTA Button: **"Shop Now"** → links to All Products or featured collection

#### Section 2: Value Proposition Bar (Icon + Text)
Three columns:
- ✦ **Handmade** — Every piece is crafted with care
- ✦ **Fast Shipping** — Ships within X business days
- ✦ **Easy Returns** — 30-day hassle-free returns

#### Section 3: Featured Collections
- Display 3–4 top collections with images
- Labels: e.g., *Bags, Jewellery, Home Decor, Kids*

#### Section 4: Best Sellers / New Arrivals
- Product grid (8–12 products)
- Use Shopify's built-in "Featured Products" or "Product Grid" section

#### Section 5: About / Brand Story Snippet
- Short paragraph (2–3 sentences) about OmartsCrafts
- Link to full About Us page
- Optional: photo of maker/workspace

#### Section 6: Customer Reviews
- Use your reviews app (Judge.me / Loox) widget section
- Display 3–5 star reviews

#### Section 7: Instagram Feed (optional)
- Install a free Instagram feed app (e.g., Instafeed)
- Shows recent posts as social proof

#### Section 8: Email Signup Banner
- Offer an incentive: *"Get 10% off your first order — join our newsletter"*
- Connect to Shopify Email or Klaviyo

#### Section 9: Footer
- Links: Home, Shop, About, Contact, FAQ, Policies
- Payment icons
- Social media links
- Copyright notice

---

## 6. Page Creation — About Us

Go to **Online Store → Pages → Add page**.

**Title:** About Us  
**URL handle:** `/pages/about-us`

### Suggested Content Structure:

```
[Hero image — maker at work or craft close-up]

## Our Story
[2–3 paragraphs about how OmartsCrafts started, the passion behind it,
what makes the products special. Be personal and specific.]

## What We Make
[Brief description of product range — materials used, techniques, 
what customers can expect in terms of quality and uniqueness.]

## Our Values
• Handmade with care — every item is made by hand, not mass-produced
• Sustainable where possible — materials sourced responsibly
• Community — supporting local suppliers and artisan traditions

## Meet the Maker
[Photo + short bio of the person(s) behind OmartsCrafts.
This is the most important trust-builder for a crafts business.]

## Why Shop With Us?
• Free shipping over $X
• Ready to ship in X–X days
• Custom orders welcome — [contact us link]

[CTA Button: "Shop Our Collection"]
```

---

## 7. Page Creation — Contact Us

Go to **Online Store → Pages → Add page**.

**Title:** Contact Us  
**URL handle:** `/pages/contact`

Shopify has a built-in contact form. Use the `contact` page template.

### Page Content:

```
## Get in Touch

We'd love to hear from you! Whether you have a question about an order,
want to request a custom piece, or just want to say hello — reach out.

[Contact Form — Name, Email, Message, Send button]

---

📧  Email: [your email]
📱  Instagram: @omartscrafts
⏱  Response time: We aim to reply within 24–48 hours

---

## Custom Orders
Interested in a personalised or custom piece? Tell us what you have 
in mind via the form above or email us directly. We love creating 
one-of-a-kind items for special occasions!

---

## FAQs
[Link to FAQ page if created separately, or inline Q&A:]

**How long does shipping take?**
[Answer]

**Do you ship internationally?**
[Answer]

**Can I track my order?**
[Answer]
```

### Enable the Contact Form:
1. In the page editor, click **"..."** → **Template** → Select `page.contact`
2. This activates Shopify's native contact form (no app needed)

---

## 8. Policy Pages

Shopify has a dedicated section for legal/policy pages. Go to **Settings → Policies**.

Shopify can auto-generate draft policies — always review and customise before publishing.

### 8.1 Refund / Returns Policy

Go to **Settings → Policies → Refund policy**

**Key points to include:**
- Return window: e.g., 30 days from delivery
- Condition of return: unused, original packaging
- Who pays return shipping
- Process: how to initiate a return (email, form)
- Exclusions: custom/personalised items (typically non-returnable)
- Refund timeline: e.g., 5–7 business days after receipt

**Template:**
```
We want you to love your OmartsCrafts purchase. If you're not completely 
satisfied, we accept returns within [X] days of delivery.

ELIGIBLE ITEMS
Items must be unused, undamaged, and in original packaging.
Custom or personalised orders are final sale and cannot be returned.

HOW TO RETURN
Email us at [email] with your order number and reason for return.
We will provide return instructions within 48 hours.

REFUNDS
Once your return is received and inspected, we'll process your refund 
within [X] business days to your original payment method.

SHIPPING COSTS
Return shipping is the customer's responsibility unless the item arrived 
damaged or incorrect.
```

### 8.2 Privacy Policy

Go to **Settings → Policies → Privacy policy**

Use Shopify's generator as a base, then add:
- What data is collected (name, email, address, payment — last handled by Shopify/Stripe)
- How data is used (order fulfilment, marketing if opted in)
- Third-party apps used (Shopify, Google Analytics, email provider)
- Cookie policy (Shopify adds a cookie banner automatically)
- Contact for data requests

### 8.3 Terms of Service

Go to **Settings → Policies → Terms of service**

Include:
- Jurisdiction / governing law
- Intellectual property (your product photos, designs)
- Limitation of liability
- Age requirement (18+)
- Account terms

### 8.4 Shipping Policy

Go to **Settings → Policies → Shipping policy**

**Key points:**
- Processing time: e.g., *"Orders are processed within 1–3 business days"*
- Domestic shipping options and estimated times
- International shipping (if offered) — countries, times, customs note
- Free shipping threshold (if any)
- Tracking: when/how customers receive tracking info
- Lost/damaged packages: what to do

**Template:**
```
PROCESSING TIME
All OmartsCrafts orders are handmade or prepared with care. 
Please allow [X–X] business days for order processing before shipment.

DOMESTIC SHIPPING ([Country])
Standard: [X–X] business days — $[X] (free over $[X])
Express: [X–X] business days — $[X]

INTERNATIONAL SHIPPING
We ship to [list countries / "worldwide"].
Estimated delivery: [X–X] weeks
International customers are responsible for any customs duties or taxes.

TRACKING
A tracking number will be emailed to you once your order ships.

LOST OR DAMAGED ITEMS
If your order arrives damaged, please email us within 7 days with a 
photo and your order number. We'll make it right.
```

### 8.5 Additional Recommended Pages

#### FAQ Page
Go to **Online Store → Pages → Add page**, title "FAQ"

Organise by topic:
- Orders & Payment
- Shipping & Delivery
- Returns & Refunds
- Custom Orders
- Product Care & Materials

#### Size Guide (if applicable)
If products have sizing (clothing, jewellery), create a dedicated size guide page with a comparison table.

---

## 9. Navigation & Menus

Go to **Online Store → Navigation**.

### 9.1 Main Menu (Header)

```
Home
Shop ▾
  ├── All Products
  ├── [Collection 1]
  ├── [Collection 2]
  ├── New Arrivals
  └── Sale
About Us
Contact
```

### 9.2 Footer Menu

```
Shop
  ├── All Products
  └── Collections

Help
  ├── FAQ
  ├── Shipping Policy
  ├── Returns Policy
  └── Contact Us

Company
  ├── About Us
  ├── Privacy Policy
  └── Terms of Service
```

### 9.3 Utility Links

Add to theme header:
- Search icon
- Cart icon
- Account icon (optional — enable customer accounts in Settings → Customer accounts)

---

## 10. Theme & Branding

### 10.1 Choose a Theme

Recommended free themes for a handmade/crafts store:
- **Dawn** (Shopify default — clean, fast, highly customisable)
- **Sense** (product-focused, clean lifestyle feel)
- **Craft** (from Shopify Theme Store — specifically designed for artisan/handmade)

Recommended paid themes (~$200–$350 one-time):
- **Impulse** — strong collection pages, good for varied inventory
- **Prestige** — premium feel, great for gift/luxury positioning
- **Stiletto** — artisan/boutique aesthetic

### 10.2 Branding Configuration

In **Theme → Customize → Theme settings:**

- [ ] Upload logo (recommended: SVG or PNG, transparent background)
- [ ] Set brand colors (primary, secondary, accent, background)
- [ ] Set typography (heading font + body font — Google Fonts are free)
- [ ] Configure button styles (rounded corners suit a crafts brand)
- [ ] Set favicon (small square logo, 32×32px minimum)

### 10.3 Recommended Font Pairings for a Crafts Brand

| Heading | Body | Feel |
|---------|------|------|
| Playfair Display | Lato | Elegant, artisan |
| Cormorant Garamond | Raleway | Luxe, handcrafted |
| Libre Baskerville | Source Sans Pro | Warm, trustworthy |

---

## 11. Domain Migration

**Important:** Do this step last, after the Shopify store is fully built and tested.

### 11.1 Option A — Transfer Domain to Shopify (Recommended)

1. Unlock your domain at your current registrar.
2. Get the transfer authorization (EPP/auth) code.
3. In Shopify Admin → Settings → Domains → Transfer domain.
4. Enter domain and auth code.
5. Approve the transfer email from your registrar.
6. Transfer takes 5–7 days.

### 11.2 Option B — Point DNS to Shopify (Faster, Recommended for Go-Live)

1. In Shopify Admin → Settings → Domains → Connect existing domain.
2. Enter `omartscrafts.com`.
3. Shopify will display the required DNS records.
4. At your domain registrar/DNS provider:
   - Set **A record** for `@` to: `23.227.38.65`
   - Set **CNAME** for `www` to: `shops.myshopify.com`
5. Remove the old Ecwid DNS records.
6. DNS propagation: 15 minutes to 48 hours.

### 11.3 SSL Certificate

Shopify auto-provisions a free SSL certificate (Let's Encrypt) once your domain is connected. This typically activates within 30 minutes.

### 11.4 Preserve Email Routing

If you use `@omartscrafts.com` email:
- Keep your MX records pointing to your email provider (Gmail/Workspace, etc.)
- Only change A and CNAME records — do not touch MX records

---

## 12. SEO Preservation

### 12.1 URL Structure Mapping

Ecwid and Shopify use different URL structures. Map your old URLs to new ones:

| Old Ecwid URL | New Shopify URL |
|--------------|----------------|
| `/#!/product/123` | `/products/product-handle` |
| `/#!/category/bags` | `/collections/bags` |
| `/#!/about` | `/pages/about-us` |
| `/#!/contact` | `/pages/contact` |

### 12.2 Set Up 301 Redirects

For every URL that changes, create a redirect in Shopify:
Go to **Online Store → Navigation → URL Redirects → Add redirect**

Or import bulk redirects via CSV:
```csv
Redirect from,Redirect to
/#!/product/123,/products/craft-bag
/#!/category/bags,/collections/bags
```

**This is critical to preserve Google rankings.**

### 12.3 SEO Fields for All Pages

For every product and page, fill in:
- **SEO Title** (50–60 characters) — unique, include primary keyword
- **Meta Description** (120–160 characters) — compelling, include a CTA
- **URL handle** — short, lowercase, hyphenated, keyword-rich

### 12.4 Submit New Sitemap to Google

1. Shopify auto-generates a sitemap at `https://www.omartscrafts.com/sitemap.xml`
2. Go to Google Search Console → Sitemaps → Submit new sitemap
3. Enter: `https://www.omartscrafts.com/sitemap.xml`

### 12.5 Update Google Analytics / Tag Manager

1. Install **Google & YouTube** app from Shopify App Store (free)
2. Or add GA4 tracking ID via **Online Store → Preferences → Google Analytics**
3. Verify data is flowing post-launch

---

## 13. Payment & Shipping Setup

### 13.1 Payment Providers

Go to **Settings → Payments**

**Recommended setup:**
- **Shopify Payments** (if available in your country) — no transaction fees, lowest rates
  - Accepts: Visa, Mastercard, Amex, Apple Pay, Google Pay, Shop Pay
- **PayPal** — add as additional option (many customers prefer it)
- **Afterpay / Klarna** — buy-now-pay-later; especially good for higher-priced handmade goods

If Shopify Payments isn't available: use **Stripe** as payment gateway.

**Note:** Shopify charges a 0.5%–2% transaction fee for third-party gateways unless you use Shopify Payments.

### 13.2 Shipping Rates

Go to **Settings → Shipping and delivery**

**Recommended structure:**

```
Shipping Zone: Domestic ([Your Country])
  - Standard Shipping: $X.XX (or free over $XX)
  - Express Shipping: $X.XX

Shipping Zone: International
  - Rest of World: $X.XX (flat rate or carrier-calculated)
```

**Enable carrier-calculated rates** if you want live rates from Australia Post, USPS, etc. (requires Shopify Advanced plan or Shopify Shipping).

### 13.3 Tax Settings

Go to **Settings → Taxes and duties**

- Shopify auto-calculates tax based on customer location
- If prices include GST/VAT, enable "Include taxes in prices"
- Set up tax overrides for specific products if needed (e.g., some handmade goods are GST-exempt)

---

## 14. Testing Checklist

Run through this completely on the staging store **before** switching the domain.

### Store & Products
- [ ] All products visible with correct images, prices, descriptions
- [ ] All product variants work (size, color dropdowns)
- [ ] Sold-out items show correctly
- [ ] Collections display correct products
- [ ] Search returns relevant results
- [ ] Product SEO titles/meta descriptions populated

### Purchase Flow
- [ ] Add to cart works on product pages
- [ ] Cart page shows correct items, quantities, subtotals
- [ ] Discount codes work
- [ ] Checkout steps complete without errors (use Shopify test mode)
- [ ] Order confirmation email received
- [ ] Abandoned cart email triggers (if configured)

### Pages & Content
- [ ] Home page sections display correctly on desktop and mobile
- [ ] About Us page renders correctly
- [ ] Contact Us form submits and you receive the email
- [ ] All policy pages are accessible from footer
- [ ] FAQ page loads
- [ ] 404 page is customised (not default Shopify)

### Navigation
- [ ] Header menu links all work
- [ ] Footer menu links all work
- [ ] Mobile navigation (hamburger menu) works
- [ ] Logo links to homepage

### Technical
- [ ] Site loads in < 3 seconds (test with Google PageSpeed Insights)
- [ ] SSL certificate active (padlock shows in browser)
- [ ] Mobile responsive on iPhone and Android screen sizes
- [ ] Google Analytics receiving data
- [ ] All 301 redirects working

---

## 15. Go-Live Cutover

### Timeline (Recommended: Low-Traffic Period, e.g., Tuesday Night)

| Step | Time | Action |
|------|------|--------|
| T-7 days | Week before | Complete all products, pages, theme |
| T-3 days | 3 days before | Full test run, fix all issues found |
| T-1 day | Evening before | Final backup of Ecwid, announce maintenance if needed |
| T=0 | Go-live night | Switch DNS, remove password from Shopify store |
| T+1 hour | After DNS | Verify site loads, test a real purchase |
| T+24 hours | Next day | Monitor orders, check all emails, verify GA data |

### DNS Cutover Steps

1. Log into Shopify Admin → Settings → Domains → Confirm domain is added
2. Log into your domain registrar
3. Update DNS records:
   - A record `@` → `23.227.38.65`
   - CNAME `www` → `shops.myshopify.com`
4. Remove Ecwid-specific DNS records
5. **Keep Ecwid store active (read-only) for 30 days** as a fallback reference
6. In Shopify → Settings → Domains → Set as primary domain → `www.omartscrafts.com`
7. Remove Shopify password protection: **Online Store → Preferences → uncheck "Restrict access"**

---

## 16. Post-Launch Monitoring

### Week 1 — Daily Checks
- [ ] Check Shopify Admin → Overview for orders and sessions
- [ ] Verify all order notification emails sending correctly
- [ ] Monitor contact form submissions
- [ ] Check Google Search Console for crawl errors
- [ ] Watch for 404 errors (add missing redirects promptly)

### Month 1 — Weekly Checks
- [ ] Compare Google Analytics traffic to pre-migration baseline
- [ ] Review Search Console for ranking changes on key product terms
- [ ] Check site speed with PageSpeed Insights
- [ ] Review customer feedback / support emails for friction points
- [ ] Confirm Ecwid subscription can be cancelled (once stable on Shopify)

### Ecwid Cancellation
- Wait **at least 30 days** after go-live before cancelling Ecwid
- Download a final backup of all Ecwid data before cancelling
- Cancel Ecwid subscription via their billing settings

---

## Summary Timeline

```
Week 1:  Export data from Ecwid → clean CSVs → Shopify account setup → install apps
Week 2:  Import products → create collections → upload images → verify data quality
Week 3:  Build theme → create all pages (Home, About, Contact, Policies, FAQ)
Week 4:  Configure payments, shipping, taxes → set up navigation and menus
Week 5:  Full testing → fix issues → set up redirects → connect analytics
Week 6:  DNS cutover → go live → monitor → cancel Ecwid after 30 days stable
```

---

*This plan covers a standard Ecwid-to-Shopify migration for a handmade retail store. Adjust timelines and specifics (product counts, shipping zones, currencies) to match OmartsCrafts' actual setup.*
