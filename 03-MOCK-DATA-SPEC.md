# Mod Construction MVP — Mock Data Spec

All data is fake but should feel real and construction-flavored. Build these as TypeScript/JSON files in a `/data` folder and import directly. Numbers should look precise (not round). Give each company its own slice so the company toggle has something to filter.

## The three companies (entities)

```
companies = [
  { id: "all",          name: "All",              color: "neutral" },
  { id: "design",       name: "Mod Design",       color: "blue" },
  { id: "construction", name: "Mod Construction", color: "green" },
  { id: "partners",     name: "Mod Partners",     color: "purple" },
]
```

Every project, contractor, request, and document carries a `companyId` so toggling filters them.

## Dashboard stats (per company)

| Metric | All | Mod Design | Mod Construction | Mod Partners |
|---|---|---|---|---|
| Active Projects | 12 | 4 | 6 | 2 |
| Contractors | 84 | 21 | 52 | 11 |
| Open Payment Requests | 23 | 6 | 14 | 3 |
| COIs Expiring Soon | 3 | 1 | 2 | 0 |

## Projects (sample set)

Use luxury/notable-sounding but clearly fictional project names. Examples:

```
projects = [
  { id: "p1", name: "Yellowstone Club Residence", company: "construction",
    client: "Private Client A", address: "Big Sky, MT",
    contractValue: 6100000, paidToDate: 5240000, percentComplete: 86 },
  { id: "p2", name: "Lake Forest Estate", company: "construction",
    client: "Private Client B", address: "Lake Forest, IL",
    contractValue: 3450000, paidToDate: 1380000, percentComplete: 40 },
  { id: "p3", name: "Gold Coast Penthouse — Interiors", company: "design",
    client: "Private Client C", address: "Chicago, IL",
    contractValue: 890000, paidToDate: 712000, percentComplete: 80 },
  { id: "p4", name: "Winnetka New Build", company: "construction",
    client: "Private Client D", address: "Winnetka, IL",
    contractValue: 4200000, paidToDate: 945000, percentComplete: 22 },
  { id: "p5", name: "Aspen Retreat — Design", company: "design",
    client: "Private Client E", address: "Aspen, CO",
    contractValue: 1250000, paidToDate: 1250000, percentComplete: 100 },
  { id: "p6", name: "Hinsdale Renovation", company: "partners",
    client: "Private Client F", address: "Hinsdale, IL",
    contractValue: 760000, paidToDate: 228000, percentComplete: 30 },
]
```

(Keep client names generic/anonymized — "Private Client A," etc. — to reinforce the discretion/privacy theme that matters to this customer.)

## Contractors (sample set)

Each contractor has compliance status that may differ per company. Include at least one with an expiring COI and one with a missing/expired doc to demonstrate the guardrail.

```
contractors = [
  { id: "c1", name: "Apex Plumbing", trade: "Plumbing",
    contact: { name: "Tom Reilly", phone: "(312) 555-0142", email: "tom@apexplumbing.com" },
    coi: { construction: { status: "valid", through: "2026-09-15" },
           design: { status: "expired", through: "2026-03-01" } },
    w9: "on_file", subAgreement: "signed",
    projects: ["p1", "p3"] },

  { id: "c2", name: "Meridian Electric", trade: "Electrical",
    contact: { name: "Dana Cole", phone: "(312) 555-0188", email: "dana@meridianelec.com" },
    coi: { construction: { status: "expiring", through: "2026-06-09" } },
    w9: "on_file", subAgreement: "signed",
    projects: ["p1", "p2", "p4"] },

  { id: "c3", name: "Heritage Millwork", trade: "Custom Millwork",
    contact: { name: "Sam Ford", phone: "(847) 555-0210", email: "sam@heritagemill.com" },
    coi: { construction: { status: "valid", through: "2027-01-20" } },
    w9: "missing", subAgreement: "pending",
    projects: ["p2", "p3"] },

  { id: "c4", name: "Crystal Glass & Glazing", trade: "Glazing",
    contact: { name: "Lee Park", phone: "(312) 555-0273", email: "lee@crystalglass.com" },
    coi: { design: { status: "valid", through: "2026-11-30" } },
    w9: "on_file", subAgreement: "signed",
    projects: ["p3", "p5"] },
]
```

