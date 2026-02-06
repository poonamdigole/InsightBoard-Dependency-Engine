# InsightBoard — Dependency Engine

Convert meeting transcripts into interactive dependency graphs using AI. Paste a meeting transcript, and the app extracts tasks, infers dependencies, detects cycles, and renders an interactive graph you can use to track completion.

## Features

- **LLM-powered extraction** — Uses OpenAI to parse meeting transcripts into structured tasks with dependencies
- **Cycle detection** — Identifies and marks circular dependencies
- **Interactive graph** — Visualize tasks as nodes; click to toggle completion (ready / blocked / completed)
- **Dual view** — Switch between graph and list views
- **Persistent storage** — Transcripts and graphs saved in SQLite

## Tech Stack

| Layer    | Stack                          |
| -------- | ------------------------------ |
| Frontend | React, Vite, TypeScript, Tailwind CSS, React Flow |
| Backend  | Express, TypeScript, SQLite, OpenAI API |
| Deploy   | Vercel (frontend), Render (backend) |

## Project Structure

```
InsightBoardDepEngine_full_project/
├── backend-ts/           # Express API
│   ├── src/
│   │   ├── server.ts     # Routes & middleware
│   │   ├── llm_adapter.ts # OpenAI integration
│   │   ├── db.ts         # SQLite persistence
│   │   ├── validator.ts  # Sanitization & cycle detection
│   │   └── types.ts
│   └── package.json
├── frontend/             # React SPA
│   ├── src/
│   │   ├── App.tsx
│   │   ├── components/
│   │   │   └── GraphView.tsx
│   │   └── main.tsx
│   └── package.json
├── render.yaml           # Render (backend) config
└── README.md
```

## Local Development

### Prerequisites

- Node.js 18+
- npm

### Backend

```bash
cd backend-ts
npm install
cp .env.example .env
```

Edit `.env` and set:

- `OPENAI_API_KEY` — Your OpenAI API key

```bash
npm run dev
```

Runs at `http://localhost:4000`.

### Frontend

```bash
cd frontend
npm install
cp .env.example .env
```

`.env` (optional for local):

- `VITE_API_URL` — Defaults to `http://localhost:4000`

```bash
npm run dev
```

Runs at `http://localhost:5173`.

## Environment Variables

### Backend (`backend-ts/.env`)

| Variable        | Required | Description                                      |
| --------------- | -------- | ------------------------------------------------ |
| `OPENAI_API_KEY`| Yes      | OpenAI API key for LLM                           |
| `PORT`          | No       | Server port (default: 4000)                      |
| `FRONTEND_ORIGIN` | No    | Allowed CORS origin (for production)             |

### Frontend (`frontend/.env`)

| Variable       | Required | Description                                      |
| -------------- | -------- | ------------------------------------------------ |
| `VITE_API_URL` | No       | Backend API URL (default: `http://localhost:4000`) |

## API Endpoints

| Method | Endpoint       | Description                          |
| ------ | -------------- | ------------------------------------ |
| GET    | `/`            | API info and available endpoints     |
| GET    | `/api/health`  | Health check                         |
| POST   | `/api/parse`   | Parse transcript → tasks + graph     |
| GET    | `/api/graph/:id` | Fetch stored graph by ID           |

### POST /api/parse

**Request**

```json
{ "transcript": "Meeting transcript text..." }
```

**Response**

```json
{
  "graphId": "uuid",
  "transcriptId": "uuid",
  "transcript": "original text",
  "tasks": [{ "id": "T1", "description": "...", "dependencies": [] }],
  "blockedTaskIds": []
}
```

## Deployment

### Backend (Render)

1. Connect the repo to [Render](https://render.com).
2. Create a **Web Service** using `render.yaml` (or set manually):
   - **Root directory:** `backend-ts`
   - **Build command:** `npm install && npm run build`
   - **Start command:** `npm run prod`
3. Set `OPENAI_API_KEY` and `FRONTEND_ORIGIN` in Render environment variables.

### Frontend (Vercel)

1. Connect the repo to [Vercel](https://vercel.com).
2. Set **Root Directory** to `frontend`.
3. Add environment variable:
   - `VITE_API_URL` = `https://your-backend.onrender.com`
4. Deploy.

## License

Private / Unlicensed.
