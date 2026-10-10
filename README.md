# Webhook Keylogger Detection

**Detecting webhook-based keylogger exfiltration using network traffic analysis.**


## Overview

Modern info-stealers and keyloggers increasingly exfiltrate captured data through legitimate webhook endpoints (Discord, Telegram, Slack) instead of custom C2 infrastructure. Because the traffic rides ordinary HTTPS to a trusted, whitelisted domain, it blends in with normal application usage and evades signature-based antivirus.

This project demonstrates a **three-layer, tool-based detection methodology** that identifies this class of exfiltration without any prior knowledge of the malware sample:

1. **Network Layer — Wireshark**: inspect TLS SNI and traffic timing/size patterns to spot covert channels
2. **Host Layer — TCPView / Process Explorer**: correlate the flagged connection to the exact local process generating it
3. **Persistence Layer — Autoruns**: check startup, registry, and scheduled tasks to establish intent

A self-built proof-of-concept sample (`watcher`) — a keylogger with screenshot and webcam capture that exfiltrates via a Discord webhook — was used to generate real traffic and validate the methodology end to end.

## Repo contents

```
webhook-keylogger-detection/
├── README.md                    — you are here
├── docs/
│   ├── abstract.md               — project abstract (Intro, Problem, Objectives, Methodology, Outcome)
│   ├── base-paper.md             — base research paper and relevance
│   └── methodology.md            — full write-up of the 3-layer detection approach
├── screenshots/                  — evidence from the test run (see below)
└── presentation/
    └── Webhook_Keylogger_Detection.pptx
```

## Screenshots

| File | What it shows |
|---|---|
| `00-test-environment-*` | Isolated Windows 10 VirtualBox VM used for testing |
| `01-bait-vulnbank-login` / `02-bait-vulnbank-dashboard` | Self-hosted test banking site used to generate realistic credential-entry traffic |
| `03-exfil-discord-screenshot-capture` / `04-exfil-discord-keystroke-capture` | The sample's captured output landing in a private Discord channel |
| `05-detection-tcpview-before` / `06-detection-tcpview-after` | TCPView before/after — the sample's outbound HTTPS connection appearing under its own process name |

All testing was performed against a self-hosted, fake banking site (`vulnbank.org`, a local test target with a dummy `TestUser` account) inside an isolated VM. No real systems, accounts, or third parties were involved.

## What's *not* in this repo

The keylogger sample (`watcher`) itself — binary or source — is **intentionally not included**. This repo documents the *detection methodology*, not the exfiltration tool. The screenshots above are sufficient to demonstrate the technique and validate the detection approach without distributing working malware.

## Base paper

Bortolameotti et al., *"DECANTeR: DEteCtion of Anomalous outbouNd HTTP TRaffic by Passive Application Fingerprinting"*, ACSAC 2017. See [`docs/base-paper.md`](docs/base-paper.md) for details and relevance.

## Author

Syed Zabiullah Mehdi
