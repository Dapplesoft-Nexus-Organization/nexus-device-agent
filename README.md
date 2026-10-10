# Nexus Device Agent

Low-footprint, highly secure device runner capable of executing audited shell commands and GUI interactions across bare-metal or virtual environments.

## Architecture & Security
- **Outbound-Only mTLS:** Connects outbound to Nexus Core via gRPC; no inbound firewall ports required.
- **Windows Session 0 Isolation:** Background Service delegates desktop UI interactions to a dedicated interactive helper process via local IPC named pipes.
- **Device-Side Policy Enforcement:** Every incoming signed command is validated locally prior to execution.
- **A/B Slot Auto-Updater:** Zero-downtime, verified updates signed with Cosign/Authenticode.

## Directory Structure
```text
nexus-device-agent/
├── Agent.Core/
├── Agent.Ports/
├── Agent.Host.Service/
├── Agent.Adapters.Windows/
├── Agent.Adapters.Linux/
├── Agent.GuiHelper.Windows/
├── Agent.GuiHelper.Linux/
├── Agent.Updater/
└── tests/
```
