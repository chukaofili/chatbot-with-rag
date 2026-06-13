# Build a "Chat With Your Docs" Assistant — No Code, in Google AI Studio

**Workshop:** 2026 GFSAA — Build with Gemini and Google's generative language models
**Format:** ~45 min hands-on, no coding required
**You'll need:** A Google account, a laptop with a browser, and 1–3 documents about your startup (pitch deck, FAQ, product doc, policy — PDF, Google Doc, or text).

---

## What you'll build

A chatbot that answers questions **using only your own documents** — your pricing, your FAQ, your onboarding guide — instead of making things up. This pattern is called **RAG (Retrieval-Augmented Generation)**: the model retrieves the relevant pieces of *your* content, then writes an answer grounded in them.

By the end you'll have a working assistant you can share, plus a clear mental model of how to plug this into your own product.

> **Why founders care:** This is the fastest way to turn a pile of company docs into a support bot, an internal "ask-me-anything," or a customer-facing assistant — without hiring an ML team.

---

## Before we start: the 3 ideas behind RAG

1. **The model is smart but doesn't know *your* business.** Gemini wasn't trained on your internal pricing or your refund policy.
2. **RAG fixes this by retrieval.** You give it a knowledge source. When a user asks a question, the system finds the relevant chunks and hands them to the model *with* the question.
3. **Grounded answers cite their source.** Good RAG answers point back to the document they came from — so you can trust and verify them.

Keep this picture in mind: **Your docs → chunked & indexed → user asks → relevant chunks retrieved → Gemini answers from them.**

---

## Step 0 — Open Google AI Studio (2 min)

1. Go to **aistudio.google.com**.
2. Sign in with your Google account.
3. If prompted, accept the terms. You're now in the studio — free to experiment.

> No credit card or install needed to follow this workshop. Everything runs in the browser.

**Checkpoint:** You can see the AI Studio interface with a chat/prompt area and a left or right panel of settings.

---

## Step 1 — Try the model raw, so you feel the problem (5 min)

Before adding your docs, let's prove *why* we need RAG.

1. In the prompt box, ask something only your company would know, e.g.:
   *"What is [Your Startup]'s refund window for annual plans?"*
2. Send it.

**What happens:** The model either says it doesn't know, or it **makes up** a plausible-sounding answer. That confident-but-wrong behaviour is exactly what RAG prevents.

> **Talking point:** This is the "aha." A general model has no access to your private truth. We're about to give it that truth.

---

## Step 2 — Set the assistant's behaviour with System Instructions (5 min)

System instructions tell the model *who it is* and *how to behave* for the whole conversation.

1. Find the **System instructions** field (top of the settings panel).
2. Paste this starter (edit the brackets for your startup):

```
You are the support assistant for [Your Startup].
Answer ONLY using the information in the provided documents.
If the answer is not in the documents, say:
"I don't have that in our docs yet — I'll flag it for the team."
Be concise, friendly, and never invent prices, dates, or policies.
When possible, mention which document the answer came from.
```

3. Pick a model. Use **Gemini 3 Flash** (fast, cheap — great for chat) or **Gemini 3 Pro** (deeper reasoning) from the model dropdown.

> **Why this matters:** The "only answer from the documents" rule is what keeps your bot honest. This single instruction is doing a lot of work.

**Checkpoint:** Your system instruction is saved and a model is selected.

---

## Step 3 — Give it your knowledge (the RAG part) (10 min)

You have two no-code paths. **Path A** is the proper RAG setup; **Path B** is the quick version. Pick based on time and how many docs you have.

### Path A — File Search (managed RAG) ✅ recommended

AI Studio's **File Search** tool does real RAG for you: it chunks your documents, turns them into embeddings, stores them, and retrieves the right pieces at question time. No code, no vector database to manage.

1. In the **Tools** section of the settings panel, enable **File Search** (also called the file/knowledge tool).
2. Click **Upload** / **Add files** and add your 1–3 documents. Wait for indexing to finish (a few seconds per doc).
3. That's it — your docs are now a searchable knowledge base the model will draw from.

> **Founder note:** This is the same mechanism you'd call from your app via the Gemini API later. What you build here maps 1:1 to production.

### Path B — Drop files straight into context (quick version)

If you can't find the File Search tool or have just one short doc:

1. Click the **media / + (insert)** button in the prompt area.
2. Upload your document(s) directly into the conversation.
3. Gemini's long context window reads the whole document and answers from it.

> **Trade-off:** Path B re-reads the full doc every turn (fine for a few small files). Path A scales to many large docs and only retrieves what's relevant — cheaper and faster at scale.

**Checkpoint:** Your documents are uploaded (via File Search or directly in context).

---

## Step 4 — Ask the same question again (5 min)

1. Re-ask your Step 1 question:
   *"What is [Your Startup]'s refund window for annual plans?"*
2. Send it.

**What happens now:** The answer comes straight from your document — correct, specific, and (if you asked it to) citing the source file.

Try a few more:
- A question whose answer **is** in your docs → should answer correctly.
- A question whose answer is **not** in your docs → should say "I don't have that in our docs yet," *not* invent one.

> **This is the whole game.** Same model, same question — but now it's grounded in your truth. That contrast is the most powerful thing to show.

---

## Step 5 — Add live web facts with Grounding (optional, 5 min)

Some questions need *fresh* info (today's FX rate, a competitor's latest launch). Turn on **Grounding with Google Search** in the Tools panel.

- Now the bot answers from **your docs** *and* can pull **current web facts** when needed, with links.
- Great for a founder bot that mixes internal policy with market context.

> **Caution:** With search grounding on, tighten your system instruction so the bot prefers your documents for anything internal, and only uses web search for genuinely external questions.

---

## Step 6 — Make it real: turn it into an app (5 min)

1. Switch to **Build** mode in AI Studio.
2. Describe your assistant in plain English, e.g.:
   *"A customer support chatbot for [Your Startup] that answers from my uploaded docs and politely declines anything not in them."*
3. AI Studio scaffolds a shareable app around your configuration. Start simple, test, then add one feature at a time.
4. Use **Get API key** / **Get code** when you're ready to drop this into your own product — the code mirrors exactly what you built by hand.

**Checkpoint:** You have a shareable app, or an API key + code snippet to take to your engineers.

---

## You did it 🎉

You built a grounded "chat with your docs" assistant with **zero code**. You now understand:

- Why a raw model can't answer business-specific questions
- How RAG retrieves *your* content to ground answers
- The difference between managed RAG (File Search) and dropping files into context
- How to add live web grounding
- The path from prototype → API → product

---

## Take it further

- **Swap models:** Try Gemini 3 Pro for complex reasoning, 3 Flash for cheap high-volume chat.
- **Tune the system prompt:** Add tone, escalation rules, or a "always answer in the user's language" instruction.
- **Scale the knowledge base:** Add your full help center via File Search.
- **Productionize:** Use the Gemini API File Search endpoint to wire the same RAG into your app or website.
- **Add multimodal:** Gemini reads images and PDFs with diagrams — upload a screenshot-heavy doc and ask about it.

## Resources

- Google AI Studio — aistudio.google.com
- Gemini API docs — ai.google.dev/gemini-api/docs
- File Search (managed RAG) — ai.google.dev/gemini-api/docs/file-search
- Grounding with Google Search — developers.googleblog.com/en/gemini-api-and-ai-studio-now-offer-grounding-with-google-search

> **Note on UI labels:** AI Studio ships changes weekly. Button names ("File Search," "Tools," "Build") may differ slightly on the day — the *concepts* (system instruction → upload knowledge → ask → ground) stay the same. Always do a dry run the morning of the talk.
