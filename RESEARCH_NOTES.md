# 🎙️ RESEARCH NOTES — Digital Rights & Surveillance Technology

> **Podcast episode research notes**  
> Compiled from GitHub open-source projects in the surveillance, privacy, and counter-surveillance space.  
> Focus project: **Portmaster** (safing/portmaster) — 13,739★, GPL-3.0, Go  
> Companion project: **ShadowBroker** (BigBodyCobain/Shadowbroker) — 11,184★, AGPL-3.0, Python/Next.js

---

## 1. THE LANDSCAPE — What We Found

### 1A. Portmaster (safing/portmaster)

**What it is:** A free, open-source application firewall that intercepts every packet at the kernel level to give users full visibility and control over their network activity. Its tagline: *"Love Freedom — ❌ Block Mass Surveillance."*

**Key technical features:**
- **Kernel-level packet interception** — Uses `nfqueue` on Linux and Windows Filtering Platform (WFP) on Windows. Every packet is seen and can be stopped.
- **eBPF-based connection ownership** — Identifies which process owns each connection (Linux). On Windows, uses kernel driver + IP Helper API.
- **Secure DNS** — Intercepts "astray" DNS queries and reroutes to DoT/DoH resolvers. Split-horizon validation defends against rebinding attacks.
- **Privacy Filter** — Rules based on domains, IPs, countries. Filter lists block malware, ad, and tracker domains.
- **SPN (Safing Privacy Network)** — Onion encryption over multiple hops (like Tor), but routes are chosen to maximize distance within the network and exits are chosen near destination servers for geo-unblocking.
- **Per-app controls** — Granular rules for individual applications, including Windows Store apps, Snap, AppImage, and scripts.
- **100% local** (except SPN) — All processing happens on-device. Updates are signed and downloaded automatically.

**Community & reach:**
- 13,739 stars, 578 forks on GitHub
- GPL-3.0 licensed
- Written in Go
- Featured on Heise Online, ghacks.net, Techlore, Lifehacker
- 103 open issues (mostly technical bugs/feature requests — not ethics-focused)
- Topics: `application-firewall`, `dns`, `firewall`, `privacy-by-design`, `privacy-enhancing-technologies`, `privacy-protection`, `privacy-tools`

**What the open issues reveal:**  
The 103 open issues are overwhelmingly technical (bug reports, feature requests, UI improvements, compatibility). There are *no* open issues tagged or discussing ethics, civil liberties, or societal concerns. This is itself notable — it suggests either (a) the community sees Portmaster as a tool, not a movement, or (b) ethical questions are discussed elsewhere (forums, mailing lists) and not surfaced as GitHub issues.

### 1B. ShadowBroker (BigBodyCobain/Shadowbroker) — Companion Project

**What it is:** A decentralized global intelligence platform that aggregates real-time telemetry from 60+ live feeds into a single map interface. Aircraft, ships, satellites, conflict zones, CCTV networks, GPS jamming, police scanners, mesh radio — all on one screen.

**Why it matters for the episode:**  
ShadowBroker is the *other side* of the surveillance coin. Where Portmaster blocks surveillance, ShadowBroker *aggregates and visualizes* publicly available surveillance data. It raises the question: **Is aggregating public surveillance data a form of surveillance itself?**

**Key features with ethical weight:**
- **22,000+ live CCTV cameras** across 10 countries (UK, US, Spain, Singapore, Austria, Netherlands, etc.)
- **Satellite imagery at 10m resolution** (Sentinel-2) — sufficient to identify individuals in some contexts
- **Shodan integration** — search internet-connected devices: cameras, SCADA systems, databases
- **Telegram OSINT scraping** — public war/conflict feeds geoparsed onto the map
- **AI agent command channel** — any compatible LLM agent can autonomously query all data layers, run recon, and place "intel pins" on the map
- **InfoNet mesh** — obfuscated messaging (explicitly labeled as *NOT* end-to-end encrypted; the project warns: *"Do not transmit anything sensitive on any channel"*)
- **"Sovereign Shell" governance** — on-chain petitions, upgrade-hash voting, dispute markets for the decentralized intelligence platform

