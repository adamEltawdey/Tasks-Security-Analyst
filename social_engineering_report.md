# Research Report: Social Engineering Attacks

## Introduction

Social engineering is the practice of manipulating people — rather than
software or hardware — into taking an action that compromises security:
handing over credentials, wiring money, plugging in an infected device, or
granting physical or system access. It works because it targets something
organizations can't patch: human trust, urgency, curiosity, and the desire
to be helpful. This makes it consistently one of the most effective attack
vectors available, often cheaper and more reliable than finding and
exploiting a software vulnerability.

The scale is significant. Human element factors are present in around 60%
of data breaches, and in Verizon's 2025 Data Breach Investigations Report,
social engineering accounted for roughly 4,000 recorded incidents, with the
large majority resulting in confirmed data disclosure. Within that category,
phishing remains the dominant technique, making up 57% of social engineering
incidents according to the same report, with pretexting a distant but
significant second. Business email compromise (BEC) — often built on
pretexting or spear phishing — is disproportionately costly, with average
losses per incident reported in the millions of dollars, and the FBI's
Internet Crime Complaint Center recorded billions of dollars in BEC losses
in a single year. These figures illustrate why security awareness training
and process controls, not just technical defenses, are essential parts of
any organization's security posture.

---

## 1. Phishing

### How It Works
Phishing is the use of deceptive communications — typically email, but also
text messages, voice calls, or social media — to trick a target into
revealing credentials, clicking a malicious link, opening an infected
attachment, or transferring money. Attackers impersonate a trusted sender
(a colleague, bank, vendor, or executive) and create a pretext that pressures
the target to act quickly without stopping to verify.

**Types of phishing:**

- **Spear phishing** — Highly targeted phishing aimed at a specific
  individual or small group, using personal or organizational details
  (name, role, recent projects, colleagues) to appear credible.
- **Whaling** — A form of spear phishing that specifically targets senior
  executives or other high-value individuals (the "big fish"), often to
  authorize large wire transfers or access sensitive data.
- **Vishing (voice phishing)** — Phishing conducted over a phone call,
  frequently impersonating IT support, a bank, or a government agency to
  extract credentials or one-time passcodes verbally.
- **Smishing (SMS phishing)** — Phishing delivered via text message,
  commonly impersonating delivery notifications, banks, or toll-payment
  services, and containing a malicious link.

### Real-World Case Study: The Google/Facebook $100M+ BEC Scheme
Between 2013 and 2015, Lithuanian national Evaldas Rimasauskas orchestrated
a whaling/BEC scheme against Google and Facebook. He incorporated a shell
company in Latvia using a name nearly identical to Quanta Computer, a real
Taiwan-based hardware supplier both companies did business with. He then
sent phishing emails containing forged invoices, contracts, and corporate
stamps to employees responsible for approving vendor payments, convincing
them the wire requests were legitimate. Over roughly two years, the scheme
successfully diverted more than $100 million — around $23 million from
Google and $99 million from Facebook — before it was uncovered. Rimasauskas
was extradited to the U.S., pleaded guilty to wire fraud, and was sentenced
to five years in prison along with orders to forfeit and repay tens of
millions of dollars. Facebook stated it recovered the bulk of the stolen
funds; Google's recovery has not been fully disclosed.

### Impact
- Direct financial loss exceeding $100 million across two companies before detection.
- Demonstrated that even sophisticated tech companies with mature finance processes are vulnerable when a request looks routine.
- Contributed to a broader wave of BEC-driven losses; the FBI has reported BEC losses growing sharply since incidents like this one came to light.

### Prevention Recommendations
1. **Verify payment changes through a separate, trusted channel** — Any request to change bank details or approve a large wire transfer should be confirmed by phone using a previously known number, not one provided in the email itself.
2. **Deploy email authentication protocols (SPF, DKIM, DMARC)** — Reduces the ability of attackers to spoof or closely mimic a trusted domain in the "From" field.
3. **Run regular phishing simulation and awareness training** — Employees who have practiced spotting invoice fraud, urgency cues, and lookalike domains are measurably less likely to comply with a fraudulent request.
4. **Enforce dual-approval controls on high-value transactions** — Require a second, independent authorizer for wire transfers above a defined threshold, so no single deceived employee can complete the transaction alone.

---

## 2. Pretexting

### Definition
Pretexting is a social engineering technique in which an attacker invents a
fabricated scenario (a "pretext") — often impersonating a trusted role such
as IT support, a vendor, an auditor, or a coworker — to build enough
credibility to convince a target to hand over information or access. Unlike
generic phishing, pretexting usually involves sustained interaction:
research on the target and organization, a plausible cover story, and
often direct conversation (in person, by phone, or via chat) rather than a
single message.

