# Week 3 Template — Parent Setup Guide

Two files matter:

- **`index.html`** — Benji's file. The only one he touches. The `✏️ EDIT ZONE` (the `SYSTEM_PROMPT`) is his playground.
- **`api/claude.js`** — your plumbing. A Vercel serverless function that holds the API key server-side so it's never exposed in the browser. Benji doesn't edit this in Week 3, but the comments explain it for when he's curious.

## Setup (~15 min, do before Benji's session)

1. **API key**: console.anthropic.com → API Keys → create one named `benji-course`. Then **Settings → Limits → set a monthly spend limit** (~$5). The proxy also caps message sizes and response length, so runaway costs are basically impossible.
2. **Repo**: push this folder to a new GitHub repo (this can be under your account for now — Benji creates his own in Week 4 and forks/copies it, which is a nice lesson in itself).
3. **Deploy**: vercel.com → Add New Project → import the repo → before deploying, add environment variable `ANTHROPIC_API_KEY` = your key → Deploy. No build settings needed; Vercel auto-detects the static page + API function.
4. Open the live URL, ask it something, confirm you get an answer.

> **Local option:** `npx vercel dev` in this folder runs it on your machine (it will pull the env var after you link the project). Honestly, deploying straight to Vercel and iterating on the live URL is easier — pushes redeploy automatically.

## Test checklist (5 min, before he sees it)

- [ ] Ask a normal question → get a "beep boop" style answer
- [ ] Empty box + button → friendly error, no crash
- [ ] Edit the SYSTEM_PROMPT to something silly, push, refresh → personality changes
- [ ] Airplane-mode your wifi mid-request → readable error message

## Running the session

1. Don't explain first — let him type a question and hit the button. Magic before mechanics.
2. Then open `index.html` together and read from the top. Stop at the EDIT ZONE.
3. He edits the personality, saves, refreshes. That loop — *edit → save → refresh → different behavior* — is the entire lesson.
4. **Challenge (his solo session):** make the AI answer as three different personalities of his choosing, by editing only the EDIT ZONE. Ideas if he's stuck: pirate, sports commentator, extremely dramatic Shakespeare actor, an AI that answers only in questions.

## What to say if he asks "what's the stuff below the edit zone?"

"That's the delivery system — it carries your instructions to the AI and brings the answer back. You'll build one yourself in a few weeks." Resist explaining `async/await`. Week 3 wins if he leaves thinking *I changed how an AI behaves by editing code.*

## Cost reality check

Each question costs a fraction of a cent. If he asked it 1,000 questions this week, you'd spend roughly a dollar. The $5 cap is a seatbelt, not a budget.
