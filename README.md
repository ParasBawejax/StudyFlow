# 📖 StudyFlow — Personal Study Planner PWA

> A fully offline-capable Progressive Web App for serious learners. Smart flashcard scheduling, deep resource tracking, Pomodoro timer, daily tasks, and study analytics — all in a single HTML file with zero dependencies to install.

[![PWA Ready](https://img.shields.io/badge/PWA-Ready-6366f1?style=flat-square&logo=pwa)](https://web.dev/progressive-web-apps/)
[![Offline First](https://img.shields.io/badge/Offline-First-10b981?style=flat-square)](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
[![No Build Step](https://img.shields.io/badge/Build%20Step-None-f59e0b?style=flat-square)](.)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](./LICENSE)

---

## ✨ Features

### 🧠 Smart Flashcard System (SM-2+ Algorithm)
- **Spaced repetition** powered by an enhanced SM-2 algorithm — the same science behind Anki
- **5-grade rating** system: Again / Hard / Good / Easy / Perfect, each adjusting your ease factor and next interval
- **Urgency scoring** automatically prioritises overdue cards, hard cards (low ease factor), and unstable new cards
- **Leech detection** — cards forgotten 4+ times in a row are flagged as leeches and surfaced for priority review
- **7-day forecast** shows how many cards are due each day so you can plan your review load
- **Per-subject review** — filter your review session to a single subject
- **Card health panel** — tracks Mature (21d+), Young, New, and Leech counts across your deck

### 📖 Deep Resource & Content Tracker
Organise any learning material in a **4-level hierarchy**:

```
Resource (Book / Video Course / Article / Podcast / Website)
  └── Section  (e.g. "Part I – Foundations")
        └── Chapter  (e.g. "Limits & Continuity")
              └── Topic  (e.g. "Chain Rule")
                    └── Subtopic  (e.g. "From first principles")
```

- Check off topics and subtopics as you complete them
- Ticking a **topic** auto-completes all its subtopics; ticking all subtopics auto-completes the parent
- Live **progress bars** roll up through every level to the resource
- Link resources to subjects for filtered views
- Supports any resource type: 📖 Book, 🎬 Video Course, 📄 Article/PDF, 🎧 Podcast, 🌐 Website

### ⏱️ Pomodoro Timer
- Customisable work duration (default 25 min)
- Automatic short breaks (5 min) and long breaks (15 min every 4 pomodoros)
- Visual ring timer with colour-coded modes (work / short break / long break)
- Automatically logs a study session when a work interval completes (if a subject is selected)

### ✅ Daily Task Manager
- Add tasks with a **date**, **priority** (High / Medium / Low), and optional subject link
- Tasks are grouped into **Today / Upcoming / Past** with completion progress bars
- Pending tasks and due card counts shown as notification badges in the app header
- Dashboard preview of today's top tasks

### 📚 Subjects
- Create subjects with custom colours and category tags (Exam / Certification, University / School, Language Learning, Other)
- Set a **daily study goal** per subject with live progress bar
- Per-subject stats: today's minutes, this week's minutes, flashcard count, resource count

### 📝 Study Sessions
- Log study sessions manually (or they're auto-logged by the Pomodoro timer)
- Track subject, date, duration, and an optional note

### 📈 Progress & Analytics
- Weekly study time bar chart
- Subject breakdown by percentage of weekly minutes
- Flashcard health panel (Mature / Young / New / Leech)
- Resource completion tracker
- All-time totals for sessions, study time, flashcards, resources, tasks, and subjects

---

## 🚀 Getting Started

### Option 1 — Netlify Drop (recommended, free, ~2 minutes)

1. Download and unzip `studyflow-pwa.zip`
2. Go to **[app.netlify.com/drop](https://app.netlify.com/drop)**
3. Drag the `studyflow-pwa` folder onto the page
4. Netlify gives you a live `https://` URL immediately (e.g. `studyflow-abc123.netlify.app`)
5. Open that URL on your device

### Option 2 — GitHub Pages (free, permanent URL)

1. Fork or clone this repository
2. Go to **Settings → Pages**
3. Set Source to `Deploy from a branch` → `main` → `/ (root)` → Save
4. Your app will be live at `https://yourusername.github.io/studyflow`

### Option 3 — Any static file host

Upload the three files (`index.html`, `manifest.json`, `sw.js`) to any host that serves static files over HTTPS:

- [Vercel](https://vercel.com) — drag and drop or CLI
- [Cloudflare Pages](https://pages.cloudflare.com)
- [Firebase Hosting](https://firebase.google.com/docs/hosting)
- Any shared web hosting with file manager access

> ⚠️ The app must be served over **HTTPS** for the service worker and "Add to Home Screen" prompt to work. `localhost` is also fine for local development.

### Option 4 — Run locally

```bash
# Any simple static server works. Examples:

# Python 3
python3 -m http.server 8080

# Node.js (npx, no install needed)
npx serve .

# VS Code Live Server extension
# Right-click index.html → "Open with Live Server"
```

Then open `http://localhost:8080` in your browser.

---

## 📱 Installing on Android (Add to Home Screen)

Once the app is open in **Chrome on Android**:

1. Tap the **⋮ three-dot menu** (top right of Chrome)
2. Tap **"Add to Home screen"**
3. Tap **"Add"**

A **StudyFlow icon** will appear on your home screen. It opens full-screen with no browser bar — exactly like a native app.

> The app also shows an **"Install StudyFlow as an app"** banner automatically after a few seconds on Android Chrome.

### Installing on iOS (Safari)

1. Open the app URL in **Safari**
2. Tap the **Share button** (box with arrow pointing up)
3. Scroll down and tap **"Add to Home Screen"**
4. Tap **"Add"**

---

## 💾 Data Storage

All your data is saved to your **device's localStorage** — nothing is sent to any server. This means:

- ✅ Works completely offline after the first load
- ✅ Your data is private and stays on your device
- ✅ No account or sign-up required
- ⚠️ Clearing your browser data / site data will erase the app's data
- ⚠️ Data does not sync across devices (this is intentional — no server)

**To back up your data**, open the browser console and run:

```js
// Export all StudyFlow data
const backup = {};
['sf_subjects','sf_sessions','sf_flashcards','sf_resources','sf_tasks']
  .forEach(k => backup[k] = localStorage.getItem(k));
console.log(JSON.stringify(backup));
// Copy the output and save it somewhere safe
```

**To restore**:

```js
const backup = /* paste your JSON here */;
Object.entries(backup).forEach(([k,v]) => localStorage.setItem(k,v));
location.reload();
```

---

## 🧬 How the SM-2+ Algorithm Works

StudyFlow uses an enhanced version of the [SM-2 spaced repetition algorithm](https://www.supermemo.com/en/blog/application-of-a-computer-to-improve-the-results-obtained-in-working-with-the-supermemo-method).

### Rating scale

| Rating | Label | What it means |
|--------|-------|---------------|
| 1 | 😖 Again | Completely forgot — resets card to day 1 |
| 2 | 😕 Hard | Recalled with difficulty — small interval increase |
| 3 | 🙂 Good | Recalled correctly — normal interval progression |
| 4 | 😄 Easy | Recalled easily — 1.3× interval bonus |
| 5 | 🤩 Perfect | Effortless recall — 1.5× interval bonus |

### Interval progression

| Reps | Next interval |
|------|---------------|
| 0 (new) | 1 day |
| 1 | 4 days |
| 2+ | Previous interval × ease factor |

### Ease Factor (EF)
- Starts at 2.5 for every card
- Adjusts after each rating: harder ratings decrease EF, easier ratings increase it
- Minimum EF is 1.3 (very hard cards plateau here)
- Maximum EF is 4.0

### Urgency Score (daily deck sort order)

Cards are sorted each day by an urgency score combining:

```
urgency = (overdue days × 10) + (2.5 - EF) × 5 + max(0, 5 - reps)
```

Plus a **+20 bonus for leeches**, so problem cards always surface early.

### Leeches

A card becomes a leech after **4+ consecutive "Again" ratings**. Leeches:
- Are flagged with a ⚠️ badge
- Appear at the top of your daily deck
- Can be reviewed in isolation via the "Leeches" button
- Can be reset once you've properly learned the card

---

## 📁 File Structure

```
studyflow-pwa/
├── index.html      # The entire app — React + all logic, embedded via Babel standalone
├── manifest.json   # PWA manifest — name, icons, theme colour, display mode
└── sw.js           # Service worker — offline caching strategy
```

The app has **no build step, no npm, no bundler**. React 18 and Babel are loaded from the Cloudflare CDN at runtime. On first load the service worker caches everything, so the app runs fully offline from then on.

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| UI Framework | React 18 (UMD build via CDN) |
| JSX Transform | Babel Standalone (browser-side) |
| Styling | Inline styles (no CSS framework) |
| State | React `useState` + `useMemo` + `useRef` |
| Persistence | `localStorage` (via custom `usePersist` hook) |
| Offline | Service Worker (Cache-first strategy) |
| Install | Web App Manifest + `beforeinstallprompt` |
| Algorithm | Custom SM-2+ implementation |
| Build | None — pure static files |

---

## 🗺️ Roadmap / Ideas

Pull requests welcome! Some ideas for future improvements:

- [ ] **Data export/import UI** — Export to JSON directly from the app without using the console
- [ ] **Cloud sync** — Optional sync via a simple backend or Supabase
- [ ] **Push notifications** — Remind you when cards are due (requires a backend or Web Push)
- [ ] **Markdown support** — Rich text in flashcard fronts/backs
- [ ] **Image flashcards** — Support for image-based cards
- [ ] **Statistics graphs** — Retention rate, review history, streak calendar
- [ ] **Custom card templates** — Cloze deletion, multi-answer cards
- [ ] **Pomodoro sounds** — Audio cue when a session ends
- [ ] **Dark/light theme toggle**
- [ ] **Multi-language UI**

---

## 🤝 Contributing

1. Fork the repository
2. Make your changes in `index.html` (the app lives entirely in the `<script type="text/babel">` block)
3. Test locally with any static server (see [Run locally](#option-4--run-locally) above)
4. Open a pull request with a clear description of what you changed and why

Since there's no build step, the feedback loop is instant — edit and refresh.

---

## 📄 License

MIT © 2025 — free to use, modify, and distribute.

---

## 🙏 Acknowledgements

- [SuperMemo SM-2 algorithm](https://www.supermemo.com/en/blog/application-of-a-computer-to-improve-the-results-obtained-in-working-with-the-supermemo-method) — the foundational spaced repetition research
- [Anki](https://apps.ankiweb.net) — inspiration for leech detection and card health concepts
- [React](https://react.dev) — UI library
- [Netlify](https://netlify.com) — recommended free hosting

---

<p align="center">Made with ☕ and spaced repetition</p>
