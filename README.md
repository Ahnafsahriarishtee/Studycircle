# StudyCircle v1 — Shikho take-home (SOC-0312)

A working v1 of **StudyCircle**: a social layer on the Free-Trial path that turns activation into a group behavior. Mock OTP login → Shikho-styled dashboard → free-trial study group with a **persistent** friend circle and a **shared lesson checklist**.

## Run locally
```bash
npm install
npm run dev        # http://localhost:3000
```

## Deploy (Vercel)
1. Push this repo to GitHub (see commit sequence below).
2. vercel.com → **Add New Project** → import the repo → framework auto-detects Next.js → **Deploy**. No env vars needed.

## What the grader will hit
- **/login** — phone + OTP. **Any 4-digit code works** (stated on-screen). No real SMS.
- **/dashboard** — profile bar (`Tasnim | Class 10 | Science`), the **ফ্রি-তে শিখতে ক্লিক করো** StudyCircle banner, the four content cards (Live Coaching / Animated / Test Papers / Board Paper Solving), 5-icon bottom nav.
- **/circle** — the v1 feature: add friends (one tap), shared checkered lesson list with peer avatars on each finished lesson, peer-activity feed.

**Persistence test (the strict requirement):** add friends, tick lessons, close the tab / kill the browser, come back → session, circle, and checklist are all still there (localStorage, key `shikho_studycircle_v1`). Log-out isn't needed to re-test: clear the key or use a private window.

## Architecture (deliberately boring)
- Next.js 14 App Router, React 18, **zero other dependencies** — nothing to flake when someone pokes it off-script.
- All state flows through `lib/store.js` (`loadState` / `updateState`), so every mutation persists by construction.
- Client-side auth gate on `/` , `/dashboard`, `/circle` → redirects to `/login` when no session.
- Bengali-first UI (Hind Siliguri), brand geometry logo (pink/blue/yellow), rounded-card system.

## Suggested commit sequence (this becomes the build trace)
Commit in this order so the history reads as the actual build:
1. `chore: scaffold next.js app + shikho design tokens`
2. `feat: mock OTP login (any 4-digit code, demo hint shown)`
3. `feat: dashboard — profile bar, free-trial banner, content card grid, bottom nav`
4. `feat: studycircle — add-friend flow with persistent circle state`
5. `feat: shared lesson checklist + peer progress avatars + activity feed`
6. `fix: auth gate redirects + first-visit add-friends auto-open`
7. `docs: readme + v1/v2/v3 plan`

## v1 → v2 → v3 (summary — full plan in `PLAN.md`)
- **v1 (this build):** Free-Trial Study Groups — activation via peer visibility.
- **v2:** **@ShikhoBot** in the group space — curriculum-trained hints + deep links to animated-lesson timestamps.
- **v3:** Group leaderboards + peer bounties (points → discounts → paid conversion). No parent notifications, by design.
