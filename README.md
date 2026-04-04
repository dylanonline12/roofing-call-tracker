# Roofing Call Tracker

A lightweight, single-file call tracking app for Dylan's roofing company outreach. Built with plain HTML/JS and Firebase Realtime Database so your data syncs across every device — phone, laptop, tablet — in real time.

---

## What it does

- Tracks every roofing company you're calling through the Day 1 → 3 → 5 → 7 follow-up cadence
- Auto-calculates next follow-up dates so you never have to think about it
- Shows overdue calls, today's calls, and upcoming calls right on the dashboard
- Logs call outcomes (no answer, gatekeeper blocked, reached owner, demo booked, etc.)
- Built-in script reference tab so you always have the right words ready
- Syncs live across all your devices via Firebase
- Works in light and dark mode automatically

---

## Setup — step by step

### Step 1: Create a free Firebase project

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project**
3. Name it anything (e.g. `roofing-tracker`)
4. Disable Google Analytics if prompted (you don't need it)
5. Click **Create project**

### Step 2: Set up Realtime Database

1. In your Firebase project, click **Build** in the left sidebar → **Realtime Database**
2. Click **Create Database**
3. Choose your region (US is fine)
4. When asked about security rules, select **Start in test mode** — this lets the app read/write freely while you're getting started. You can lock it down later (see Security section below).
5. Click **Enable**

### Step 3: Get your config values

1. In your Firebase project, click the **gear icon** (top left) → **Project settings**
2. Scroll down to **Your apps**
3. Click the **</>** (web) icon to register a web app
4. Name it anything and click **Register app**
5. You'll see a `firebaseConfig` object like this:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-app.firebaseapp.com",
  databaseURL: "https://your-app-default-rtdb.firebaseio.com",
  projectId: "your-app-id",
  ...
};
```

6. Copy those four values — `apiKey`, `authDomain`, `projectId`, and `databaseURL`

### Step 4: Run the app

**Option A — Open locally (simplest)**
Just open `index.html` in any browser. On first load it will ask for your Firebase config. Paste in the four values and hit **Save and connect**. Done.

**Option B — Host on GitHub Pages (access from any device)**

1. Push this repo to GitHub (see below)
2. Go to your repo → **Settings** → **Pages**
3. Under **Branch**, select `main` and folder `/root`
4. Click **Save**
5. GitHub will give you a URL like `https://yourusername.github.io/roofing-call-tracker`
6. Open that URL on any device, enter your Firebase config once, and you're live

---

## Push to GitHub

If you're new to GitHub:

```bash
# 1. Install git if you haven't: https://git-scm.com/downloads

# 2. Create a new repo on github.com (click + → New repository)
#    Name it: roofing-call-tracker
#    Set it to Public (required for free GitHub Pages)

# 3. In this folder, run:
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/roofing-call-tracker.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

---

## Security (optional but recommended)

When you're ready to lock down your Firebase database so only you can access it, update your Realtime Database rules:

1. Go to Firebase Console → Realtime Database → **Rules**
2. Replace the rules with:

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

Then add Firebase Authentication to the app (Email/Password is simplest). For now, test mode is fine if you're the only one who knows the URL.

---

## How the call cadence works

| Call | When |
|------|------|
| Day 1 | Date you enter when adding the company |
| Day 3 | 2 days after Day 1 |
| Day 5 | 2 days after Day 3 |
| Day 7 | 2 days after Day 5 |

The app calculates these automatically from the date you log each call. Once all 4 calls are logged, the company is marked as loop closed.

---

## File structure

```
roofing-call-tracker/
├── index.html    ← the entire app (one file)
└── README.md     ← this file
```

Everything is in `index.html`. No build step, no dependencies to install, no Node.js required. Just open it.

---

## Troubleshooting

**"Firebase connection failed"** — Double-check that you copied all four config values correctly, especially `databaseURL`. It should end in `.firebaseio.com`.

**Data not syncing** — Make sure your Realtime Database is in test mode (or you've set up auth). Check the Firebase Console → Realtime Database to see if data is appearing there.

**Blank page on GitHub Pages** — Make sure the repo is public and Pages is set to the `main` branch, root folder.
