# Detection Methodology

Three layers, one evidence chain. Each layer is independently useful; together they form a full attribution and intent chain.

## Layer 1 — Network (Wireshark)

**Goal:** spot the exfiltration traffic without necessarily knowing the destination in advance.

- Capture all outbound traffic on the host.
- If the destination is unknown, use **Statistics → Conversations** and sort by packet count — a single destination receiving disproportionately many small, regular packets is the anomaly to chase.
- Confirm the destination via the TLS Client Hello's **SNI field**, which is sent in cleartext even over HTTPS.
- Apply a targeted filter once the domain is known, e.g.:
  ```
  tls.handshake.extensions_server_name == "discord.com"
  ```
- Use **Statistics → IO Graph** to visualize the traffic pattern: a steady, evenly-spaced "drip" of small packets (keystrokes) plus larger, occasional bursts (screenshots/camera frames) is characteristic of automated exfiltration — distinct from bursty, irregular human browsing.

## Layer 2 — Host (TCPView / Process Explorer)

**Goal:** attribute the flagged network connection to the exact local process responsible.

- TCPView maps every active connection to its owning process and PID in real time.
- Cross-reference the IP/SNI identified in Layer 1 against TCPView's live connection list — the process name (e.g. `watcher.exe`) making the connection is immediately visible, even if it has no visible window.
- Process Explorer adds a deeper layer: loaded DLLs, parent-child process relationships, and digital signature status (unsigned binaries are a red flag).

## Layer 3 — Persistence (Autoruns)

**Goal:** determine whether the sample is designed to survive a reboot, which speaks to its sophistication and intent.

- Take a baseline snapshot of Autoruns (Logon, Scheduled Tasks, Services tabs) before running the sample.
- Re-scan after execution and diff against the baseline.
- A new entry pointing to the sample confirms persistence; **no new entry is also a valid, reportable finding** — it indicates the sample requires manual re-execution rather than surviving reboot on its own.

## Why three layers instead of one

| Layer | Catches | Defeated by |
|---|---|---|
| Network (SNI/pattern) | Destination + traffic shape | VPN / DNS-over-HTTPS run on the same host |
| Host (process mapping) | Exact process, regardless of destination | Nothing in this chain — always available |
| Persistence | Reboot survivability / intent | N/A — informational, not evasion-dependent |

Layer 2 (host-level detection) is the most resilient: it depends only on local socket ownership, not on being able to see or decode the network destination. This makes the combined methodology meaningfully harder to evade than any single layer alone.

## Generalization

The same three-layer approach applies to any webhook-style exfiltration channel — Discord, Telegram Bot API, Slack Incoming Webhooks, or a generic REST endpoint — because they all share the same underlying structure: a legitimate, whitelisted API endpoint receiving periodic small payloads from an unexpected local process.
