# Catchsera Landing Page — PROJECT NOTES
## Repository: catchsera-landing (Public)
## Last Updated: 2026-04-22

---

## 🌐 LIVE URLS

| Resource | URL | Status |
|----------|-----|--------|
| Landing Page (Vercel) | https://catchsera-landing.vercel.app | ✅ LIVE |
| Landing Page (Subdomain) | https://restaurant.catchsera.com | ⏳ DNS PENDING |
| Restaurant Dashboard | https://orders.catchsera.com | ✅ LIVE |
| Admin Portal | https://admin.catchsera.com | ✅ LIVE |
| n8n Workflows | https://n8n.catchsera.com | ✅ LIVE |

---

## ⚠️ CRITICAL DOMAIN RULES — NEVER VIOLATE

- ❌ `catchsera.com` root = **MEDICAL FIELD** — DO NOT TOUCH
- ❌ `www.catchsera.com` = **MEDICAL FIELD** — DO NOT TOUCH  
- ✅ `restaurant.catchsera.com` = Restaurant landing page (this repo)
- ✅ `orders.catchsera.com` = Restaurant dashboard (catchsera-restaurant repo)
- ✅ `admin.catchsera.com` = Admin portal

---

## ✅ COMPLETED THIS SESSION (2026-04-22)

### Landing Page Changes (index.html):

1. **Arabic removed** — All "Arabic" references removed from FAQ, features, JSON-LD schema
   - FAQ: "English only. Spanish available as paid add-on."
   - Features: "handles orders in natural English" (was "English and Arabic")

2. **Credit card removed** — No mention of credit card anywhere
   - Hero trust badge: "7-day free demo included"
   - CTA footer: "7-day free demo • Cancel anytime • No commitment"
   - Pricing note: updated (no credit card mention)

3. **Pricing simplified** — Removed all 3 plan cards
   - Now: single card "Plans start as low as $99/month"
   - "Contact Us for Details" CTA
   - Lists all features in one card

4. **Reviews/Testimonials — fully removed** — No fake reviews
   - Replaced with 4 value prop cards (Zero Missed Calls, Orders in Seconds, Free Your Staff, 🇺🇸 Boston MA)
   - Section heading: "Built for Busy Restaurant Teams"

5. **Contact Form — fully built** replacing the simple email input:
   - Name (required ★)
   - Phone Number (required ★)
   - Business Email (optional)
   - Business Name / Website (optional)
   - "How many calls per day?" dropdown (1–10, 11–30, 31–60, 61–100, 100+)
   - Opt-in checkbox (SMS/email marketing consent)
   - Submit: "Book My Free 7-Day Demo →"
   - Below: "Or email us directly: Contact@catchsera.com"

6. **Form submission logic**:
   - Primary: POST to `https://n8n.catchsera.com/webhook/landing-contact` (no-cors)
   - Fallback: Opens `mailto:Contact@catchsera.com` with all fields pre-filled
   - **The mailto fallback means NO lead is ever lost**, even before n8n webhook is set up

7. **Location** — USA only everywhere:
   - Removed UAE, Dubai, London, Chicago, New York
   - All text: Massachusetts, Boston & USA

8. **Vercel domain** — `restaurant.catchsera.com` already added to Vercel project, waiting for Cloudflare DNS

### Commits This Session:
- `eb3d5b2` — Remove Arabic/non-USA refs, contact form, single pricing $99+, no reviews, no credit card
- `eb5208d` — Fix: contact form sends to n8n webhook, direct email link
- `cd3cc87` — Fix: contact form with n8n webhook + mailto fallback to Contact@catchsera.com

---

## ⏳ PENDING — TODO TOMORROW

### 1. 🔑 Cloudflare DNS (MANUAL — cannot be automated)
Go to: Cloudflare dashboard → `catchsera.com` → DNS → Records → Add:

| Type | Name | Value | Proxy |
|------|------|-------|-------|
| CNAME | `restaurant` | `99300f29f09e1e5c.vercel-dns-017.com.` | ☁️ OFF (grey) |
| TXT | `_vercel` | `vc-domain-verify=restaurant.catchsera.com,26b78a2636ad4974b52d` | ☁️ OFF (grey) |

After adding → Vercel → catchsera-landing → Domains → click **Refresh** next to `restaurant.catchsera.com`

> ⚠️ Note: Vercel shows "linked to another Vercel account" warning — the TXT record at `_vercel.catchsera.com` resolves this.

