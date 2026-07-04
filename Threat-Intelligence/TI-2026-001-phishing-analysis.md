# Technical Report: Analyzing Phishing Campaign Infrastructure

* **Research ID:** TI-2026-001
* **Publication Date:** July 4, 2026
* **Research Group:** Sentry Point Cyber Intelligence
* **Focus:** Threat Intelligence / OSINT

---

## 1. Executive Summary

Recently, there has been a significant increase in targeted phishing attacks aimed at various user segments. Threat actors disguise their web resources as legitimate services (authorization panels, public services, email platforms) to steal user credentials and session tokens.

This report demonstrates an OSINT analysis methodology to identify phishing infrastructure, determine adversary servers, and collect Indicators of Compromise (IoCs).

## 2. Infrastructure OSINT Methodology

During the analysis of suspicious links, our team relies exclusively on passive open-source intelligence gathering methods to avoid alerting the threat actors.

### Step 1: Domain Name Analysis (WHOIS)
Phishing domains exhibit specific anomalies that help distinguish them from legitimate ones:
* **Domain Age:** Most phishing sites are deployed on "fresh" domains registered anywhere from a few hours to a few weeks ago.
* **Registrar:** Attackers often use registrars with a low barrier to entry and cheap or free TLD zones.
* **Data Privacy:** Registrant data is completely hidden behind Privacy Protection services.

### Step 2: DNS & Hosting Analysis
Analyzing DNS records (A, MX, NS, TXT) allows us to determine:
* The real IP addresses of the servers hosting the phishing scripts.
* The use of reverse proxy services (such as Cloudflare's free tiers) to conceal the actual hosting provider.

### Step 3: SSL/TLS Certificate Analysis
Threat actors commonly use automated services to generate free certificates (e.g., *Let's Encrypt* or *ZeroSSL*). Researching certificate history through Certificate Transparency logs (such as `crt.sh`) helps uncover other subdomains being prepared for future campaigns.

## 3. Indicators of Compromise (IoCs)

To protect your perimeter and configure firewalls, it is recommended to add the following indicators to your blocklists.

*Note: Domains and IPs are defanged using `[.]` and `[:]` to prevent accidental navigation.*

| Indicator Type | Value | Threat Description / Context |
| :--- | :--- | :--- |
| **Domain** | `secure-login-portal[.]com` | Phishing page mimicking a corporate login panel |
| **Domain** | `update-verification-service[.]net` | User session token harvesting page |
| **IP Address** | `192[:]0[:]2[:]105` | Phishing script hosting server in a low-reputation zone |
| **IP Address** | `198[:]51[:]100[:]42` | Discovered Command and Control (C2) server for log gathering |

## 4. Detection & Mitigation Strategies

To counter the described threat, organizations and individuals are advised to:

1. **DNS Filtering:** Implement enterprise-grade solutions or home systems (e.g., Pi-hole, AdGuard Home) integrated with TI feeds to block requests to Newly Registered Domains (NRDs).
2. **Typosquatting Monitoring:** Automatically monitor the registration of domains that are visually similar to your brand (e.g., lookalike characters, transposed letters).
3. **Multi-Factor Authentication (MFA):** Deploy phishing-resistant MFA methods (such as FIDO2/YubiKey hardware keys) instead of standard SMS or OTP codes, as modern Adversary-in-the-Middle (AitM) phishing platforms (like Evilginx) can intercept one-time codes in real time.
