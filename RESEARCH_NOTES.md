# 🎙️ Podcast Research Notes: Portmaster & the Ethics of Counter-Surveillance

**Repository:** [safing/portmaster](https://github.com/safing/portmaster)  
**Forked to:** `bro26man-hash/portmaster`  
**Stars:** 13,737 | **Forks:** 577 | **License:** GPL-3.0 | **Language:** Go  
**Tagline:** 🏔 Love Freedom — ❌ Block Mass Surveillance  

---

## 1. Project Overview

Portmaster is a free and open-source **application firewall** for Windows and Linux that operates at the raw packet level. It intercepts every network connection, blocks trackers/malware by default, enforces secure DNS (DoT/DoH), and offers per-app filtering. It was developed in Austria (EU) by Safing GmbH.

**Why it matters for the podcast:** Portmaster sits at the intersection of three forces that define the modern surveillance debate:
- **Civil liberties** — its explicit mission is to resist mass surveillance
- **Open-source philosophy** — its claim to FOSS status is contested
- **Commercial sustainability** — it paywalls advanced features behind a subscription

---

## 2. Core Societal Concerns

### A. The FOSS Identity Crisis

Portmaster labels itself as free and open-source, yet multiple features are paywalled:
- **Network History** (local connection logging)
- **Bandwidth Visibility** (per-app usage monitoring)
- **VPN Compatibility Mode**
- **Docker / VM Support**
- **Weekly Reports**

Only the **SPN (Safing Privacy Network)** is fully commercial — and even that is a Value-Added Service on top of a GPL-licensed codebase.

**Key tensions to explore on the podcast:**
- *Can you call a tool with paywalled privacy features truly "free"?* Users can't audit what the paid features do because the source for those modules isn't published.
- *Does the Free Software Foundation definition matter if the core firewall still works for free?* The community is split.
- *Sustainability vs. ethos:* Can open-source privacy projects survivelong-term without giving in to freemium models? What does it mean when a "privacy tool" becomes a business?

**Relevant Issue:** [#2132 — "Portmaster is NOT FOSS"](https://github.com/safing/portmaster/issues/2132) (7 comments, 6 👍). GNOME reportedly removed Portmaster from its FOSS list over paywalled features.

### B. The Surveillance Paradox: A Firewall That Sees Everything

Portmaster operates at **kernel level** (WFP on Windows, nfqueue + eBPF on Linux). This means:
- It sees **every packet** on the system
- It has the power to **block, redirect, or log** any connection
- It requires **full system control** (admin/root) to function

**The irony:** A tool designed to protect you from surveillance *becomes* a surveillance-capable node. Any kernel-level tool that can intercept all traffic *could* be repurposed — by design or by compromise.

**Key tensions to explore:**
- *Is deep packet inspection a civil liberties tool or a surveillance enabler, depending on who controls it?*
- *What happens when a privacy tool becomes the single point of failure?* If Portmaster is compromised, an attacker sees everything.
- *The kernel-access tradeoff:* Users must trust Safing (and the kernel itself) not to abuse its position. This mirrors the trust model concerns with VPNs and Tor.

### C. The SPN Dilemma — Replacing One Intermediary with Another

SPN (Safing Privacy Network) uses **onion encryption over multiple hops**, similar to Tor. But:
- Nodes are **hosted by Safing (the company) and the community**
- Exits are chosen near destination servers — meaning Safing knows your traffic pattern
- It's a **closed commercial service**, not a decentralized network
- The SPN Whitepaper ([Gate17.pdf](https://safing.io/files/whitepaper/Gate17.pdf)) describes the architecture, but the node operators are not independently auditable

**Key tensions to explore:**
- *Are you really safer with SPN if you're just replacing your ISP with Safing as the intermediary?*
- * "Between VPN and Tor" — what does that even mean for threat models?* SPN threatens neither your ISP nor a powerful adversary the way Tor does.
- *Centralization risk:* If Safing shuts down, is compromised, or is pressured by authorities, SPN users lose their protection.
- *The "peace of mind" marketing problem:* Portmaster promises "peace of mind" — but is that feeling grounded in technical reality?

### D. Microsoft WebView: The Privacy Tool That Depends on the Surveillance Vendor

Portmaster V2 **forces users to install Microsoft Edge WebView2** to run the UI. Multiple issues document this:
- [#2096 — "Microsoft WebView Is Not Privacy"](https://github.com/safing/portmaster/issues/2096) (14 comments)
- [#2031 — "Love Freedom, hate Webview"](https://github.com/safing/portmaster/issues/2031) (8 comments)
- [#1932 — "Portmaster V2 forces microsoft webview installation"](https://github.com/safing/portmaster/issues/1932) (17 comments)

Users who removed WebView2 found the firewall **would not launch at all**.

**Key tensions to explore:**
- *A tool called "Love Freedom — ❌ Block Mass Surveillance" that requires a Microsoft component to run. The symbolism is extraordinary.*
- *Why does a network firewall need a web rendering engine?* V1 ran without WebView. V2's dependency is a architectural choice, not a technical necessity.
- *Vendor lock-in as a form of surveillance compliance:* Even if the data isn't sent to Microsoft, forcing users to install proprietary Microsoft code on their systems is a form of dependency that undermines the tool's purpose.

### E. The EU Framing — "Developed in the EU" as Political Statement

Portmaster markets itself as "Developed in the EU 🇪🇺, Austria." This is a deliberate political signal:
- EU has its own surveillance apparatus (INGV in Italy, BND in Germany, etc.)
- The "EU-developed" label implies a different threat model than US-based tools
- But the EU also passes mass surveillance legislation (e.g., Chat Control, ePrivacy regulation debates)

**Key tensions to explore:**
- *Does geography matter for surveillance tools?* Is "EU" actually more privacy-respecting, or is it just a different flavor of state surveillance?
- *Can a capitalist EU-based company be a reliable steward of civil liberty tools?* Safing is a business, not a nonprofit.

---

## 3. Ethical Tensions Summary Table

| Tension | Side A | Side B |
|---------|--------|--------|
| FOSS labeling | Core firewall is free & open | Paid features are not auditable |
| Kernel access | Necessary for real protection | Creates a single point of failure |
| SPN vs. Tor | More user-friendly | Less decentralized, less trustless |
| Microsoft dependency | Required for V2 UI | Contradicts the freedom mission |
| Freemium model | Funds development | Undermines FOSS integrity |
| EU positioning | Implies stronger privacy norms | EU has its own surveillance state |
| "Peace of mind" marketing | Accessible to non-technical users | May create false sense of security |

---

## 4. Podcast Angles & Discussion Prompts

### 🔥 Hot Takes for Episode Hooks

1. **"The surveillance tool that sees everything"** — What does it mean when your anti-surveillance tool operates at kernel level and could theoretically be turned against you?

2. **"Free but not free"** — When a privacy tool paywalls its own features, is it still fighting for freedom, or has it sold out?

3. **"Microsoft in the machine"** — A tool called "Block Mass Surveillance" can't run without Microsoft's consent. Is this metaphor too obvious to miss?

4. **"Trust the company"** — SPN asks you to trust Safing Inc. with your traffic. How does that compare to trusting your ISP? Is it better? Worse?

5. **"The EU is not the answer"** — Even tools developed under supposed stronger privacy norms still operate within surveillance capitalism.

### 🎤 Guest Ideas

- A **kernel security researcher** who can explain the risks of user-facing firewalls operating at ring 0
- A **FOSS legal expert** who can parse what "open source" legally means vs. "source available"
- A **digital rights advocate** from the EFF or Privacy International on the gap between tool-level and policy-level privacy
- A **Tor Project developer** to contrast decentralized vs. commercial privacy networks

### 📚 Further Reading & Sources

- [Portmaster SPN Whitepaper](https://safing.io/files/whitepaper/Gate17.pdf)
- [EFF: Surveillance Don'ts](https://www.eff.org/issues/surveillance)
- [GNU Definition of Free Software](https://www.gnu.org/philosophy/free-sw.en.html)
- [/privacy.sexy](https://github.com/undergroundwires/privacy.sexy) — Windowsanti-surveillance hardening guide
- [simplewall](https://github.com/henrypp/simplewall) — FOSS alternative mentioned by critics

---

## 5. Key GitHub Issues for Further Research

| Issue | Topic | Why It Matters |
|-------|-------|----------------|
| [#2132](https://github.com/safing/portmaster/issues/2132) | FOSS identity | Core question: what does "free" mean for a privacy tool? |
| [#2111](https://github.com/safing/portmaster/issues/2111) | Paywall scope | Community asks for non-SPN features to stay free |
| [#2096](https://github.com/safing/portmaster/issues/2096) | Microsoft WebView | Privacy tool forced to depend on surveillance vendor |
| [#2031](https://github.com/safing/portmaster/issues/2031) | WebView alternative | User requests CEF instead of Microsoft's WebView |
| [#1932](https://github.com/safing/portmaster/issues/1932) | WebView forced install | Documented incompatibility with hardened Windows systems |
| [#1122](https://github.com/safing/portmaster/issues/1122) | DNS control | Who should manage your DNS — the user or the tool? |

---

## 6. Bottom Line for the Episode

Portmaster is a **remarkable tool with a remarkable contradiction at its core.** It fights mass surveillance while becoming a deep-interception node itself. It calls itself free while paywalling features. It champions user control while depending on Microsoft's proprietary code. It's the perfect case study for a podcast about the **gap between digital-rights ideals and the compromises required to build tools that actually work in a hostile world.**

The most important question isn't "Is Portmaster good or bad?" — it's: **"Can you build a tool that genuinely defends civil liberties in a world where every layer of the stack is potentially compromiseable?"**

---

*Notes compiled from GitHub repository analysis, open issue review, and community discussion. Last updated: 2026-09-17.*
