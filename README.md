# Amazon DF → Wiise SO Tool

Aggregates a monthly Amazon Vendor Central Direct Fulfilment invoice export by SKU,
ready to key straight into a Wiise Sales Order. Runs entirely in the browser —
no backend, no build step, no data leaves the page.

**Live tool:** _(add your `*.pages.dev` URL here once deployed)_

## What it does

- Drop in the month's PAID Direct Fulfilment CSV export from Amazon Vendor Central
- Flags any row not marked `PAID` and excludes it from totals by default
- Aggregates line items by SKU: total qty, total value (ex GST and inc GST),
  average unit price, order count, and source Order IDs for tracing
- Download the summary as CSV, or copy it as tab-separated text to paste
  straight into Wiise

**Important:** Amazon's `Item Cost` column is already the extended line amount
for that SKU/order — not a per-unit price. This tool does not multiply it by
Quantity. GST (from the `Tax Rate` column) is added on top separately to get
the inc-GST total, which should reconcile to the `Amount in Payment Currency`
total in Amazon's separate Payments export for the same batch (allow a few
cents of rounding across many lines).

## Running locally

It's a single static file — just open `index.html` in a browser. No install,
no server, no dependencies to fetch beyond the PapaParse CDN script tag
already in the file.

## Making updates

1. Edit `index.html`
2. `git add -A && git commit -m "describe the change"`
3. `git push`

Cloudflare Pages (once connected — see below) redeploys automatically on
every push to `main`. Nothing else to run.

## Deploying / connecting Cloudflare Pages

One-time setup, using Cloudflare's Git integration so every push auto-deploys:

1. Push this repo to GitHub (see **First-time setup** below if you haven't yet)
2. In the Cloudflare dashboard: **Workers & Pages → Create → Pages → Connect to Git**
3. Pick this repo
4. Build settings:
   - Framework preset: **None**
   - Build command: _(leave blank)_
   - Build output directory: `/`
5. Deploy — Cloudflare gives you a `*.pages.dev` URL immediately

From then on, every `git push` to `main` triggers a new deploy automatically —
no manual redeploy step, no CLI required for day-to-day updates.

## First-time setup (push this to GitHub)

From inside this folder:

```bash
git init
git add -A
git commit -m "Initial commit: Amazon DF SKU tool"
git branch -M main
git remote add origin https://github.com/<your-username>/amazon-df-sku-tool.git
git push -u origin main
```

Create the empty repo on GitHub first at github.com/new (recommend **private**,
since this is an internal ops tool) — don't initialize it with a README, or
the push above will conflict.

## Change log

- **v1.1** — Fixed a bug where line values were multiplied by Quantity;
  Amazon's `Item Cost` is already the extended line total. Added a
  Total Value (inc GST) column/stat for reconciling against the Payments export.
- **v1.0** — Initial tool: CSV upload, SKU aggregation, CSV/clipboard export.