**How an attacker builds a false scenario:**
1. **Reconnaissance** — Gathering names, titles, internal tools, vendor
   relationships, and organizational structure from public sources (LinkedIn,
   press releases, leaked data) to make the pretext specific and believable.
2. **Identity construction** — Choosing a role the target is inclined to
   trust or comply with quickly, such as an internal help-desk technician,
   a new hire, or a senior executive.
3. **Escalating legitimacy** — Using internal jargon, real employee names,
   or previously obtained low-level access to make each subsequent request
   look like a natural continuation of a legitimate process.
4. **Creating urgency or authority** — Framing the request as time-sensitive
   or coming from someone the target doesn't want to question, discouraging
   the target from pausing to verify.

### Real-World Case Study: The 2020 Twitter Employee Hack
In July 2020, attackers used a phone-based pretexting/vishing campaign
against a small number of Twitter employees, posing as internal IT staff
addressing a work-from-home VPN or system issue. Twitter's own investigation
described it as "a significant and concerted attempt to mislead certain
employees and exploit human vulnerabilities to gain access to internal
systems." The attackers first tricked lower-privilege employees into
handing over credentials, then used the internal knowledge gained from that
access to identify and target additional employees who did have permission
to use Twitter's internal account-support tools. With that access, the
attackers took over more than 130 high-profile accounts — including those
belonging to major public figures and companies — and used them to promote
a Bitcoin scam. Three individuals were later criminally charged in
connection with the incident.

### Impact
- Compromise of over 130 high-profile Twitter accounts, several used to solicit a cryptocurrency scam.
- Significant reputational damage to Twitter and erosion of public trust in the platform's internal controls ahead of a U.S. election year.
- Exposed how internal administrative tools, if broadly accessible, can turn a single successful pretext into a much larger breach through lateral movement.

### Prevention Measures
1. **Establish strict identity verification procedures for IT/help-desk requests** — Especially for password resets or access changes, require verification through a secondary channel or pre-established callback number, never information provided by the caller.
2. **Apply least-privilege access to internal tools** — Limit which employees can access sensitive account-management or administrative tools, so that compromising one lower-level employee doesn't provide a path to a highly privileged one.
3. **Train employees to recognize and question urgency and authority cues** — Awareness training should specifically cover phone-based pretexting, not just email phishing, since attackers increasingly favor voice channels to bypass email filters.

---

## 3. Baiting

### Definition
Baiting lures a target with the promise of something appealing — a free
item, useful file, or convenient device — in order to get them to take an
action that compromises security. It can be physical (an infected USB
drive) or digital (a fake "free download" or pirated software bundled with
malware).

- **Physical baiting** — Leaving infected USB drives, CDs, or other media in
  places where targets (employees, visitors) are likely to find them, such
  as a parking lot, lobby, or break room, relying on curiosity or a sense of
  obligation to "return" the device to its owner.
- **Digital baiting** — Offering free software, cracked games, movie
  downloads, or "too good to be true" online deals that bundle malware with
  the promised file.

### Real-World Case Study: USB Drop Attacks and Stuxnet
Physical USB baiting has been used in some of the most consequential
cyberattacks on record. The Stuxnet worm, discovered in 2010, is widely
believed to have initially reached its target — Iranian nuclear enrichment
facilities on an air-gapped network — via an infected USB drive, since the
network wasn't otherwise reachable from the internet. Separately, in 2008
a U.S. Department of Defense facility in the Middle East suffered what has
been called the worst breach in DoD history at the time, after an infected
USB drive left in a parking lot was plugged into a military laptop; the
resulting worm spread across both classified and unclassified networks and
took the Pentagon roughly 14 months to fully remove. The effectiveness of
this technique isn't just historical: a controlled academic study by
researchers from Google and the University of Illinois that dropped nearly
300 USB drives across a university campus found that a large share were
picked up, and roughly half were both plugged in and had files opened,
most often because the finder wanted to identify the drive's owner rather
than out of malicious curiosity.

### Impact
- Stuxnet caused physical damage to centrifuges at an Iranian nuclear facility, demonstrating that baiting can bridge the gap into supposedly isolated ("air-gapped") networks.
- The 2008 DoD incident required a 14-month, dedicated remediation effort and directly contributed to the creation of U.S. Cyber Command.
- Academic research shows the technique remains highly effective against the general population even without any technical sophistication in the drive itself.

### Prevention Measures
1. **Disable or restrict removable media via endpoint policy** — Use device-control software to block unauthorized USB drives from executing files automatically, or block them entirely on sensitive systems.
2. **Physically secure air-gapped and critical systems** — Treat USB ports on critical infrastructure as an attack surface; restrict physical access and log/monitor any removable media use.
3. **Train employees never to plug in unknown media** — Establish a clear policy and reporting process for found devices (hand it to IT/security, never plug it in "just to check"), reinforced through the same awareness programs used for phishing.

---

