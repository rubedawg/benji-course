// ============================================================
// PARENT ZONE 🔒 — Benji doesn't edit this file.
//
// This is a Vercel serverless function. It runs on the server,
// not in the browser. Its one job: receive the question from
// index.html, add the SECRET API key (stored safely in Vercel's
// environment variables, never in the code), ask Claude, and
// send the answer back.
//
// Why it exists: if the API key lived in index.html, anyone
// visiting the page could steal it. This is the Week 4+ lesson:
// "never put keys in public code."
// ============================================================

export default async function handler(req, res) {
  if (req.method !== "POST") {
    return res.status(405).json({ error: "POST requests only." });
  }

  const apiKey = process.env.ANTHROPIC_API_KEY;
  if (!apiKey) {
    return res.status(500).json({
      error: "No API key found. (Parent: set ANTHROPIC_API_KEY in Vercel → Project → Settings → Environment Variables.)"
    });
  }

  const { system, message } = req.body || {};
  if (!message) {
    return res.status(400).json({ error: "No message received." });
  }

  // Guardrails: keep requests small so a bug can't burn money.
  if (message.length > 2000 || (system && system.length > 4000)) {
    return res.status(400).json({ error: "That message is too long for this lab." });
  }

  try {
    const anthropicResponse = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "x-api-key": apiKey,
        "anthropic-version": "2023-06-01"
      },
      body: JSON.stringify({
        model: "claude-sonnet-4-6",
        max_tokens: 500, // caps the length of each answer (and the cost)
        system: system || "You are a helpful assistant.",
        messages: [{ role: "user", content: message }]
      })
    });

    const data = await anthropicResponse.json();

    if (!anthropicResponse.ok) {
      const detail = data?.error?.message || "The AI service returned an error.";
      return res.status(502).json({ error: detail });
    }

    // Claude's reply comes back as a list of blocks; join the text ones.
    const answer = (data.content || [])
      .filter((block) => block.type === "text")
      .map((block) => block.text)
      .join("\n");

    return res.status(200).json({ answer });

  } catch (err) {
    return res.status(500).json({ error: "Could not reach the AI service. Check your internet connection." });
  }
}
