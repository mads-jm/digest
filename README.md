# Digest

A local-first Electron desktop app for AI-powered email management and summarization.

Forked from [EmailEssence](https://github.com/EmailEssence/EmailEssence.github.io) — a capstone project by Emma Melkumian, Joseph Madigan, Shayan Shahla, and Ritesh Samal.

## Overview

Digest takes EmailEssence's proven backend — FastAPI, the summarization pipeline, the repository pattern — and re-homes it as a local-first desktop application. No cloud deployment, no remote database. Your email stays on your machine, summarized by your choice of AI provider.

## Features

- Secure OAuth 2.0 authentication with Google
- AI-powered email summarization with pluggable providers
- Clean, clutter-free reader view
- Local-first — all data stored on your machine
- Tokens secured via OS keychain (Electron `safeStorage`)

## Architecture

```
Electron App (app/)
  ├─ Renderer: React 19 + Vite
  ├─ Preload: typed IPC bridge (contextBridge)
  └─ Main Process
       ├─ IPC handlers
       ├─ OAuth (Electron-native, loopback redirect)
       └─ Sidecar manager → FastAPI backend subprocess
                              ├─ Routers → Services → Repositories
                              └─ SQLite (aiosqlite)
```

All renderer-to-backend communication flows through Electron IPC — no direct `fetch()` calls.

## Technical Stack

### Electron App (`app/`)
- TypeScript — main process, preload, shared types
- React 19 — renderer UI
- Vite — renderer dev server and build
- tsup — main/preload bundling
- electron-builder — packaging and distribution

### Backend (`backend/`)
- Python 3.12+
- FastAPI — API framework
- SQLite via aiosqlite — local database
- Pluggable AI summarization: local (Ollama, llama.cpp, LM Studio), OpenAI, Google Gemini, OpenRouter

## Getting Started

### Prerequisites
- Node.js (v18 or higher)
- Python (v3.12 or higher)
- UV package manager (for Python dependency management)

### Installation

#### Backend Setup

Create a `.env` file in `backend/` using `.env.example` as a template. Then:

**Windows:**
```powershell
.\backend\setup.ps1
```

**Linux/macOS:**
```bash
chmod +x backend/setup.sh
./backend/setup.sh
```

The setup scripts install UV, create a virtual environment, and install all dependencies.

#### Electron App Setup
```bash
cd app
npm install
```

### Development

```bash
# Start backend (from backend/)
uvicorn main:app --reload

# Start Electron app with hot reload (from app/)
npm run dev
```

### Packaging

```bash
cd app
npm run package    # Outputs to app/release/build/
```

The packaged app bundles the Python backend so end users don't need a separate Python install.

## Documentation

Project documentation lives in `digest - docs/` (an Obsidian vault). Key references:

- [FastAPI](https://fastapi.tiangolo.com/)
- [Electron](https://www.electronjs.org/docs)
- [React 19](https://react.dev/)
- [aiosqlite](https://aiosqlite.omnilib.dev/)

## License

TODO

## Origin

Digest is a solo continuation of [EmailEssence](https://github.com/EmailEssence/EmailEssence.github.io), carrying the full git history of the original capstone project. The fork inherits the backend engine and places it in a local-first, Electron-native context.
