# NETWORKWALKS-B084N-WK2-PM1-PENETRATION-TESTING-REPORT
<div align="center">

# PENETRATION-TESTING REPORT

## WEEK 2 | PROJECT MODULE 1

# FOOTPRINTING & RECONNAISSANCE ATTACKS
### USING MULTIPLE KALI LINUX TOOLS

<p>
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Focus-Footprinting%20%26%20Reconnaissance-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Tools-6-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/NetworkWalks-Internship-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-Project-181717?style=flat-square&logo=github&logoColor=white" />
</p>

**A practical report documenting domain footprinting and reconnaissance using six tools in Kali Linux.**

</div>

---

## 📌 Project Overview

| Field | Details |
|---|---|
| **Project** | Week 2 – Project Module 1 |
| **Project Title** | Footprinting & Reconnaissance Attacks Using Multiple Kali Tools |
| **Intern Name** | [Your Name] |
| **Program / Batch** | [Your NetworkWalks Batch] |
| **Date of Activity** | 13 November 2026 |
| **Module Completed** | W2-PM1 – Multiple Kali Tools |
| **Client / Target** | `networkwalks.com` |
| **Permission secured from client?** | [Confirm and enter Yes only if you have explicit permission] |
| **Operating System** | Kali Linux |
| **Phases Covered** | Phase 1 – Footprinting & Reconnaissance |

> **Authorization note:** The sample report states that its author had written permission. That permission statement cannot be assumed to apply to this project. Confirm your own authorization before stating that permission was secured or publishing target-specific findings. Only perform testing within the scope approved by the domain owner.

## 1. Introduction

This report documents the Week 2 – Project Module 1 practical activity completed as part of my cybersecurity training with **NetworkWalks**.

The objective of this activity was to understand how footprinting and reconnaissance are performed using different tools available in Kali Linux. I used six tools to gather publicly observable information about the `networkwalks.com` domain, including domain registration details, DNS information, web technologies, HTTP response headers and Web Application Firewall (WAF) detection.

The tools used for this activity were **WHOIS, WhatWeb, nslookup, curl, wafw00f and DNSRecon**.

For each task, I have included the command used, the observations from my terminal output, a screenshot as evidence and a short explanation of why the information is useful during a security assessment.

This practical helped me understand how information gathering forms the first stage of a penetration-testing workflow.

## 2. Tools Used

The following tools were used during the footprinting and reconnaissance activity.

| Tool | Purpose |
|---|---|
| **Kali Linux** | Operating system used to perform the reconnaissance tasks. |
| **WHOIS** | Retrieves publicly available domain registration information, registrar details and name servers. |
| **WhatWeb** | Identifies web technologies such as the web server, CMS, frameworks and plugins. |
| **nslookup** | Queries DNS to resolve a domain name to its IP address. |
| **curl** | Sends an HTTP request and displays response headers using the `-I` option. |
| **wafw00f** | Checks whether a website is protected by a detectable Web Application Firewall. |
| **DNSRecon** | Enumerates DNS information such as A, NS, MX, TXT and SRV records. |

## 3. Activities Performed

### 3.1 Footprinting & Reconnaissance

I performed the footprinting and reconnaissance activity against the `networkwalks.com` domain using six Kali Linux tools. Each tool helped me collect a different type of information about the domain and its web infrastructure.

### Task 1 – WHOIS Lookup

First, I used the `whois` command to collect publicly available domain registration information. This helped me identify the registrar, domain creation and expiry dates, domain status and name-server details.

**Command used:**

```bash
whois networkwalks.com
```

**Observations:**

- Domain name: `NETWORKWALKS.COM`
- Registrar: GoDaddy.com, LLC
- Creation date: 6 November 2019
- Registry expiry date shown: 6 November 2027
- Name servers included `NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM`
- DNSSEC status: unsigned
- Registrant information was privacy-protected in the registrar output.

**Why this matters:** WHOIS information can help build an initial understanding of a domain's registration history and DNS hosting infrastructure.

**Screenshot / Evidence:**

![WHOIS command output](screenshots/WHOIS-result.png)

---

### Task 2 – WhatWeb Technology Identification

Next, I used WhatWeb to identify the technologies used by the website. The tool followed the HTTP-to-HTTPS redirect and reported information about the web server, CMS, plugins and other web technologies.

**Command used:**

```bash
whatweb networkwalks.com
```

**Observations:**

