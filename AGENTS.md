# Nexus Device Agent: Master Knowledge Base & Agent Guidelines (`AGENTS.md`)

This document serves as the master reference, binding architectural blueprint, and mandatory instruction set for any AI assistant (Antigravity, Gemini, Cursor, Copilot) and developer working on `nexus-device-agent`.

---

## 1. Universal AI Git & Commit Policy (STRICT - MUST FOLLOW)

### A. Never Commit (`git commit` is FORBIDDEN for AI)
- The AI assistant **MUST NEVER** execute `git commit`.
- All C# code, worker services, ConPTY handlers, and newly created files must remain **locally uncommitted** in the git working tree.
- The user will personally inspect the diffs, verify all changes in GitHub Desktop / Git CLI, and execute `git commit` manually.

### B. Never Push (`git push` is FORBIDDEN for AI)
- The AI assistant **MUST NEVER** execute `git push` to `origin` or any remote repository.
- Never publish branches or code online to GitHub. All modifications must stay strictly in the local workspace.

### C. Git Branching & Naming Standards
- **Feature Branch Prefix**: MUST use the full prefix **`feature/`** (lowercase), followed by kebab-case (e.g., `feature/conpty-streamer`, `feature/session-zero-helper`).
  - **STRICTLY PROHIBITED**: Never use shorthand prefixes like `feat/`. Always use `feature/`.
- **Bugfix Branch Prefix**: Use `fix/<issue-name>` or `feature/<issue-name>`.
- **Base Branch**: Every feature or fix branch MUST branch directly off **`dev`** (never from `main`).
- **Workflow**: Branch off `dev` -> Perform work -> Leave changes uncommitted for user review -> User manually commits -> User pushes to `origin/feature/<branch-name>` -> PR targets `dev`.

### D. Mandatory Self-Updating Documentation Policy (Zero-Forgotten Rule)
- Whenever device communication protocols, GUI helpers, or terminal runners are modified, immediately update `AGENTS.md` and `GEMINI.md`.

### E. Mandatory Prohibition on Automated Browser Testing (STRICT ZERO BROWSER TEST RULE)
- **Automated Browser Testing Strictly Forbidden**: The AI assistant **MUST NEVER** launch automated browser subagents or headless test sessions.
- **Code-Level Verification Only**: All verification must be performed strictly at the **code level** (`dotnet test`, compilation, mock IPC tests).

---

## 2. Directory Structure

```text
nexus-device-agent/
├── src/
│   ├── Nexus.Agent.Service/    # .NET 10 Background Service (Daemon)
│   ├── Nexus.Agent.GuiHelper/  # Session-1 User GUI Helper (FlaUI / Win32)
│   ├── Nexus.Agent.Terminal/   # ConPTY / Pseudo-terminal stream manager
│   └── Nexus.Agent.Shared/     # mTLS gRPC client & IPC protocols
├── tests/
│   ├── Nexus.Agent.Tests/
│   └── Nexus.Agent.IpcTests/
└── README.md
```

---

## 3. Principles Transferred from eShop & SRV

1. **Security & Zero Plaintext Secrets**:
   - Device agent connects via **outbound-only mTLS gRPC** to `nexus-core`. No inbound ports are opened on target devices.
   - Device tokens and TLS client certificates must never be committed.
2. **Isolation & Non-Interference Mandate (from SRV)**:
   - The device agent operates strictly inside approved execution directories. It never touches or modifies host directories without explicit authorization.
3. **Audit Trail**:
   - Every terminal command, GUI interaction, and exit code is streamed and permanently recorded in PostgreSQL.

---

## 4. Verification Commands

```bash
# Build agent solution
dotnet build

# Run unit tests
dotnet test --no-build
```

---

*Authorized by Dapplesoft Architecture & Nexus Governance Council*
