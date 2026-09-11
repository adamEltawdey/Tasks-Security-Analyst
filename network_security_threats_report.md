# Research Report: Common Network Security Threats

## Introduction

Modern organizations run almost every part of their business over networks —
payments, communications, customer data, and internal operations all pass
through infrastructure that is, by design, reachable from the outside world.
That same connectivity is what attackers exploit. Network-level threats are
attractive to attackers precisely because they don't require compromising an
application's logic or tricking a specific user; they target the plumbing
that everything else depends on — routing, name resolution, and the sheer
availability of a service. A single successful network attack can take down
a business for hours (denial of service), silently steal credentials and
financial data (interception attacks), or quietly redirect millions of users
to a malicious destination (spoofing and DNS attacks) — all without ever
touching an application's source code. Understanding how these attacks work
is the first step toward building defenses that address the network layer
itself, not just the applications running on top of it.

---

## 1. Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS) Attacks

### How It Works
A DoS attack aims to make a service unavailable to legitimate users by
overwhelming it — with traffic, connection requests, or resource-exhausting
operations — until it can no longer respond. A DDoS attack is the same idea
carried out from many sources simultaneously, usually a "botnet" of
compromised devices, which makes the traffic harder to block by simply
denying one IP address. Attackers may flood a target directly, or use
**amplification/reflection**: sending a small, spoofed request to a
third-party server (like an open DNS resolver) so that the much larger
response is sent to the victim instead of the attacker.

### Real-World Example
On October 21, 2016, the **Mirai botnet** was used to launch a massive DDoS
attack against **Dyn**, a major DNS provider. The flood of traffic — reportedly reaching around 1 terabit per second — overwhelmed Dyn's DNS infrastructure, which is responsible for translating domain names into IP addresses, leaving users unable to reach affected sites for extended periods. Over fifty major platforms were disrupted, including PayPal, Twitter, Reddit, Amazon, Netflix, Spotify, and Sony. The botnet behind the attack was built by scanning the internet for IoT devices — routers, DVRs, security cameras — still using factory-default passwords, and enrolling them as remotely controlled bots. (Source: Dyn/NJCCIC/CoverLink incident reporting on the October 2016 Mirai-Dyn attack.)

### Impact
- Loss of revenue and business interruption for every company relying on Dyn's DNS service, not just Dyn itself.
- Reputational damage and loss of user trust.
- Demonstrated that IoT devices with weak default credentials could be weaponized at internet scale.

### Mitigation Strategies
1. **Rate limiting and traffic filtering** — Configure network edge devices and CDNs to detect and throttle abnormal traffic spikes before they reach core infrastructure.
2. **DDoS protection / scrubbing services and anycast routing** — Use a provider (e.g., Cloudflare, Akamai) that can absorb and distribute attack traffic across many data centers rather than letting it hit a single point of failure.
3. **Redundant DNS and infrastructure diversity** — Use multiple, geographically distributed DNS providers so that an attack on one does not take your entire service offline.

---

## 2. Man-in-the-Middle (MITM) Attacks

### How It Works
In a MITM attack, an attacker secretly positions themselves between two
communicating parties (e.g., a user and a website) and intercepts, reads, or
alters the traffic without either party realizing it. Common techniques
include ARP spoofing on a local network, rogue Wi-Fi access points, and SSL/
TLS interception, where the attacker presents a forged certificate so the
victim's browser believes it's talking directly to the legitimate server.

### Real-World Example
In February 2015, it was discovered that **Lenovo** had been shipping
consumer laptops preloaded with adware called **Superfish**. Superfish installed its own self-generated root certificate into the operating system's trust store, then silently re-signed the certificates of legitimate HTTPS sites so it could inject ads into encrypted traffic without triggering a browser security warning — textbook man-in-the-middle behavior. Because every affected laptop shipped with the identical root certificate, and its private key was later cracked and published, anyone who obtained it could forge trusted certificates for arbitrary sites — including bank login pages — and impersonate them convincingly. (Source: PCWorld, Schneier on Security, and Keyfactor incident reporting on the 2015 Lenovo/Superfish disclosure.)

### Impact
- Every HTTPS connection on an affected laptop could be silently decrypted and modified by anyone possessing the certificate's key — including banking sessions.
- Widespread loss of consumer trust in Lenovo and pre-installed OEM software generally.
- Triggered a U.S. CERT security advisory and lawsuits against Lenovo.

### Mitigation Strategies
1. **Enforce certificate pinning / validate certificate chains** — Applications and browsers should treat unexpected or self-signed root certificates as untrusted rather than silently accepting them.
2. **Use encrypted, authenticated protocols end-to-end (TLS with HSTS)** — Ensures traffic can't be silently downgraded or transparently proxied without detection.
3. **Avoid untrusted networks / use a VPN** — Especially on public Wi-Fi, routing traffic through a trusted encrypted tunnel prevents local attackers from inserting themselves between the user and the destination.

---

## 3. IP Spoofing

### How It Works
IP spoofing is the practice of forging the source IP address in a packet so
it appears to come from a different, often trusted or victim, machine.
Attackers use this to hide their identity, bypass IP-based access controls,
or — most commonly today — to enable reflection/amplification DDoS attacks,
where a forged "victim" address is used so that responses from a third-party
server are sent to the victim instead of the attacker.

### Real-World Example
In March 2013, the anti-spam organization **Spamhaus** was hit by what was
at the time the largest DDoS attack ever recorded, peaking at roughly 300
Gbps. The attackers amplified their DDoS traffic by spoofing requests to open DNS resolvers using Spamhaus's IP address as the forged source, so the (much larger) DNS responses were sent back to Spamhaus instead of the attacker, quickly consuming the victim's available bandwidth. Over 30,000 different DNS servers were abused as unwitting amplifiers in the attack. (Source: Cisco Blogs and DarkReading incident reporting on the March 2013 Spamhaus DDoS attack.)

