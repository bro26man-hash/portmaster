# RESEARCH_NOTES.md — Podcast Episode: Digital Rights & Surveillance Technology

> **Project:** [Safing/Portmaster](https://github.com/safing/portmaster) — 🏔 Love Freedom · ❌ Block Mass Surveillance
> **Stars:** 13,739 · **Forks:** 578 · **License:** GPL-3.0 · **Language:** Go
> **Forked to:** `bro26man-hash/portmaster`
> **Date:** September 2026

---

## 1. Project Overview

Portmaster is a free, open-source **application firewall** for Windows and Linux that intercepts all network traffic at the packet level. Its stated mission is to "restore privacy and take back control over all your computer's network activity." It was developed in the EU (Austria) and is published under the GNU GPL v3.0.

Key technical features:
- **Packet-level interception** via `nfqueue` (Linux) and Windows Filtering Platform / WFP (Windows)
- **eBPF-based connection ownership** tracking on Linux
- **Kernel driver** on Windows for deep packet inspection
- **Per-app rules** — granular control over which applications can access the network
- **Secure DNS** (DoT/DoH) with split-horizon validation against rebinding attacks
- **Automatic tracker/malware blocking** via filter lists
- **SPN (Safing Privacy Network)** — a commercial multi-hop onion-routing privacy network (paid)

---

## 2. Societal Concerns & Ethical Tensions

### 2.1 The Surveillance Capitalism Feedback Loop

Portmaster exists *because* of mass surveillance — corporate and state. Its entire value proposition is reactive: it blocks what surveillance systems deploy. This raises a foundational question for the podcast:

- **Does counter-surveillance technology merely treat symptoms while the surveillance economy grows more entrenched?**
- Every tracker Portmaster blocks is also a revenue source for someone. Blocking it disrupts the advertising-driven internet economy. What are the economic consequences of widespread adoption?

### 2.2 The Privacy Paywall Problem

Portmaster's core is free, but advanced features — **SPN, Network History, Bandwidth Monitoring** — are behind a paywall. This creates a **"privacy paywall"** dynamic:

- **Is privacy a luxury good?** If effective counter-surveillance requires a subscription, then the people who can afford privacy are the wealthy, while the rest remain surveilled.
- This mirrors broader critiques of the "surveillance divide" — those with resources escape surveillance, while the poor and marginalized are subject to it more intensely.
- The SPN, in particular, is a **commercial privacy infrastructure** — you are paying a company to route your traffic through their onion network. This introduces trust assumptions that are philosophically at odds with the open-source ethos.

### 2.3 The Trust Paradox: Centralized Privacy Infrastructure

SPN uses onion encryption over multiple hops (like Tor), but:
- **Nodes are hosted by Safing (the company) and the community.**
- Safing controls the entry and exit node selection algorithm.
- The SPN whitepaper is available for review ([Gate17.pdf](https://safing.io/files/whitepaper/Gate17.pdf)), but how many users actually verify it?
- **The tool that promises to liberate you from surveillance requires you to trust a specific company's infrastructure.** This is the central paradox of commercial privacy tools: they replace state/corporate surveillance with corporate privacy gatekeeping.

**Podcast angle:** Compare Portmaster/SPN to Tor. Tor is community-run, decentralized, and non-commercial. SPN is commercial, semi-centralized, and optimized for speed and geo-unblocking. Which model better serves civil liberties?

### 2.4 The Kernel-Level Power Dilemma

Portmaster operates at the **kernel level** — it installs a WFP driver on Windows and uses nfqueue on Linux. This means:

- It has **the same kind of deep system access** that surveillance malware uses.
- A vulnerability in Portmaster could be catastrophically exploitable — it sees *every* packet on the machine.
- The project uses the **Contributor Covenant** Code of Conduct, but there's no formal security audit framework documented in the repo.
- **Who watches the watchers?** A tool designed to block surveillance must itself be scrutinized for potential misuse.

**Podcast angle:** This mirrors the broader debate around "backdoors" — the argument that encryption can't have a "backdoor for good guys" applies here too. A firewall that intercepts all traffic *could* be modified to intercept it for someone else.

### 2.5 Jurisdictional Arbitrage & the EU Model

Portmaster is developed in Austria (EU), which means:

- It operates under **GDPR** and **EU Charter of Fundamental Rights** protections.
- The EU has a different relationship with surveillance than the US — the Court of Justice of the EU has struck down mass surveillance laws (e.g., the *Schrems* decisions).
- But: **what happens when EU-developed privacy tools are used globally?** The creators can control what happens in the EU, but not what authoritarian regimes do with the tool.
- Could a future EU government pressure Safing to comply with local surveillance mandates? The company's commitment to "Love Freedom · Block Mass Surveillance" is an editorial stance, not a legal one.

**Podcast angle:** This is a story about **jurisdictional privilege** — the idea that privacy tools are only as free as the country they're based in. What happens when the EU itself tightens surveillance requirements (e.g., for CSAM scanning, NIS2 compliance)?

### 2.6 The Dual-Use Dilemma

Every counter-surveillance tool is also a potential surveillance enabler:

- Portmaster can block connections — but it could also be configured to **allow only specific connections**, creating a per-app firewall that could restrict what *you* can access.
- The per-app filtering that empowers users could be repurposed by employers, parents, or authoritarian states.
- The open-source license (GPL-3.0) means anyone can fork and modify the code — including for surveillance purposes.

**Podcast angle:** This is the classic **"the tool is neutral, but power is not"** argument. Compare to VPNs, which in some countries (China, Iran, Russia) are illegal or restricted.

---

## 3. Community & Governance Observations

### 3.1 Open Issues Reflect Real-World Tensions

The most-commented open issues reveal that users are wrestling with the practical implications of the tool:

| Issue | Theme | Relevance to Podcast |
|-------|-------|---------------------|
| #1141 (70 comments) | Slow connections when allowed | **The performance trade-off of deep packet inspection** — privacy has a cost in speed and convenience |
| #306 (35 comments) | NixOS packaging | **Accessibility** — privacy tools should be installable on all OSes, including niche ones |
| #388 (33 comments) | Import/Export settings | **Data portability** — if you switch tools, do you lose your privacy configuration? |
| #329 | Self-defense and kill-switch | **What happens when the tool fails?** — a firewall that can't block is a false sense of security |

### 3.2 The "Self-Defense" Issue (#329)

This is the most philosophically rich issue in the repo. A user requested a **kill-switch** — a feature that cuts all network access if the firewall itself is compromised or shut down. This is essentially a **"fail-secure"** design:

- It acknowledges that the tool can fail, and that failure should default to *blocking* (privacy-preserving) rather than *allowing* (surveillance-exposing).
- This is a **design philosophy** question: should privacy tools default to open or closed when they fail?
- The issue was closed but not resolved — suggesting the community hasn't fully grappled with this.

**Podcast angle:** This is a perfect metaphor for the episode. When your defense against surveillance fails, does the surveillance win by default? Or should the system fail-safe and lock everything down?

### 3.3 Moderation & Speech

The Code of Conduct is the standard Contributor Covenant — inclusive and well-intentioned. But:

- The project is actively maintained by a small team (dhaavi, stenya, ppacher, etc.).
- **Who decides what "harassment" means in the context of surveillance discourse?** If someone argues that mass surveillance is legitimate state security, is that "trolling" or "political attacks"?
- The maintainers have the power to remove comments, commits, and contributions that are "not aligned" with the CoC — this is a **gatekeeping power** that shapes the political direction of the project.

---

## 4. Angles Worth Exploring on the Podcast

### 4.1 "The Privacy Paywall" — Can Liberty Be Bought?
- Portmaster's free core vs. paid SPN creates a two-tier privacy system
- Parallel to the "surveillance divide" in academic literature
- Question: Is commercial privacy a stepping stone toward mass adoption, or a dead end?

### 4.2 "Who Guards the Guardians?" — The Kernel-Level Trust Problem
- A firewall at kernel level has the same power as rootkit-level surveillance software
- The SPN whitepaper exists, but how many users verify it?
- Compare Tor's community-node model vs. SPN's company-controlled model

### 4.3 "Fail-Secure or Fail-Open?" — The Design Philosophy of Defense
- The self-defense / kill-switch debate (#329)
- What should happen when privacy tools fail? Default to blocking (safe) or allowing (functional)?
- This is a microcosm of the broader surveillance debate

### 4.4 "Jurisdictional Privilege" — The Country You're Based In Determines Your Freedom
- EU-based development vs. global usage
- What happens when EU governments turn toward digital sovereignty (AI Act, NIS2, CSAM scanning)?
- The illusion of "free" privacy when it's subject to geopolitical forces

### 4.5 "The Dual-Use Trap" — Can a Freedom Tool Become a Control Tool?
- Per-app firewalling can enable employee monitoring, parental controls, or state censorship
- GPL-3.0 means anyone can fork the code — including for surveillance
- Historical parallel: VPNs are illegal in some countries now

### 4.6 "The Attention Economy's Kryptonite" — What Happens When Everyone Blocks Trackers?
- If mass adoption of counter-surveillance tools destroys the advertising model, what replaces it?
- Does the internet become unsustainable, or does it evolve toward subscription-based models?
- The collateral damage of privacy: many free services depend on surveillance revenue

### 4.7 "Open Source vs. Open Trust" — The SPN Paradox
- The code is open-source, but the privacy infrastructure (SPN) is commercial
- You can audit the code, but you must trust the company to operate the network honestly
- This is the new privacy debate: **code transparency ≠ operational transparency**

---

## 5. Key Resources for Further Research

| Resource | URL |
|----------|-----|
| Project repository | https://github.com/safing/portmaster |
| SPN Whitepaper (PDF) | https://safing.io/files/whitepaper/Gate17.pdf |
| Safing website | https://safing.io |
| Portmaster wiki | https://wiki.safing.io |
| Settings handbook | https://docs.safing.io/portmaster/settings |
| Privacy Guides (related project) | https://github.com/privacyguides/privacyguides.org |
| Privacy Tools (related project) | https://github.com/privacytools/privacytools.io |

### Related GitHub Projects for Cross-Reference
- **safing/portmaster** — Application firewall / counter-surveillance (13.7k stars)
- **privacyguides/privacyguides.org** — Privacy recommendation guide (4.3k stars)
- **privacytools/privacytools.io** — Privacy tools directory (3.1k stars)
- **data-privacy-stack/presidio** — PII anonymization framework (10.9k stars)
- **hukkelas/DeepPrivacy** — GAN-based face anonymization (1.3k stars)
- **Anon-Planet/thgtoa** — Online anonymity & OpSec guide (849 stars)

---

## 6. Initial Podcast Pitch (Draft)

> **Title:** "The Privacy Paywall: Can You Buy Your Way Out of Surveillance?"
>
> **Logline:** Portmaster — the open-source firewall with 13,000+ stars that literally says "Block Mass Surveillance" — faces its deepest paradox: the most effective privacy features are behind a paywall. We explore the ethical tensions at the heart of the privacy-tech industry: when privacy becomes a commodity, who gets to be free? And can a tool built by a commercial company truly liberate you from corporate surveillance?
>
> **Key themes:** Surveillance capitalism, the privacy paywall, kernel-level trust, jurisdictional privilege, dual-use technology, the fail-secure debate, open-source vs. open-trust.
>
> **Tone:** Thoughtful, accessible, slightly provocative. Not anti-technology, but deeply skeptical of the idea that technology alone can solve structural problems.

---

*Notes compiled from GitHub repository analysis of safing/portmaster, including open issues, Code of Conduct, project documentation, and cross-referenced privacy-tech ecosystem projects.*
