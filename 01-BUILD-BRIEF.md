# Mod Construction MVP — Build Brief

## What this is

A clickable, front-end-only prototype of a custom operations platform for **Mod Construction**, a luxury construction firm that operates three companies (Mod Design, Mod Construction, Mod Partners) on shared projects. This prototype is a **sales demo** — it will be shown to the client (Karl and his operations lead, Maryna) alongside a proposal. It does not need a real backend, real authentication, or real data persistence.

The goal is a polished, navigable demo that makes the client *feel* the product. It should look like a real, shipping product — not a wireframe.

## Who's it for / the emotional job

Karl runs the business and wants ONE place to find anything — and ideally to just *ask an AI* and get an answer. His exact origin story: a friend opened an AI app and asked "how much did I pay for the Yellowstone Club project?" and it instantly read the answer off a document. That moment is the heart of this demo. The prototype must recreate that feeling.

Maryna is the daily operator. She lives in spreadsheets today — tracking contractor insurance, payment requests, sworn statements, and project financials by hand. The demo should show her work becoming effortless.

## Hard constraints (read carefully)

- **Front-end only.** No database, no backend, no auth flows. Use local mock data (JSON files or in-component constants).
- **No browser storage** (localStorage/sessionStorage). Hold all state in React state. Data resets on refresh — that's fine for a demo.
- **Mock data only.** Realistic, construction-flavored fake data. No real client information.
- **It must be clickable and navigable**, not static mockups. The user should be able to move between screens, toggle the company filter, open records, and interact with the assistant.
- **Looks-real bar:** This should feel like a production SaaS app — clean, modern, confident. Not a prototype-y gray-box wireframe.

## Tech stack

- **Next.js (App Router) + React + TypeScript**
- **Tailwind CSS** for styling
- **shadcn/ui** component patterns where helpful (cards, tables, dialogs, badges, tabs)
- **lucide-react** for icons
- **recharts** for any charts/dashboards
- Mock data in `/data/*.ts` or `/data/*.json` files, imported directly into components

## Design language (match this aesthetic)

The client has seen two of our existing products; the MVP should share their visual DNA:

- **Clean, light, professional.** White/very-light-gray backgrounds, generous whitespace, rounded cards with subtle borders and soft shadows.
- **A primary accent color** used for active states, primary buttons, and key figures. (Use a confident blue or the Parallel Strategies red — pick one and stay consistent. Blue reads more "enterprise software"; recommend blue for this build.)
- **A top navigation bar** with the main module tabs, plus a prominent primary action button on the right (e.g. "Add New Job" style) and a user avatar.
- **A pill-style segmented toggle** in the top-right of data views for filtering — THIS IS CRITICAL (see Screen Map). Like a tab switcher with rounded pill background.
- **Dense, scannable data tables** with sortable column headers, status badges (colored pills like "Active," "Expired," "Pending"), and expandable rows.
- **A right-side AI assistant panel** that can slide in/out, with a friendly intro ("Hello, I'm [name]"), suggested prompt chips, and a chat input at the bottom with a voice-input mic icon.
- **A module-card dashboard** as a landing view — each module shown as a card with an icon, title, short description, and a few live-looking stats.

Typography: clean sans-serif (Inter or similar). Bold headers, comfortable body text. Numbers should feel precise and prominent.

## The five demo screens (priority order)

Full detail in the Screen Map doc. In short:

1. **Hub Dashboard** — landing page; module cards + the all-important COMPANY TOGGLE (All / Mod Design / Mod Construction / Mod Partners)
2. **Contractor Card** — a single contractor showing COI/W-9 status, linked projects, and outstanding items
3. **Payments / Requests** — the weekly payment request view with percentage-of-completion and compliance guardrails
4. **Sworn Statement + AI Math Check** — a generated document with an AI verification banner ("Math verified" / "Error found")
5. **Concierge (AI Assistant)** — the slide-in panel; recreates Karl's "how much did I pay for [project]?" moment

## The single most important moment

The **Concierge answering "How much did we pay on the Yellowstone Club project?"** with a real, specific, document-sourced answer. If only one thing is great, it's this. It can be scripted/canned (pre-written response keyed to expected questions) — it does not need a live AI connection for the demo, though a live connection is a bonus. The response should feel like it read an actual sworn contractor statement and pulled the number.

## What success looks like

Karl clicks through, toggles between his three companies, opens a contractor and sees their insurance status at a glance, looks at a payment request, sees the AI catch a math error on a sworn statement, then asks the assistant a question and gets an instant answer. He thinks: "This is exactly what I described. I want this."

## Out of scope (do NOT build)

- Real authentication / login (a fake login screen is optional eye candy, not required)
- Real data persistence or backend
- QuickBooks, banking, e-signature, time tracking integrations (these are future add-ons, not in the MVP)
- Mobile native app (responsive web is enough; phone-friendly is a plus)
- Settings/admin depth beyond what's visually suggested
