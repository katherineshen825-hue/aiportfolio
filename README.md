# Quarterly Royalty Report Generation & Automated Payouts

A tool that turns a manual, multi-hour royalty reconciliation process into one click: it reads quarterly sales transactions, calculates what's owed to each licensing partner, and produces a report, a drafted payment email, and the payment record ready to hand off to Ramp.

**[Try it live →](#)** *(add your deployed link here once it's on GitHub Pages — no account needed, click "See a sample run")*

## The problem

Each quarter, sales transactions tied to licensed products had to be pulled from Airtable, sorted by licensing company, and turned into a royalty report per SKU. That meant manually calculating what was owed to each licensor, writing a payment notification email for each one, and then entering the payment details into Ramp — a repetitive process spread across formats and requiring the source data to be handled carefully and accurately.

## The approach

The math is never left to the AI. Every royalty total is calculated directly in code from the transaction data — deterministic, auditable, and exactly reproducible. Claude's role is the part that actually benefits from a language model: drafting a clear, professional payment email for each licensor and writing the memo that goes on the Ramp payment record, based on the verified numbers it's handed.

That division is the point of the project: know which parts of a workflow need to be exact, and which parts benefit from judgment and language — and don't blur the two.

## How it works

1. Click **See a sample run** for an instant, no-setup demo — or upload your own transactions as a CSV file and add an API key to run it live
2. Transactions are parsed into a table (Customer, SKU, Royalty %, Price, Royalty Owed, Licensing Company) and grouped by licensor
3. A summary table shows the last 4 quarters of payment history next to what's due this quarter, per licensor, with the change
4. Totals are summed directly in code — Claude never touches the arithmetic
5. Claude drafts a payment email and a Ramp memo for each licensor from the verified totals
6. Each licensor gets a card: the SKU breakdown, the draft email with a **Send email** button that opens a pre-filled draft in Gmail, a Ramp payment record (payee, amount, invoice #, memo), and a **Send payment to Ramp** button

### On the "Send payment to Ramp" button
Clicking it generates a one-page PDF royalty report (invoice number, amount, payee, full SKU breakdown) using [jsPDF](https://github.com/parallax/jsPDF), downloads it, and opens a pre-filled email to `ap@ramp.com` with the payment details. Browsers can't attach a file to an email link automatically — that's a security restriction, not a limitation of this tool — so the email includes a note to attach the just-downloaded PDF before sending.

It's a single static HTML file — no backend, no build step. The live mode calls the Anthropic API directly from the browser using your own API key, kept in memory for the session only. The sample run needs no key at all.

## Stack

Vanilla HTML/CSS/JS, Anthropic API (Claude). No frameworks, no build tools.

## Notes / next steps

- A real version of this would pull transactions directly from Airtable's API instead of a CSV upload, and post the payment record to Ramp's API instead of just displaying it
- Could add: multi-quarter comparison, a running payment history log, CSV export of the per-licensor reports
- Swap the model constant in `index.html` to `claude-haiku-4-5-20251001` for a cheaper/faster run on large batches