### Impact
- Took the Spamhaus website and portions of its email service offline.
- Caused measurable collateral congestion for internet infrastructure and other organizations near the attack path.
- Became a landmark case that pushed the industry toward widespread adoption of source-address validation standards.

### Mitigation Strategies
1. **Ingress/egress filtering (BCP38 / RFC 2827)** — Internet service providers and network operators should block outbound packets whose source IP doesn't belong to their own address range, preventing spoofed traffic from ever leaving the network it originated on.
2. **Disable or restrict open resolvers/reflectors** — Ensure DNS resolvers, NTP servers, and similar UDP services aren't openly accessible to the whole internet, removing potential amplification vectors.
3. **Deploy anycast and traffic-scrubbing at the network edge** — Distributes and filters incoming traffic across many nodes so a spoofed flood can't concentrate on a single point.

---

## 4. DNS Poisoning / Spoofing (Bonus Threat)

### How It Works
DNS poisoning (also called DNS cache poisoning or spoofing) corrupts the
data a DNS resolver stores, causing it to return a fraudulent IP address for
a domain name. Because resolvers cache answers and other resolvers query
each other, a single poisoned entry can propagate across many networks. A
related but distinct technique, **DNS hijacking**, compromises the
authoritative records themselves (e.g., via a compromised registrar
account), redirecting *every* user regardless of which resolver they use.

### Real-World Example
The **Sea Turtle** campaign, identified in 2019, was a state-sponsored DNS
hijacking operation. The campaign spanned 13 countries and targeted at least 40 public and private entities, altering DNS A-records to reroute victims to spoofed login pages and harvest their credentials. Unlike typical cache poisoning, the attackers first compromised domain registrar accounts through credential theft and then modified the authoritative DNS records directly — meaning every user querying those domains was affected, regardless of which resolver they used. (Source: SentinelOne and LogicMonitor incident reporting on the 2019 Sea Turtle DNS hijacking campaign.)

### Impact
- Credential theft across government and critical-infrastructure organizations spanning multiple countries.
- Because the attack modified authoritative DNS rather than a single resolver's cache, it was far more persistent and widespread than a typical cache-poisoning incident.
- Undermined trust in DNS as a foundational, largely unauthenticated protocol.

### Mitigation Strategies
1. **Deploy DNSSEC** — Cryptographically signs DNS records so resolvers and clients can verify that a response hasn't been tampered with in transit or in cache.
2. **Enforce multi-factor authentication on registrar and DNS provider accounts** — Prevents attackers from hijacking authoritative records even if they steal a password.
3. **Use DNS query randomization and monitor for anomalies** — Randomizing query IDs/source ports makes cache-poisoning harder to pull off blindly, while monitoring for unexpected record changes provides early detection.

---

## 5. Comparison Table

| Threat | Attack Vector | Who Is At Risk | Difficulty to Execute | Ease of Mitigation |
|---|---|---|---|---|
| **DoS/DDoS** | Traffic/connection flooding, often via a botnet or amplification | Any internet-facing service; especially high-traffic or single-point-of-failure infrastructure (e.g., DNS providers) | Low–Medium (botnets and DDoS-for-hire services are cheap and widely available) | Medium (requires investment in scrubbing/CDN services and redundancy; hard to fully prevent, only mitigate) |
| **Man-in-the-Middle** | Interception of communications via ARP spoofing, rogue Wi-Fi, or forged certificates | Users on untrusted/public networks; any organization relying on unvalidated certificate trust | Medium (requires network positioning or a compromised trust anchor) | Medium–High (TLS, cert pinning, and VPNs are effective and well-established) |
| **IP Spoofing** | Forging the source IP address of packets | ISPs and networks without source-address validation; any service usable as a reflector/amplifier | Low–Medium (spoofing itself is simple; amplification requires abusable open services) | High (BCP38 ingress filtering is a known, effective, long-standing fix — but requires broad adoption) |
| **DNS Poisoning/Spoofing** | Corrupting resolver caches or hijacking authoritative DNS records | Anyone relying on unauthenticated DNS — effectively all internet users; highest-value targets are registrars and DNS providers | Medium–High (cache poisoning requires precise timing/guessing; registrar hijacking requires credential compromise) | Medium (DNSSEC is effective but adoption is still incomplete across the internet) |

---

## Conclusion: 3 Key Takeaways for a Network Administrator

1. **Availability, integrity, and authenticity all need separate defenses.** DoS/DDoS attacks threaten availability, MITM and DNS attacks threaten integrity and authenticity of communications, and IP spoofing underlies several attack types. A resilient network posture needs redundancy and traffic filtering for uptime, and encryption/authentication (TLS, DNSSEC) for trust — one set of controls doesn't cover both.

2. **Foundational protocols (IP, DNS) are unauthenticated by default, so the network layer is a genuine attack surface, not just the application layer.** Many of the incidents above — Spamhaus, Dyn, Sea Turtle — succeeded not because of a coding bug in an app, but because core internet protocols trust data they shouldn't. Administrators should assume DNS responses and packet source addresses can be forged unless explicitly protected (DNSSEC, ingress filtering).

3. **The weakest link is often the easiest one to fix: default credentials, open resolvers, and unmonitored accounts.** Mirai spread through devices with default passwords; Spamhaus's attackers relied on open DNS resolvers that shouldn't have been publicly accessible; Sea Turtle relied on stolen registrar credentials that MFA would have blocked. Basic hygiene — changing defaults, closing unnecessary open services, and enforcing MFA — prevents a disproportionate share of real-world network attacks.
