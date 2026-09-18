# 🎙️ Podcast Research Notes: Digital Rights & Surveillance Technology

**Project:** Portmaster — `safing/portmaster` (forked to `bro26man-hash/portmaster`)
**License:** GPL-3.0 | **Language:** Go | **Stars:** 13,739 | **Forks:** 578
**Topics:** `application-firewall`, `dns`, `firewall`, `privacy-by-design`, `privacy-enhancing-technologies`, `privacy-protection`, `privacy-tools`

---

## 1. Project Overview

**Portmaster** is a free and open-source application firewall that intercepts all network traffic at the raw packet level to give users control over their computer's networking. Its tagline — *"Love Freedom — ❌ Block Mass Surveillance"* — makes its political orientation explicit.

### How It Works
- **Linux:** Integrates via `nfqueue` in the kernel network stack; uses eBPF and `/proc` for connection ownership.
- **Windows:** Uses a kernel driver (WFP — Windows Filtering Platform) + IP Helper API (`iphlpapi.dll`) for per-app ownership.
- **DNS:** Intercepts "astray" DNS queries and reroutes them to itself, resolving via DoT/DoH resolvers. Supports split-horizon and horizon validation against rebinding attacks.
- **Per-App Control:** Every connection can be monitored and blocked per application, including tricky processes (Snap, AppImage, Windows Store apps, `svchost.exe`).
- **Filter Lists:** Automatic blocking of malware, ad, and tracker domains via regularly updated lists.
- **SPN (Safing Privacy Network):** A proprietary privacy network (onion routing over multiple hops, positioned between VPN and Tor). Exit nodes are chosen near destination servers for geo-unblocking. Community-hosted nodes available.

### Key Tension Built Into the Project
> "Everything is 100% local on your device. (except the SPN, naturally)"

This parenthetical is a window into the project's own acknowledgment that its privacy network introduces a *trusted intermediary* — the company Safing itself hosts some nodes. This is a design choice that deserves podcast scrutiny.

### Media Appearances
Featured on **Heise Online**, **ghacks.net**, **Techlore** (YouTube), and **Lifehacker**.

---

## 2. Societal Concerns & Ethical Tensions

