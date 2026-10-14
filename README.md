# ChatGPT Clone

This project contains a React frontend and an Express backend for a chat interface. The frontend calls the backend API, which stores conversation messages in MySQL and uses the Google GenAI SDK to generate assistant replies.

For component-level details, see the [frontend README](./frontend/README.md) and [backend README](./backend/README.md).

## Project structure

```text
chatgpt-clone/
├── frontend/     # React and Vite chat interface
│   ├── package.json
│   └── src/
└── backend/      # Express API, MySQL schema, and chat service
    ├── package.json
    ├── index.js
    ├── db/
    └── src/
```

Each application has its own `package.json`, lockfile, and npm scripts. Install and run them from their respective directories.

## Requirements

- Node.js and npm
- A MySQL server and database for the backend
- A Google GenAI API key for generating assistant responses

## Configure the backend

The backend loads environment variables using `dotenv`. Set the following in `backend/.env`:

| Variable | Purpose |
| --- | --- |
| `DB_HOST` | MySQL host; defaults to `localhost` if omitted |
| `DB_USER` | MySQL user |
| `DB_PASSWORD` | MySQL password |
| `DB_DATABASE` | Database name |
| `DB_PORT` | MySQL port |
| `GEMINI_API_KEY` | API key used by the Google GenAI client |
| `GEMINI_MODEL` | Optional model name; the chat service defaults to `gemini-3.5-flash-lite` |

Create the configured database and apply [`backend/db/schema.sql`](./backend/db/schema.sql). The schema creates the `conversations` table if it does not already exist. The backend tests the MySQL connection before starting its HTTP server.

## Install and run

From the repository root, run each block separately to install dependencies in its application directory:

```sh
cd backend
npm ci
```

```sh
cd frontend
npm ci
```

Start the backend from `backend/`:

```sh
npm run dev
```

The backend listens on port `3888`; its API is mounted under `/api`.

Start the frontend from `frontend/` in a separate terminal:

```sh
npm run dev
```

Open the URL printed by Vite. The frontend is configured to call `http://localhost:3888/api`, so the backend must be running at that address for chat requests to work.

The backend can also be started without the development watcher using `npm start` from `backend/`.

## Chat API

- `GET /api/chat/conversations` returns up to 100 recent conversation messages.
- `POST /api/chat/conversations` accepts a JSON body containing a `question` string and returns the saved user message and generated assistant answer.

The backend uses up to five recent messages as context for a new assistant response. Its system instruction limits responses to code, computer science, software engineering, and IT topics.

## Frontend scripts

Run these from `frontend/`:

```sh
npm run build
npm run preview
npm run lint
```

- `npm run build` creates the production build with Vite.
- `npm run preview` serves the production build locally for preview.
- `npm run lint` runs ESLint.
