# Nexus Device Agent: Master Knowledge Base & Agent Guidelines (`GEMINI.md`)

This document serves as the master reference, binding architectural blueprint, and mandatory instruction set for any AI assistant (Antigravity, Gemini, Cursor, Copilot) and developer working on `nexus-device-agent`.

---

## 1. Ecosystem Context & Multi-Repository Boundary

This repository is an **independent, loosely coupled component** of the **Dapplesoft Nexus Ecosystem** (`Dapplesoft-Nexus-Organization`).

### Repository Responsibility & Scope

| What Belongs Here (`nexus-device-agent`) | What Belongs in Other Repositories |
|---|---|
| • Go / .NET 10 cross-platform background worker / daemon<br>• Windows ConPTY terminal runner & Session 0 GuiHelper IPC<br>• Linux PTY runner & systemd service adapter<br>• Outbound-only mTLS gRPC client streaming to Nexus Core API<br>• A/B slot cryptographic auto-updater | • **Web UI & Backend APIs**: Belongs in `dapplesoft-nexus`<br>• **Policies & Risk Gates (R0–R3)**: Belongs in `nexus-governance`<br>• **Docker Sandboxes / Browser Automations**: Belongs in `nexus-computer-use`<br>• **Trace Curation & ML Training**: Belongs in `nexus-learning`<br>• **Production Compose & Runbooks**: Belongs in `nexus-deployment` |

---

## 2. Universal AI Git & Commit Policy (STRICT - MUST FOLLOW)

### A. Never Commit (`git commit` is FORBIDDEN for AI)
- The AI assistant **MUST NEVER** execute `git commit`.
- All code changes, worker services, ConPTY handlers, and newly created files must remain **locally uncommitted** in the git working tree.
- The user will personally inspect the diffs, verify all changes in GitHub Desktop / Git CLI, and execute `git commit` manually.

### B. Never Push (`git push` is FORBIDDEN for AI)
- The AI assistant **MUST NEVER** execute `git push` to `origin` or any remote repository.
- Never publish branches or code online to GitHub. All modifications must stay strictly in the local workspace.

### C. Git Branching & Naming Standards
- **Feature Branch Prefix**: MUST use the full prefix **`feature/`** (lowercase), followed by kebab-case (e.g., `feature/conpty-streamer`, `feature/session-zero-helper`). Never use shorthand `feat/`.
- **Bugfix Branch Prefix**: Use `fix/<issue-name>` or `feature/<issue-name>`.
- **Base Branch**: Every feature or fix branch MUST branch directly off **`dev`** (never from `main`).
- **Workflow**: Branch off `dev` -> Perform work -> Leave changes uncommitted for user review -> User manually commits -> User pushes to `origin/feature/<branch-name>` -> PR targets `dev`.

### D. Mandatory Self-Updating Documentation Policy (Zero-Forgotten Rule)
- Whenever device communication protocols, GUI helpers, or terminal runners are modified, immediately update `AGENTS.md` and `GEMINI.md`.

### E. Mandatory Prohibition on Automated Browser Testing (STRICT ZERO BROWSER TEST RULE)
- **Automated Browser Testing Strictly Forbidden**: The AI assistant **MUST NEVER** launch automated browser subagents or headless test sessions.
- **Code-Level Verification Only**: All verification must be performed strictly at the **code level** (compilation, unit tests, mock IPC tests).

---

## 3. Directory Structure

```text
nexus-device-agent/
├── src/
│   ├── Nexus.Agent.Service/    # Background Service (Daemon)
│   ├── Nexus.Agent.GuiHelper/  # Session-1 User GUI Helper (FlaUI / Win32)
│   ├── Nexus.Agent.Terminal/   # ConPTY / Pseudo-terminal stream manager
│   └── Nexus.Agent.Shared/     # mTLS gRPC client & IPC protocols
├── tests/
│   ├── Nexus.Agent.Tests/
│   └── Nexus.Agent.IpcTests/
└── docs/                       # Device agent design & protocol specs
```

---

## 4. Architectural Principles
1. **Security & Zero Plaintext Secrets**: Outbound-only mTLS gRPC connection to `dapplesoft-nexus` core-api. Zero inbound open ports.
2. **Isolation & Non-Interference**: Operates strictly inside authorized working directories.
3. **Audit Streaming**: Every executed command, exit code, and interaction streams live to Core API for PostgreSQL storage.
