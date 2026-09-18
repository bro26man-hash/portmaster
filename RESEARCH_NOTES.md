# 🎙️ Podcast Research Notes: Digital Rights & Surveillance Technology

**Project:** safing/portmaster — "Love Freedom — ❌ Block Mass Surveillance"
**Source:** https://github.com/safing/portmaster (13,738 stars, GPL-3.0, Go)
**Forked for:** Podcast episode research on digital rights, surveillance tech, and civil liberties

---

## 1. PROJECT OVERVIEW

**Portmaster** is a free and open-source application firewall for Windows and Linux that intercepts all network traffic at the raw packet level to give users visibility and control over their digital communications.

- **Core tech:** Integrates into the network stack via `nfqueue` on Linux and a kernel driver (WFP) on Windows. Every packet is seen and can be stopped.
- **Ownership tracking:** Uses eBPF and `/proc` on Linux; kernel driver + IP Helper API on Windows.
- **Per-app control:** Granular rules per application, including support for Snap, AppImage, scripts (Linux) and Windows Store apps / `svchost.exe` services (Windows).
- **Secure DNS:** Intercepts "astray" DNS queries, reroutes to itself, resolves via DoT/DoH resolvers. Includes split-horizon and horizon validation against rebinding attacks.
- **SPN (Safing Privacy Network):** A paid, onion-routing privacy network positioned "between VPN and Tor." Uses multi-hop encryption with routes chosen to maximize distance within the network, and exits chosen near destination servers for geo-unblocking.
- **Philosophy:** "100% local on your device" (except SPN). All intelligence data (block lists, geoip) downloaded and applied automatically. Updates are fully signed.

**Key tension:** The project's mission is anti-surveillance ("Block Mass Surveillance"), yet it operates within the proprietary Windows ecosystem and increasingly monetizes features that were once free.

---

## 2. SOCIETAL CONCERNS & ETHICAL TENSIONS

### A. The FOSS Identity Crisis — "Portmaster is NOT FOSS"

**Issue #2132** (closed as stale, 6 👍 reactions) ignited a fierce debate about whether a privacy tool that paywalls core features can still claim the "F" in FOSS.

**Key arguments from the community:**
- **GNOME dropped Portmaster** from its recommended software list, reclassifying it as proprietary rather than FOSS — a significant institutional judgment.
- **Paywalling features contradicts FOSS philosophy.** The Free Software Foundation's definition holds that users must have the freedom to run, study, share, and modify the software. When Network History, Bandwidth Visibility, VPN Compatibility Mode, VM Support, and Docker Support are locked behind a paywall, critics argue the "Free" in FOSS is hollow.
- **Kernel-level access raises the stakes.** A critic pointed out that Portmaster runs in kernel space on Windows and asked, "What happens when it BSODs someone?" They demonstrated an easy overflow attack that could crash a machine running Portmaster — raising the question of whether a security tool that can destabilize the system it's meant to protect should have such deep access.
- **"Simplewall" as a counter-example.** A lightweight, truly FOSS Windows firewall was cited as proof that paywalling isn't necessary for sustainability. However, Portmaster defenders noted that Simplewall doesn't support filter lists (malware, gambling, social-media blocking) and doesn't catch all `svchost.exe` connections — meaning it may miss surveillance vectors that Portmaster catches.

**🎙️ Podcast angle:** *If a privacy tool paywalls its own surveillance-blocking features, who gets protected? Does the "freedom" in FOSS matter more when the tool is fighting mass surveillance — or is sustainability more important than ideological purity?*

---

### B. The Microsoft WebView Dilemma — "Love Freedom, Hate Webview"

**Issue #2031** and **#2096** (14+ comments combined) reveal a deep contradiction: a privacy firewall that *requires* Microsoft WebView2 to function.

**The problem:**
- Portmaster V2 forces users to install Microsoft Edge WebView2 or the firewall won't run at all.
- WebView2 is a Microsoft proprietary component that auto-updates, carries telemetry, and is foundationed on Chromium — the same engine that powers Google Chrome.
- Users who have spent years "hardening" Windows (removing telemetry, disabling Microsoft services) are forced to re-introduce a Microsoft component they deliberately eliminated.
- One user called it: "It would be too ironic if the program that protects my privacy from Microsoft, forced me into installing ms-edge-webview."

**Defender's counterpoint:**
- A PowerUser argued that Windows is a "Registry-based system" with DCOM, and that asking Portmaster to avoid WebView2 is "like asking Windows to be Linux." They explained that WebView2 is a bare-minimum requirement for interacting with `svchost.exe` and networking services.
- In V1, Portmaster ran without WebView2. The V2 architecture made it mandatory — a trade-off between functionality and ideological consistency.

