# 🎙️ Podcast Research Notes: Portmaster & the Counter-Surveillance Frontier

> **Project:** [safing/portmaster](https://github.com/safing/portmaster) — "Love Freedom – Block Mass Surveillance"
> **Forked to:** `bro26man-hash/portmaster`
> **Stars:** 13,737 | **Forks:** 578 | **License:** GPL-3.0 | **Language:** Go
> **Developed in:** EU (Austria) 🇪🇺

---

## 1. Project Overview

Portmaster is a free and open-source **application firewall** for Windows and Linux that intercepts network packets at the kernel level to give users granular control over which applications can communicate with the outside world. Its stated mission is to "restore privacy and take back control over all your computer's network activity."

### Core Capabilities
- **Raw packet interception** via `nfqueue` (Linux) and WFP kernel driver (Windows)
- **Per-app firewall rules** — block or allow connections on a per-application basis
- **Automatic tracker/malware blocking** via filter lists
- **Secure DNS** (DoT/DoH) with split-horizon and rebinding attack defense
- **SPN (Safing Privacy Network)** — a paid, onion-routing privacy network positioned "between VPN and Tor"
- **Network history & bandwidth monitoring** (paid features)

### Technical Architecture
- Integrates into the network stack at the raw packet level
- Uses eBPF and `/proc` on Linux; kernel driver + IP Helper API on Windows
- Supports complex processes (Snap, AppImage, scripts on Linux; Store apps, `svchost.exe` on Windows)
- 100% local processing (except SPN, which routes through Safing-hosted and community nodes)
- UI built with Electron (noted as a future change target)

---

## 2. Societal Concerns & Ethical Tensions

### A. The "Is It Really FOSS?" Debate — Issue #2132
One of the most heated discussions in the community revolves around whether Portmaster's **freemium model** (core firewall is free; SPN, Network History, Bandwidth Visibility, VPN Compatibility, VM/Docker Support are paywalled) is compatible with the spirit of open source.

**Key tensions:**
- **The four freedoms vs. sustainable development:** The critic argues that paywalling features that are purely local (Network History, Bandwidth Visibility) violates FOSS philosophy. The developer side argues they need revenue to sustain the project.
- **Kernel-level trust:** A concern was raised that requiring kernel permissions on Windows while being a closed-source-ish product creates a trust paradox — you're asked to hand over system-level control to software whose full behavior you can't audit.
- **GNOME delisting:** GNOME removed Portmaster from its official extensions/recommendations, labeling it as non-FOSS, which amplified the debate.
- **Comparison with simplewall:** Users point to simplewall as a truly FOSS alternative — lightweight, no paywalls — but note it lacks filter lists and per-app granularity that Portmaster provides.

**Podcast angle:** *Can a privacy tool that restricts user freedom (via paywalls and kernel dependencies) truly claim to be fighting for user sovereignty? Where's the line between "open core" and "open wash"?*

### B. Microsoft Webview2 Dependency — Issue #2031
Portmaster required users to install **Microsoft's WebView2** (a Chromium-based embedded browser) as a hard dependency — even on systems where users had deliberately avoided Microsoft ecosystem software.

**Key tensions:**
- **Privacy tool depending on surveillance-adjacent tech:** Users who chose Portmaster to escape Microsoft's data collection were confronted with the app requiring a Microsoft component to function.
- **Vendor lock-in vs. practical necessity:** The developer side argued that on Windows, interacting with `svchost.exe` and the networking stack requires WebView2 — it's a platform limitation, not a choice. Critics argue this is exactly the kind of "acceptable inconvenience" that surveillance normalizers use.
- **CEF alternative:** A user proposed using Chromium Embedded Framework (CEF) instead, which would be fully open-source and not tied to Microsoft's update pipeline — but the project hasn't adopted this.

**Podcast angle:** *When your anti-surveillance tool requires the same company you're trying to protect against as a hard dependency, what does that say about the feasibility of digital autonomy on proprietary platforms?*

### C. DNS Over-HTTPS Interference — Issue #500
Portmaster was reported to **disable Firefox's built-in DoH (DNS over HTTPS)** and force users into DoT (DNS over TLS) instead — even when users explicitly configured DoH and disabled the "Block Bypassing" feature.

**Key tensions:**
- **Who controls your DNS?** Portmaster intercepts all DNS queries at the system level, which means it can override browser-level privacy settings. This creates a central point of control that the very privacy-conscious would find ironic.
- **Transparency:** Users reported that even with "Block Bypassing" disabled, Portmaster still forced DoT, and the override wasn't socket-level — it was deep enough to alter browser behavior (writing an empty `user.js` file).
- **The paradox of firewall-induced DNS leaks:** A tool designed to prevent surveillance was itself interfering with a key privacy protocol (DoH), potentially creating a less private experience than the user intended.

**Podcast angle:** *If a privacy tool can override your browser's privacy settings without clear consent, is it a guardian or a gatekeeper? Who decides what "privacy" means — the user or the tool?*

### D. The Windows Telemetry Dilemma — Issue #894
A user asked whether Portmaster can actually block **all** Windows network traffic, including Windows' own telemetry calls to Microsoft servers — especially during boot before Portmaster loads.

**Key tensions:**
- **The boot gap:** Even with a kernel-level firewall, there's a window during Windows startup where Microsoft's services can phone home before Portmaster's rules are active.
- **Closed-source OS dependency:** Windows itself contains closed-source components that Portmaster cannot monitor or control — a fundamental limitation of building privacy tools on proprietary operating systems.
- **Full system control request:** Portmaster requests full system control on Windows, which critics argue is itself a form of surveillance — the tool needs to see everything to block everything.

**Podcast angle:** *Is it possible to achieve meaningful privacy on a platform designed by a company whose business model depends on data collection? Or are we just rearranging deck chairs on the Titanic?*

---

## 3. Broader Ethical & Civil Liberties Angles for the Podcast

### The Arms Race Between Surveillance and Privacy
Portmaster sits in a long lineage of counter-surveillance tools — from Tor to Tails to iptables. Talk to your audience about:
- **The cat-and-mouse game:** As governments and corporations develop more sophisticated surveillance, privacy tools must innovate — but innovation requires resources, which creates pressure toward commercialization.
- **The accessibility gap:** Tools like Portmaster require technical knowledge to configure properly. Does privacy become a luxury good for the technologically literate?

### The Freemium Privacy Dilemma
- **Sustainability vs. ideology:** Can privacy tools survive without paywalls? Or do paywalls inevitably erode the trust that makes privacy tools valuable?
- **The "open core" justification:** If the core (firewall) is fully functional and free, is it fair to charge for the "convenience" features? Or does any paywall in a privacy tool undermine its mission?

### Platform Dependency as a Civil Liberties Issue
- **Windows as a surveillance platform:** Building anti-surveillance tools on Windows is inherently compromised. The OS itself reports to Microsoft. Portmaster's kernel-level access is both its power and its vulnerability.
- **The WebView2 problem:** Requiring a Microsoft component in a privacy tool is a microcosm of how deeply surveillance infrastructure is embedded in everyday computing.

### The Trust Paradox
- **Kernel access = total visibility:** Portmaster sees every packet on your system. This means the developers *could* theoretically log everything. The GPL license gives you the code to verify, but does anyone actually audit 40,000 lines of Go for behavioral analysis?
- **SPN's centralized nodes:** The Safing Privacy Network uses nodes hosted by Safing themselves and the community. Centrally hosted exit nodes are a single point of failure/trust — similar to the concerns about VPN providers.

### The FOSS Identity Crisis in Privacy Tech
- **GNOME delisting as a watershed moment:** When a major Linux distribution removes a privacy tool for being non-FOSS, it signals that the community sees FOSS as a *de facto* requirement for privacy tools — not just a preference.
- **"Open wash" concerns:** As privacy tech gains mainstream attention, there's risk of projects using the "open source" label for marketing while restrictively encumbering the user experience.

---

## 4. Community Sentiment Summary

| Sentiment | Evidence |
|---|---|
| **Strong appreciation for core functionality** | Many users praise Portmaster as "like nothing else on Windows" for blocking webview, trackers, and svchost.exe telemetry |
| **Deep frustration with paywalls** | Issue #2132 (7 comments, 5 👍, 1 😂, closed as stale) — no maintainer engagement on the core question |
| **Resentment of Microsoft dependencies** | Issue #2031 (8 comments) — users refuse to use the app until WebView2 dependency is removed |
| **Technical debts acknowledged** | Issues around CPU waste (#2195), DNS failures (#2066), crashes (#2265) suggest the project is still maturing |
| **Security concerns** | Issue #2193 reports a "CRIT intel update path" vulnerability — coordinated disclosure ready |

---

## 5. Suggested Podcast Episode Structure

### Segment 1: The Promise (5 min)
- Introduce Portmaster: what it does, why it matters, who uses it
- The libertarian ideal: "Take back control of your network"

### Segment 2: The Contradictions (15 min)
- The FOSS debate: Can a privacy tool be both free and sustainable?
- The WebView2 problem: Anti-surveillance software that depends on surveillance infrastructure
- The DNS paradox: A firewall that overrides your browser's privacy settings
- The Windows dilemma: Building liberty on a platform designed for data collection

### Segment 3: The Bigger Picture (10 min)
- The economics of privacy: Who gets to have privacy?
- The trust paradox: We hand kernel-level control to open-source code we never audit
- The platform problem: Is privacy on Windows a contradiction in terms?

### Segment 4: What's Next (5 min)
- Could Portmaster evolve to address these tensions?
- What would a truly libertarian privacy tool look like?
- Call to action: Audiences should read the source, demand transparency, and support genuinely open privacy projects

---

## 6. Key Sources & Further Reading

- **Portmaster repository:** https://github.com/safing/portmaster
- **Portmaster wiki:** https://wiki.safing.io/
- **SPN Whitepaper:** https://safing.io/files/whitepaper/Gate17.pdf
- **Safing Code of Conduct:** Contributor Covenant v1.4 (standard, no unique provisions)
- **Issue #2132 (FOSS debate):** https://github.com/safing/portmaster/issues/2132
- **Issue #2031 (WebView2 debate):** https://github.com/safing/portmaster/issues/2031
- **Issue #500 (DoH interference):** https://github.com/safing/portmaster/issues/500
- **Issue #894 (Windows telemetry):** https://github.com/safing/portmaster/issues/894
- **Related projects:** simplewall (truly FOSS Windows firewall), Tor, Tails, Linux iptables/nftables

---

*Notes compiled from GitHub repository analysis, open issue review, and community discussion examination. Forked to `bro26man-hash/portmaster` for reference.*
