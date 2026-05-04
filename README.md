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

session_loader.json          ← fetch this at session start
chunks/
meta.behaviour_personality.json   ← auto-loaded, defines Claude's behaviour
[your chunks]
index/
[domain].[subtopic].json          ← lightweight indexes, fetched before chunk decisions
---

## Getting started

1. Fork or copy this repository (or download files locally for standalone)
2. Edit `session_loader.json` — replace all `YOUR_*` placeholders with your values
3. Edit `chunks/meta.behaviour_personality.json` — review behaviour rules, adapt or remove as needed
4. Add your session start instruction to Claude's user preferences (see template below)

**User preferences template:**

At session start, fetch [YOUR_SESSION_LOADER_URL] using web_fetch and acknowledge when loaded. Then follow the instructions inside the file.

---

## Adapting the behaviour rules

`chunks/meta.behaviour_personality.json` contains communication and reasoning rules. These are recommendations based on one user's working style — not universal defaults.

Read through each rule before using. Ask yourself:
- Does this match how I want Claude to interact with me?
- Are the failure patterns relevant to my use case?
- Do I want bidirectional discipline (Claude flags your reasoning too) or one-directional?

The rules work best when they're specific to how you actually work. Vague or inherited rules you haven't thought through will be ignored in practice — by you and by Claude.

---

## Chunk size limit

Keep individual chunk files under 3KB (~3000 characters of JSON). Larger files cause silent failures with some MCP tools. When a chunk grows beyond that, split it — see naming conventions in `session_loader.json`.

---

## Notes

- This system was designed for use with Claude but the architecture is model-agnostic
- The KB is only as useful as what you put in it — sparse chunks are fine, vague chunks are not
- Version your loader file when making structural changes