Status badge logic:
- COI `valid` → green "Valid through [date]"
- COI `expiring` (within ~30 days) → amber "Expires [date]"
- COI `expired` → red "Expired"
- W-9 `on_file` → green "On File"; `missing` → red "Missing"
- Sub Agreement `signed` → green; `pending` → amber

Guardrail: any contractor with an expired COI, missing W-9, or pending agreement = payment requests **blocked** for the affected company.

## Payment requests (the Wednesday view)

```
requests = [
  { id: "r1", contractor: "Apex Plumbing", project: "p1", company: "construction",
    contractAmount: 420000, percentComplete: 86, retainagePct: 10,
    status: "ready" },
  { id: "r2", contractor: "Meridian Electric", project: "p2", company: "construction",
    contractAmount: 310000, percentComplete: 40, retainagePct: 10,
    status: "ready" },
  { id: "r3", contractor: "Heritage Millwork", project: "p2", company: "construction",
    contractAmount: 185000, percentComplete: 55, retainagePct: 10,
    status: "blocked", blockReason: "W-9 missing" },
  { id: "r4", contractor: "Crystal Glass & Glazing", project: "p3", company: "design",
    contractAmount: 96000, percentComplete: 80, retainagePct: 10,
    status: "ready" },
  { id: "r5", contractor: "Apex Plumbing", project: "p3", company: "design",
    contractAmount: 64000, percentComplete: 25, retainagePct: 10,
    status: "blocked", blockReason: "COI expired (Mod Design)" },
]
```

Math to show live (calculated, not hard-coded, so editing % updates it):
- Amount earned = contractAmount × (percentComplete / 100)
- Retainage = amount earned × (retainagePct / 100)
- Amount due this request = amount earned − retainage − (previously paid, if you track it)
- Balance = contractAmount − amount earned

## Sworn statement (for the AI math-check screen)

One clean example (all correct) and one with a planted error.

**Clean example — Yellowstone Club Residence (Mod Construction):**
Line items like: Site Prep, Framing, Plumbing Rough-In, Electrical Rough-In, HVAC, Insulation, Drywall, Tile Installation, Millwork, Glazing, Paint, Flooring, Fixtures, Final. Each with scheduled value, % complete, amount earned, retainage, amount due. All math correct. Banner: green "✓ Math verified — all 14 line items check out."

**Error example — Lake Forest Estate (Mod Construction):**
Same structure, but Line 7 (Tile Installation) has amount earned = $14,200 when scheduled value × % complete = $12,400. Banner: red "⚠ Math error found — Line 7 (Tile Installation): amount earned shows $14,200 but should be $12,400 based on 40% of $31,000. Off by $1,800."

## Concierge canned responses

Key these to the suggested chips. Make them specific and document-sourced-feeling:

```
"How much did we pay on the Yellowstone Club project?"
→ "Based on the most recent sworn contractor statement (dated May 12, 2026),
   you've paid $5,240,000 on the Yellowstone Club Residence to date, against a
   contract value of $6,100,000 — 86% complete, with $860,000 remaining.
   Want me to break it down by trade?"

"Which COIs expire this month?"
→ "Two certificates of insurance expire within 30 days:
   • Meridian Electric (Mod Construction) — expires June 9
   • Apex Plumbing (Mod Design) — already expired March 1; payments are blocked.
   Want me to draft renewal reminders?"

"What's our margin on the Lake Forest build?"
→ "On Lake Forest Estate, approved contract value is $3,450,000 against internal
   costs of $2,760,000 — a margin of 20% ($690,000). Labor is running at 18%
   margin, materials at 23%. Want the line-item breakdown?"

"Show me open payment requests for Mod Design"
→ "Mod Design has 6 open payment requests totaling $312,000 this week.
   One is blocked: Apex Plumbing's COI for Mod Design expired March 1.
   The other 5 are ready to approve."
```

## Tone for all mock content

Precise, confident, specific numbers. Construction vocabulary (sworn statements, retainage, draws, COI, W-9, % complete, line items, trades). Anonymized clients. Everything should make Karl and Maryna think "this person understands our business."
