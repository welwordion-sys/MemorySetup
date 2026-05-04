# MemoryAccess — Session Memory System for Claude

A lightweight, structured memory system that gives Claude persistent context across sessions using JSON files stored wherever you prefer.

---

## What it does

Claude has no memory between sessions by default. This system provides a structured knowledge base (KB) that Claude fetches at session start, giving it persistent context: behaviour rules, project notes, decisions, entities, and anything else you want carried forward.

The system is file-based and interface-agnostic. You control what Claude knows, how it reasons, and what it remembers.

---

## How it works

At session start Claude fetches `session_loader.json`, which tells it:
- Where to find other files
- What to auto-load
- How to handle lookups and writes

From there Claude loads chunks on demand — only what's relevant, only when needed.

---

## Setup options

### Option 1: GitHub (remote)
Store files in a GitHub repository. Claude accesses them via GitHub MCP or raw URL fetch.

- Best for: cross-device access, version control, collaboration
- Requires: public or MCP-accessible GitHub repo
- Session start: provide raw URL to session_loader.json once, or add it to Claude's user preferences

### Option 2: Raw URL fetch
Public GitHub repo, no MCP required. Claude fetches files directly via `web_fetch`.

- Best for: simple setup, no MCP configuration
- Requires: public GitHub repo
- Limitation: URL must be provided by user at session start (Claude cannot construct raw URLs autonomously on first fetch)

### Option 3: Local (standalone)
Files live on your device. No internet dependency.

- Best for: privacy, offline use, mobile use
- Access methods:
  - **File upload**: upload `session_loader.json` at session start, Claude reads it directly
  - **Local MCP server**: point a local MCP server at your KB directory, Claude accesses files natively
- Limitation: file upload requires manual upload each session; local MCP requires one-time setup

---

## Repository structure
