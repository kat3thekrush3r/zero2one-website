# Zero2One site — clean version

## Chart of Accounts: email gate + QBO import file

The `/chart-of-accounts` page now has two tiers:

- **Free PDF** — one click, no email (unchanged).
- **QBO import CSV** (`zero2one-chart-of-accounts-qbo-import.csv`) — gated behind an email modal. On submit it emails the lead to your inbox (via Formspree), releases the CSV download, and fires a Google Ads lead event.

The CSV is the full Zero2One chart of accounts (balance sheet + income statement), formatted for QuickBooks Online's native importer (**Gear → Import Data → Chart of Accounts**) with valid `Account type` / `Detail type` values and sub-account nesting, so it imports without manual account setup.

**Setup status:**

1. **Formspree endpoint** — ✅ wired to `https://formspree.io/f/mbdnwbzv`. Leads arrive in the
   inbox tied to that Formspree form. Note: the *first* submission to a new Formspree form triggers a
   one-time confirmation email — click it once to activate the form, then submissions flow normally.
2. **Google Ads conversion label** — ✅ wired. The "COA Download" (Submit lead form) conversion action
   `AW-18288167379/mQvXCKjt5dkcENOTvZBE` fires on a successful email-gate submit. Note: Google can take a
   few hours to show the action as "active" and start recording conversions — that's expected.

---


## What changed

- Nav now shows: **About · Free Consultation · Client Portal** on every page (consistent).
- "Financial Scorecard" removed from nav, hero CTAs, About CTAs, and the contact page card.
- `/financial-scorecard` → renamed to `/free-consultation` (URL + page title + content).
- `/onboarding` hidden from public routing.
- Footer on the consultation page now has the dark background (was bleeding before).
- Pricing and Free Audit pages now have Client Portal in nav (were missing it).
- "Get Started" buttons on the pricing page now go to `/free-consultation` instead of `/free-audit` — felt more natural with the new nav, but easy to flip back if you want.

## Files

Replace these files in your repo:

- `index.html`
- `about.html`
- `pricing.html`
- `free-audit.html`
- `vercel.json`

Add one new file:

- `free-consultation.html`

Delete or just leave on disk (not linked anywhere now):

- `financial-scorecard.html` — kept around if you want to delete after deploy goes clean
- `onboarding.html` — keep on disk for v2 when you build real OAuth

## vercel.json — what it does

```json
{
  "rewrites": [
    { "source": "/about", "destination": "/about.html" },
    { "source": "/free-consultation", "destination": "/free-consultation.html" },
    { "source": "/pricing", "destination": "/pricing.html" },
    { "source": "/free-audit", "destination": "/free-audit.html" }
  ],
  "redirects": [
    { "source": "/financial-scorecard", "destination": "/free-consultation", "permanent": true },
    { "source": "/onboarding", "destination": "/free-consultation", "permanent": false }
  ]
}
```

- Old `/financial-scorecard` URLs get **permanently redirected** to `/free-consultation` (301) — so any existing inbound links don't 404.
- `/onboarding` gets a **temporary** redirect (302) to `/free-consultation` — temporary because you're planning to bring it back for real later, and you don't want search engines caching it as gone-forever.

## Deploy

Drop the files in, commit, push. Vercel handles the rest.

## After deploy — quick smoke test

1. Visit `/` — nav should show About / Free Consultation / Client Portal. No "Financial Scorecard".
2. Click Free Consultation → lands at `/free-consultation` with Calendly embedded.
3. Visit old URL `/financial-scorecard` → should redirect to `/free-consultation`.
4. Visit `/onboarding` → should also redirect to `/free-consultation`.
5. Check `/pricing` and `/free-audit` directly (they're not linked in nav, but they're still reachable by URL).
