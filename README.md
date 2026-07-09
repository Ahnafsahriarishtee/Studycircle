**StudyCircle v1**

A working v1 of **StudyCircle**: a social layer wired directly onto the Free-Trial path, built to attack Shikho's biggest leak — **activation** (students who register but never start a first content session). Mock OTP login → Shikho-styled dashboard → a free-trial study group with a **persistent friend circle** and a **shared lesson checklist**, where seeing a classmate check off Lesson 1 is the pressure that gets you to start yours.

**Live demo:** (https://studycircle-ahnafsahriar.vercel.app/)


---

## How to run it

It's a single self-contained `index.html` — no build step, no dependencies.

- **Online:** just open the live URL above.
- **Locally:** download `index.html` and double-click it, or serve the folder with any static server (`npx serve .`). That's it.

## What a grader should try (it works off-script)

1. **Login** — enter any Bangladeshi-format number (`01XXXXXXXXX`), then **any 4-digit code** logs you in. It's a mock OTP — no SMS is sent, and the screen says so.
2. **Dashboard** — Shikho profile bar (`Ahnaf | Class 10 | Science`), the geometric bird logo, the **ফ্রি-তে শিখতে ক্লিক করো** free-trial banner, the four content cards (Live Coaching / Animated Lessons / Test Papers / Board Paper Solving), and the 5-icon bottom nav.
3. **StudyCircle** — tap the banner. Add classmates to your circle, tick lessons on the shared checklist, watch peer progress and the activity feed.

### The persistence test (the strict requirement)
- Add a friend, tick a lesson, **close the tab, reopen** → everything is still there.
- Saved state is **keyed per phone number, per device**: log in as one number and build a circle; log out; log in as a *different* number → you start clean and don't see the first user's circle; log back in as the first number → your circle is restored.
- **Scope note (honest boundary):** persistence is per-device via `localStorage`. The same number on a different device starts fresh — true cross-device sync is a v2 backend concern, and the client already partitions state by phone number so that migration is a small delta.

## Tech & design notes

- Single-file **HTML/CSS/JS**, zero dependencies — deliberately, so nothing flakes when someone pokes it and it runs anywhere.
- **Bengali-first UI** (Hind Siliguri), Shikho brand system: white background, rounded content cards in green/red/purple/teal, the pink/blue/yellow geometric bird mark (inline SVG — no external asset, so the logo can't break in production).
- Logo is **inline SVG on purpose**: an earlier build referenced an external PNG that 404'd on deploy; inlining it removes that failure mode entirely.
- State layer is a single store keyed as `shikho_studycircle_v1::<phone>`, with an auth pointer for "who's logged in on this device."

## The roadmap

Full reasoning in **`PLAN.md`**. In brief:

- **v1 (this build) — Free-Trial Study Groups · Activation.** Shared checklist inside the trial; a peer's checkmark is the social pressure that triggers a first session.
- **v2 — @ShikhoBot · Scalable learning.** A curriculum-trained assistant inside the group that unblocks a stuck student with a hint + a deep link to the exact animated-lesson timestamp.
- **v3 — Gamification & peer bounties · Retention → revenue.** Group leaderboards and a points→discount + peer-bounty economy that converts free users to paid. No parent notifications, by design.

## Build trace

The GitHub commit history is the build trace — each commit maps to a real stage (scaffold → login → dashboard → circle → persistence → logo/brand fixes → per-number state). (https://studycircle-ahnafsahriar.vercel.app/)
