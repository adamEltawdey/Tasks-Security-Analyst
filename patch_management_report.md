# Patch Management: Closing the Most Common Attack Surface

## Introduction

Patch management is the process of identifying, acquiring, testing, and
deploying software updates that fix known security flaws, bugs, or
performance issues across an organization's systems and applications. NIST
defines enterprise patch management as the process of identifying,
prioritizing, acquiring, installing, and verifying the installation of
patches, updates, and upgrades throughout an organization.

Patching sits at the very end of the **vulnerability lifecycle** — the
sequence a security flaw travels through from the moment it's introduced
into software to the moment it's fixed everywhere it exists:

1. **Introduction** — A flaw is unintentionally written into software during development.
2. **Discovery** — A researcher, vendor, or attacker finds the flaw.
3. **Disclosure** — The flaw is reported and typically assigned a CVE (Common Vulnerabilities and Exposures) identifier, entering public vulnerability databases like NIST's National Vulnerability Database (NVD).
4. **Weaponization** — Proof-of-concept or working exploit code is developed, sometimes by researchers, sometimes by attackers.
5. **Patch availability** — The vendor releases a fix.
6. **Patch management (this report's focus)** — Organizations identify which of their systems are affected, test the fix, and deploy it.
7. **Residual exposure** — Every system that remains unpatched after a fix exists is exposed for as long as the gap persists — and this gap is where the overwhelming majority of real-world breaches occur.

Patch management is the organizational discipline that determines how long
step 7 lasts. Because attackers routinely reverse-engineer patches to build
exploits targeting the *unpatched* population, publishing a fix doesn't
close a vulnerability — deploying it does.

---

## Why Patches Matter

### How Vulnerabilities Are Discovered, Reported, and Exploited
Vulnerabilities are typically found by security researchers, bug bounty
participants, vendors' internal QA/security teams, or attackers themselves.
Once confirmed, most vulnerabilities are assigned a **CVE ID** (e.g.,
CVE-2017-0144) through the CVE Program, and rated for severity using the
**Common Vulnerability Scoring System (CVSS)**, with entries tracked in
NIST's National Vulnerability Database. This public disclosure is a
double-edged sword: it lets defenders know what to fix, but it also gives
attackers a detailed roadmap — they can often reverse-engineer a patch to
build a working exploit within days. Modern data shows this gap has
collapsed dramatically: the average time between a vulnerability's
disclosure and its first observed exploitation fell from roughly 63 days in
2018 to just around 5 days in recent analyses, with a significant share of
vulnerabilities now exploited on or before the day they're publicly
disclosed. Meanwhile, defenders' average time to remediate a critical
vulnerability still commonly exceeds 60 days — meaning the "patch gap" is
often the single biggest window of exposure an organization has.

### Real-World Breach #1: WannaCry / EternalBlue (2017)
In March 2017, Microsoft released patch **MS17-010**, fixing a critical
remote code execution flaw (CVE-2017-0144 and related CVEs) in how Windows
implemented the SMB (Server Message Block) file-sharing protocol. Weeks
later, a hacking group called the Shadow Brokers leaked an NSA-developed
exploit for this same flaw, known as **EternalBlue**. On May 12, 2017, the
**WannaCry** ransomware worm combined EternalBlue with self-propagating
code and spread to more than 200,000 computers across over 150 countries
within days — despite a working patch having existed for nearly two
months. The UK's National Health Service was among the hardest hit, with
thousands of appointments and operations cancelled and estimated damages
around £19 million; other major victims included Renault-Nissan, Honda, and
FedEx. The same EternalBlue exploit was reused weeks later in the even more
destructive NotPetya attack, which caused billions of dollars in damage
globally. WannaCry remains the textbook example of a patch that existed but
wasn't applied widely or quickly enough.

### Real-World Breach #2: Equifax (2017)
In March 2017, the Apache Software Foundation disclosed and patched a
critical remote code execution vulnerability in Apache Struts 2
(CVE-2017-5638, CVSS score 10.0 — the maximum possible severity), used in
Equifax's online dispute-portal web application. Equifax's own security
team was reportedly aware of the vulnerability at the time it was
disclosed, but the affected system was not patched. Attackers exploited the
flaw starting in mid-May 2017, and by the time the breach was discovered in
late July, the personal data of approximately 143–147 million people —
including Social Security numbers, birth dates, and addresses — had been
exposed, making it one of the largest data breaches in history. The
incident resulted in a $700 million settlement, an FTC investigation, and
significant executive turnover, all traceable back to a single patch that
had been publicly available for over two months before the breach began.

---

## Consequences of Not Patching

Unpatched systems are one of the most common initial access vectors
attackers use, and the consequences compound across several dimensions:

- **Data breaches** — As shown above, both WannaCry and Equifax stemmed directly from patches that existed but weren't deployed. Industry-wide, vulnerability exploitation is now estimated to drive around 20% of breaches, and this figure has been rising year over year.
- **Ransomware** — WannaCry alone infected hundreds of thousands of systems in days; more broadly, over half of ransomware incidents have been traced back to email and unpatched software vulnerabilities as the initial entry point.
- **Compliance violations and financial penalties** — Frameworks such as PCI DSS (Requirement 6.3), NIST SP 800-53 (control SI-2, "Flaw Remediation"), and CISA's Binding Operational Directive 22-01 (which mandates federal agencies remediate Known Exploited Vulnerabilities within 14 calendar days) explicitly require timely patching. Failing to meet these obligations can trigger regulatory fines, mandatory disclosures, and contractual penalties — Equifax's breach alone resulted in a $700 million settlement with U.S. regulators and affected consumers.
- **Scale of the ongoing problem** — Roughly 48,000+ new CVEs were published in a recent year alone (over 130 per day), and even for vulnerabilities added to CISA's Known Exploited Vulnerabilities catalog — meaning they are confirmed to be under active attack — around half remain unpatched 55 days after a fix becomes available. That gap between "patch exists" and "patch applied" is consistently where breaches occur.

---

## The Patch Management Lifecycle

Effective patch management, as outlined in frameworks like NIST SP 800-40
Revision 4, follows a repeatable cycle rather than a one-time project.

### 1. Discovery
Maintain a complete, current inventory of all hardware, operating systems,
applications, and firmware across the environment — including cloud, IoT,
and mobile assets. You cannot patch what you don't know you have. This
phase also includes continuously monitoring vulnerability feeds (NVD, CISA
KEV catalog, vendor advisories) to learn when a new patch or vulnerability
affects assets in that inventory.

### 2. Assessment
Evaluate each newly identified vulnerability against the organization's own
environment: is the affected software actually deployed and exposed? What
is the CVSS severity score? Is it listed in CISA's Known Exploited
Vulnerabilities (KEV) catalog, indicating active exploitation in the wild?
This phase produces a prioritized list — critical, internet-facing, or
actively exploited vulnerabilities should be scheduled far ahead of
low-severity, internal-only ones.

### 3. Testing
Before wide deployment, patches are applied to a representative
non-production or staging environment to confirm they don't break existing
functionality, introduce performance regressions, or conflict with other
software. This step is critical for reducing the operational risk that
often makes teams hesitant to patch quickly.

### 4. Deployment
Once validated, patches are rolled out to production systems, typically in
a phased manner (e.g., a small pilot group first, then broader waves) to
catch any unexpected issues before they affect the whole organization.
Deployment windows and rollback plans should be defined in advance,
especially for systems with strict uptime requirements.

### 5. Verification
After deployment, confirm the patch was actually applied successfully
across all intended systems — via vulnerability scanning, configuration
management tools, or endpoint agents — and that no systems were missed.
This closes the loop back to Discovery: verification data feeds directly
into the next cycle's inventory and assessment.

---

## Best Practices: A Prioritized 7-Step Patch Management Checklist

1. **Maintain a complete, continuously updated asset inventory** — including shadow IT, cloud instances, and IoT/OT devices — since unknown assets can never be patched.
2. **Subscribe to and monitor authoritative vulnerability sources** — the CVE database, NIST NVD, vendor security bulletins, and CISA's Known Exploited Vulnerabilities (KEV) catalog, which flags vulnerabilities under active attack.
3. **Risk-prioritize patches using CVSS severity, exploitability (e.g., EPSS score), and business context** — a critical, internet-facing, actively exploited vulnerability should be patched in days, not weeks.
4. **Set and enforce clear SLA timelines by severity** — a common benchmark is roughly 14–30 days for critical/high severity, 30–90 days for high/medium, and up to 120 days for low severity, with KEV-listed vulnerabilities remediated fastest of all.
5. **Test patches in a staging environment before production deployment** — to catch compatibility and stability issues without risking uptime.
6. **Deploy in phased waves with a documented rollback plan** — start with a low-risk pilot group, monitor for issues, then expand.
7. **Verify and report on patch compliance continuously** — use scanning or endpoint tools to confirm patches were actually applied, track exceptions with documented risk acceptance, and feed results back into the next Discovery cycle.

---

## Challenges: Why Organizations Struggle to Patch Promptly

| Challenge | Why It Happens | How to Overcome It |
|---|---|---|
| **Legacy systems** | Older operating systems or applications (e.g., unsupported Windows versions, end-of-life frameworks) may no longer receive vendor patches, or newer patches may be incompatible with legacy dependencies. | Maintain an inventory that flags end-of-life software; isolate legacy systems on segmented networks; use compensating controls (virtual patching/WAF rules) where a real patch isn't available; budget proactively for legacy system replacement. |
| **Downtime concerns** | Patching production systems, especially in manufacturing, healthcare, or finance, can require taking a service offline, which stakeholders resist due to revenue or safety impact. | Use phased/rolling deployments and redundant/failover systems so patching one node doesn't take the whole service down; negotiate defined maintenance windows in advance; prioritize based on exploitability so the highest-risk patches get the fastest exception to normal change windows. |
| **Extensive testing requirements** | Complex enterprise applications may take weeks or months to fully regression-test a patch, as seen in the Equifax case where remediating the Struts vulnerability required rewriting and redeploying application code. | Maintain realistic staging environments that mirror production; automate regression testing where possible; use risk-based triage so critical/actively-exploited vulnerabilities can bypass some testing depth in favor of speed, with compensating monitoring afterward. |
| **Resource and staffing constraints** | Many organizations, especially smaller ones, lack dedicated patch management staff, so patching competes with other IT priorities. | Automate patch deployment tools (e.g., WSUS, endpoint management platforms) to reduce manual effort; adopt risk-based prioritization so limited staff hours go to the highest-impact patches first. |
| **Fragmented visibility across environments** | Cloud, on-prem, IoT, and mobile assets are often managed by different teams with different tools, making it hard to get a single view of what's patched and what isn't. | Consolidate vulnerability and patch data into a single dashboard/CMDB; assign clear ownership for each asset class; require regular cross-team reporting on patch compliance status. |

---

## References

1. National Institute of Standards and Technology (NIST). *SP 800-40 Revision 4: Guide to Enterprise Patch Management Planning: Preventive Maintenance for Technology*. Defines the enterprise patch management process and lifecycle. https://csrc.nist.gov/pubs/sp/800/40/r4/final
2. Cybersecurity and Infrastructure Security Agency (CISA). *Known Exploited Vulnerabilities (KEV) Catalog and Binding Operational Directive 22-01*. Source for federal remediation timelines and actively-exploited vulnerability tracking. https://www.cisa.gov/known-exploited-vulnerabilities-catalog
3. MITRE / NIST National Vulnerability Database (NVD). CVE Program records, including CVE-2017-0144 (EternalBlue/WannaCry) and CVE-2017-5638 (Apache Struts/Equifax). https://nvd.nist.gov/
4. Equifax Inc. *Equifax Releases Details on Cybersecurity Incident*. Company disclosure confirming the Apache Struts CVE-2017-5638 root cause. https://investor.equifax.com/news-events/press-releases/detail/237/equifax-releases-details-on-cybersecurity-incident
5. Akamai. *What Is WannaCry Ransomware*. Background on the EternalBlue exploit and the 2017 WannaCry outbreak. https://www.akamai.com/glossary/what-is-wannacry-ransomware
6. The Apache Software Foundation. *Media Alert: The Apache Software Foundation Confirms Equifax Data Breach Due to Failure to Install Patches*. https://news.apache.org/foundation/entry/media-alert-the-apache-software
