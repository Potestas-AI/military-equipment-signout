# AI-Powered Equipment Sign-Out System
### Built by Joseph Cirello, U.S. Army veteran. Brought to you by [Potestas AI](https://potestasai.com).

A real-time, AI-powered military equipment dispatch and readiness board. Scan a Soldier's
name tape and rank insignia with a phone camera, sign equipment in or out by voice or tap,
and a live big-screen command board updates instantly across all devices — no wires, no app
install, no server to run.

Built as a proof of concept during Annual Training to show what a single Soldier with AI
knowledge can build that entire teams haven't thought to make yet.

---

## What It Does

- **AI Name Tape Scanner** — point a phone camera at a Soldier's uniform, AI reads the name
  tape and rank insignia in one shot. No CAC card required, no personal data stored.
- **Live Readiness Board** — a full-screen tactical display showing every item's status,
  who has it, FMC rate gauge, and category breakdowns. Updates in real time across all devices.
- **Voice Controlled** — the system speaks confirmations and closes with "Safe travels" for
  vehicles. Hands-free operation in a live motor pool environment.
- **Voice Input** — say the equipment name instead of tapping the dropdown.
- **Flexible Sign-In** — any Soldier can return any item. Proxy returns are logged
  automatically for accountability without blocking the line.
- **Manual Entry Fallback** — if the camera fails, type rank and name to keep moving.
- **Admin Override** — add new equipment on the fly, deadline (NMC) any item instantly.
- **Transaction Feed** — live scrolling log of every sign-out, sign-in, proxy return, and
  admin action with timestamps.

---

## Demo

**Board view** (TV / laptop big screen):
`https://your-netlify-site.netlify.app`

**Scanner view** (phone):
`https://your-netlify-site.netlify.app?view=scan`

The two views talk to each other live through Firebase. Your phone signs equipment out, the
board updates in under a second. No page reload. No cable between devices.

---

## Tech Stack

- **Single HTML file** — the entire app is one file. No framework, no build step, no
  dependencies to install.
- **Firebase Realtime Database** — free tier handles all real-time sync between devices.
- **Anthropic Claude API** — reads name tapes and rank insignia from camera images using
  Claude Haiku vision. Each scan costs a fraction of a penny.
- **Web Speech API** — built-in browser speech synthesis and recognition. No third-party
  voice service needed.
- **Netlify** — free static hosting. Drag the file, get a live HTTPS link.

---

## Setup (About 20 Minutes)

### Step 1 — Firebase (your free real-time backend)

1. Go to `console.firebase.google.com` → **Add project**
2. Name it anything (e.g. `motorpool`)
3. **Build → Realtime Database → Create Database** → choose a location → **Start in test mode**
4. Click the **`</>`** web icon on the project home to register a web app
5. Give it a nickname, click **Register app**
6. Copy the `firebaseConfig` object — you need all five fields including `databaseURL`

### Step 2 — Anthropic API Key (the AI scanner brain)

1. Go to `console.anthropic.com` → **API Keys** → **Create Key**
2. Copy the key — you only see it once
3. Add a small credit balance — each scan costs less than a penny

### Step 3 — Configure the File

Open `motorpool_GITHUB.html` in any text editor. At the top, fill in the two config blocks:

```javascript
const FIREBASE_CONFIG = {
  apiKey: "YOUR_FIREBASE_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT_ID",
  appId: "YOUR_FIREBASE_APP_ID"
};

const ANTHROPIC_API_KEY = "YOUR_ANTHROPIC_API_KEY";
```

> **Security note:** This key lives in the browser. Fine for a controlled demo on your own
> devices. Do not publish this file publicly with a real key in it. For production use,
> proxy the Anthropic call through a serverless function.

### Step 4 — Customize Your Equipment List

Near the top of the file, find the `SEED` array. Each item follows this format:

```javascript
{id:'unique_id', cat:'VEHICLE', name:'BUMPER NUMBER', sub:'NOMENCLATURE'}
```

Categories available: `VEHICLE`, `GSA RENTAL`, `RADIO`, `BUILDING KEY`, `CONTROLLER`

Set `SEED_AS_ISSUED = true` to pre-load items as already signed out with real names.
Set `SEED_AS_ISSUED = false` for a clean all-available starting board.

Bump `SEED_VERSION` by 1 any time you want to force a fresh reseed from your list.

### Step 5 — Deploy

1. Go to `app.netlify.com/drop`
2. Drag your configured file onto the page
3. Get a live HTTPS link instantly — no account required for a temporary deploy,
   or sign up free to make it permanent

### Step 6 — Open Both Views

| Screen | URL |
|---|---|
| Board (TV / laptop) | `https://your-site.netlify.app` |
| Scanner (phone) | `https://your-site.netlify.app?view=scan` |

Look for the **green radar dot** top-right on both screens. Green = connected and live.

---

## How to Use

### Signing Equipment Out
1. Tap **Activate Scanner** on the phone — allow camera on first use
2. Aim at the Soldier's chest — name tape and rank tab in frame
3. Tap **Capture Name Tape** — AI identifies rank and last name, speaks the confirmation
4. Select equipment from dropdown or tap **Speak Equipment** and say the name
5. Tap **Sign Out** — board updates, voice says *"Transaction recorded. Safe travels."*

### Signing Equipment In
1. Scan the returning Soldier's name tape
2. Select their item (signed-out items appear at the top of the dropdown)
3. Tap **Sign In** — item goes green on the board, transaction logged

### Manual Entry (camera backup)
Tap **Enter Manually (camera down)**, type rank and name, tap **Confirm** — proceeds
exactly like a normal scan from that point.

---

## Reading the Readiness Board

| Stat | Meaning |
|---|---|
| On Hand | Total equipment tracked |
| Mission Capable | Everything not deadlined |
| Signed Out | Currently dispatched |
| Deadlined | Down / NMC |
| FMC Rate | Gauge: green ≥90%, amber, red |

Filter by category (All / Vehicles / GSA / Radios / Keys / Ctrl) using the filter buttons.

---

## Voice Troubleshooting

- Use **Google Chrome** on Android — best Web Speech API support
- Turn **media volume up** — voice plays through media, not ringer
- Take phone off **silent / Do Not Disturb**
- Tap the screen once before scanning — browsers require a user interaction to unlock audio
- Check the **Voice Announcements toggle** is ON (green) in the scanner

---

## Extending This

Some ideas for where to take this next:

- **Serverless API proxy** — move the Anthropic key to a Netlify Function or Cloudflare
  Worker so it never lives in the browser
- **Authentication** — add Firebase Auth so only your unit can access the board
- **Export / reports** — write the transaction log to Firestore and generate a DA-style
  report on demand
- **Additional equipment types** — weapons, comms gear, night vision — the category system
  is already built for it
- **Mobile PWA** — add a manifest and service worker so the scanner installs as an app
  icon on the phone home screen

---

## About the Author

**Joseph Cirello** is a U.S. Army veteran and the founder of **Potestas AI**, an independent
AI testing company in Providence, Rhode Island.

This project was built during a 3-week Annual Training period as a proof of concept
demonstrating that individual Soldiers with AI knowledge can build operational tools that
solve real logistics problems — without a development team, without a budget, and without
waiting for a program of record.

Brought to you by Potestas AI. Website: [potestasai.com](https://potestasai.com)

---

## License

MIT License — free to use, modify, and distribute. Attribution appreciated but not required.

If you build something useful with this, reach out. Always interested in what the force
is doing with AI.

---

*"No Soldiers are building products with AI. I want to show I can."*
*— CW3 Cirello, Annual Training 2025*