**Community workaround discovered:**
- A developer (stenya) shared a hack: enable "Development Mode" in `config.json`, start the Core service, and access the UI via `http://127.0.0.1:817/` in any browser — bypassing WebView2 entirely.
- But this is clearly a workaround, not a real solution.

**🎙️ Podcast angle:** *Can you fight surveillance on a platform that's inherently surveillant? The WebView2 dependency exposes a deeper question: when your anti-surveillance tool depends on the very ecosystem it's fighting, is it empowering users or creating a surrogate sense of Security?*

---

### C. The Monetization-Accessibility Tension

**Issue #2111** (closed as stale) asked: keep all non-SPN features free.

**The concern:**
- Beyond SPN, features like Network History, Bandwidth Visibility, Weekly Reports, VPN Compatibility Mode, VM Support, and Docker Support are paywalled.
- These are *local* features that don't require Safing's infrastructure. Putting them behind a paywall feels especially "petty" to critics.
- The suggestion: monetize SPN (which does use Safing's servers) freely, but keep the core surveillance-blocking functionality accessible to all.

**The broader pattern:**
- Multiple issues (#2132, #2031, #2096) were all closed as "stale" or "not planned" without explicit Safing team responses visible in the thread.
- The community feels the project has drifted from its open-source, freedom-first mission toward a freemium model that creates financial barriers to privacy.

**🎙️ Podcast angle:** *When privacy becomes a premium feature, does it stop being a right and start being a luxury? The digital divide of surveillance — if you can't pay for the pro version of your firewall, are you surveilled by default?*

---

### D. User Autonomy vs. "Protective" Paternalism

**Issue #1122** asked for an option to let the system/user manage DNS configuration instead of Portmaster intercepting it.

**The core tension:**
- Some users have complex DNS setups (DNSSEC, .loki, .snode resolution) that Portmaster breaks.
- They want Portmaster to "just obey" the system's DNS config — or better, to just sit and watch without interfering.
- The request: less "nosey" software that respects user autonomy over their own network stack.

**This mirrors a broader debate in privacy tech:**
- Should a privacy tool *override* user choices to "protect" them, or should it *defer* to user expertise?
- Portmaster's default "great out-of-the-box" experience means it intercepts DNS whether you want it to or not.
- The "100% local" claim is partially undermined by automatic block-list and geoip downloads — the tool makes sovereign decisions about what to block, based on Safing's intelligence data.

**🎙️ Podcast angle:** *Who decides what you're allowed to see? When a privacy tool automatically blocks certain domains and routes, is it protecting you — or imposing its own logic on your digital experience? The paternalism of "security by default."*

---

### E. The Kernel-Space Security Paradox

One of the most alarming criticisms (from #2132) deserves its own spotlight:

- Portmaster runs in **kernel space** on Windows (via WFP). This means it has the highest possible level of system access.
- A community member demonstrated an **easy buffer overflow attack** that could BSOD a machine running Portmaster — they explicitly said they wouldn't share the method publicly.
- The irony: a security tool running in kernel space that *within reach of privilege-escalation attacks* could crash the very system it's protecting.
- The question: should surveillance-blocking tools have kernel-level access, or should they operate in user space with more limited (but safer) capabilities?

**🎙️ Podcast angle:** *The surveillance paradox: to fight surveillance, you must see everything. But the deeper you look, the more damage you can do if you're compromised. Is kernel-level privacy software a trustworthy guardian — or a single point of catastrophic failure?*

---

## 3. KEY THEMES FOR THE PODCAST

### Theme 1: **"The Illogic of Fighting Surveillance with Surveillance Infrastructure"**
- Portmaster blocks trackers and surveillance — but requires Windows (a surveillant OS), Microsoft WebView2 (a Microsoft telemetry component), and automatic downloads of intelligence data from Safing's servers.
- Can you build a surveillant-free experience on a surveillant foundation?

### Theme 2: **"FOSS or Paywalled? The Digital Rights Credibility Crisis"**
- When a tool that claims to defend digital rights paywalls its own features, it undermines its moral authority.
- GNOME's delisting of Portmaster as FOSS is an institutional verdict that matters.
- The question isn't just "is it free?" but "who gets to decide what privacy costs?"

### Theme 3: **"The Privacy Divide — When Protection Becomes Privilege"**
- Network History, Bandwidth Visibility, VPN Compatibility — these are local features that cost nothing to provide but are locked behind a paywall.
- If the baseline of digital self-defense requires a subscription, privacy becomes a luxury good, not a universal right.

### Theme 4: **"Kernel Power, Kernel Risk"**
- Running in kernel space gives Portmaster god-eye view of all network traffic.
- It also gives it god-mode access to crash or potentially be exploitationized.
- The trade-off between visibility and vulnerability is real and under-discussed.

### Theme 5: **"The Paternalism of 'Great Defaults'"**
- Portmaster's philosophy is "privacy without effort" — but that means it makes decisions for you (DNS routing, block lists, what to intercept).
- The tension between "easy privacy for everyone" and "respect for advanced users who know their own DNS setup better than the software does."

### Theme 6: **"The Platform Trap"**
- You can't fight Microsoft's surveillance apparatus from inside it — or at least, it's complicated.
- WebView2 dependency, `svchost.exe` quirks, Windows Store app support — all evidence that anti-surveillance tools are constrained by the very platform they scrutinize.

---

## 4. NOTABLE COMMUNITY VOICES & RESOURCES

| Voice | Perspective | Where |
|-------|-------------|-------|
| **LCSOGthb** | FOSS purist; argues Portmaster is NOT FOSS; cites GNOME delisting; praises Simplewall | Issue #2132 |
| **fossFriend** | "Love Freedom, hate Webview"; demands CEF alternative to MS WebView2; production-environment security perspective | Issue #2031 |
| **8374954** | Privacy-hardcore Windows user; refuses to install WebView2; uses privacy.sexy to strip Microsoft telemetry | Issue #2096 |
| **Minoresa** | Balanced user of both Simplewall and Portmaster; acknowledges each has strengths | Issue #2132 |
| **doctorsangria** | Accepts SPN paywall but draws the line there; "The F in FOSS is supposed to stand for something" | Issue #2132 |
| **ouroborus** | Points out that local features like Network History shouldn't be paywalled | Issue #2132 |
| **HarriBuh** | "An open-sourced firewall must not rely on Big Tech companies who collect and sell user data" | Issue #2031 |
| **CommanderTurtle** | Windows architecture defender; argues WebView2 is unavoidable in the Windows ecosystem | Issue #2031 |
| **stenya (Safing dev)** | Shared the WebView2 bypass workaround (browser UI on port 817) | Issue #2096 |

**Related projects mentioned in discussions:**
- **Simplewall** — lightweight, truly FOSS Windows firewall (cited as superior in some respects)
- **privacy.sexy** — Windows hardening script (undergroundwires/privacy.sexy)
- **GNOME** — removed Portmaster from FOSS recommendations

---

## 5. OPEN QUESTIONS & DISCUSSION PROMPTS

1. **Is it ethical for a surveillance-blocking tool to run in kernel space?** What's the risk/reward calculus?
2. **Should "privacy" be monetized?** If the best firewall for Windows charges for local features, what does that mean for digital equality?
3. **Can you claim "Love Freedom" while depending on Microsoft's proprietary WebView2?** Is ideological consistency more important than practical functionality?
4. **Who decides what gets blocked?** When a privacy tool downloads automatic block lists, is it making sovereign decisions on your behalf?
5. **What happens when the tool becomes the vulnerability?** Kernel-level access is both the superpower and the Achilles' heel.
6. **Is FOSS a spectrum — or a binary?** Can a tool be "mostly open source" and still claim the FSF's definition of free?
7. **The "Great Default" paradox:** Does making privacy effortless actually empower users, or does it infantilize them and remove their agency?

---

## 6. QUICK REFERENCE — KEY ISSUES

| # | Title | Ethical Dimension | Status |
|---|-------|-------------------|--------|
| #2132 | "Portmaster is NOT FOSS" | FOSS integrity, paywall ethics, GNOME delisting | Closed (stale) |
| #2031 | "Love Freedom, hate Webview" | Microsoft dependency, vendor lock-in, CEF vs WebView2 | Closed (stale) |
| #2096 | "Microsoft WebView Is Not Privacy" | Telemetry, platform surveillance, WebView2 removal | Closed (completed) |
| #2111 | "Keep All Features Free Except SPN" | Access to privacy as universal right vs. freemium | Closed (stale) |
| #1122 | "Let the system/user manage DNS" | User autonomy vs. tool paternalism | Closed (completed) |
| #1141 | Performance degradation (70 comments) | Usability vs. security trade-offs | Open |
| #1932 | "V2 forces Microsoft WebView installation" | Forced dependencies, user choice | Closed (stale) |

---

*Notes compiled from GitHub issue analysis. Original discussions at safing/portmaster. Forked to bro26man-hash/portmaster for podcast reference.*