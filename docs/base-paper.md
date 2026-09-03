# Base Research Paper

**Title:** DECANTeR: DEteCtion of Anomalous outbouNd HTTP TRaffic by Passive Application Fingerprinting

**Authors:** Riccardo Bortolameotti, Thijs van Ede, Marco Caselli, Maarten H. Everts, Pieter Hartel, Rick Hofstede, Willem Jonker, Andreas Peter

**Published:** 2017, Proceedings of the 33rd Annual Computer Security Applications Conference (ACSAC 2017), ACM

## Summary

DECANTeR passively fingerprints normal outbound HTTP traffic per application on a host, then flags traffic that deviates from these fingerprints as anomalous. It is designed to detect unknown malware and backdoor communication, including data exfiltration by information stealers, without relying on known malware signatures. Evaluated on real organizational traffic, it achieved a 97.7% detection rate with a 0.9% false positive rate across 441 monitored hosts, and detected 96.8% of information stealers in its dataset.

## Relevance to this project

This project applies the same core principle — identifying malicious outbound traffic by how it deviates from normal application behavior — but demonstrates it manually using accessible tools (Wireshark, TCPView, Autoruns) rather than an automated fingerprinting system, specifically targeting webhook-based keylogger exfiltration.
