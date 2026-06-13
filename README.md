# chatbot-with-rag

A hands-on, **no-code workshop** for building a grounded "chat with your docs" assistant (RAG) using **Google AI Studio** and Gemini, built for the **2026 GFSAA** session _"Build with Gemini and Google's generative language models."_

RAG (Retrieval-Augmented Generation) connects a great model to _your_ content so it answers from your documents instead of guessing. This repo has everything to run the session: the workshop materials, a ready-made sample knowledge base to point File Search at, and code samples that do the same thing with the Gemini API.

## What's inside

```
chatbot-with-rag/
├── docs/                                       # Run-the-session materials
│   ├── Build_with_Gemini_RAG_Workshop.pptx     # 10-slide deck
│   ├── codelab_rag_chatbot_ai_studio.md        # Step-by-step hands-on guide
│   └── facilitator_notes.md                    # Minute-by-minute run of show
├── knowledge-base/                             # Sample docs to ingest into the RAG
│   ├── markdowns/                              # Markdown source documents
│   │   ├── 01_Company_Overview_FAQ.md
│   │   ├── 02_Products_and_Pricing.md
│   │   ├── 03_Customer_Support_SLA.md
│   │   └── 04_People_and_Culture_Handbook.md
│   └── pdfs/                                   # Same documents as PDFs
│       ├── 01_Company_Overview_FAQ.pdf
│       ├── 02_Products_and_Pricing.pdf
│       ├── 03_Customer_Support_SLA.pdf
│       └── 04_People_and_Culture_Handbook.pdf
└── scripts/                                    # Gemini API code samples (Node.js)
    ├── upload.js                               # Index docs into a File Search store
    ├── search.js                               # Query the store with grounded, cited answers
    ├── package.json                            # @google/genai dependency + npm scripts
    └── .env.example                            # API key + optional question override
```

## The knowledge base

The `knowledge-base/` folder is a fictional-but-realistic dataset for **Interstellar Labs**, used as the demo company. The docs are intentionally cross-referenced (e.g. the 14-day annual refund policy appears in pricing and is referenced from support) so the bot can give precise, citable answers. The same four documents are provided as both Markdown (`markdowns/`) and PDF (`pdfs/`) so you can demo ingesting either format.

> ⚠️ Sample data for demonstration only - not real company policy.

## Run the workshop

1. Open [Google AI Studio](https://aistudio.google.com) and sign in.
2. Add a system instruction telling the model to answer **only** from the provided documents.
3. Enable the **File Search** tool and upload the files in `knowledge-base/`.
4. Ask questions — the bot answers from the docs and cites its source.

Full walkthrough: [`docs/codelab_rag_chatbot_ai_studio.md`](docs/codelab_rag_chatbot_ai_studio.md).

## Run the code samples (optional)

Prefer code over the UI? The `scripts/` folder does the same RAG flow with the Gemini API and the `@google/genai` SDK.

```bash
cd scripts
npm install
cp .env.example .env          # then set GEMINI_API_KEY (loaded automatically via dotenv)

# 1. Index the knowledge base into a File Search store
npm run upload -- ../knowledge-base/pdfs/*.pdf

# 2. Paste the FILE_SEARCH_STORE id printed by step 1 into .env, then ask a question
npm run search -- "What is our refund window for annual plans?"
```

The scripts load your `.env` via [dotenv](https://github.com/motdotla/dotenv), so once `GEMINI_API_KEY` and `FILE_SEARCH_STORE` are set there you don't need to pass them on the command line.

- [`scripts/upload.js`](scripts/upload.js) — creates (or reuses) a File Search store and indexes the files you pass it, then prints the store id for `.env`.
- [`scripts/search.js`](scripts/search.js) — queries the store in `FILE_SEARCH_STORE` with the question you pass on the command line (`npm run search -- "your question"`), prints a grounded answer and the source documents it cited.

Get an API key at [aistudio.google.com/apikey](https://aistudio.google.com/apikey).

## Try these demo questions

- "What's Interstellar Labs' refund window for annual plans?"
- "How much is Flagship's Growth plan annually?"
- "What are the support response targets for an Enterprise customer?"
- "How many days of paid time off do employees get?"
- "Something not in the docs" → the bot should decline instead of inventing.

## License

Workshop materials © Interstellar Labs. Sample knowledge-base data is fictional.