**Self-described stance:**  
> "The project does not introduce new surveillance capabilities — it aggregates and visualizes existing public datasets. It is fully open-source so anyone can audit exactly what data is accessed and how."

This disclaimer is itself an ethical statement worth unpacking on the podcast.

---

## 2. SOCIETAL CONCERNS — The Big Themes

### 2A. The Asymmetry of Surveillance

**The core tension:** States and corporations have near-unlimited capacity to surveil citizens. Individuals have almost no comparable capacity. Tools like Portmaster attempt to rebalance this asymmetry — but do they succeed?

**Discussion angles:**
- Portmaster gives an *individual* the power to see and block network surveillance. Is this enough when the other side has nation-state resources?
- ShadowBroker gives an *individual* the power to see what states and corporations are doing (tracking Air Force One, billionaire jets, military satellites). This is "reciprocal transparency" — but does it create accountability or just escalation?
- **Podcast question:** *Can privacy tools ever truly levels the playing field, or do they just create an illusion of parity?*

### 2B. Who Watches the Watchmen?

**Portmaster's own architecture raises this:**
- Portmaster runs as a **system service** with kernel-level access. It sees *every* packet on your computer. This means Portmaster itself becomes a surveillance tool — just one pointed inward instead of outward.
- The SPN (Privacy Network) routes traffic through **nodes operated by Safing** (the company behind Portmaster). Safing can see your traffic. The project says nodes are also hosted by the community, but the company's role as a potential surveillance point is underexplored.
- **GPL-3.0 license** means the code is auditable — but *who actually audits it?* Open-source is necessary but not sufficient for trust.
- **Podcast question:** *If a privacy tool has deep enough access to protect you, doesn't it also have enough access to surveil you? How do you build a lock that the locksmith can't pick?*

**ShadowBroker amplifies this:**
- ShadowBroker integrates **Shodan** (internet device search), **recon toolkits** (WHOIS, DNS, BGP, CVE lookups), and **AI agents** that can autonomously run surveillance. The "watchmen" here are any user with a Docker install.
- The project explicitly states it does NOT introduce new surveillance capabilities — but the *aggregation* of 60+ feeds into a single, AI-queryable interface *is* a new capability. It's the difference from having 60 separate tabs open.
- **Podcast question:** *Is aggregation a form of creation? If you assemble publicly available fragments into a coherent picture, have you created something new — and do the original sources have any say?*

### 2C. The Ethics of "Public Data" Aggregation

**ShadowBroker's disclaimer:** "A surprising amount of global telemetry is already public."

**But is it truly public?**
- ADS-B broadcasts are public by protocol design — but the *implication* of tracking every private jet globally is profound.
- AIS vessel data is public — but tracking billionaire superyachts raises stalking concerns.
- Police scanner feeds are public — but real-time eavesdropping on emergency communications feels different from reading archived transcripts.
- CCTV feeds are public-facing — but 22,000+ cameras aggregated into one dashboard is a pandemonium of visibility.
- Telegram channels are public — but geoparsing war feeds and risk-scoring them introduces editorial judgment.

**The "public data" defense mirrors Big Tech's argument:** "We're just connecting publicly available information." But the *synthesis* is the product, and the synthesis changes the ethical calculus.

**Podcast question:** *When does aggregation become surveillance? When does "publicly available" become "publicly dangerous"?*

### 2D. AI Agents and Autonomous Surveillance

**This is the most urgent new concern.**

ShadowBroker's **Agentic AI Command Channel** allows any compatible LLM agent to:
- Query all 40+ data layers autonomously
- Run recon toolkits (IP/DNS/WHOIS/sanctions/CVE/MAC/subnet sweeps)
- Place "intel pins" on the map with confidence scores
- Fly the operator's map view to any coordinate
- Generate structured intelligence reports
- Participate in the InfoNet mesh and Sovereign Shell governance

**The ethical chain:**  
Human operator → configures AI agent → AI agent autonomously surveys the world → AI agent reports findings → AI agent can take map actions

**Who is responsible when the AI agent surveils the wrong target?** The operator? The developer? The open-source community? No one?

**Podcast question:** *When AI agents can autonomously surveil the globe using open-source tools, who bears moral responsibility for what they find — and what they do with it?*

