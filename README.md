<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Benji's AI Lab — Week 3</title>
<style>
  /* ============================================================
     STYLES — this is what makes the page look good.
     You don't need to touch this in Week 3.
     (But if you're curious: try changing --accent and refresh!)
     ============================================================ */
  :root {
    --bg: #F4F6FB;
    --card: #FFFFFF;
    --ink: #1B1F2A;
    --muted: #5A6272;
    --accent: #4F46E5;      /* the main purple-blue */
    --accent-dark: #3B34B8;
    --highlight: #FFE24A;   /* highlighter yellow */
    --ok: #16A34A;
    --bad: #DC2626;
  }
  * { box-sizing: border-box; }
  body {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    background: var(--bg);
    color: var(--ink);
    line-height: 1.5;
  }
  .wrap { max-width: 640px; margin: 0 auto; padding: 24px 16px 64px; }

  .badge {
    display: inline-block;
    background: var(--ink);
    color: var(--highlight);
    font-weight: 800;
    font-size: 12px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    padding: 4px 10px;
    border-radius: 999px;
  }
  h1 {
    font-size: clamp(28px, 7vw, 40px);
    font-weight: 900;
    letter-spacing: -0.02em;
    margin: 10px 0 4px;
  }
  h1 .zap { color: var(--accent); }
  .sub { color: var(--muted); margin: 0 0 24px; }

  .mission {
    background: var(--card);
    border: 2px solid var(--ink);
    border-radius: 14px;
    padding: 16px 18px;
    box-shadow: 4px 4px 0 var(--ink);
    margin-bottom: 24px;
  }
  .mission h2 { margin: 0 0 6px; font-size: 16px; }
  .mission p { margin: 6px 0; font-size: 14px; }
  .mission code {
    background: var(--highlight);
    padding: 1px 6px;
    border-radius: 4px;
    font-weight: 700;
  }

  label { font-weight: 700; font-size: 14px; display: block; margin-bottom: 6px; }
  textarea {
    width: 100%;
    min-height: 90px;
    font: inherit;
    padding: 12px;
    border: 2px solid var(--ink);
    border-radius: 12px;
    resize: vertical;
  }
  textarea:focus { outline: 3px solid var(--accent); outline-offset: 2px; }

  button {
    margin-top: 12px;
    width: 100%;
    font: inherit;
    font-weight: 800;
    font-size: 17px;
    color: #fff;
    background: var(--accent);
    border: 2px solid var(--ink);
    border-radius: 12px;
    padding: 14px;
    cursor: pointer;
    box-shadow: 4px 4px 0 var(--ink);
    transition: transform 0.06s ease, box-shadow 0.06s ease;
  }
  button:hover { background: var(--accent-dark); }
  button:active { transform: translate(3px, 3px); box-shadow: 1px 1px 0 var(--ink); }
  button:disabled { opacity: 0.6; cursor: wait; }
  button:focus-visible { outline: 3px solid var(--ink); outline-offset: 2px; }

  .answer {
    margin-top: 24px;
    background: var(--card);
    border: 2px solid var(--ink);
    border-radius: 14px;
    padding: 16px 18px;
    min-height: 80px;
    white-space: pre-wrap;
  }
  .answer.empty { color: var(--muted); font-style: italic; }
  .answer.error { border-color: var(--bad); color: var(--bad); }
  .thinking { color: var(--accent); font-weight: 700; }
</style>
</head>
<body>
<div class="wrap">

  <span class="badge">Week 3 · Lab</span>
  <h1>Benji's AI Lab <span class="zap">⚡</span></h1>
  <p class="sub">Same AI brain you've been chatting with — but now YOU control it with code.</p>

  <div class="mission">
    <h2>🎯 Your mission</h2>
    <p>1. Type something in the box and press the button. See what comes back.</p>
    <p>2. Open this file in your editor and find the <code>✏️ EDIT ZONE</code>.</p>
    <p>3. Change the AI's personality by editing <strong>only that text</strong>. Save, refresh, try again.</p>
  </div>

  <label for="userMessage">Say something to the AI:</label>
  <textarea id="userMessage" placeholder="Try: What should I have for breakfast?"></textarea>
  <button id="askButton">Ask the AI 🚀</button>

  <div id="answer" class="answer empty">The AI's answer will show up here…</div>

</div>

<script>
/* ==============================================================
   THE CODE — read it top to bottom, it's shorter than you think.
   ============================================================== */


/* ╔════════════════════════════════════════════════════════════╗
   ║                                                            ║
   ║   ✏️  EDIT ZONE — THIS PART IS YOURS                       ║
   ║                                                            ║
   ║   This text is called a SYSTEM PROMPT. It's the AI's       ║
   ║   job description — it obeys this before anything else.    ║
   ║                                                            ║
   ║   Change the words between the backticks (` `), save       ║
   ║   the file, refresh the page, and ask again.               ║
   ║                                                            ║
   ╚════════════════════════════════════════════════════════════╝ */

const SYSTEM_PROMPT = `You are a helpful robot assistant for a 13-year-old
named Benji. Keep your answers short — 3 sentences max — and end every
answer with a robot noise like "beep boop".`;

/* ╔════════════════════════════════════════════════════════════╗
   ║   END OF EDIT ZONE — everything below makes the page work. ║
   ║   You'll understand all of it by Week 6. Promise.          ║
   ╚════════════════════════════════════════════════════════════╝ */


// Grab the three things on the page we need to control:
const askButton = document.getElementById("askButton");
const userMessage = document.getElementById("userMessage");
const answerBox = document.getElementById("answer");

// This runs every time the button is clicked:
askButton.addEventListener("click", askTheAI);

async function askTheAI() {
  const question = userMessage.value.trim();
  if (!question) {
    answerBox.className = "answer error";
    answerBox.textContent = "Type something first! The AI can't read minds. Yet.";
    return;
  }

  // Show that we're working on it:
  askButton.disabled = true;
  answerBox.className = "answer";
  answerBox.innerHTML = '<span class="thinking">🤔 The AI is thinking…</span>';

  try {
    // This sends your question to OUR server (the file in /api),
    // which passes it to the AI and sends the answer back.
    const response = await fetch("/api/claude", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        system: SYSTEM_PROMPT,   // ← your edit zone text goes here!
        message: question
      })
    });

    const data = await response.json();

    if (!response.ok) {
      throw new Error(data.error || "Something went wrong on the server.");
    }

    // Put the AI's answer on the page:
    answerBox.className = "answer";
    answerBox.textContent = data.answer;

  } catch (err) {
    answerBox.className = "answer error";
    answerBox.textContent = "😵 Error: " + err.message;
  }

  askButton.disabled = false;
}
</script>
</body>
</html>