### 2. 📧 n8n Webhook — Create `landing-contact` workflow
In n8n (https://n8n.catchsera.com), create a new workflow:

**Trigger Node:** Webhook
- Path: `landing-contact`
- Method: POST
- Response: Immediately

**Action Node:** Send Email (or Gmail)
- To: `Contact@catchsera.com`
- Subject: `New Demo Request: {{$json.name}} — {{$json.phone}}`
- Body: All fields from payload:
  - Name: `{{$json.name}}`
  - Phone: `{{$json.phone}}`
  - Email: `{{$json.email}}`
  - Business: `{{$json.website}}`
  - Call Volume: `{{$json.call_volume}}`
  - Opt-in: `{{$json.optin}}`
  - Date: `{{$json.date}}`

**Activate** the workflow when done.

> ✅ Until this is set up, the mailto: fallback in the form already delivers leads to Contact@catchsera.com

### 3. 🤖 Internal AI Chatbot — DEFERRED (not today)
User said: "add internal AI chat boot in the final step after we do the needed update not today after we confirm"
- Do NOT add until user explicitly confirms
- Will be added as a chat widget on the landing page

### 4. 📱 Caller Phone Auto-Capture (Path B) — NOT STARTED
- Dashboard feature: auto-capture caller's phone number
- Requires n8n workflow update + dashboard UI change

### 5. 👤 Customer Memory / Profiles — NOT STARTED
- Store customer order history by phone number
- Requires database schema update in Supabase

### 6. 🔧 n8n Build System Prompt Fix — NOT STARTED

---

## 🏗️ PAGE STRUCTURE (index.html)

| Section | ID | Status |
|---------|----|--------|
| Nav | `#nav` | ✅ Done |
| Hero | `#home` | ✅ Done |
| Logo Bar | (no ID) | ✅ Done |
| How It Works | `#how-it-works` | ✅ Done |
| Features | `#features` | ✅ Done |
| Stats Counter | (no ID) | ✅ Done |
| ROI Calculator | (no ID) | ✅ Done |
| Trusted by Restaurants | `#testimonials` | ✅ Done (no reviews, value props instead) |
| Pricing | `#pricing` | ✅ Done ($99/mo single card) |
| FAQ | `#faq` | ✅ Done |
| Contact Form / CTA | `#get-started` | ✅ Done (full form) |
| Footer | (no ID) | ✅ Done |

---

## ⚙️ TECH STACK

- **Frontend:** Pure HTML/CSS/JS (no framework)
- **Hosting:** Vercel (auto-deploy from main branch)
- **Domain:** restaurant.catchsera.com (DNS pending in Cloudflare)
- **Form backend:** n8n webhook → email (mailto fallback)
- **Dark/Light mode:** CSS variables + localStorage toggle
- **Animations:** IntersectionObserver with will-animate class system
- **SEO:** JSON-LD schema (SoftwareApplication + FAQPage), sitemap.xml, robots.txt

---

## 🎨 BRANDING / CONTENT RULES

| Rule | Value |
|------|-------|
| AI Name | **Sera** (not Sara) |
| Company | Catchsera |
| Service Location | Massachusetts, Boston & USA ONLY |
| Languages | English only (Spanish = paid add-on) |
| Free Trial | 7-day free demo |
| Credit Card | Do NOT mention either way |
| Reviews | None yet (business just started) |
| Delivery Apps | NOT connected (phone orders only) |
| Hardware | Provided at low cost if needed |
| Main Value Prop | Solves busy-time / rush-hour phone call problem |
| Contact Email | Contact@catchsera.com |
| Dashboard URL | https://orders.catchsera.com |

---

## 🔐 KEY CREDENTIALS (for AI context only — do not expose publicly)

| Item | Value |
|------|-------|
| Demo Login | admin@demo.com / demo123 |
| Admin Login | romany@catchsera.com / Catchsera2026! |
| Supabase Project | eqwvjxhcuvlvfcfgjipk.supabase.co |
| Demo Restaurant ID | d640e6e6-c45a-446f-bec8-11e3835471bb |
| Vercel Project | catchsera-coders-projects/catchsera-landing |

---

## 🚫 DO NOT DO (critical constraints)

1. ❌ Do NOT touch catchsera.com root domain (medical field)
2. ❌ Do NOT add AI chatbot until user explicitly confirms
3. ❌ Do NOT add fake reviews or testimonials
4. ❌ Do NOT mention credit card required/not required
5. ❌ Do NOT reference Arabic language support
6. ❌ Do NOT reference delivery apps (DoorDash, Uber Eats, Talabat, etc.) as integrated
7. ❌ Do NOT use Supabase Auth (db.auth.signInWithPassword) in dashboard
8. ❌ Do NOT use `supabase` variable name — always use `db`
9. ❌ Do NOT show table numbers (not relevant for small restaurants)
10. ❌ Do NOT mention non-USA locations (UAE, Dubai, London, etc.)

---

## 📋 DASHBOARD REPO NOTES (catchsera-restaurant — separate repo, private)

- Auth: custom `users` table with `password_hash` (NOT Supabase Auth)
- Session: `localStorage.catchsera_user = { email, restaurant_id, role }`
- Auth query: `db.from('users').select('*, restaurants(*)').eq('email', email).eq('password_hash', password).eq('is_active', true).single()`
- Print: ALWAYS build all copies in ONE div, call window.print() ONCE — never in a loop
- Latest commit: `42bc0fd` — Printer settings UI added

---

*Notes maintained by AI session. Update after each work session.*
