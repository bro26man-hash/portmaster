# 🎙️ Podcast Research Notes: Digital Rights & Surveillance Technology

> **Primary Project:** [safing/portmaster](https://github.com/safing/portmaster) — "Love Freedom — Block Mass Surveillance"
> - 13,739 stars | Go | GPL-3.0 | Application firewall for privacy
> - Forked to: `bro26man-hash/portmaster`
>
> **Secondary Project:** [Shawn-Shan/fawkes](https://github.com/Shawn-Shan/fawkes) — Privacy preservation against facial recognition
> - 5,607 stars | Python | BSD-3-Clause | Academic research project (SANDLab, Univ. of Chicago)

---

## 1. Executive Summary

This document synthesizes research from two of the most notable open-source projects in the surveillance/privacy-tech space, along with community discussions touching on ethics, civil liberties, and the fundamental tensions in building and using counter-surveillance tools. It is intended as a starting point for podcast episode planning.

---

## 2. Project Profiles

### 2a. Portmaster (safing/portmaster)

**What it is:** A free, open-source application firewall for Windows and Linux that intercepts every network packet at the raw level (via `nfqueue` on Linux, WFP kernel driver on Windows). It gives users granular, per-app control over network activity — blocking trackers, malware, and any connection the user chooses.

**Key features relevant to the podcast:**
- **Per-app firewalling:** Every process on the computer can be monitored and controlled.
- **Secure DNS (DoT/DoH):** Prevents ISP-level DNS surveillance.
- **SPN (Safing Privacy Network):** A paid, centralized "privacy network" that uses onion routing (like Tor) but with exit nodes near destinations for geo-unblocking.
- **Network history & bandwidth monitoring:** Local-only recording of all connections.
- **GPL-3.0 license:** The core software is fully open-source and copylefted.

**What makes it newsworthy:**
- Explicitly branded as an anti-surveillance tool ("❌ Block Mass Surveillance").
- Developed in the EU (Austria), subject to GDPR and European digital rights frameworks.
- 13,739 GitHub stars, 578 forks — significant community adoption.
- The SPN introduces a **commercial, centralized layer** to an otherwise decentralized/open-source tool — a tension worth exploring.

### 2b. Fawkes (Shawn-Shan/fawkes)

**What it is:** A privacy protection system developed by SANDLab at the University of Chicago that applies subtle, invisible perturbations to facial images, making them unrecognizable to facial recognition models while still appearing normal to human eyes. Published at *USENIX Security 2020*.

**Key features relevant to the podcast:**
- **Data poisoning approach:** Fawkes doesn't encrypt or hide — it *changes* your face data just enough to confuse ML models.
- **Three perturbation modes:** Low, mid, high — trading off between image quality and protection strength.
- **Academic paper:** [Fawkes: Protecting Personal Privacy against Unauthorized Deep Learning Models](https://www.shawnshan.com/files/publication/fawkes.pdf)

**What makes it newsworthy:**
- Represents the "counter-surveillance" end of the spectrum — not just hiding from surveillance, but actively *poisoning* the surveillance models.
- Research-grade tool with real academic rigor, but community usage has revealed fundamental limitations.

---

## 3. Societal Concerns & Ethical Tensions

### 3a. The Effectiveness Paradox

**Core tension:** Counter-surveillance tools can only work if the surveillance systems they target are imperfect. Once those systems adapt, the tools become obsolete — and users may develop a false sense of security.

**Evidence from Fawkes community issues:**
- **Issue #138** ("No effect on AWS Rekognition?"): A user tested Fawkes-cloaked images against AWS Rekognition and found 100% similarity matching. Even at `--mode=high`, the perturbations were visible and AWS still identified the same person.
- **Issue #138 comment by tbeckenhauer:** Tested two different cloaked images of the same person against facial recognition — got 99.9%, 99.5%, and 99.3% similarity for low/mid/high modes. Concluded: *"I imagine these facial recognition tools saw all the publicity for Fawkes and started training their networks to recognize cloaked images."
- **Issue #138 comment by ghost (Oct 2023):** Linked to a [Register article](https://www.theregister.com/2022/03/15/research_finds_data_poisoning_cant/) concluding that **large players have already trained their models to resist data poisoning** — essentially declaring Fawkes effectively defeated at scale.
- **Issue #192** ("Does this still work today?"): A user asked whether Fawkes still works against modern models — no one has confirmed it still does.

**Podcast angle:** *"The cat-and-mouse game between privacy tools and surveillance AI is asymmetric. Companies have near-infinite data and compute; individual users have a checkbox and a thesis. What does 'winning' even look like?"

### 3b. The Centralization Tension

**Core tension:** Tools that claim to protect individual privacy can simultaneously introduce new centralized points of control.

**Evidence from Portmaster:**
- **SPN (Safing Privacy Network):** While Portmaster's core is GPL-3.0 and fully local, the SPN is a **paid, proprietary, centralized service** run by the same company that built the firewall. Users route traffic through Safing-operated nodes.
- The SPN whitepaper describes onion encryption over multiple hops (Tor-like), but **exit nodes are chosen near destination servers** — a design choice that prioritizes speed and geo-unblocking over maximum anonymity.
- **Community concern:** The open-source community (13,739 stars, 578 forks) has at times debated whether a commercial privacy service built on top of an open-source anti-surveillance tool creates a trust problem. What data does Safing collect about SPN users? Can they be compelled to disclose it?

**Podcast angle:** *"Can a company that sells you privacy be trusted with your privacy? The Portmaster SPN asks us to reconsider what 'open-source privacy' means when the most convenient option is a paid, closed service."

### 3c. The False Security Problem

**Core tension:** Privacy tools may give users a false sense of protection, leading them to behave more recklessly online.

**Evidence:**
- Fawkes' own developer (Shawn Shan) acknowledged in **Issue #95** that all cloaked images of the same person are still recognized as the same person by facial recognition systems: *"It would be great to make all images to appear as different person. But it is a much harder task and we can't support that currently."
- This means a Fawkes user might believe their face is fully protected, when in reality it's only protected against *some* models under *specific* conditions.
- Portmaster's "great defaults" philosophy (as stated in its README) — *"With great defaults your privacy improves without any effort"* — could similarly lull users into believing they're fully protected when they're only partially shielded.

**Podcast angle:** *"Is the privacy app on your phone a shield or a comfort blanket? The psychology of 'feeling safe' versus 'being safe' in the digital age."

### 3d. The Arms Race Asymmetry

**Core tension:** Surveillance technology is funded by nation-states and corporations; counter-surveillance tools are typically built by academics and volunteers.

**Evidence:**
- Fawkes is a university research project (SANDLab, University of Chicago) with academic funding. Its maintenance has lagged — Issue #192 (April 2026) asks if it "still works today" with no authoritative answer from the team.
- Portmaster is built by a small Austrian company (Safing). While it has persisted and evolved, its SPN commercial model raises questions about sustainability vs. mission drift.
- Surveillance systems (facial recognition, mass DNS monitoring, social media scraping) are deployed by entities with vastly more resources.

**Podcast angle:** *"David vs. Goliath, but Goliath has a budget 1,000x bigger. What does it mean for the future of digital rights when privacy is technologically outmatched but culturally ahead?"

### 3e. The "Who Watches the Watchmen" Problem

**Core tension:** Any tool that can block surveillance can also be used to enhance surveillance. The same network-interception technology that protects you can be repurposed.

**Evidence:**
- Portmaster uses `nfqueue` (Linux) and WFP (Windows) — kernel-level packet interception. This is the same class of technology used by:
  - Corporate DLP (Data Loss Prevention) systems
  - Government deep packet inspection (DPI)
  - Censorship firewalls (e.g., Great Firewall of China)
- The open-source nature of Portmaster (GPL-3.0) means the code is available for anyone to modify — including for surveillance purposes.
- **No open issues directly address this**, but it's a glaring absence in the project's ethical discourse.

**Podcast angle:** *"The technology that protects your privacy is the same technology that protects theirs. Who gets to decide which side it's on?"

### 3f. Bias & Misidentification in Facial Recognition

**Core tension:** Facial recognition systems are known to have higher error rates for people of color, women, and non-binary individuals. Counter-surveillance tools that claim to protect "everyone" may not account for these disparities.

**Evidence:**
- Fawkes' testing methodology (per Issue #67 and #125) relies on the **WebFace dataset** (10,000+ labels) — a standardized academic benchmark. But real-world deployment involves biased, unevenly trained models.
- The developer himself cautioned against testing with celebrity images (**Issue #67**): *"Please do not use celebrity pictures for test. Because most of the facial recognition models have already trained on celebrities."
- This implies Fawkes' effectiveness may vary significantly across different demographics — a concern that has **never been formally studied or published**.

**Podcast angle:** *"If you're a Black woman, does Fawkes protect you? If facial recognition misidentifies you 35x more often, does 'protection' mean something different for you?"

---

## 4. Key GitHub Discussions & Issues Reference

| Issue | Project | Topic | Podcast Relevance |
|-------|---------|-------|-------------------|
| [#138](https://github.com/Shawn-Shan/fawkes/issues/138) | Fawkes | Fawkes doesn't work against AWS Rekognition | ⭐⭐⭐ Central to effectiveness debate |
| [#192](https://github.com/Shawn-Shan/fawkes/issues/192) | Fawkes | "Does this still work today?" | ⭐⭐⭐ Questions about current relevance |
| [#95](https://github.com/Shawn-Shan/fawkes/issues/95) | Fawkes | All cloaked images show as same person | ⭐⭐⭐ Fundamental limitation |
| [#67](https://github.com/Shawn-Shan/fawkes/issues/67) | Fawkes | Doesn't protect against Face++ | ⭐⭐ Real-world model variation |
| [#125](https://github.com/Shawn-Shan/fawkes/issues/125) | Fawkes | AWS Rekognition high similarity | ⭐⭐ Reinforces #138 findings |
| [#152](https://github.com/Shawn-Shan/fawkes/issues/152) | Fawkes | Non-adversarial training versions don't work | ⭐⭐ Technical gap |
| Portmaster issues | Portmaster | Mostly technical bugs/features | ⭐ SPN commercial tension not discussed in issues |

**Notable absence:** Neither project has dedicated issues or discussions tagged with "ethics," "civil liberties," or "surveillance" — suggesting these conversations happen in external spaces (academic papers, news articles, social media) rather than within the projects themselves.

---

## 5. Suggested Podcast Episode Angles

### Angle A: "The Illusion of Control"
Explore how privacy tools create a *feeling* of agency without delivering *actual* protection. Use Fawkes' limitations and Portmaster's SPN as case studies.

### Angle B: "Open Source, Closed Reality"
Investigate the gap between what open-source privacy tools *promise* (transparency, community control) and what they *deliver* (often commercialized, centralized, or effectively defeated).

### Angle C: "The Datenschutz Dilemma" (The Data Protection Dilemma)
Focus on the EU-specific angle: Portmaster is built in Austria under GDPR. How does European regulatory culture shape what "privacy" means — and does it create a false sense of superiority?

### Angle D: "Arms Race Economics"
Compare the resource asymmetry between surveillance deployers (governments, corporations) and counter-surveillance builders (academics, FOSS communities). What funding models could level the playing field?

### Angle E: "Who Is Privacy For?"
Examine whether privacy tools are designed with the needs of privileged users (celebrities, tech-literate Westerners) in mind, and whether they fail marginalized communities who face the most surveillance.

---

## 6. Additional Resources for Research

- **Fawkes academic paper:** https://www.shawnshan.com/files/publication/fawkes.pdf
- **Portmaster SPN Whitepaper:** https://safing.io/files/whitepaper/Gate17.pdf
- **The Register article on data poisoning defeat:** https://www.theregister.com/2022/03/15/research_finds_data_poisoning_cant/
- **Portmaster website:** https://safing.io
- **Fawkes project page:** https://sandlab.cs.uchicago.edu/fawkes/
- **PrivacyGuides (companion project):** https://github.com/privacyguides/privacyguides.org

---

## 7. Open Questions for Guests/Interviewees

1. Should open-source privacy tools be required to publish formal effectiveness assessments? Should there be an independent "Privacy Tool Audit" analogous to security audits?
2. Is it ethical for a company to monetize a privacy network (SPN) built on top of an anti-surveillance firewall? Where's the line between sustainability and mission drift?
3. If counter-surveillance tools are effectively defeated at scale, should researchers be more transparent about this — or would that discourage adoption and make things worse?
4. How should facial recognition regulation account for the existence of tools like Fawkes? Should using Fawkes be legally protected as a form of expression?
5. Can "privacy by design" coexist with "surveillance by design" in the same operating system? What would a truly neutral network stack look like?

---

*Created for podcast research purposes. All GitHub issues referenced are publicly available at the URLs above.*
