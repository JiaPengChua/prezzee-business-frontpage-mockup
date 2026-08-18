# Prezzee Business — front page mockup (Ada-powered FAQ)

A UI mockup of the [Prezzee Business AU front page](https://prezzee.com/business/m/en-au) where the
live page's **collapsible FAQ accordion is replaced by an ask bar wired into an Ada agent**.

> **Unofficial demo.** Not affiliated with, endorsed by, or operated by Prezzee. Built as a
> UI mockup for an Ada agent demonstration. Brand assets are hot-linked from Prezzee's public CDN.

## What it does

- Replicates the front page top-to-bottom: hero, retailer marquee, why-choose, 4-step how-it-works,
  use cases, solutions, pricing, Smart Card, testimonials, closing CTA, footer — using the real
  Prezzee design tokens (Rebond Grotesque / Sofia Pro, mulberry `#381926`, accent `#d14424`).
- In place of the FAQ accordion: an **ask bar** plus **quick-option chips** (pricing, payment
  methods, receiving / redeeming / using eGift cards, what is Prezzee Business, bulk orders,
  accounts & users).
- Clicking a chip or submitting the bar pushes the exact question to Ada via `sendMessage()`; on the
  first reply the FAQ panel swaps in place into a full conversation view (markdown-rendered bubbles,
  typing indicator, follow-up chips, `Back to FAQs` / `New conversation`).

## Ada wiring

Handle: **`prezzee-business`** (headless Embed SDK 2, `enableProgrammaticControl: true`).

`window.adaSettings` is defined *before* `embed2.js`; the script tag with `id="__ada"` +
`data-handle` auto-bootstraps the conversation, and `reset()` mints a fresh conversation per page
load. Bot replies arrive on `ada:message:received`; canned greetings are held back so they never
read as the answer.

Everything configurable lives in the `CONFIG` object at the top of the `<script>` block — handle,
cluster, bot name, disclaimer, quick options (each with the exact `send` string), and the marketing
copy for every section.

## Running it

The Ada chat frame is origin-gated. **Add this page's exact HTTPS origin to the
`iframe_allow_list` on the `prezzee-business` bot** or no conversation is created — the page detects
this and prints the origin that needs whitelisting instead of failing silently.

| URL | Behaviour |
|---|---|
| `index.html` | live Ada agent |
| `index.html?demo` | offline preview with canned replies — Ada is never contacted |
| `index.html?debug` | Ada event/console panel (subscriptions, `getMessages`, `getConversation`) |

`?demo` and `?debug` combine.
