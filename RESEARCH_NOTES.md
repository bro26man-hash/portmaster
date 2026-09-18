# 🎙️ Podcast Research Notes: Portmaster & the Ethics of Counter-Surveillance

> **Project:** [safing/portmaster](https://github.com/safing/portmaster) — "🏔 Love Freedom — ❌ Block Mass Surveillance"
> **Forked to:** `bro26man-hash/portmaster`
> **Stats:** 13,738 stars · 578 forks · GPL-3.0 · Written in Go · Developed in Austria (EU)
> **License:** Free & open-source, with paid "Plus/Pro" tiers for advanced features

---

## 1. PROJECT SUMMARY

Portmaster is a free, open-source **application firewall** that intercepts every network packet on a user's computer at the raw level (using `nfqueue` on Linux, Windows Filtering Platform on Windows). Its stated mission is unequivocal: **"Block Mass Surveillance."**

Key technical capabilities:
- **Raw packet interception** — every packet is seen and can be stopped
- **per-app firewall rules** — block or allow network access on a per-application basis
- **Automatic tracker & malware domain blocking** using community-maintained filter lists
- **Secure DNS** (DNS-over-TLS / DNS-over-HTTPS) with split-horizon validation against rebinding attacks
- **Safing Privacy Network (SPN)** — a multi-hop onion-routing privacy network positioned "between VPN and Tor"
- **100% local processing** (except SPN traffic) — no phoning home beyond signed updates and intelligence data

The project has been featured in *Heise Online*, *ghacks.net*, *Techlore*, and *Lifehacker*, and is widely cited in privacy-orientated communities.

---

## 2. SOCIETAL CONCERNS & ETHICAL TENSIONS

### A. The "Failsafe" Dilemma — Fail Open vs. Fail Closed

**Source:** [Issue #329 — "Self-defense and kill-switch"](https://github.com/safing/portmaster/issues/329) (6 comments, closed as completed)

A community member requested two features:
1. **Process protection** — preventing Portmaster from being force-killed by third-party process managers
2. **A kill-switch** — automatically cutting the internet connection if Portmaster detects it is malfunctioning

**Maintainer response (dhaavi):**
> *"You'll need to be SysAdmin/root to stop Portmaster… I don't think there is a feasible protection against someone who is SysAdmin/root. Once malware has administrator rights you'll have a very bad time anyway."\*

**Community developer (ppacher):**
Discussed Windows "Protected Processes" and kernel driver unload prevention, noting that even protected processes can sometimes be bypassed via driver deregistration.

**Power-user perspective (youtdontneedtoknow22):**
> *"If the Firewall was being used to enforce all connections through a VPN (kind of a Kill-Switch for VPNs with no clients), killing the firewall means revealing the real IP address."*

**🎙️ Podcast angles:**
- **The asymmetry of defense**: Privacy tools are only as strong as the least-privileged layer. If an adversary gains admin access, the entire security stack is compromised. This mirrors real-world scenarios where authoritarian regimes push legislation to criminalize VPN use or mandate "backdoor" access — the tool becomes illegal before it can be used.
- **Should a privacy tool be designed to fail-closed (block all traffic when it detects a problem) or fail-open (keep traffic flowing)?** Portmaster currently fails open. The community wanted a failsafe that fails closed. This is a profound design philosophy question with civil-liberties implications: a fails-closed tool protects your anonymity even at the cost of connectivity, while a fails-open tool prioritizes convenience — potentially exposing you.
- **The "protected process" paradox**: Windows has a "Protected Process Light" mechanism designed specifically for anti-malware services. Portmaster *could* theoretically leverage this, but doing so would make it behave like antivirus software — blurring the line between "privacy tool" and "security tool" in ways that might attract unwanted regulatory attention.

---

### B. The Fragility of Anonymity — Tiny Leaks, Real-World Consequences

**Source:** [Issue #313 — "Portmaster is compatible with Mullvad when setting custom DNS"](https://github.com/safing/portmaster/issues/313) (55 comments, closed as "not planned")

A user reported that running Mullvad VPN alongside Portmaster resulted in no connectivity. A lengthy debugging session revealed:
- Mullvad's own DNS handling conflicted with Portmaster's DNS interception
- The fix required configuring Mullvad to use `127.0.0.1` as its DNS server (letting Portmaster handle DNS)
- However, another user (TinyJay42) then discovered a **DNS leak** — their real IP was visible to DNS leak detection sites, with a Cloudflare IP appearing instead of their actual address

Most critically, TinyJay42 noticed something disturbing:
> *"During [signing up for Mailchimp], they populated my real location (city) — with no autofill or storage of my real location anywhere in the browser or even this particular computer."*

The DNS leak had allowed Mailchimp to geolocate them, and the leak detection test itself was a **false positive** — VPN providers flag "leaks" when you're not using *their* DNS server, even though Portmaster's encryption still protects your queries.

**🎙️ Podcast angles:**
- **The domino theory of privacy**: A single misconfigured DNS setting cascaded into a real-world identification event. This is a powerful narrative device — anonymity isn't binary; it's a chain, and the weakest link is often the one you didn't know existed.
- **The false-positive paradox**: VPN DNS leak tests are designed to sell anxiety. Portmaster's maintainer explicitly documented that these tests produce false positives, yet users *still* panic. This speaks to the broader problem: the privacy industry monetizes fear, and the line between "educated caution" and "commercial Surveillance Theater" is often blurred.
- **Re-identification from "anonymous" data**: Mailchimp guessed the user's city from an IP leak — no cookies, no login, no fingerprinting. Just a DNS misconfiguration. This is the same technique used by researchers to "anonymize" datasets that are trivially re-identifiable. The lesson: in a data-rich world, *almost nothing* is truly anonymous.
- **The layer-cake problem**: Portmaster + Mullvad + DoH/DoT is a *three-layer* privacy stack, yet a single misconfiguration negated all of it. This mirrors how citizens who use multiple privacy tools (encrypted messaging, VPNs, anonymous browsing) can still be de-anonymized through operational security mistakes. The technology is only as strong as the human operating it.

---

### C. The Arms Race — Counter-Surveillance Tools in a Hostile World

**Source:** Project mission statement + Issue #329 + overall project philosophy

Portmaster's tagline — **"Block Mass Surveillance"** — is a political statement, not just a feature list. It explicitly names the adversary: *mass surveillance*. This positions the tool not as a general security utility, but as a **civil-liberties instrument**.

Yet the self-defense issue reveals a troubling reality: **the tool cannot protect itself from a determined adversary with admin access.** A state-level actor who gains administrative control can simply kill Portmaster, uninstall it, or inspect its traffic logs.

**🎙️ Podcast angles:**
- **The legality gap**: In some jurisdictions (China, Iran, Russia, UAE), using VPNs or anonymity tools is already criminalized. Portmaster's "kill-switch" feature would be less about malware protection and more about **resisting state coercion** — a function that existing law doesn't accommodate.
- **The "attack surface" paradox**: To intercept every packet, Portmaster must run at the kernel level. This gives it maximum power — and maximum risk. A kernel-level vulnerability in Portmaster would be *more* dangerous than having no firewall at all. The very architecture that makes it powerful also makes it a high-value target.
- **Why "open source" matters for civil liberties**: Portmaster's GPL-3.0 license means anyone can audit the code for backdoors or surveillance chips. In an era of supply-chain attacks (e.g., the SolarWinds hack), **transparent, auditable code is itself a civil-liberties safeguard**. A proprietary "privacy tool" is indistinguishable from a surveillance tool unless you can read the source.

---

### D. The Sustainability Paradox — Can Privacy Be a Public Good?

**Source:** Project structure (free base + paid SPN/network history features), GitHub discussion culture

Portmaster is developed by **Safing**, a company based in Austria. The core firewall is free, but advanced features (SPN routing, network history, bandwidth monitoring) are behind a paywall. The free tier explicitly has "limited support."

**🎙️ Podcast angles:**
- **The gentrification of privacy**: When privacy tools become commercialized, they risk becoming products for people who can *afford* privacy — creating a two-tier system where the wealthy get anonymity and the rest get monitored. This mirrors the broader "privacy divide" in digital rights.
- **Open-source sustainability**: Portmaster relies on a small team of maintainers (dhaavi, ppacher, stenya, eugenesvk) who appear to be unpaid volunteers or underpaid staff. The project has 103 open issues and a community that is active but not always well-served. Are we expecting volunteer developers to bear the burden of defending civil liberties?
- **The "free as in freedom" tension**: GPL-3.0 ensures the code stays open, but the *evolution* of the project is guided by a commercial entity. If Safing were acquired, pivoted, or pressured by governments, the project's direction could change without community consent. This happened to OpenSSL (Heartbleed era) and LibreOffice (The Document Foundation fork).

---

## 3. KEY QUESTIONS FOR THE PODCAST

1. **Should privacy tools be designed to fail-closed?** If a firewall detects it's being bypassed, should it cut the internet — or keep you online at the risk of exposure? How does this choice reflect different philosophies about the purpose of privacy?

2. **Can an open-source tool truly resist a state actor?** Portmaster's self-defense discussion concluded that root access = game over. Does this mean that client-side privacy tools are fundamentally powerless against determined adversaries — or is that conclusion itself a form of defeatism that discourages people from trying?

3. **Who gets to define "mass surveillance?"** Portmaster's tagline names it as the enemy, but the definition is politically charged. Is mass surveillance state warrantless bulk collection? Corporate behavioral tracking? Predictive policing algorithms? The tool blocks all of these — but should it?

4. **Is the "privacy stack" (VPN + firewall + DoH + onion routing) a form of paranoia or a rational response?** The Mullvad/DNS leak issue shows that even motivated, technically literate users can misconfigure their stack. Does this argue for simpler tools that "just work" — or does it argue that *you should never trust a single layer*?

5. **What's the responsibility of privacy-tool developers to their users' civil liberties?** Is Safing obligated to resist government pressure? Should they embed "circuit breakers" that shut down the network if a government demands backdoor access? Or is that simply software engineering, not activism?

---

## 4. USEFUL LINKS & RESOURCES

| Resource | URL |
|---|---|
| **Repository** | https://github.com/safing/portmaster |
| **Fork (yours)** | https://github.com/bro26man-hash/portmaster |
| **Official Website** | https://safing.io |
| **SPN Whitepaper** | https://safing.io/files/whitepaper/Gate17.pdf |
| **Wiki / Docs** | https://wiki.safing.io |
| **Settings Handbook** | https://docs.safing.io/portmaster/settings |
| **Developer API Docs** | https://docs.safing.io/portmaster/api |
| **GitHub Issues** | https://github.com/safing/portmaster/issues (103 open) |
| **License** | GPL-3.0 (full text in repo) |
| **Code of Conduct** | Contributor Covenant v1.4 (in repo) |
| **VPN Compatibility Guide** | https://wiki.safing.io/en/Portmaster/App/Compatibility#vpn-compatibly |
| **Architecture Overview** | https://wiki.safing.io/en/Contribute |

---

## 5. OPEN ISSUES WORTH MONITORING

These current open issues may develop into newsworthy stories:

| # | Title | Why It Matters |
|---|---|---|
| #1141 | Slow connections even when allowed | 70 comments — suggests deep architectural tensions in packet interception |
| #2066 | DNS intermittently failing | Could indicate systemic instability in the Secure DNS pipeline |
| #306 | Packaging for NixOS | 35 comments — reveals the accessibility gap: privacy tools should be installable by everyone, not just advanced users |
| #388 | Import/Export settings | 33 comments — user demand for portability suggests people want to *audit* their own privacy configurations |

---

*Notes compiled from GitHub repository analysis, issue deep-dives, and community discussion review.*
