# JustiFlow IN — Judicial Optimization Platform

A high-performance, AI-assisted judicial docket management platform built for Indian High Courts, district registries, advocates, and litigants. JustiFlow IN consolidates case filing, hearing scheduling, document custody, transparency reporting, and analytics into a single role-aware workbench.

**🌐 Live Demo:** [https://nishanthhack.github.io/justiFlow/](https://nishanthhack.github.io/justiFlow/)

---

## What This Platform Does

JustiFlow IN solves five problems faced by Indian courts:

1. **Case backlog** — uses AI priority categorization (High / Time-bound / Routine) to predict disposal times and re-balance overloaded judges' dockets.
2. **Document custody** — e-filing intake with AI-generated NLP summaries for Vakalatnamas, FIRs, writ petitions, and affidavits.
3. **Scheduling chaos** — a visual cause-list calendar showing every hearing across court halls with stage tracking.
4. **Public transparency** — citizen-facing Advocate Performance Index, clearance rates, and case progress timelines.
5. **National compliance** — simulated NJDG (National Judicial Data Grid) synchronization with statewise statistics.

---

## Tech Stack

| Layer | Choice | Notes |
|---|---|---|
| Framework | React 19 | Functional components + hooks |
| Build tool | Vite 8 | With `@tailwindcss/vite` plugin |
| Styling | Tailwind CSS v4 | Class-based dark mode (`@custom-variant dark`) |
| Animations | Framer Motion 11 | Page transitions, modal entrance, stagger grids |
| Icons | Lucide React | Tree-shaken per-icon imports |
| Database | Supabase (optional) | Falls back to `localStorage` sandbox if no `.env` |
| Routing | HashRouter | GitHub Pages friendly (no 404 shim needed) |
| Deployment | GitHub Pages | Via `gh-pages` package |

---

## Project Structure

```
project/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/                  # Landing-page sections
│   │   ├── Navbar.jsx
│   │   ├── Hero.jsx
│   │   ├── Features.jsx
│   │   ├── Testimonials.jsx
│   │   ├── FAQ.jsx
│   │   ├── Contact.jsx
│   │   └── Footer.jsx
│   ├── data/
│   │   └── mockData.js              # Demo users, cases, hearings, documents
│   ├── pages/                       # Top-level views
│   │   ├── LandingPage.jsx
│   │   ├── Login.jsx
│   │   ├── Dashboard.jsx
│   │   ├── CaseDirectory.jsx
│   │   ├── HearingCalendar.jsx
│   │   ├── DocumentVault.jsx
│   │   ├── TransparencyPortal.jsx
│   │   ├── NJDGAnalytics.jsx
│   │   ├── AdminControls.jsx
│   │   └── SupabaseSetupInstructions.jsx
│   ├── utilities/
│   │   ├── supabaseClient.js        # Supabase init with fallback detection
│   │   └── dbService.js             # Abstraction layer: Supabase OR localStorage
│   ├── App.jsx                      # Root controller, modals, navigation shell
│   ├── index.css                    # Tailwind v4 + dark mode variant
│   └── main.jsx                     # Entry point (mounts React + HashRouter)
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

---

# 📖 Page-by-Page Guide

## 1. Landing Surface (Public — no login required)

### `LandingPage.jsx`
The marketing front door. Lays out the public hero, feature grid, social proof, FAQ, contact, and footer. Includes a floating **back-to-top** button that appears after 400 px scroll.

### `components/Navbar.jsx`
Sticky top navigation. Contains:
- Logo (clicking scrolls to top)
- Desktop section anchors: Features / Impact / FAQ / Contact
- Dark-mode toggle (Sun ↔ Moon icon swap, animated rotation)
- "Launch Console" CTA → opens Login page
- Mobile hamburger menu that slides down with height transition
- Keyboard-friendly focus rings on every interactive element

### `components/Hero.jsx`
Above-the-fold pitch. Spring-animated stagger entrance of:
- Badge: "Empowering the Indian Judiciary with AI"
- Headline with gradient text
- Subtext explaining docket balancing, NJDG sync, e-filing
- Twin CTAs (Launch Console / Explore Features)
- 4-stat panel (Disposal Rate, Sync Latency, Adjournment Limit, AI Balancing)
- Dashboard mockup with animated AI risk progress bar (0 → 80%)

### `components/Features.jsx`
Grid of platform capabilities (AI prioritization, secure e-filing, NJDG sync, etc.) with `whileInView` fade-up entrance and hover lift on each card.

### `components/Testimonials.jsx`
Quotes from retired judges, advocates, registrars. Scale-in entrance staggered by index.

### `components/FAQ.jsx`
Accordion of 5 common questions:
- What is a CNR and how is it generated?
- How does AI Priority Categorization work?
- Can lawyers upload documents securely?
- How is bench caseload optimized?
- Does this integrate with NJDG?

Icon rotates 45° (Plus → ×) on open. Content height + opacity transition.

### `components/Contact.jsx`
Two-panel section: indigo info card (email, phone, address) + glassmorphic form. Submit button cycles **Idle → Loading spinner → Success checkmark** with animated text swap.

### `components/Footer.jsx`
Quick links, compliance badges, copyright, social icons.

---

## 2. Authentication

### `pages/Login.jsx`
Two-column login screen.

- **Left panel** (hidden on mobile): JustiFlow branding, "Back to Website" arrow, and live database status indicator (Supabase Connected / Local Sandbox Mode)
- **Right panel**: email + password form, animated error reveal with height transition, loading spinner during auth, and **demo-role chips** (Admin / Judge / Clerk / Lawyer / Litigant) that autofill credentials with one click

Password for all demo accounts is `password`.

---

## 3. Authenticated Workspace

Once logged in, the user lands in the workspace shell (sidebar + top header + main viewport). The sidebar shows different navigation depending on role. All authed pages are **code-split** (React.lazy) and load on demand. Tab switching uses a fade-up transition.

### `pages/Dashboard.jsx` — "Console Home"
The role-aware home screen. Renders a different layout depending on `currentUser.role`:

| Role | What they see |
|---|---|
| **Admin** | National pendency stats, adjournment rate, disposal time, judge allocation, case-age bar chart, administrative alerts, and an "Optimize Benches" button |
| **Judge** | Active docket count, disposal yield, allowed adjournments, active injunctions, plus a priority-tracking table of every case assigned to them |
| **Clerk** | Pending Vakalatnamas, registry fees collected, summons to issue, bail bonds audited, and an intake queue showing pending case verifications |
| **Lawyer** | Active briefs count, upcoming hearings, four quick-action tiles (Upload Vakalatnama / File Interim Petition / Log Adjournment / Submit Evidence), and a hearing schedule sidebar |
| **Litigant** | 3 large action tiles (Pay Court Fee, Request Free Legal Aid via NALSA, Download Certified Orders) plus a vertical court-proceeding timeline per active case |

All stat cards stagger in with spring physics; cards lift on hover.

### `pages/CaseDirectory.jsx` — "Case Registry"
The case-search table. Features:
- Search by **CNR number** or **party title** (live filter)
- Filter dropdowns: **Priority** (All / High / Time-bound / Routine) and **Status** (All / Pending / Scheduled / Reserved for Orders / Disposed)
- Sortable column view showing CNR, title, type, priority badge, age category, adjournment count, filing date, status badge
- "e-File New Suit" button for Admin/Clerk/Lawyer roles → opens the Case Registration modal
- Disabled pagination placeholder

### `pages/HearingCalendar.jsx` — "Cause List Calendar"
Monthly calendar grid + selected-day sidebar.

- Left (2/3): full month view with prev/next navigation. Days with hearings show a colored indicator and up to 2 hearing chips inline (`+N more` if overflow). Click a day to view its full cause list.
- Right (1/3): selected-day details panel showing time, courtroom, case title, CNR, and hearing stage (Hearing / Arguments / Mediation / Orders)
- "Schedule Hearing" button for Admin/Clerk → opens the Hearing Scheduler modal with the selected date prefilled

### `pages/DocumentVault.jsx` — "Digital Case Vault & AI Intake"
Split into upload panel + listings panel.

- **Upload panel** (left): pick target case, document type (Vakalatnama / FIR Copy / Writ Petition / Affidavit / Interim Order), drop a PDF, watch the animated progress bar fill in 5 steps simulating a cryptographic-tunnel upload
- **Listings** (right top): every uploaded document with case linkage, filing date, type badge
- **AI Summary** (right bottom): click "AI Summary" on any document → a 1.5s NLP simulation runs and renders a monospace report with case record, classification, key arguments, statutory sections cited, and priority recommendation

### `pages/TransparencyPortal.jsx` — "Public Advocate Index"
Citizen-facing performance dashboard, no login required to view (mounted inside the authed shell for demo, but rendered logic is public).

- Two large metric cards: **National Case Clearance Index** (104.2%) and **Average Adjournment Ratio** (1.4 Days)
- **Advocate Performance Index (API)** table — ranked list of advocates with cases handled, success rate, average adjournments, and tier (A++ / A+ / A / B). Searchable by counsel name.
- Dark sidebar showing compliance status (Public Access: Authorized, DPDP Act Certified, NIC High Court Core verification source)

### `pages/NJDGAnalytics.jsx` — "National Judicial Data Grid"
Statewise statistical view simulating sync with the national database.

- **State selector** dropdown (All India / Maharashtra / Delhi / Karnataka / Tamil Nadu / Uttar Pradesh) — statistics recompute on change
- **Refresh button** with spinning icon to simulate live re-sync
- 4 KPI cards: Total Active Cases (with civil/criminal breakdown), Clearance Rate, Average Age of Cases, Digital Filings Ratio
- **Disposal Clearance Index** historical bar chart (Oct '25 → Feb '26) showing month-over-month improvement
- **Case Category Breakout**: Civil (CPC) 25%, Criminal (CrPC) 65%, Tax/Writ 6%, Family 4% — color-coded progress bars

### `pages/AdminControls.jsx` — "Admin Controls" (Admin only)
The bench-balancing operations console.

- **Bench Caseload Optimizer** (left, 2/3): shows every active judge with their current docket count (color-coded: Overloaded ≥ 4 cases / Optimal 1-3 / Available 0). Click "Analyze Docket Balancing" → 1.2s simulation → AI suggests case transfers from overloaded judges to underutilized ones. Click "Approve and Reallocate" to apply the transfers (persists via `dbService.updateCase`).
- **Authorized Registry Accounts** (right, 1/3): scrollable list of every user with role badge and email.

### `pages/SupabaseSetupInstructions.jsx` — "Database Settings"
Self-service Supabase wiring guide.

- **Left**: connection status indicator (green if `.env` credentials are present, amber if running in localStorage sandbox), step-by-step setup checklist, and `.env` template
- **Right**: full SQL schema for the 4 tables (users / cases / hearings / documents) with foreign keys and cascade deletes — one-click "Copy SQL" button
- "**Seed Live Supabase Database**" button — pushes the entire mock dataset (users, cases, hearings, documents) into your Supabase project in one click

---

## 4. Cross-cutting Modals (Triggered from any page via `App.jsx`)

All 4 modals fade their backdrop and spring-scale the content card on enter/exit.

| Modal | Triggered by | What it does |
|---|---|---|
| **Register New Case** | Clerk/Lawyer/Admin clicking "e-File New Suit" | Generates a CNR like `DLND02012345672026`, assigns priority, predicts disposal days based on priority |
| **Schedule Hearing** | Admin/Clerk on calendar day click | Picks case, date, time, courtroom, stage (Hearing / Arguments / Mediation / Orders) |
| **Upload Document** | Any role from quick-action tiles | Picks case, type (Vakalatnama / Interim Application / Adjournment / Evidence), filename |
| **Pay e-Challan** | Litigant from Citizen Console | Mock card payment ₹1,500 with success animation |

Each successful submit triggers a **toast notification** (bottom-right) confirming the action.

---

## 🔑 Demo Access Credentials

Click any role chip on the login screen to autofill, or enter manually. Password is `password` for all.

| Role | Email | Capabilities |
|---|---|---|
| **System Admin** | `admin@hc.gov.in` | Run AI Docket Balancing, view administrative metrics, manage user registry |
| **Presiding Judge** | `rsharma@courts.gov.in` | View assigned trials, examine case risk priorities, verify orders |
| **Registry Clerk** | `registry@courts.gov.in` | Verify Vakalatnamas, register cases (issue CNR), dispatch notices |
| **Counsel / Lawyer** | `vikram@lawchambers.in` | e-File suits, upload affidavits, request adjournments, submit evidence |
| **Citizen / Litigant** | `rahul@email.com` | Pay filing fees, track case progress timeline, request NALSA legal aid |

---

## ⚡ Setup & Local Development

```bash
# 1. Install dependencies
npm install

# 2. Run dev server (http://localhost:5173)
npm run dev

# 3. Production build
npm run build

# 4. Preview the production build locally
npm run preview
```

---

## 🗄️ Supabase Integration (Optional)

Out of the box, JustiFlow runs in **Local Sandbox Mode** using `localStorage`. To switch to a live Postgres backend:

1. Create a free project at [https://supabase.com](https://supabase.com).
2. Open **SQL Editor** → paste the schema from the in-app **Database Settings** page → Run.
3. Create `.env` in the project root:
   ```env
   VITE_SUPABASE_URL=https://your-project-id.supabase.co
   VITE_SUPABASE_ANON_KEY=your-anon-api-key
   ```
4. Restart `npm run dev`, open the **Database Settings** sidebar tab, click **Seed Live Supabase Database**.

---

## 🚀 Deployment

### GitHub Pages (currently used)
```bash
npm run deploy
```
This rebuilds and pushes `dist/` to the `gh-pages` branch. The base path `/justiFlow/` is set in `vite.config.js`.

### Vercel
```bash
npm install -g vercel
vercel
```
Add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` under Project Settings → Environment Variables.

### Netlify
Drag `/dist` onto the Netlify dashboard, or link the repo with build command `npm run build` and publish directory `dist`.

---

## ✨ Animation & UX Polish

Animations powered by **Framer Motion 11**:

- **Modal entrance**: backdrop fade + content spring scale (stiffness 280, damping 24)
- **Mobile menu**: height + opacity slide (0.25s ease-out)
- **Tab switching**: fade-up between authed pages (0.2s)
- **Stat cards**: stagger entrance with `staggerChildren: 0.08`, hover lift `-4 px`
- **FAQ icon**: 45° rotation transition instead of swap
- **Form submits**: animated spinner → success checkmark transition
- **Icon swaps**: dark-mode toggle and mobile menu icons rotate on state change
- **Toast notifications**: spring slide-in from bottom-right after every modal success
- **Code-splitting**: `React.lazy` per authed page — landing page no longer ships dashboard code (main bundle 701 KB → 630 KB / 180 KB gzipped)

---

## License & Compliance

- National e-Governance Plan (NeGP) compliant
- DPDP Act 2023 certified data handling
- Compatible with NJDG (National Judicial Data Grid) standards
- © Department of Justice, India
