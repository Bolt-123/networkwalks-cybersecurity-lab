# Week 2 Penetration Testing & Reconnaissance Report (`W2-PM-FINAL`)

**Author:** Ramadan Adesoye  
**Date:** November 22, 2026  
**Course:** Networkwalks Cybersecurity Lab (`B084E`)  
**Environment:** Windows 11 Host + Kali Linux 2026.3 VM (Oracle VirtualBox 7)  

---

## 1. Executive Summary
During Week 2, passive and active reconnaissance (`W2-PM1`) was conducted against the authorized external target `networkwalks.com` using Kali Linux CLI tools, followed by local subnet discovery and topology mapping (`W2-PM5`) across `192.168.1.0/24` using Nmap/Zenmap 7.993 on Windows[cite: 3, 4, 5, 6, 7, 13].

---

## 2. Module 1: Web & DNS Footprinting (`W2-PM1` — `networkwalks.com`)

| Task | Tool | Key Findings |
| :--- | :--- | :--- |
| **Task 1** | `whois` | Domain registered via HostGator/Launchpad; RDAP service notice observed[cite: 1]. |
| **Task 2** | `whatweb` | Resolved to `192.232.216.135` (US); running **Apache**, **WordPress 7.1.5**, **Bootstrap 7.1.5**, **jQuery 3.7.1**, and **WordPress Download Manager 3.3.78**; contact `info@networkwalks.com`[cite: 4]. |
| **Task 3** | `nslookup` | Confirmed primary `A` record mapping `networkwalks.com` to `192.232.216.135`. |
| **Task 4** | `curl -I` | Inspected HTTP response headers and server caching directives (`x-endurance-cache-level`, `x-nginx-cache`)[cite: 4]. |
| **Task 5** | `wafw00f` | Evaluated Web Application Firewall protections (HostGator ModSecurity triggered `406 Not Acceptable` on minimal User-Agent strings)[cite: 3]. |
| **Task 6** | `dnsrecon` | Enumerated `SOA` (`ns6135.hostgator.com`), `NS` (`ns6135` / `ns6136.hostgator.com`), `MX` (`mail.networkwalks.com`), `TXT` (SPF & Google Site Verification), and 8 `_autodiscover._tcp` `SRV` records; DNSSEC not enabled[cite: 6, 7]. |

---

## 3. Module 2: Local Network Discovery (`W2-PM5` — `192.168.1.0/24`)

* **Scan Command:** `nmap -sn 192.168.1.0/24` (Ping Scan)[cite: 13]
* **Scan Result:** 256 IP addresses scanned in 4.38 seconds; **3 live hosts discovered**[cite: 13].

| Host IPv4 Address | MAC Address | Role / Interface Details |
| :--- | :--- | :--- |
| `192.168.1.1` | `86:A8:79:38:6E:E8` | Default Gateway / Wireless Router (0.011s latency)[cite: 12, 13] |
| `192.168.1.4` | `F0:D5:BF:CD:52:6C` | Assessor Windows Host (`Intel Dual Band Wireless-AC 8260`)[cite: 12, 13, 14] |
| `192.168.1.9` | `C6:ED:F9:BC:8C:12` | Active Local LAN Client (0.048s latency)[cite: 13] |

* **Topology Deliverable:** Exported visual network map with legend to [`Zenmap-Topology.pdf`](./w2-pm5-zenmap/Zenmap-Topology.pdf)[cite: 15, 16].

---

## 4. Security Observations & Recommendations
1. **CMS & Plugin Patch Management (`networkwalks.com`):** Publicly exposed WordPress and plugin version metadata (`WordPress Download Manager 3.3.78`) allows attackers to map known CVEs[cite: 4]. Restrict version banners and maintain timely plugin updates.
2. **DNSSEC Deployment (`networkwalks.com`):** `dnsrecon` indicated no DNSSEC record configured for the domain[cite: 6, 7]. Enabling DNSSEC prevents DNS spoofing and cache poisoning attacks.
3. **WAF User-Agent Filtering:** While ModSecurity blocks basic automated User-Agents (`406 Not Acceptable`), standard browser User-Agent strings still bypass basic signature checks[cite: 3, 4]; enforce rate-limiting alongside header filtering.