# Project Abstract

**Title:** Detecting Webhook-Based Keylogger Exfiltration Using Network Traffic Analysis

## Introduction

Data exfiltration through legitimate web services — commonly known as webhook-based exfiltration — has emerged as a covert technique used by malware including keyloggers and information stealers. By routing stolen data through trusted platforms like Discord, Telegram, or Slack, attackers blend malicious traffic with normal application usage, evading signature-based detection.

## Problem Definition

Traditional intrusion detection systems rely on known malware signatures or content inspection, both of which fail against webhook-based exfiltration since the traffic uses standard HTTPS to legitimate, whitelisted domains. Existing anomaly-based systems, such as DECANTeR, focus on automated fingerprinting of application-level HTTP behavior but require adaptation for lightweight, tool-based detection accessible to smaller networks or individual analysts.

## Objectives

To design a multi-layer detection methodology that identifies webhook-based data exfiltration using:

1. Network-level traffic analysis (TLS SNI inspection and traffic timing/size patterns)
2. Host-level process-to-connection correlation
3. Persistence analysis

— without relying on prior malware signatures.

## Proposed Methodology

A test malware sample exhibiting keylogging and webhook-based exfiltration behavior is executed in an isolated virtual environment. Wireshark captures and analyzes outbound traffic for anomalous SNI patterns and periodic payload signatures characteristic of automated exfiltration. TCPView and Process Explorer correlate flagged connections to their originating process. Autoruns identifies persistence mechanisms confirming malicious intent. The combined evidence chain is evaluated against normal application traffic as a baseline.

## Expected Outcome

A validated, tool-based detection methodology capable of identifying webhook-based exfiltration with high confidence, generalizable across multiple webhook platforms (Discord, Telegram, Slack), deployable without specialized infrastructure, and documented with reproducible traffic signatures for practical SOC/blue-team use.


