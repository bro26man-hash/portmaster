# 🎙️ RESEARCH NOTES — Portmaster & the Surveillance-Tech Debate
## Podcast Episode Research: Digital Rights & Surveillance Technology

**Repository:** [safing/portmaster](https://github.com/safing/portmaster)  
**Forked to:** [bro26man-hash/portmaster](https://github.com/bro26man-hash/portmaster)  
**Stars:** 13,738  |  **Forks:** 578  |  **License:** GPL-3.0  |  **Language:** Go  
**Tagline:** "🏔 Love Freedom — ❌ Block Mass Surveillance"

---

## 1. PROJECT OVERVIEW

Portmaster is a free, open-source **application firewall** for Windows and Linux that intercepts every network packet at the raw level using:
- **nfqueue** (Linux) and **WFP kernel driver** (Windows) for packet interception
- **eBPF** and `/proc` (Linux) / **IP Helper API** (Windows) for connection-to-process ownership
- **DoT/DoH** for secure DNS resolution with split-horizon and rebinding attack defense
- **Per-app rules** with support for tricky processes (Snap, AppImage, Windows Store apps, svchost.exe)

**Key selling point:** "With great defaults your privacy improves without any effort."

It also offers **SPN (Safing Privacy Network)** — a multi-hop onion-routing privacy network positioned "between VPN and Tor," with community-hosted exit nodes chosen near destination servers for geo-unblocking.

---

## 2. SOCIETAL CONCERNS & ETHICAL TENSIONS

### A. THE SURVEILLANCE-AS-A-SERVICE PARADOX
Portmaster blocks mass surveillance by intercepting **every packet** on your machine. This means the tool itself has **total visibility into all network activity**. The same kernel-level access that lets you block trackers can be repurposed to monitor citizens. Several open issues touch on this:

- **Issue #2096 — "Microsoft WebView Is Not Privacy"** (closed, 14 comments, 👍 2 / 👎 1): A user pointed out that Portmaster v2 requires Microsoft WebView — a surveillance-adjacent Microsoft component — to function. The tension: a privacy tool that *depends on the surveillance infrastructure it claims to fight*. This echoes a broader pattern: tools that fight surveillance often embed themselves deep in the same systems they seek to expose.

- **Kernel driver access (WFP on Windows):** Portmaster installs a kernel-mode driver, giving it **the highest possible privilege level** on your machine. This is technically necessary for packet interception, but it means the *company behind Portmaster* (Safing, an Austrian entity) could theoretically be compelled to provide backdoor access. The same technology that protects you from Huawei could be turned against you by a government order.

### B. THE SPN CENTRALIZATION PROBLEM
SPN is the most ethically charged feature. It's positioned as "between VPN and Tor" — but it's **run by a commercial company**:

- **Nodes are hosted by Safing and the community** — but Safing controls the routing algorithm, exit node selection, and the whitepaper architecture
- **Onion encryption over multiple hops** sounds like Tor, but SPN is a **profit-motivated service** with paid tiers ($$)  
- The SPN Whitepaper ([Gate17.pdf](https://safing.io/files/whitepaper/Gate17.pdf)) claims routes are "chosen to cover most distance within the network" — but **who audits the node operators?** What prevents a malicious exit node from logging traffic?
- **Dependency tension:** The core tool is GPL-3.0 open-source, but SPN is a proprietary commercial add-on. This creates a **dual-classPrivacy model**: open-source for the masses, premium privacy for those who can pay.

### C. WHO DECIDES WHAT TO BLOCK? (CENSORSHIP & FILTER LIST BIAS)
Portmaster ships with **pre-built filter lists** that block "malware, ad, tracker domains." But several issues reveal the political weight of these decisions:

- **Issue #326 — "Add custom block list"** (closed): Users wanted the ability to add their own block lists, raising the question: if *you* decide what's "bad," whose definition of "bad" applies?
- **Issue #1000 — "Online Custom Filter List"** (closed): Online filter lists could be **weaponized** — a state actor could push a filter list that blocks opposition sites under the guise of "security"
- **Issue #606 — "Own blocked-list + Wildcards/Regexps"** (closed): Advanced filtering power in the hands of everyday users is a **double-edged sword** — it empowers the privacy-conscious but also enables authoritarian filtering

The core philosophical question: **Is blocking surveillance inherently political?** When Portmaster blocks a Chinese IP address, is it protecting a Uyghur activist — or is it just another firewall that could be repurposed by the Chinese government?

### D. THE PRIVACY-IS-A-LUXURYPROBLEM
Portmaster's business model (free core + paid SPN + paid network history) creates a **surveillance-as-luxury** dynamic:

- **Network History** ($$): Records and stores all your connections locally — but only if you pay? The feature is behind a paywall, meaning **the poor get surveillance, the rich get privacy**
- **SPN** ($$): Multi-hop privacy routing is a paid feature — **anonymity as a subscription service**
- This mirrors the broader digital rights crisis: **privacy tools that are free often mine your data; tools that are paid privilege those who can afford them**

### E. THE ELECTRON PROBLEM (TELEMETRY & TRUST)
Portmaster's UI is built on **Electron** ("the main UI still uses electron as a wrapper :/"). This is acknowledged in the README itself — a rare moment of developer honesty. Electron apps:
- Ship with **large attack surfaces** (Chromium + Node.js)
- Can **phone home** for updates, crash reports, usage analytics
- Are **incompatible with the privacy mission** of the tool itself

The question for the podcast: **Can a surveillance-blocking tool built on a telemetry-heavy framework truly be trusted?**

### F. GPS/LOCATION PRIVACY & THE ALPR ARCADE
While not directly in Portmaster, the broader surveillance-tech ecosystem reveals parallel concerns:
- **Issue #1297 — BSOD (KERNEL_MODE_HEAP_CORRUPTION):** Portmaster's kernel driver has caused **Blue Screens of Death** — meaning the tool can **break your entire system** in the name of protection. What's the cost of "security" that crashes your machine?
- **MaluteD/scarecrow** (370 stars): An adversarial frame pattern optimizer for evading **Automated License Plate Recognition (ALPR)**. This raises the question: **should evasion technology be legal?** When you block a police camera, are you protecting civil liberties or enabling escape from accountability?

---

## 3. KEY OPEN ISSUES TO WATCH

| # | Title | State | Why It Matters for the Podcast |
|---|-------|-------|-------------------------------|
| #1141 | Connections slow even when allowed | **Open** (70 comments) | The performance cost of surveillance-blocking: does the cure hurt worse than the disease? |
| #2096 | Microsoft WebView Is Not Privacy | Closed (14 comments) | A privacy tool *dependent* on surveillance infrastructure — the paradox in a nutshell |
| #1297 | BSOD: KERNEL_MODE_HEAP_CORRUPTION | Closed (33 comments) | When your "armor" crashes your system — the reliability cost of kernel-level access |
| #306 | Packaging for NixOS | **Open** (35 comments) | Accessibility: can privacy tools reach non-technical users? |
| #1898 | UI crashes on Wayland with NVIDIA | **Open** (12 comments) | Linux desktop fragmentation makes privacy tools unreliable for ordinary users |
| #2066 | DNS intermittently failing | **Open** (30 comments) | Even the privacy tool can't always reach the internet — what breaks when the censor breaks the tools? |

---

## 4. PODCAST ANGLES & STORY HOOKS

### 🎯 Angle 1: "The Surveillance Firewall That Surveils You Back"
**Premise:** Portmaster sees *everything* — every connection, every DNS query, every packet. The tool that promises to protect you from mass surveillance has **more visibility into your life than your ISP**. Is the cure worse than the disease?

**Questions to explore:**
- Can you trust a company (Safing) with total visibility, even if their intentions are noble?
- What happens when a government orders Safing to hand over logs?
- Does GPL-3.0 actually protect against this, or is it just moral pressure with no enforcement?

### 🎯 Angle 2: "Privacy Is a Subscription Now"
**Premise:** Portmaster's free tier blocks ads; the paid tier (SPN, Network History) actually protects *you*. The result: **the wealthy get anonymity, the poor get targeted advertising — and state surveillance.**

**Questions to explore:**
- Is "premium privacy" a form of **digital redlining**?
- Should anonymity be a human right, or a market commodity?
- What would "privacy as a public good" look like?

### 🎯 Angle 3: "The Filter List War"
**Premise:** Every block list is a **censorship decision**. When Portmaster blocks `twitter.com` as a "tracker," is that protecting you — or is it redefining what you're allowed to see?

**Questions to explore:**
- Who writes the filter lists? What biases are embedded?
- Could a government co-opt filter lists as **soft censorship**?
- Is the "ad-blocking" framing a way to smuggle censorship decisions past users?

### 🎯 Angle 4: "The Kernel Driver Dilemma"
**Premise:** To block surveillance, Portmaster installs a **kernel-mode driver** — the same level of access that rootkits use. You must trust Safing (and Microsoft/Windows) completely.

**Questions to explore:**
- Is it possible to intercept packets **without** kernel-level access?
- What's the trade-off between **security through depth** vs. **trust minimization**?
- Could a compromised driver become the next **SolarWinds**?

### 🎯 Angle 5: "Evading the Cameras — Civil Liberties or Criminal Enabler?"
**Premise:** Projects like `scarecrow` (ALPR evasion) and `WhereAreTheEyes` (surveillance camera mapping) occupy a **legal and ethical gray zone**. When you block a police camera, are you a freedom fighter or someone helping a criminal escape?

**Questions to explore:**
- Is ALPR evasion **protected speech** under the First Amendment?
- Should mapping surveillance cameras be **mandatory** (transparency) or **illegal** (obstructing law enforcement)?
- How do authoritarian regimes use the same "counter-surveillance" rhetoric to **justify oppression**?

---

## 5. BIBLIOGRAPHY & FURTHER READING

- **Portmaster GitHub:** https://github.com/safing/portmaster
- **SPN Whitepaper (Gate17):** https://safing.io/files/whitepaper/Gate17.pdf
- **Safing blog:** https://safing.io/blog/
- **Issue #2096 (Microsoft WebView):** https://github.com/safing/portmaster/issues/2096
- **Issue #1141 (Performance):** https://github.com/safing/portmaster/issues/1141
- **Issue #1297 (BSOD):** https://github.com/safing/portmaster/issues/1297
- **Related project — Scarecrow (ALPR evasion):** https://github.com/Meltedd/scarecrow
- **Related project — WhereAreTheEyes (camera mapping):** https://github.com/DaylightingSociety/WhereAreTheEyes
- **Related project — Flock-You-Android (counter-surveillance):** https://github.com/MaxwellDPS/Flock-You-Android

---

## 6. QUICK-REFERENCE CHEATSHEET

| Concept | Portmaster's Position | The Tension |
|---------|----------------------|-------------|
| Packet interception | "We see everything to block bad stuff" | same visibility as an ISP |
| Kernel driver | "Necessary for real protection" | same privilege level as a rootkit |
| SPN | "Privacy between VPN and Tor" | commercial, centralized, paid |
| Filter lists | "Block malware & trackers" | who decides what's "bad"? |
| Electron UI | "We know, it's imperfect" | telemetry-heavy framework for a privacy tool |
| Network History | "Know your traffic" | paid feature = privacy as luxury |
| DNS interception | "Secure DoT/DoH for all" | Portmaster becomes your DNS potentate |

---

*Notes compiled for podcast research. All issues and data sourced from the safing/portmaster GitHub repository on 2026-09-18.*