- HTTP returned `301 Moved Permanently` and redirected to HTTPS.
- HTTPS returned `200 OK`.
- Web server identified: Apache.
- CMS identified: WordPress 7.1.4.
- WordPress Download Manager identified: 3.3.76.
- Other detected technologies included Bootstrap 7.1.4 and jQuery 3.7.1.
- Google Tag Manager was also detected.
- Website title: Networkwalks Academy.
- IP address reported: `192.232.216.135`.

**Why this matters:** Technology fingerprinting helps a security tester understand which platforms and components are publicly detectable. Version information can guide further authorized security review, but it does not by itself confirm a vulnerability.

**Screenshot / Evidence:**

![WhatWeb command output](screenshots/whatweb-result.png)

---

### Task 3 – DNS Lookup Using nslookup

I then used `nslookup` to resolve the domain name and identify the IP address returned by the DNS resolver.

**Command used:**

```bash
nslookup networkwalks.com
```

**Observations:**

- DNS server used: `192.168.1.1`
- DNS server address: `192.168.1.1#53`
- Domain name: `networkwalks.com`
- Resolved IP address: `192.232.216.135`

**Why this matters:** DNS lookup helps identify the IP address associated with a domain at the time of testing. This is one of the basic steps in understanding a website's internet-facing infrastructure.

**Screenshot / Evidence:**

![nslookup command output](screenshots/nslookup-result.png)

---

### Task 4 – HTTP Header Inspection Using curl

After that, I used `curl` with the `-I` option to inspect the HTTP response headers returned by the website.

**Command used:**

```bash
curl -I https://networkwalks.com
```

**Observations:**

- HTTP response: `200 OK`
- Server header: Apache
- Content type: `text/html; charset=UTF-8`
- The response included security-related headers such as `Permissions-Policy` and `Referrer-Policy`.
- The `Link` header exposed WordPress REST API references, including `/wp-json/`.
- A cookie named `__wpdm_client` was returned with `Secure` and `HttpOnly` attributes.
- Cache-related headers included `X-Endurance-Cache-Level` and `X-nginx-cache`.

**Why this matters:** HTTP response headers can reveal information about the server and web application. Reviewing them helps a tester understand what technical details are exposed to a remote client.

**Screenshot / Evidence:**

![curl command output](screenshots/curl-result.png)

---

### Task 5 – Web Application Firewall Detection Using wafw00f

I used `wafw00f` to check whether the website was protected by a detectable Web Application Firewall.

**Command used:**

```bash
wafw00f networkwalks.com
```

**Observations:**

- The tool identified the website as being behind **ModSecurity (SpiderLabs) WAF**.
- Number of requests reported: 2.
- wafw00f completed its detection check.

**Why this matters:** Identifying a WAF provides information about a website's visible defensive layer. However, detecting a WAF does not confirm how effectively it is configured or whether it blocks every type of attack.

**Screenshot / Evidence:**

![wafw00f command output](screenshots/wafw00f-result.png)

---

### Task 6 – DNS Enumeration Using DNSRecon

Finally, I used DNSRecon to enumerate DNS records associated with the domain. The output provided information about name servers, mail servers, TXT records and service records.

**Command used:**

```bash
dnsrecon -d networkwalks.com
```

**Observations:**

- DNSRecon reported 8 records found.
- SOA record: `ns6135.hostgator.com` – `50.87.144.87`.
- Name servers included:
  - `ns6135.hostgator.com` – `50.87.144.87`
  - `ns6136.hostgator.com` – `192.232.216.131`
- MX record: `mail.networkwalks.com` – `192.232.216.135`.
- A record: `networkwalks.com` – `192.232.216.135`.
- TXT records included an SPF policy and a Google site-verification value.
- SRV records were found for `_autodiscover._tcp`, pointing to cPanel email-discovery hosts.
- The output displayed a DNSSEC-query error.
- The tool also reported that recursion was enabled on the two listed name servers.

**Why this matters:** DNS enumeration helps map the DNS infrastructure associated with a domain, including web, mail and service-related records. Messages reported by the tool should be reviewed by the responsible administrator before being treated as security issues.

**Screenshot / Evidence:**

![DNSRecon command output](screenshots/dnsrecon-result.png)

---

## 4. Summary of Findings

The following table summarizes the main observations collected from the six tools.

| No. | Tool | Main Finding |
|---|---|---|
| 1 | WHOIS | Registrar identified as GoDaddy.com, LLC; name-server and registration details were retrieved. |
| 2 | WhatWeb | Apache, WordPress 7.1.4, WordPress Download Manager 3.3.76, Bootstrap 7.1.4 and jQuery 3.7.1 were detected. |
| 3 | nslookup | `networkwalks.com` resolved to `192.232.216.135`. |
| 4 | curl | Website returned `200 OK`; HTTP response headers and WordPress REST API references were observed. |
| 5 | wafw00f | ModSecurity (SpiderLabs) WAF was detected. |
| 6 | DNSRecon | 8 records were reported, including SOA, NS, A, MX, TXT and SRV information. |

