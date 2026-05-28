# Mod Construction MVP — Design System Notes

Since the full component library couldn't be shared, this describes the target aesthetic so the MVP matches our existing products (Jetson and the Parallel Strategies platform). Build clean, modern, production-grade — not wireframe-y.

## Overall feel

Light, airy, professional enterprise SaaS. White and very-light-gray (#F7F8FA-ish) backgrounds. Rounded corners (8–12px) on cards and controls. Subtle borders (#E5E7EB) and soft shadows. Generous whitespace. Nothing cramped, nothing flat-gray.

## Color

- **Background:** white / very light gray
- **Primary accent:** a confident blue (e.g. #2563EB / blue-600) for active tabs, primary buttons, key figures, and the active state of the company toggle. (Alternatively the Parallel Strategies red, but blue reads more enterprise — recommend blue.)
- **Status colors:** green (#16A34A) for good/valid, amber (#D97706) for warning/expiring/pending, red (#DC2626) for error/expired/blocked.
- **Text:** near-black (#111827) for headings, gray-600/700 for body and secondary text.

## Typography

Clean sans-serif — Inter is ideal. Bold, prominent headings. Comfortable body text. Numbers should feel precise and slightly emphasized (medium/semibold weight), especially dollar figures and percentages.

## Key recurring components

**Top navigation bar:** White bar, logo left, centered module tabs (active tab in accent color with an underline or pill), right side has a primary action button (filled accent, e.g. "+ New"), a bell icon, and a circular user avatar.

**Company toggle (signature element):** A rounded, pill-shaped segmented control. Inactive segments are plain text; the active segment has a filled accent (or white-on-accent) pill background. Sits top-right of data views. Options: All · Mod Design · Mod Construction · Mod Partners. Must actually filter the data when clicked.

**Stat cards:** Small rounded cards in a row — a label (gray, small), a big bold number, optionally a small trend or sub-note. Warning stats (like expiring COIs) use the amber accent.

**Module cards:** Larger rounded cards with a soft-colored icon tile (rounded square, light tint of accent), a bold title, a one-line gray description, and a small live-looking stat or badge. Hover state lifts slightly. Clicking navigates to that module.

**Data tables:** Clean rows, light row separators, sortable column headers (with up/down arrows), comfortable row height. Status shown as colored pill badges. Support an expandable row or row-click-to-open-detail pattern. A toolbar above the table with search, filters, and an export button (export can be cosmetic).

**Status badges:** Small rounded pills with a tinted background and matching text color — green "Valid," amber "Expiring," red "Expired"/"Blocked," neutral gray for inactive.

**AI assistant panel:** A right-side panel (can slide in/out, ~380–420px wide). Top: a small assistant icon and a friendly heading ("Hello — I'm your Mod assistant"). A short subtitle ("Ask me anything"). A few suggested-prompt chips (rounded, bordered, clickable). A scrollable message area. Bottom: a chat input with a "/" or placeholder text, a mic icon for voice, and a send button. Messages from the assistant feel conversational and specific.

**Document view (sworn statement):** Looks like a real document — a header band with company name, a clean line-item table, totals. The AI verification banner sits prominently at the top: green success state or red/amber error state with a specific explanation.

## Interaction polish

- Hover states on everything clickable.
- The company toggle visibly re-filters data (the demo's wow moment for the multi-entity story).
- Live math on the payment screen — editing a % updates the dollars in real time.
- The Concierge returns its answer with a tiny "typing" delay (200–600ms) so it feels real, then the specific answer appears.
- Smooth slide for the assistant panel.

## What to avoid

- Gray boxes / Lorem ipsum / obvious placeholder vibes.
- Cluttered, cramped layouts.
- Generic Bootstrap-y look. This should feel custom and premium — these clients build luxury homes; the tool should feel luxury too.
