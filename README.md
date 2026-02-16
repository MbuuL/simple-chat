# Chat

A real-time chat application with public and private messaging, online presence tracking, and dark/light theme support.

## Features

- **Public chat** — broadcast messages to all connected users
- **Private rooms** — 1-on-1 messaging between users
- **Online presence** — live tracking of who's online
- **Authentication** — JWT-based register/login
- **Dark/light mode** — theme toggle with persistence

## Tech Stack

| Layer    | Technologies                                                    |
| -------- | --------------------------------------------------------------- |
| Frontend | React 19, TanStack Router/Query/Form, Tailwind CSS v4, Vite 7  |
| Backend  | Express 5, WebSocket (ws), Drizzle ORM, Zod                     |
| Database | PostgreSQL                                                      |
| Auth     | JWT, bcryptjs                                                   |

## Getting Started

### Prerequisites

- Node.js
- pnpm
- PostgreSQL

### Cloning

The `backend/` and `frontend/` directories are Git submodules pointing to separate repos. Clone with submodules included:

```bash
git clone --recurse-submodules git@github.com:MbuuL/chat.git
```

If you already cloned without `--recurse-submodules`, initialize them:

```bash
git submodule init
git submodule update
```

To pull the latest changes including submodule updates:

```bash
git pull --recurse-submodules
```

To update submodules to their latest remote commits:

```bash
git submodule update --remote
```

### Backend

```bash
cd backend
pnpm install
```

Create `backend/.env`:

```
PORT=3002
VERSION=1.0.0
DATABASE_URL=postgresql://user:password@localhost:5432/chatdb
JWT_SECRET=your-secret-key
```

Run migrations and start:

```bash
pnpm db:migrate
pnpm dev
```

### Frontend

```bash
cd frontend
pnpm install
```

Create `frontend/.env`:

```
VITE_BACKEND_URL=http://localhost:3002/api/v1
```

Start the dev server:

```bash
pnpm dev
```

## Project Structure

```
backend/                    # Express + WebSocket API
  src/
    index.ts                # HTTP server entry point, mounts WebSocket
    app.ts                  # Express middleware and route mounting
    ws.ts                   # WebSocket server (public/private messages, presence)
    routes/v1.ts            # REST endpoint definitions
    controllers/            # auth, public chat, private chat handlers
    middleware/auth.ts       # JWT authentication middleware
    db/schema.ts            # Drizzle schema (users, chats, privateRooms, privateChats)
    __tests__/              # Jest + supertest tests

frontend/                   # React SPA
  src/
    main.tsx                # Entry point, TanStack Router setup
    routes/                 # File-based routes (login, register, home, private rooms)
    components/ChatArea/    # Chat UI panels (Left, Main, Right)
    hooks/                  # usePublicChat, useOnlineUsers, useTheme
    lib/api.ts              # HTTP helpers and JWT utilities
```

## How It Works

1. Users register/login via REST — server returns a JWT
2. Frontend stores the JWT in localStorage
3. WebSocket connects with the token (`/ws?token=...`)
4. **Public messages** are broadcast to all clients and persisted to `chats`
5. **Private messages** are routed to room participants and persisted to `privateChats`
6. **Presence** updates are broadcast whenever users connect or disconnect
7. Frontend updates TanStack Query cache on WebSocket events for instant UI updates

## Scripts

### Backend (run from `backend/`)

| Command            | Description                                |
| ------------------ | ------------------------------------------ |
| `pnpm dev`         | Start dev server with hot reload           |
| `pnpm build`       | Compile TypeScript to `dist/`              |
| `pnpm start`       | Run compiled JS                            |
| `pnpm test`        | Run all tests                              |
| `pnpm db:generate` | Generate Drizzle migrations                |
| `pnpm db:migrate`  | Run migrations                             |
| `pnpm db:push`     | Push schema changes directly to database   |
| `pnpm db:studio`   | Open Drizzle Studio GUI                    |

### Frontend (run from `frontend/`)

| Command        | Description                     |
| -------------- | ------------------------------- |
| `pnpm dev`     | Start Vite dev server           |
| `pnpm build`   | TypeScript compile + Vite build |
| `pnpm preview` | Preview production build        |
| `pnpm lint`    | Run ESLint                      |
