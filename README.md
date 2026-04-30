# 🎯 Your Positioning Decoded

> Answer 5 questions. Get a brand bible your whole team can actually use.

A positioning generator for founders and marketers who know their product but struggle to explain it sharply. Built as a single-file web app — no framework, no build step, no backend.

**Live:** [shivt123.github.io/YourPositioningDecoded](https://shivt123.github.io/YourPositioningDecoded/)

---

## What it does

Walk through 5 questions about your product — category, customer, problems, differentiators, stage — and get back a complete brand bible in seconds:

| Output | What it is |
|--------|-----------|
| **Positioning statement** | One crisp paragraph that defines what you are, for whom, and why it matters |
| **Tagline** | With strategic rationale for why it works |
| **Your enemy** | The behaviour, status quo, or alternative you're positioned against |
| **What makes you weird** | The thing about you that sounds like a weakness but is actually the point |
| **3 messaging angles** | Functional, emotional, and contrarian — each ready to use in copy |
| **3 content ideas** | Specific to your positioning, with platform and format |
| **A challenge** | One provocation to sharpen your thinking further |

---

## Interesting engineering decisions

**Single HTML file — no build step**
The entire app — UI, logic, API calls, state — lives in one `index.html`. Intentional. Zero dependency drift, instant deploy to GitHub Pages, anyone can fork and run it by opening the file.

**Cloudflare Worker proxy for the free tier**
Free users (3 runs per device) don't need an API key — calls route through a Cloudflare Worker that holds the API key server-side. After 3 runs, users can paste their own Anthropic key to continue. Clean separation of free vs. bring-your-own-key.

**Device fingerprinting instead of auth**
Free run tracking uses a browser fingerprint (user agent + screen size + timezone + language) hashed into a local key. No login, no cookie banner, no friction — but also not bypassable by just clearing one cookie.

**Shareable results via URL hash**
Results are base64-encoded into the URL hash on generation. Share a link, the recipient sees your full brand bible with no server, no database, no account. Works offline.

**Known company context injection**
50+ well-known companies (Figma, Stripe, Linear, Notion, Loom, Apollo, Anthropic...) have hardcoded market perception and competitive tension injected into the Claude prompt. Generating positioning for a real company you recognise produces sharper, more contextually aware output than a blank-slate prompt would.

**Ranked multi-select chips**
Selecting differentiators and problems shows a numbered badge (1, 2, 3) in the order you picked them. The prompt treats first selection as highest priority. Order changes the output — subtle but important for getting specific results rather than generic ones.

---

## Tech stack

| | |
|--|--|
| **Language** | Vanilla JavaScript — no framework |
| **Styling** | Plain CSS — Grid, Flexbox, CSS variables |
| **AI** | Anthropic Claude (claude-sonnet-4) |
| **Proxy** | Cloudflare Worker (free tier API calls) |
| **PDF export** | html2pdf.js |
| **Hosting** | GitHub Pages |
| **Storage** | None — localStorage for run tracking, URL hash for sharing |

---

## Running locally

No install needed. Just open the file:

```bash
open index.html
```

To use the AI features you'll need an Anthropic API key — paste it in the app when prompted, or set up your own Cloudflare Worker proxy.

---

## Prompt design

The Claude prompt is written to be diagnostic, not descriptive:

> *"You are a world-class brand strategist. Every output must feel tailor-made, not generic. Prefer sharp strategic interpretation over factual description. You are not summarising — you are diagnosing. Say what others won't."*

Output is strict JSON (14 fields). The prompt explicitly bans em-dashes and sentence-starting dashes — a small constraint that meaningfully changes the writing style toward confident, direct copy.

---

Built to scratch my own itch. Feedback welcome.
