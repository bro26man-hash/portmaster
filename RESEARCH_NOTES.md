# 🎙️ Podcast Research Notes: Digital Rights & Surveillance Technology

> **Project:** Safing Portmaster — "Love Freedom, Block Mass Surveillance"
> **Repository:** [github.com/safing/portmaster](https://github.com/safing/portmaster)
> **Stars:** 13,742 | **Forks:** 578 | **License:** GPL-3.0 | **Language:** Go
> **Developed in:** EU (Austria) — subject to GDPR by design

---

## 1. PROJECT OVERVIEW

**Portmaster** is a free, open-source application firewall for Windows and Linux that intercepts all network traffic at the raw packet level using `nfqueue` (Linux) and a Windows Filtering Platform (WFP) kernel driver (Windows). It uses eBPF and `/proc` to identify which process owns each connection, enabling per-app privacy rules.

### Key Technical Capabilities
- **Raw packet interception** — every packet is seen and can be stopped
- **Per-process ownership tracking** — knows which app is making each connection
- **Automatic tracker/malware blocking** — with regularly updated filter lists
- **Secure DNS (DoH/DoT)** — with split-horizon and rebinding attack defense
- **SPN (Safing Privacy Network)** — onion-routing multi-hop privacy network (paid)
- **Network History & Bandwidth Monitoring** — local connection logging (paid features)

### What Makes It Notable for a Surveillance Podcast
Portmaster isn't just a privacy tool — it's a **statement**. Its tagline "Love Freedom — ❌ Block Mass Surveillance" positions it as a direct countermeasure against state-level and corporate surveillance. It was developed in the EU, meaning it was built under the philosophical and legal umbrella of GDPR — the world's strongest privacy regulation. It represents the belief that **network transparency is a civil liberty issue**, not just a technical preference.

---

## 2. SOCIETAL CONCERNS & ETHICAL TENSIONS

### A. The "Mass Surveillance" Paradigm
Portmaster operates on the premise that **passive observation of network traffic is inherently a form of surveillance** — whether conducted by governments (NSA, GCHQ), corporations (Google, Meta), or ISPs. The tool assumes that:
- Default network behavior enables monitoring
- Internet users are subjects, not sovereigns
- Transparency about who collects what is a fundamental right

This framing raises a core podcast question: **Is privacy a privilege for those who can afford tools like Portmaster, or should it be the default state of the internet?**

### B. The FOSS Identity Crisis (Issue #2132)
A major community debate exploded when a user argued that **Portmaster is NOT FOSS** because key features are paywalled. The critique, which garnered 6 reactions including 5 upvotes, leveled several devastating charges:

> *"Stop calling things that are not true, Portmaster is NOT FOSS. Paywalling features goes against the spirit and philosophy of FOSS... Even GNOME has removed Portmaster as FOSS and put it under proprietary for that reason."*

**The paywalled features include:**
- Network History (local connection logging)
- Bandwidth Visibility (per-app monitoring)
- VPN Compatibility Mode
- VM & Docker Support
- Weekly Reports

**Ethical tension:** Can a tool that claims to liberate users from surveillance ethically restrict its own surveillance capabilities (connection logging) behind a paywall? If you're building a firewall to block others from watching you, is it contradictory to charge users for the ability to watch themselves?

**Counterpoint from the community:** Some users note the core firewall is genuinely free, and only the SPN (which routes through Safing's own infrastructure) is truly commercially valuable. One commenter observed: *"The firewall is 100% free. The SPN network is paid."*

**Podcast angle:** The commodification of privacy. When privacy becomes a product, who owns your data? Does a commercial entity providing "privacy" replicate the same power dynamics it claims to resist?

### C. The Microsoft Webview Dilemma (Issues #1932, #2031)
Portmaster V2 **forces users to install Microsoft WebView2** — a proprietary component from the very company most criticized for data collection. This created a crisis of conscience:

> *"I used a script with over 6000 lines of code to clean up the mess that Windows is, and installed an open source firewall software called Portmaster... as a part of the cleanup process, of course Microsoft Edge including Webview have been fully removed of my OS, i dont want Microsoft's Webview on my personal computer, as a privacy company you should agree with that and not force me to install Webview anyway."*

Another user captured the paradox perfectly:

> *"I was shocked when I was prompted to (re-)install Microsoft Webview as a hard requirement for Portmaster... I also second the argument that an open-sourced firewall must not rely on Big Tech companies who are known to collect and sell their user's data."*

**Ethical tension:** A surveillance-blocking tool that depends on surveillance-enabling infrastructure. This is the **colonization problem** — even tools designed to resist surveillance can become vectors for it. The dependency on WebView2 means:
- Microsoft can push updates that break the firewall
- The tool inherits a trust dependency on the entity it's supposed to protect against
- Users on clean systems (with WebView removed) are forced to re-enable the surveillance vector

**Podcast angle:** The "no ethics in engineering" problem. Can any privacy tool built on Windows truly be free? Is the platform itself the surveillance instrument?

### D. Kernel Space & Security Accountability (Issue #2132)
The FOSS critique also raised a profound security concern:

> *"This app should not run in the kernel space, AT ALL... If developers do not master and can't fix bugs in the realm of security, kernel space and the mechanics of firewall, then I've zero faith, period."*

The critic claimed to have found that Portmaster could be made to crash (BSOD) via a buffer overflow attack. A **privacy firewall that can be crash-bombed** is a surveillance enabler — if your "protection" tool is unstable, your traffic reverts to unprotected defaults.

**Podcast angle:** The fragility of digital rights. If the tools we use to resist surveillance are themselves fragile, unreliable, or potentially exploitable, do we even have meaningful digital rights? Or are we just performing resistance?

### E. User Access Control & Democratic Governance (Issue #2269)
An open issue reveals that **any user on a multi-user system can see and modify firewall rules**, even low-privilege accounts. The requester notes:

> *"I have to setup 10 hacky configurations to restrict low-privileged users from accessing the front-end which may break in the next update."*

**Ethical tension:** A surveillance-blocking tool with no access control means the person you're protecting *against* (or your less-tech-savvy family member) can accidentally disable your protections. This is a **governance problem** — who controls the controls?

### F. The Critical Vulnerability Disclosure (Issue #2193)
A **critical security issue** was disclosed privately regarding the "CRIT intel update path" — the channel through which Portmaster receives intelligence data (block lists, geo-IP data). If this channel is compromised, an attacker could:
- Push malicious block lists (blocking access to specific sites)
- Inject false geo-location data
- Potentially fingerprint users through update responses

**Podcast angle:** The single point of failure. When a privacy tool has a centralized update mechanism, it becomes a surveillance vector. The paradox: **the feature that makes Portmaster work (automatic intelligence updates) is the same feature that could be weaponized against its users.**

---

## 3. BROADER SOCIETAL THEMES FOR THE PODCAST

### Theme 1: The Illusion of Individual Privacy in a Mass Surveillance World
Portmaster protects the individual device. But what about the metadata? Even with Portmaster blocking trackers, your IP address, connection timing, and traffic patterns still reveal volumes. The tool addresses **application-level surveillance** but not **network-level surveillance** by your ISP or nation-state. This raises the question: **Is the anti-surveillance tool just a comfort blanket, or does it represent meaningful resistance?**

### Theme 2: Whose Freedom? The EU vs. the User
Portmaster is developed in Austria under GDPR, which gives EU residents strong privacy rights. But the tool is built on Windows (a Microsoft OS) and increasingly depends on WebView2. **Can a European privacy tool truly be sovereign when its foundational infrastructure is American?** This echoes the broader geopolitical tension: digital sovereignty is meaningless if your stack depends on the entities you're trying to resist.

### Theme 3: The Paywall as a New Form of Surveillance
When connection logging is paywalled, the free users get less transparency. This creates a **two-tier privacy** system:
- **Paid users:** Full visibility into their network activity
- **Free users:** Basic blocking but no awareness of what's being tracked

This mirrors the broader pattern where privacy is luxury and surveillance is the default for those who can't pay. **Is this the privatization of civil liberties?**

### Theme 4: The FOSS Authenticity Debate as a Civil Liberties Issue
The claim that "Portmaster is NOT FOSS" isn't just about licensing — it's about **trust**. If a surveillance-blocking tool restricts its own capabilities behind a paywall, can you trust it to block surveillance by others? The GNOME project's decision to reclassify Portmaster as proprietary is a **peer review by the free software community** — and it's damning.

**Key question for listeners:** If the people who build privacy tools can't agree on what "free" means, how can ordinary users trust any of them?

### Theme 5: The Infrastructure Paradox
Every surveillance-resistant tool faces the same dilemma:
- To resist Google, you need an OS (Windows/Linux)
- To resist Microsoft, you need hardware (Intel/AMD/NVIDIA)
- To resist nation-states, you need internet infrastructure (ISPs, undersea cables)

**You can't build freedom from within a system of control.** Portmaster's WebView2 dependency is just the most visible example. This is the **Marxist moment of digital rights**: the means of production (hardware, OS, protocols) are controlled by the entities conducting the surveillance.

### Theme 6: Security vs. Usability vs. Freedom
Portmaster sits at the intersection of three competing demands:
- **Security:** Requires kernel-level access, automatic updates, and centralized intelligence
- **Usability:** Requires simple interfaces and automatic configuration
- **Freedom:** Requires open source, no dependencies on proprietary entities, and user control

You can optimize for any two, but never all three. This is the **privacy trilemma** — and every tool reveals your priorities.

---

## 4. SPECIFIC PODCAST SEGMENT IDEAS

### Segment A: "The Firewall That Became Its Own Surveillance Tool"
Explore the WebView2 dependency story. How a tool built to block surveillance became dependent on the surveillance infrastructure of Microsoft. Parallel this with real-world cases:
- How the FBI disguised its surveillance tools as software updates (Planting bug)
- How 5G infrastructure gives governments new surveillance capabilities
- How "smart" home devices have become surveillance devices

### Segment B: "Who Owns Your Data? The FOSS Privacy Betrayal"
Deep-dive into the paywall debate. Interview maintainers of truly free firewalls (like simplewall) vs. commercial privacy tools. Ask: **Is the freemium model a betrayal of civil liberties, or a sustainable way to fund resistance?**

### Segment C: "The Kernel Space Trust Problem"
When your privacy tool runs at the kernel level, it sees everything. But if it's buggy or compromised, it exposes everything. This is the **double-edged sword of deep system access**. Interview security researchers about the tradeoffs.

### Segment D: "Europe's Privacy Dreams vs. American Surveillance Reality"
Portmaster was built in the EU, but it runs on Windows. Explore the tension between GDPR's aspirations and the technical reality that most privacy tools depend on infrastructure controlled by the Five Eyes nations.

### Segment E: "The Open Issues That Keep Privacy Advocates Up at Night"
Walk through the actual GitHub issues:
- The FOSS classification debate (#2132)
- The Microsoft Webview coercion (#1932, #2031)
- The critical update channel vulnerability (#2193)
- The missing user access control (#2269)

Each issue is a **case study in the tensions inherent in building surveillance-resistant tools**.

---

## 5. KEY QUOTES FOR THE EPISODE

> *"Love Freedom — ❌ Block Mass Surveillance"* — Portmaster's tagline, which becomes irony-laden when you examine its dependencies

> *"Portmaster is NOT FOSS. Paywalling features goes against the spirit and philosophy of FOSS"* — Community critique, Issue #2132

> *"I dont want Microsoft's Webview on my personal computer, as a privacy company you should agree with that and not force me to install Webview anyway"* — User forced to re-enable surveillance infrastructure, Issue #1932

> *"An open-sourced firewall must not rely on Big Tech companies who are known to collect and sell their user's data"* — Community member, Issue #2031

> *"This app should not run in the kernel space, AT ALL"* — Security critique of Portmaster's architecture, Issue #2132

---

## 6. FURTHER RESEARCH & LISTENING

- **Portmaster Wiki:** https://wiki.safing.io
- **SPN Whitepaper:** https://safing.io/files/whitepaper/Gate17.pdf
- **EFF Atlas of Surveillance:** https://atlasofsurveillance.org/ (used by FLOCK project for camera data)
- **Code of Conduct:** Portmaster's `CODE_OF_CONDUCT.md` (3.3KB) — review for community governance values
- **Open Issues:** 103 open issues in the upstream repo — deep well of community concerns
- **Comparison:** Compare Portmaster's model with **simplewall** (fully FOSS, no paywall, no WebView dependency, lighter footprint)

---

## 7. UNANSWERED QUESTIONS TO EXPLORE

1. Does Safing (the company) share any telemetry or user data through the SPN? The transparency is unclear per community concerns.
2. What happens to Portmaster users if Safing goes bankrupt or is acquired? Does the GPL ensure the code survives?
3. Can the per-entity filter lists (which are downloaded and applied automatically) be independently audited?
4. How does Portmaster handle legal demands from law enforcement regarding its network history feature (even the free local logging)?
5. What is the community's experience with Portmaster on Linux vs. Windows? Is the surveillance-resistance story different on each platform?

---

*Notes compiled for podcast episode research. Forked from safing/portmaster for reference.*
*Last updated: September 2026*
