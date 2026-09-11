# Task 1 · Basic Network Scanning with Nmap

## 1. What is Nmap?

Nmap ("Network Mapper") is a free, open-source tool used to discover hosts and
services on a computer network. It works by sending specially crafted packets
to a target and analyzing the responses. From those responses Nmap can tell
you:

- Which hosts on a network are up
- Which ports on a host are open, closed, or filtered
- Which service (and often which version) is listening on each open port
- Clues about the target's operating system and network configuration

It's one of the most widely used tools in both offensive security
(penetration testing, red teaming) and defensive security (asset inventory,
vulnerability management, network auditing).

## 2. Why Network Scanning Matters

You can't secure what you don't know you have. Network scanning is the
foundation of security assessment because it answers a simple but critical
question: **"What is actually reachable, and what is it running?"**

- **Asset visibility** — Servers accumulate services over time (test
  services left running, forgotten admin panels, default installs). Scanning
  surfaces what's really exposed, not what's documented.
- **Attack surface reduction** — Every open port is a potential entry point.
  Identifying them lets you close or firewall off anything unnecessary.
- **Vulnerability triage** — Knowing the exact service and version (e.g.
  `OpenSSH 7.2p2`) lets you cross-reference against known CVEs.
- **Baseline & drift detection** — A documented scan today gives you
  something to compare against later, so unexpected new services stand out.
- **Compliance** — Many security frameworks (PCI-DSS, ISO 27001, NIST)
  explicitly require periodic network/port scanning as part of due
  diligence.

## 3. Ethical Use Guidelines

Nmap is a dual-use tool — the same scan that helps you secure a system could
help an attacker map one. Scanning systems you don't own or don't have
explicit permission to test is illegal in most jurisdictions (e.g. under the
U.S. Computer Fraud and Abuse Act, UK Computer Misuse Act, etc.), even if no
damage is done.

**Rules followed for this task:**

1. **Scope** — Only scan hosts you own, or a local/isolated VM you control
   (e.g. a Kali or Ubuntu VM on a NAT/host-only VirtualBox network). Never
   scan a target on a network you don't administer without written
   authorization.
2. **Isolation** — Scans in this exercise were run against a local VM on an
   isolated virtual network (host-only or NAT), not against production
   infrastructure or third-party hosts.
3. **Minimal disruption** — Basic and service-version scans are low-impact,
   but aggressive scan types (e.g. `-A`, `-T5`, vulnerability scripts) can
   generate heavy traffic or trigger IDS/IPS alerts. None of those were used
   here without a controlled environment.
4. **Purpose** — Findings are used defensively: to understand what's exposed
   and recommend hardening steps, not to exploit anything.
5. **Documentation & disclosure** — All results are recorded transparently
   in this repo for learning purposes; nothing was scanned covertly.

> If you ever scan something outside a lab environment, get explicit written
> permission first (a signed authorization / rules-of-engagement document),
> even for something as "harmless" as a basic port scan.

## 4. Installing Nmap

### Debian/Ubuntu/Kali Linux
```bash
sudo apt update
sudo apt install nmap -y
nmap --version
```

### Fedora/RHEL/CentOS
```bash
sudo dnf install nmap -y
```

### macOS (via Homebrew)
```bash
brew install nmap
```

### Windows
1. Download the installer from https://nmap.org/download.html
2. Run the `.exe` installer (includes Npcap, required for packet capture)
3. Verify in PowerShell:
   ```powershell
   nmap --version
   ```

## 5. Scans Performed

All scans were run from the host/attacker machine against the target VM's
IP address (replace `<target-ip>` with your actual VM IP, found via
`ip a` / `ifconfig` on the VM itself).

### 5.1 Basic scan
```bash
nmap <target-ip>
```
Scans the 1,000 most common TCP ports and reports open/closed/filtered
status. This is the fastest way to get an overview of what's exposed.

### 5.2 Service version detection
```bash
nmap -sV <target-ip>
```
Adds a probing stage that grabs banners and fingerprints to identify not
just that a port is open, but *which software and version* is listening
on it (e.g. `Apache httpd 2.4.41` vs just "port 80 open").

### 5.3 OS detection
```bash
nmap -O <target-ip>
```
Uses TCP/IP stack fingerprinting (analyzing quirks in how the target
responds to crafted packets) to guess the target's operating system and
kernel version. Requires root/administrator privileges to run.

### Combined (optional, run once permissions are confirmed)
```bash
sudo nmap -sV -O <target-ip>
```

## 6. Repo Contents

| File | Description |
|---|---|
| `README.md` | This file — background, install steps, ethics |
| `nmap_scan_results.txt` | Structured findings: open ports, services, versions, OS guess, risk analysis |
| `screenshots/` | Terminal output screenshots for each scan type |

## 7. Disclaimer

This exercise was performed strictly against a local, self-owned virtual
machine for educational purposes. No external or third-party systems were
scanned.
