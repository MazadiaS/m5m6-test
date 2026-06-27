# AI/ML Test (Modules 4 & 5) — Setup Guide

A 40-question web test. Students answer in the browser; their results land in **your Google Sheet**.

Files in this folder:
- `index.html`, `styles.css`, `app.js`, `questions.json` → the website
- `Code.gs` → the Google Apps Script that writes to your Sheet
- `SETUP.md` → this file

---

## Part A — Make the Google Sheet (5 min)  *(you don't have one yet — do this first)*

1. Go to **sheets.google.com** → **Blank spreadsheet**.
2. Rename the first tab (bottom-left) to **`Results`** (right-click the tab → Rename).
3. Top menu: **Extensions → Apps Script**.
4. Delete whatever code is there. Open `Code.gs` from this folder, **copy all of it, paste it in.**
5. Click **Save** (the disk icon).
6. Click **Deploy → New deployment**.
   - Click the gear ⚙️ next to "Select type" → choose **Web app**.
   - **Description:** anything (e.g. "m4m5 test").
   - **Execute as:** *Me*.
   - **Who has access:** **Anyone**.  ← important, or students can't submit.
   - Click **Deploy**, then **Authorize access** (pick your Google account, allow).
7. Copy the **Web app URL** it gives you. It looks like:
   `https://script.google.com/macros/s/AKfyc..../exec`

> Test it: open that URL in a browser. You should see *"M4/M5 test endpoint is live."*

---

## Part B — Connect the website to your Sheet (1 min)

1. Open **`app.js`**.
2. At the very top, replace the placeholder with your Web app URL from Part A:
   ```js
   const WEBHOOK_URL = "https://script.google.com/macros/s/AKfyc..../exec";
   ```
3. Save the file.

---

## Part C — Put the test online with GitHub Pages (5 min)

1. Create a new GitHub repo (e.g. `m4m5-test`).
2. Upload **all the files in this folder** (index.html, styles.css, app.js, questions.json).
3. Repo **Settings → Pages → Branch: `main` / root → Save**.
4. After a minute you get a link like `https://YOURNAME.github.io/m4m5-test/`.
5. Share that link with your students.

> (To test locally first: in this folder run `python3 -m http.server 8000`, then open
> `http://localhost:8000`. Saving to the Sheet only works once `WEBHOOK_URL` is set.)

---

## How it works for students
1. Open the link → type name + group → **Start Test**.
2. Answer all 40 questions (one option each; they can change answers; a refresh keeps progress).
3. **Submit** → they see their score + a full review of right/wrong answers.
4. Their result is saved to your **Results** sheet automatically.

## What lands in your Sheet (one row per student)
| Timestamp | Name | Group | Score | Correct | Total | Percent | M4 correct | M5 correct | Answers |
|-----------|------|-------|-------|---------|-------|---------|-----------|-----------|---------|

`Answers` is a compact string like `1:A,2:C,3:B,...` so you can see exactly what each student chose.

---

## If results are NOT appearing
- Did you set `WEBHOOK_URL` in `app.js`? (most common cause)
- In the deployment, is **Who has access = Anyone**?
- If you change `Code.gs` later, you must **Deploy → Manage deployments → Edit → New version**
  (a fresh deploy), or the changes won't take effect.
- The browser uses `no-cors`, so it can't read the reply — that's normal. Check the Sheet itself
  to confirm rows are arriving.

## Test details
- 40 multiple-choice questions: **20 from Module 4** (evaluation/metrics) + **20 from Module 5**
  (clustering up to **DBSCAN** — no PCA/t-SNE).
- 2.5 points each = 100 total. Pass mark **60**.
- Correct answers are evenly spread A/B/C/D (10 each) — no "always B" pattern.