### 2A. Court-Ordered Censorship & Internet Blocking
**Source:** [Issue #1366 — "Download blocked by Hamburg Regional Court"](https://github.com/safing/portmaster/issues/1366)

A user discovered that the **Hamburg Regional Court** had blocked access to `yt-dl.org` (the download site for the popular YouTube downloader `yt-dl`). Even using Portmaster with multiple DNS providers and VPNs, the block could not be bypassed — suggesting ISP-level or network-level enforcement that no endpoint tool can circumvent.

**Podcast Angles:**
- **When did Europe become China?** German courts can order blocks on software distribution sites. Is this the normalisation of internet censorship in the name of copyright enforcement?
- **The illusion of control:** Portmaster intercepts every packet — but it can't bypass a court-ordered block at the ISP level. This reveals a fundamental power asymmetry: individual tools can protect you from corporate surveillance, but not from state-directed censorship.
- **Chilling effects:** If a court can block a YouTube downloader site, what's to stop them from blocking privacy tools themselves? The project *is* the target of surveillance-blocking — could it one day be blocked *for* doing that?
- **Geopolitical asymmetry:** A German court's ruling is globally enforceable on German ISPs, but the tool is developed in Austria and used worldwide. Who governs the internet when courts, corporations, and open-source communities operate in different jurisdictions?

### 2B. The Microsoft Webview Controversy — Privacy Tool Dependence on Surveillance Infrastructure
**Sources:** [Issue #2031 — "Love Freedom, hate Webview"](https://github.com/safing/portmaster/issues/2031) | [Issue #1932 — "Portmaster V2 forces microsoft webview installation"](https://github.com/safing/portmaster/issues/1932)

Portmaster V2 migrated from Electron to the **Tauri framework**, which uses the native OS WebView. On Windows, that means **Microsoft's Edge WebView2** — a component that privacy-conscious users often explicitly remove from their systems.

**The Conflict:**
- **Users' perspective:** "A privacy tool that forces you to install Microsoft's WebView2 is like a security company that sells you a lock but requires you to give Microsoft a copy of your keys." One user quit ProtonVPN over the same issue.
- **Maintainers' perspective:** WebView2 is an OS-level component, pre-installed on Windows 11. It's not Microsoft "spying" through it — it's a rendering engine. They argue that if you're serious about escaping Microsoft, you should switch to Linux.
- **Community division:** Some power users argued Portmaster can still block WebView2's tracking connections using filter lists. Others said the *principle* matters more than the practical workaround.

**Podcast Angles:**
- **The impossibility of "pure" privacy on surveillance OS:** Using Windows means accepting Microsoft's telemetry infrastructure. Can a privacy tool *stay* on Windows and remain trustworthy? Or is the platform itself the surveillance problem?
- **The Tauri trade-off:** Tauri reduces installer size (no bundled Electron browser). But it outsources rendering to the OS — which on Windows means Microsoft. Is this a sustainable compromise, or a slippery slope?
- **"Offline installer" irony:** The V2 installer was supposed to be offline, yet it requires downloading WebView2 from Microsoft during installation. Users called this "ironic, and sloppy." When a privacy tool's "offline" mode depends on contacting the surveillance giant, what does "offline" even mean anymore?
- **Dependency as vulnerability:** If Microsoft can push updates to WebView2 that break Portmaster, the privacy tool's security depends on a corporation's update cycle. Who controls *your* security when the dependencies are controlled by someone else?
- **The "just switch to Linux" deflection:** The maintainers' suggestion that privacy-conscious users should abandon Windows entirely is a class privilege argument. Not everyone can or wants to run Linux. Does the open-source community have an obligation to make tools accessible on all platforms, even surveillance-heavy ones?

### 2C. The Trust Model Problem — Who Watches the Watchmen?
**Source:** SPN (Safing Privacy Network) architecture

SPN uses onion routing like Tor, but:
- **Nodes are hosted by Safing (the company) and the community.** Safing operates some exit nodes.
- **SPN is a paid feature.** Free users get the firewall; privacy network access costs money.
- **The whitepaper** is available but the network is proprietary — not independently auditable like Tor.

**Podcast Angles:**
- **Proprietary privacy:** Tor is open and auditable. SPN is a commercial product. When a privacy tool becomes a business, whose interests does it serve?
- **Exit node trust:** Who runs the exit nodes? If Safing operates one, they could theoretically observe traffic (even if encrypted in transit). This is the same debate as "can you trust your VPN?"
- **Freemium surveillance:** The free version blocks trackers; the paid version routes your traffic through a proprietary network. Is this democratizing privacy or creating a two-tier system where the wealthy get better anonymization?

### 2D. The Meta-Surveillance Paradox
**Source:** Portmaster's own architecture

Portmaster intercepts *every packet* on your computer. To block surveillance, it must *see* all surveillance. This means:
- It maintains a local database of network connections (Network History feature, paid).
- It logs per-app bandwidth usage.
- It processes DNS queries centrally.

**Podcast Angles:**
- **The panopticon you build yourself:** A tool that monitors all your network activity to protect you from being monitored is itself a monitoring tool. Who watches the watchman?
- **Local vs. remote, but still local:** The data stays on your device — unlike corporate surveillance. But the *capability* to record, search, and analyze your every connection is the same capability that intelligence agencies argue they need. Does the intent change the nature of the tool?
- **Future abuse potential:** If Portmaster's database of connections were subpoenaed, hacked, or accidentally exposed, it would be a goldmine. The tool that protects you could also be used against you.

---

## 3. Broader Themes for the Episode

### 3A. The Censorship–Surveillance Nexus
- Court-ordered blocks (Issue #1366) show that censorship and surveillance are two sides of the same coin. You can't have "metadata collection for security" without the infrastructure to also block "unwanted" content.
- The same network infrastructure that enables mass surveillance also enables court-ordered censorship. Tools that fight one often can't fight the other.

### 3B. The Platform Problem
- You cannot build meaningful privacy on a surveillance platform (Windows, macOS). The OS itself is the surveillance instrument.
- The "just switch to Linux" argument ignores accessibility, compatibility, and privilege.
- This is a structural issue, not a tool-level issue. No firewall can fully compensate for a compromised platform.

### 3C. The Open-Source Trust Problem (The Trust Question)
- Open-source code (GPL-3.0) lets you verify what the tool does. But you can't verify what the *platform* does.
- Proprietary privacy networks (SPN) introduce a trust dependency that contradicts open-source principles.
- The community debate over WebView2 reveals a deeper fracture: **pragmatists vs. purists** — those who accept Microsoft dependencies as unavoidable vs. those who see any compromise as a betrayal.

### 3D. The Freemium Dilemma
- When privacy tools become businesses, they face pressure to monetize user data or create tiered access to privacy features.
- Portmaster's SPN is a good case study: the *free* firewall protects you from corporate trackers; the *paid* network protects you from everything (including the free tier's provider?).
- Can a privacy tool be both sustainable and trustworthy? Or does monetization inevitably create conflicts of interest?

### 3E. Civil Liberties and the Law
- The Hamburg court block (Issue #1366) raises questions about **due process:** Can a court order a block without the site operator being heard? Is this a violation of the right to receive and impart information?
- **Prediction:** If courts can block software distribution sites, they can block privacy tool websites. The surveillance industry is simultaneously selling surveillance tools and trying to censor tools that resist surveillance.

---

## 4. Key Quotes from the Community

> *"I have removed webview from my system and never encountered any issues so far. Your advice is heard and highly questioned."* — Issue #1932

> *"It's pathetic a privacy product FORCES its users to install any software from Microsoft."* — Issue #1932

> *"I was a ProsonVPN customer and since they updated their software to automatically download Microsoft Webview I quit their product."* — Issue #1932

> *"This censorship is shocking."* — Issue #1366, on the Hamburg court block

> *"Since you are free and open source and love freedom and not vendor lock-in, can your app use CEF instead of webview."* — Issue #2031

> *"I absolutely second this. I was shocked when I was prompted to (re-)install Microsoft Webview as a hard requirement for Portmaster. I also second the argument that an open-sourced firewall must not rely on Big Tech companies who are known to collect and sell their user's data."* — Issue #2031

---

## 5. Suggested Podcast Segment Structure

| Segment | Topic | Key Question |
|---------|-------|-------------|
| **Cold Open** | The Hamburg Court Block | "A German court blocked a software download site — and no firewall could bypass it. What does that mean for your right to privacy tools?" |
| **Act 1** | Portmaster & the Architecture of Surveillance | "How does a network firewall actually work? And why does watching *all* your traffic to protect you from watchers create a paradox?" |
| **Act 2** | The WebView2 War | "A privacy tool forces you to install Microsoft's WebView. Is this a practical compromise or a betrayal of principles? And why did it make one user quit ProtonVPN?" |
| **Act 3** | The Trust Problem | "If a privacy network's exit nodes are run by the company selling you the product, who are you really trusting? And can open-source code fix that?" |
| **Closing** | The Bigger Picture | "Censorship and surveillance are the same infrastructure seen from different angles. The question isn't whether you have a firewall — it's whether the platform you're standing on is already surveilling you." |

---

## 6. Sources & Further Reading

| Source | Link |
|--------|------|
| Repository | https://github.com/safing/portmaster |
| Website | https://safing.io |
| SPN Whitepaper | https://safing.io/files/whitepaper/Gate17.pdf |
| Wiki | https://wiki.safing.io |
| Issue #1366 (Censorship) | https://github.com/safing/portmaster/issues/1366 |
| Issue #2031 (WebView Ethics) | https://github.com/safing/portmaster/issues/2031 |
| Issue #1932 (WebView Force) | https://github.com/safing/portmaster/issues/1932 |
| Code of Conduct | Contributor Covenant v1.4 (standard community governance) |

---

*Notes compiled for podcast pre-production. Forked from `safing/portmaster` for reference and annotation.*
