# Mod Construction MVP — Screen Map

This is the build checklist. Build these five screens in priority order. Each is described with layout, components, and what data it shows. All data is mock (see Mock Data Spec).

A persistent **top navigation bar** appears on every screen:
- Left: "MOD" logo/wordmark
- Center: module tabs — **Hub · Payments · Projects · Documents · Concierge**
- Right: a primary action button ("+ New") , a notification bell, and a user avatar
- The active tab is highlighted in the accent color

A persistent **company toggle** (the pill segmented control) appears at the top-right of every data view:
**All · Mod Design · Mod Construction · Mod Partners**
Toggling it filters the visible data. "All" is the default and shows everything. This is the signature feature — make it prominent and make it actually filter the mock data.

---

## SCREEN 1 — Hub Dashboard (landing page)

**Purpose:** The "one place for everything" landing view. Establishes the multi-company concept immediately.

**Layout:**
- Page title: "Dashboard" with subtitle "All your companies, all your work — in one place."
- The company toggle, top-right.
- A row of summary stat cards across the top: e.g. "Active Projects: 12", "Contractors: 84", "Open Payment Requests: 23", "COIs Expiring Soon: 3" (the last one in a warning color). These numbers change when the company toggle changes.
- Below: a set of **module cards**, each clickable, leading to that module:
  - **Hub** — "Every record, connected." stat: "84 contacts"
  - **Payments** — "Requests, compliance, and verification." stat: "23 open requests"
  - **Projects** — "Proposals, contracts, and progress." stat: "12 active"
  - **Documents** — "Generated and verified." stat: "47 this month"
  - **Concierge** — "Ask anything." (this card opens the assistant panel)
- A right-side slide-in assistant panel is accessible from anywhere (a button or the Concierge card opens it).

**Key behavior:** When you switch the company toggle, the stat cards and numbers update to reflect that company's slice. (Mock this — each company has its own number set.)

---

## SCREEN 2 — Contractor Card

**Purpose:** Shows Maryna's "contact card" wish — see everything about a contractor in one place, including compliance status.

**How to reach it:** From a contractors list (a simple table under Hub or Payments) — clicking a contractor row opens their card. Build a basic contractors table too (name, company, COI status badge, W-9 badge, active projects count).

**The card layout:**
- Header: contractor name, trade (e.g. "Apex Plumbing — Plumbing"), and a primary contact (phone/email with quick-action icons).
- A **compliance strip** of status badges:
  - COI (Certificate of Insurance): badge shows "Valid through [date]" in green, or "Expires in 12 days" in amber, or "Expired" in red.
  - W-9: green "On File" or red "Missing"
  - Signed Sub Agreement: green "Signed" or amber "Pending"
- Because Mod runs three companies, show that this contractor may have **separate COIs per company** — e.g. a small sub-table: "Mod Construction — COI valid through 09/2026", "Mod Design — COI expired." This reinforces the multi-entity reality.
- A **linked projects** section: list of projects this contractor is on, across all three companies, each with a small company tag, contract amount, amount paid, and balance.
- An **outstanding items** callout if anything's wrong: "⚠ COI for Mod Design expired — payments blocked until updated."

**Key behavior:** The "payments blocked" guardrail is a visible, real feeling. If a contractor is missing a doc, show that their payment requests are blocked.

---

## SCREEN 3 — Payments / Requests

**Purpose:** Recreate Maryna's weekly "Wednesday request" process, automated.

**Layout:**
- Page title: "Payment Requests" with the company toggle top-right.
- A data table, one row per contractor request, with columns:
  - Contractor name
  - Project (with a small company tag)
  - Contract amount
  - % Complete (shown as a small progress bar or editable percentage)
  - Amount this request
  - Retainage (10% held back — shown calculated)
  - Balance remaining
  - Compliance (a small icon: green check if COI/W-9 good, red lock if blocked)
  - Status badge: "Ready," "Blocked," "Approved"
- Rows that are "Blocked" (missing COI/W-9) are visually distinct — a red lock icon and a muted/disabled feel, with a tooltip "Missing valid COI."
- A summary bar at top: "Total requested this week: $XXX,XXX · 3 blocked · 20 ready"

**Key behavior:**
- Changing the % complete on a row auto-recalculates the amount and balance (show the math working live — this is a big selling point).
- The retainage (10%) is automatically calculated and shown.
- Blocked rows can't be approved — demonstrates the compliance guardrail.

---

## SCREEN 4 — Sworn Statement + AI Math Check

**Purpose:** Karl's #1 ask — AI checking the math on sworn contractor statements.

**Layout:**
- A document view that looks like a generated sworn contractor statement: company header (with the relevant company's name/logo — e.g. "Mod Construction"), customer name, project address, and a line-item table (description, scheduled value, % complete, amount earned, retainage, amount due).
- At the top of the document, a prominent **AI verification banner**:
  - Green state: "✓ Math verified — all 14 line items check out." with a subtle "Verified by AI · [timestamp]"
  - OR a red/amber state on a different example: "⚠ Math error found — Line 7 (Tile Installation): amount earned should be $12,400, not $14,200. Off by $1,800." This is the money moment — show the AI catching a real error with a specific, reasoned explanation.
- A "Download PDF" button (doesn't need to actually generate — just present for realism) and a "Generate from template" feel.
- Show that the template carries the right company branding (tie back to multi-entity).

**Key behavior:** Have at least one example showing the AI catching an error with a specific line, the wrong number, the right number, and the dollar difference. That specificity is what sells it.

---

## SCREEN 5 — Concierge (AI Assistant)

**Purpose:** The emotional centerpiece. Recreate Karl's origin story.

**Layout:**
- A right-side panel that slides in (and can be opened from anywhere). Matches the assistant-panel aesthetic: friendly intro at top ("Hello — I'm your Mod assistant. Ask me anything."), a robot/assistant icon, suggested prompt chips, and a chat input at the bottom with a mic icon for voice.
- Suggested prompt chips:
  - "How much did we pay on the Yellowstone Club project?"
  - "Which COIs expire this month?"
  - "What's our margin on the Lake Forest build?"
  - "Show me open payment requests for Mod Design"
- When the user clicks a chip or types one of the expected questions, the assistant returns a **specific, document-sourced-feeling answer.** These can be scripted/canned responses keyed to the expected prompts. Example response to the Yellowstone question:
  > "Based on the most recent sworn contractor statement (dated May 12, 2026), you've paid **$5,240,000** on the Yellowstone Club project to date, against a contract value of $6,100,000 — that's 86% complete, with $860,000 remaining. Want me to break it down by trade?"
- The answer should feel like it read a real document and pulled the number — specific figures, a source reference, and a follow-up offer.
- Bonus (optional): wire the input to the Anthropic API so free-text questions get real answers over the mock data. If doing this, all API calls must be server-side; never expose a key in the client. If not, scripted answers for the four chips is sufficient for the demo.

**Key behavior:** Clicking "How much did we pay on the Yellowstone Club project?" must produce that instant, specific, confident answer. This is the single most important interaction in the entire prototype.

---

## Build order recommendation

1. Top nav + company toggle + overall shell (the frame everything sits in)
2. Hub Dashboard (Screen 1)
3. Concierge panel (Screen 5) — highest emotional payoff
4. Contractor Card (Screen 2)
5. Payments/Requests (Screen 3)
6. Sworn Statement + AI Math Check (Screen 4)

If time is short, screens 1, 5, and 2 alone tell the story. 3 and 4 deepen it.