## 5. Risk Analysis / Impact

Based on the information collected during the reconnaissance activity, I identified the following observations that may be relevant for further security review.

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
|---|---|---|---|---|
| 1 | Web technology information exposed | WhatWeb identified WordPress, WordPress Download Manager and other technologies. | Public technology information may help an attacker identify components for further investigation. | Medium |
| 2 | Server IP address identifiable | nslookup and WhatWeb reported `192.232.216.135`. | Helps identify the IP address associated with the website. | Low |
| 3 | HTTP technical information exposed | curl returned server and application-related response headers and WordPress REST API references. | May assist technology fingerprinting and further enumeration. | Low |
| 4 | WAF technology identifiable | wafw00f detected ModSecurity (SpiderLabs). | Reveals information about the website's visible security architecture. | Low |
| 5 | DNS infrastructure information exposed | DNSRecon identified name servers, mail records, TXT records and SRV records. | May help someone build a broader picture of the domain's infrastructure. | Medium |
| 6 | DNS recursion reported as enabled | DNSRecon displayed recursion-enabled messages for the two name servers. | If unintended or unrestricted, recursive DNS service may create security and abuse risks. | Medium – verify configuration |

**Risk level key:** Critical / High / Medium / Low.

The risk levels above are preliminary observations for learning and review. They are not the result of a complete vulnerability assessment. In particular, the DNS recursion messages require verification by the responsible DNS administrator before any issue can be confirmed.

The practical activity focused on footprinting and information gathering. No exploitation, password testing or vulnerability validation was performed as part of this module.

Therefore, the presence of a software version, IP address, DNS record or WAF does not automatically mean that the website is vulnerable.

## 6. Recommendations

Based on the observations from this activity, the following security recommendations may be considered by the responsible website and infrastructure administrators:

1. **Review publicly exposed technology information**  
   Regularly review the web technologies, CMS details and plugin information that are publicly detectable.

2. **Keep software and plugins updated**  
   Maintain supported versions of the CMS, plugins, frameworks and other web components. Review relevant security advisories.

3. **Review HTTP response headers**  
   Check whether response headers expose unnecessary implementation details and maintain appropriate security-related headers.

4. **Review DNS records regularly**  
   Ensure that DNS records are accurate, necessary and maintained according to the intended infrastructure configuration.

5. **Verify DNS recursion settings**  
   Review the DNSRecon recursion messages with the DNS administrator and ensure recursive queries are restricted to intended clients where applicable.

6. **Maintain and monitor the WAF**  
   Keep ModSecurity properly configured, updated and monitored. Detection of a WAF alone does not establish its effectiveness.

7. **Perform regular security assessments**  
   Conduct further testing only with explicit authorization and within an agreed scope.

## 7. Conclusion

During Week 2 – Project Module 1 of my cybersecurity training at NetworkWalks, I completed practical activities related to **footprinting and reconnaissance using multiple Kali Linux tools**.

I used six tools to collect different types of information about the target domain. Through WHOIS, I learned how to retrieve domain registration information. WhatWeb helped me identify the technologies used by the website. nslookup helped me resolve the domain name to an IP address. With curl, I inspected HTTP response headers. wafw00f helped me identify the detectable Web Application Firewall, and DNSRecon helped me enumerate DNS records.

This practical helped me understand that information gathering is an important first step in cybersecurity and penetration testing. Even without exploiting a system, a security tester can collect useful information about a domain, its web technologies and its DNS infrastructure.

I also learned the importance of documenting each command, recording the actual output, understanding what each finding means and distinguishing an observation from a confirmed vulnerability.

Overall, this project improved my familiarity with Kali Linux reconnaissance tools and gave me practical experience in documenting the initial footprinting phase of a security assessment.


# 👤 Author

**S Bhanu Prakash**\
B.Tech Student | Cybersecurity Intern, Networkwalks (Batch B084N) 

GitHub: [https://github.com/sbhanuprakash-sbp](https://github.com/sbhanuprakash-sbp)\
LinkedIn: [https://www.linkedin.com/in/s-bhanu-prakash/](https://www.linkedin.com/in/s-bhanu-prakash/)

---

## 📌 Project Information

**Program Name:** Cybersecurity at Networkwalks | **Week:** 02 | **Project:** Penetration Testing Report | **Repository:** GitHub

</div>
