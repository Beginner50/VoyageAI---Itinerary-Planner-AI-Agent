VoyageAI — Itinerary Planner AI Agent

[image]

An AI-powered travel itinerary planner built with Next.js App Router, React 19, and LangChain. It uses Google Gemini for planning and embeddings, performs lightweight RAG over a curated Mauritius attractions dataset, and generates shareable PDFs/ICS files.

## Tech Stack

- **Framework**: Next.js (App Router)
- **UI**: React 19, MUI
- **AI/Orchestration**: LangChain, LangGraph
- **Models/Embeddings**: Google Gemini via `@langchain/google-genai`
- **Auth**: NextAuth (Google OAuth)
- **PDF & Calendar**: `@react-pdf/renderer`, `ics`
- **Data**: Local JSON dataset + in-memory vector store

## Project Structure

```
src/
  app/
    api/
      agent/route.js
      generate-itinerary/route.js
      auth/[...nextauth]/route.js
    itenerary-planner/page.jsx
    components/...  
  utils/
    RAG.js
    agents/
      itineraryGeneratorAgent.js
      orchestratorAgent.js
  llms.js
datasets/
  mauritius_attractions_dataset/*.json
  vector_store.json (auto-created)
```

[image]

## Prerequisites

- Node.js 18+ (Next.js 15)
- A Google Cloud project with access to Gemini API
- A Google OAuth 2.0 Client (for NextAuth)

## Environment Variables

Create a `.env.local` in the project root and assign your keys:

```
# Google Gemini (Generative AI and embeddings)
GEMINI_API_KEY=your_gemini_api_key_here

# NextAuth Google OAuth
GOOGLE_CLIENT_ID=your_google_oauth_client_id
GOOGLE_CLIENT_SECRET=your_google_oauth_client_secret

# NextAuth secret (use a strong random string)
AUTH_SECRET=your_generated_auth_secret

# App URL (used for callbacks and internal links)
# For local dev this can be omitted; Next falls back to http://localhost:3000
NEXT_PUBLIC_APP_URL=http://localhost:3000

# When deployed on Vercel, VERCEL_URL is provided automatically
# VERCEL_URL=your-vercel-deployment-url
```

Required keys at minimum: `GEMINI_API_KEY`, `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `AUTH_SECRET`.

Tip to generate `AUTH_SECRET`:

```bash
openssl rand -base64 32
```

## Install & Run Locally

1) Install dependencies

```bash
npm install
```

2) Start the dev server

```bash
npm run dev
```

Open http://localhost:3000.

3) Build and run production locally (optional)

```bash
npm run build
npm start
```

## Usage

1) Sign in with Google
2) Provide trip preferences and duration
3) Generate itinerary; preview results
4) Download generated PDF and ICS

[image]

## Data & RAG

- Local dataset under `datasets/mauritius_attractions_dataset/`
- Vector store is created on first run at `datasets/vector_store.json`
- Embeddings use Gemini via `GEMINI_API_KEY`

## API Routes

- `POST /api/generate-itinerary`: Generates itinerary, returns PDF and ICS as base64 when not in preview
- `GET/POST /api/auth/[...nextauth]`: Google OAuth via NextAuth
- `POST /api/agent`: Orchestrator for chat-driven workflow

[image]

## Deployment Notes

- Set the same environment variables in your hosting provider
- Ensure OAuth redirect URIs include your deployed URL (and `/api/auth/callback/google`)
- On Vercel, `VERCEL_URL` is auto-provided; set `NEXT_PUBLIC_APP_URL` if needed

## License

[image]