### 2E. The Privacy Paradox of "Anti-Surveillance" Tools

**Portmaster's SPN reveals a structural paradox:**
- SPN uses onion encryption (like Tor) — but Safing operates some of the nodes.
- The company that sells "block mass surveillance" also operates infrastructure that *could* see your traffic.
- This mirrors the larger tech industry pattern: privacy tools built by companies that need to sustain themselves commercially.
- **Can a for-profit company be a trusted steward of anti-surveillance infrastructure?**

**The "free" question:**
- Portmaster is free. ShadowBroker is free. But "free" in what sense?
- Free as in beer (no cost) — but funded by what? Safing is a company; ShadowBroker is maintained by an individual.
- Free as in speech (GPL-3.0 / AGPL-3.0) — but what does copyleft *mean* for surveillance technology?
- If the code is free, anyone can audit it — but does that actually happen?
- **Podcast question:** *Is "free and open-source" enough to trust privacy tools with your deepest secrets? Or is trust something that can't be achieved through code alone?*

### 2F. Civil Liberties and the Law

**Key legal/regulatory touchpoints:**
- **GDPR (EU):** Portmaster is developed in the EU (Austria). GDPR's principles of data minimization, purpose limitation, and transparency are architecturally embedded in Portmaster's design. But SPN operates across borders — where does GDPR apply?
- **AGPL-3.0 (ShadowBroker):** The Affero GPL specifically closes the "SaaS loophole" — if you modify and run the software on a server, you must share your modifications. This is significant for a tool that could be deployed as a surveillance service.
- **Shodan's terms of service:** ShadowBroker proxies Shodan queries server-side with SSRF guards — but Shodan's own terms restrict how results can be used. Who enforces this?
- **Telegram's terms:** Scraping public `t.me/s` channels is technically against Telegram's ToS, even if the data is publicly visible.
- **CIA triad in reverse:** Usually we talk about Confidentiality, Integrity, Availability. Here: *Transparency of the watcher, Accountability of the watched, and the Availability of deniability.*

**Podcast question:** *Do existing legal frameworks (GDPR, CFAA, ToS) even apply when the tool is open-source and self-hosted? Or does borderless surveillance data create borderless legal problems?*

---

## 3. ETHICAL TENSIONS — The Gray Areas

| Tension | Portmaster | ShadowBroker |
|---------|-----------|--------------|
| **Protection vs. Visibility** | Blocks outbound surveillance | Enables outbound visibility |
| **Centralization vs. Decentralization** | SPN nodes operated by Safing (centralized) | InfoNet mesh (decentralized but experimental) |
| **Open-source vs. Trust** | Code is auditable; who audits? | Code is auditable; 1,788 forks — who's auditing those? |
| **Individual vs. State** | Empowers the individual | Empowers the individual — but to see what states do |
| **Transparency vs. Security** | "See all your connections" — transparency as security | "See all the world's connections" — transparency as intelligence |
| **Free as in speech vs. Free as in beer** | GPL-3.0 code; commercial SPN offering | AGPL-3.0 code; no commercial offering yet |
| **Anonymity vs. Accountability** | Hides your traffic from ISPs | Reveals others' traffic to you |
| **The Lock Paradox** | The tool that protects you has kernel access to everything | The tool that reveals everything has AI agents that can act autonomously |

---

## 4. PODCAST STORY ANGLES & NARRATIVE HOOKS

### Angle 1: "The Arms Race of Visibility"
Frame the episode as a cold war of surveillance: Portmaster builds walls; ShadowBroker builds windows. The question isn't "which side is right" — it's whether a world where everyone can see everyone is more just or more dangerous.

### Angle 2: "The Locksmith's Dilemma"
Portmaster needs kernel-level access to protect you. That same access could be used to surveil you. Safing says it doesn't — but how would you *know*? This is the locksmith paradox: can you trust the person who made the lock?

### Angle 3: "When AI Becomes the Watcher"
ShadowBroker's AI agents can autonomously surveil the globe. This isn't science fiction — it's a Docker container away from anyone. What happens when the first AI agent surveils the wrong person, or the wrong country? Who answers for it?