## 4. Quid Pro Quo (Bonus)

### Explanation
Quid pro quo ("something for something") is a social engineering technique
in which the attacker offers a service or benefit in exchange for
information or access. It's closely related to baiting but is transactional
and interactive rather than a passive lure: a classic example is an
attacker cold-calling an office claiming to be from IT support offering free
troubleshooting help, then asking the employee to disable antivirus
software or provide their password "to complete the fix." Other variants
include fake IT surveys offering a small reward (a gift card) in exchange
for a password, or fraudulent "free" security audits/consulting offers used
to gain access to systems.

### Prevention
- **Verify unsolicited offers of help or services through official channels** before granting any access, disabling any protection, or sharing credentials — a genuine IT department will already have a ticket or record of the issue.
- **Train staff that legitimate IT/support staff never ask for passwords outright**, and that any request to disable security controls should be escalated rather than actioned directly.
- **Log and restrict who can request changes to system configurations or security software**, so a single employee's compliance with a quid pro quo request can't unilaterally weaken defenses.

---

## 5. Comparison Table

| Attack Type | Primary Target | Psychological Lever Exploited | Best Countermeasure |
|---|---|---|---|
| **Phishing** (incl. spear phishing, whaling, vishing, smishing) | Individual employees, especially finance/executive staff for whaling | Urgency, authority, trust in a familiar sender/brand | Out-of-band verification + email authentication (SPF/DKIM/DMARC) + awareness training |
| **Pretexting** | Help-desk/IT staff, or any employee who can grant access on request | Trust in an assumed role or authority figure; desire to be helpful and compliant | Strict identity verification procedures + least-privilege access |
| **Baiting** | Any employee or visitor with physical or online access to curiosity-driven media | Curiosity and the desire to "do the right thing" (return a lost item) or get something free | Endpoint device-control policies + employee reporting culture for found devices |
| **Quid Pro Quo** | Employees who can be persuaded by an offered benefit or "help" | Reciprocity — the instinct to repay a favor or accept a fair-seeming trade | Verification of unsolicited offers through official channels + no-password policy enforcement |

---

## 6. Organizational Recommendations: 5-Point Employee Security Awareness Training Checklist

1. **Run realistic, recurring phishing simulations** — Not a one-time exercise; track click/report rates over time and target additional training at repeat offenders and high-risk roles (finance, executive assistants, IT help desk).
2. **Teach out-of-band verification as a default habit** — Employees should know to verify any unusual request — a payment change, a password reset, a "confidential" USB drive — through a separate, previously known channel before acting.
3. **Cover all channels, not just email** — Include vishing (phone), smishing (text), in-person pretexting, and physical baiting scenarios in training, since attackers increasingly move to channels with fewer automated defenses.
4. **Establish a clear, blame-free reporting process** — Employees should know exactly how to report a suspicious email, call, or found device, and be encouraged to report near-misses or mistakes without fear of punishment, since fast reporting limits damage.
5. **Reinforce least-privilege and dual-approval policies alongside training** — Awareness training reduces the chance an employee falls for an attack, but process controls (limited access, second-approver requirements for payments) limit the damage when someone inevitably does.

---

## References

1. Verizon. *2025 Data Breach Investigations Report (DBIR)*. Statistics on social engineering, phishing, and pretexting incident volumes. https://www.verizon.com/business/resources/reports/dbir/
2. U.S. Department of Justice / SecurityWeek. "Lithuanian Man Sentenced to Prison Over BEC Scheme Targeting Facebook, Google." Case details on the Evaldas Rimasauskas $100M+ BEC/whaling scheme. https://www.securityweek.com/lithuanian-man-sentenced-prison-over-bec-scheme-targeting-facebook-google/
3. Twitter (via ESET WeLiveSecurity and Security Magazine). "Twitter breach: Staff tricked by 'phone spear phishing.'" Official incident details on the 2020 Twitter employee pretexting/vishing attack. https://www.welivesecurity.com/2020/07/31/twitter-breach-staff-tricked-phone-spear-phishing/
4. G DATA Software. "Malicious USB devices: Still a security problem." Summary of the Google/University of Illinois USB drop study and the 2020 Tesla USB baiting attempt. https://www.gdatasoftware.com/blog/2021/11/usb-drives-still-a-danger
5. We Are The Mighty. "The worst cyber attack in DoD history came from a USB drive found in a parking lot." Details on the 2008 U.S. military "Buckshot Yankee" USB baiting incident. https://www.wearethemighty.com/mighty-history/worst-cyber-attack-usb/
6. Secureframe. "110+ of the Latest Data Breach Statistics to Know for 2026 & Beyond," citing Verizon 2025 DBIR and IBM 2025 Cost of a Data Breach Report figures on phishing and social engineering prevalence and cost. https://secureframe.com/blog/data-breach-statistics