### Angle 4: "Public Data, Private Harm"
ShadowBroker's disclaimer — "the data is already public" — is technically true but morally incomplete. When you aggregate 22,000 CCTV feeds, track every private jet, and wire AI agents to recon the internet, "public data" becomes a panopticon. Is transparency always a good?

### Angle 5: "The Company That Fights Surveillance"
Safing is a *company* selling anti-surveillance software. Its SPN network runs through company-operated nodes. This is the innovation paradox: to fund anti-surveillance tools, you need revenue; to get revenue, you need infrastructure; that infrastructure can see everything. Can capitalism build genuine privacy?

### Angle 6: "The Listener's Choice"
End the episode with a direct challenge: Every listener can install Portmaster today (free) or spin up ShadowBroker (free). Both are open-source. Both are powerful. The question isn't "can you access these tools?" — it's "what do you do with them, and who do you become while using them?"

---

## 5. KEY QUOTES & REFERENCES

### From Portmaster's README
> *"Restore privacy and take back control over all your computer's network activity."*

> *"Everything is 100% local on your device. (except the SPN, naturally)"*

> *"Love Freedom — ❌ Block Mass Surveillance"*

### From ShadowBroker's README
> *"The project does not introduce new surveillance capabilities — it aggregates and visualizes existing public datasets."*

> *"Do not transmit anything sensitive on any channel. Treat all lanes as open and public for now."*

> *"The knowledge is available to all but rarely aggregated in the open, until now."*

> *"ShadowBroker has no accounts, product telemetry, or analytics."*

### Ethical Hex Points
- **The Panopticon Turn**: When everyone has the power to watch, the watch becomes mutual — and mutual watch is not the same as mutual trust.
- **The Aggregation Fallacy**: "It's just public data" confuses *individual* publicness with *synthesized* visibility. A mosaic of public dots is not "public" — it's *constructed*.
- **The Trust Default**: Open-source code is trust-minimized, not trust-eliminated. You still have to trust that (a) the code you're running matches the code on GitHub, (b) the maintainers haven't been co-opted, and (c) the infrastructure operators aren't adversaries.
- **The Asymmetry Insomnia**: If you can see everyone, everyone can see you. The question is whether that's a feature or a bug.

---

## 6. OPEN QUESTIONS FOR THE EPISODE

1. **Could Portmaster's SPN nodes be compelled by Austrian/EU law to log traffic?** What legal frameworks apply?
2. **Has anyone audited Portmaster's kernel driver for backdoors?** How does community review work for low-level systems code?
3. **What's the stopping condition for ShadowBroker?** If you can monitor everything, when do you stop monitoring?
4. **Can ShadowBroker's "Sovereign Shell" governance actually work?** Who votes? Who verifies? Who's excluded?
5. **What happens when an AI agent makes a surveillance error?** Is there a kill switch? Accountability? Redress?
6. **Is AGPL-3.0 the right license for surveillance technology?** Does copyleft create a "viral" privacy obligation — or just a viral surveillance tool?
7. **What does "privacy" mean when the tool itself becomes the surveillance target?** If Portmaster is blocked in a country, does that make it more or less trustworthy?

---

## 7. SOURCES & FURTHER READING

| Source | Link |
|--------|------|
| Portmaster repo | https://github.com/safing/portmaster |
| Portmaster fork (this repo) | https://github.com/bro26man-hash/portmaster |
| Portmaster website | https://safing.io |
| Portmaster wiki | https://wiki.safing.io |
| SPN whitepaper | https://safing.io/files/whitepaper/Gate17.pdf |
| ShadowBroker repo | https://github.com/BigBodyCobain/Shadowbroker |
| ShadowBroker threat model | docs/mesh/threat-model.md (in ShadowBroker repo) |
| ShadowBroker claims reconciliation | docs/mesh/claims-reconciliation.md (in ShadowBroker repo) |
| Privacy Guides (companion project) | https://github.com/privacyguides/privacyguides.org |
| Privacy Tools (companion project) | https://github.com/privacytools/privacytools.io |
| Google Differential Privacy | https://github.com/google/differential-privacy |

---

*Notes compiled from GitHub open-source research. Forked from safing/portmaster on 2026-09-17. All stars/forks/issue counts reflect the state at time of research.*
