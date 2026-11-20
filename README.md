# 🛡️ Complete VAPT Assessment — CEH-2602

![VAPT Project Cover](Cybersecurity%20VAPT%20Project%20Report%20Cover.png)

A hands-on **Vulnerability Assessment and Penetration Testing (VAPT)** project completed as part of the **CEH-2602** assignment. The practical assessment used an isolated VMware lab with Kali Linux as the testing machine and Metasploitable 2 as the intentionally vulnerable target.

The work covered reconnaissance, network scanning, service enumeration, vulnerability assessment, controlled exploitation, evidence collection, and remediation planning.

> **Scope and safety:** Active testing and exploitation documented here were performed against the Metasploitable 2 lab machine. The assigned public domain, `megacorpone.com`, was handled according to the assignment's restrictions: passive reconnaissance and only explicitly authorized, limited basic scanning. No exploitation of the public domain was performed.

---

## 📑 Table of Contents

- [Project Objectives](#-project-objectives)
- [Lab Environment](#-lab-environment)
- [Assessment Scope and Methodology](#-assessment-scope-and-methodology)
- [Repository Contents](#-repository-contents)
- [Key Findings](#-key-findings)
- [Exploitation Summary](#-exploitation-summary)
- [Risk and Remediation](#-risk-and-remediation)
- [Evidence and Validation Notes](#-evidence-and-validation-notes)
- [Disclaimer](#️-disclaimer)
- [Author](#-author)

---

## 🎯 Project Objectives

The objectives of this project were to:

1. Collect publicly available information about the assigned domain.
2. Discover active hosts and exposed TCP services in the isolated lab.
3. Identify service names, versions, banners, and potential misconfigurations.
4. Assess selected vulnerabilities and map them to their CVE identifiers.
5. Validate three assigned vulnerabilities through controlled lab exploitation.
6. Record original evidence, evaluate potential impact, and recommend remediation.

## 🧪 Lab Environment

| Parameter | Details |
|---|---|
| Attacker machine | Kali Linux — `192.168.101.128` |
| Target machine | Metasploitable 2 — `192.168.101.129` |
| Target hostname | `irc.Metasploitable.LAN` |
| Lab subnet | `192.168.101.0/24` |
| Virtualization | VMware |
| Network mode | VMware virtual network — confirm the exact mode in the VM settings (Host-only is preferred for an isolated lab) |
| Main tools | Nmap, Netcat, WhatWeb, cURL, `smbclient`, Metasploit Framework |

**Network isolation:** The target is an intentionally vulnerable machine. Keep it on an isolated Host-only network and do not expose it to a public or production network. If NAT was used, verify the VM network configuration and firewall rules before describing the environment as fully isolated.

## 🔍 Assessment Scope and Methodology

The assessment followed these phases:

| Phase | Activity | Environment |
|---|---|---|
| 1 | Footprinting and reconnaissance — WHOIS, DNS, public certificate information, technology research | Assigned public domain; passive methods and approved checks only |
| 2 | Host discovery, TCP port scanning, service/version detection, OS fingerprinting | Metasploitable 2 lab |
| 3 | FTP, SSH, Telnet, HTTP, SMB, database, DistCC, and IRC enumeration | Metasploitable 2 lab |
| 4 | Vulnerability identification, CVE research, CVSS review, severity assessment | Metasploitable 2 lab |
| 5 | Controlled validation/exploitation of three assigned vulnerabilities | Metasploitable 2 lab only |

### Assessment workflow

`Reconnaissance` → `Network Scanning` → `Service Enumeration` → `Vulnerability Assessment` → `Controlled Exploitation` → `Risk Analysis` → `Reporting`

## 📂 Repository Contents

The following documents contain the detailed methodology, command outputs, screenshots, findings, and remediation recommendations.

| Document | Assessment phase | Contents |
|---|---|---|
| [CEH-2602 VAPT Assignment (Final).pdf](CEH-2602%20VAPT%20Assignment%20%28Final%29.pdf) | Consolidated report | Combined assignment report, assessment summary, findings, and remediation |
| [Task-01.pdf](Task-01.pdf) | Footprinting & Reconnaissance | Public information gathering and attack-surface observations |
| [Task-02.pdf](Task-02.pdf) | Network Scanning | Host discovery, full TCP port scan, service/version detection, and OS fingerprinting |
| [Task-03.pdf](Task-03.pdf) | Service Enumeration | Enumeration and banner review of the identified services |
| [Task-04.pdf](Task-04.pdf) | Vulnerability Assessment | Vulnerability scan results, CVE mapping, CVSS references, and severity assessment |
| [Task-05.pdf](Task-05.pdf) | Exploitation | Evidence and validation results for the three assigned lab vulnerabilities |
| [Cybersecurity VAPT Project Report Cover.png](Cybersecurity%20VAPT%20Project%20Report%20Cover.png) | Project cover | Report cover image |

> File links are relative to this repository. Keep the filenames in the repository consistent with the names shown above; update the links if your uploaded filenames are different.

## 🚨 Key Findings

Three assigned vulnerabilities were investigated on the Metasploitable 2 target. The CVSS score and version are shown separately because the referenced records do not all use the same CVSS version.

| Finding | Service / Port | CVE | Referenced CVSS | Severity |
|---|---|---|---:|---|
| vsftpd 2.3.4 backdoor | FTP / `21/tcp` | [CVE-2011-2523](https://nvd.nist.gov/vuln/detail/CVE-2011-2523) | 9.8 — CVSS 3.1 | **Critical** |
| UnrealIRCd 3.2.8.1 backdoor | IRC / `6667/tcp` | [CVE-2010-2075](https://nvd.nist.gov/vuln/detail/CVE-2010-2075) | 7.5 — CVSS 2.0 | **High** |
| DistCC daemon command execution | DistCC / `3632/tcp` | [CVE-2004-2687](https://nvd.nist.gov/vuln/detail/CVE-2004-2687) | 9.3 — CVSS 2.0 | **High** |

**Scoring note:** It would be inaccurate to describe all three findings as “Critical” or to assign all of them a CVSS 3.1 score of 9.8. The table preserves the score and CVSS version associated with each cited record. CVSS scores from different versions should not be directly compared as if they used the same scoring model.

### Important vulnerability context

- **CVE-2011-2523:** The NVD description specifies affected vsftpd 2.3.4 downloads from the period **30 June 2011 through 3 July 2011**. The version string alone is not sufficient to prove that every build of 2.3.4 contains the backdoor.
- **CVE-2010-2075:** The issue relates to a trojanised UnrealIRCd 3.2.8.1 source archive distributed through certain mirror sites during the affected period. Do not imply that every copy of version 3.2.8.1 is malicious.
- **CVE-2004-2687:** The issue concerns exposed/misconfigured DistCC services that allow unauthenticated remote command execution. The service exposure and configuration are relevant to validation.

## 💥 Exploitation Summary

The following Metasploit module paths are included for documentation and reproducibility. Module names and available payloads may vary by Metasploit Framework version; the module options and actual session output in the evidence files are the source of truth.

| Vulnerability | Metasploit module path | Lab result recorded in this project |
|---|---|---|
| vsftpd 2.3.4 backdoor | `exploit/unix/ftp/vsftpd_234_backdoor` | Meterpreter session reported; `id` output recorded as `uid=0(root)` |
| UnrealIRCd 3.2.8.1 backdoor | `exploit/unix/irc/unreal/ircd_3281_backdoor` | Meterpreter session reported; `id` output recorded as `uid=0(root)` |
| DistCC command execution | `exploit/unix/misc/distcc/exec` | Command shell reported; `id` output recorded as `uid=1(daemon)` |

**Validation requirement:** Keep the session type and privilege information above only if the original Metasploit screenshots or terminal logs show those exact results. A successful service/version check is not, by itself, proof of successful exploitation. The evidence should show the module used, target address/port, session type, and post-exploitation identity output.

Official module references: [vsftpd 2.3.4 module](https://www.rapid7.com/db/modules/exploit/unix/ftp/vsftpd_234_backdoor/), [UnrealIRCd module](https://www.rapid7.com/db/modules/exploit/unix/irc/unreal_ircd_3281_backdoor/), and [DistCC module](https://www.rapid7.com/db/modules/exploit/unix/misc/distcc_exec/).

## 🛡️ Risk and Remediation

| Finding / risk | Recommended remediation |
|---|---|
| Backdoored vsftpd package | Remove the affected package/build. Reinstall from a trusted official source, verify package integrity, and restrict FTP access. Prefer SFTP where appropriate. |
| Trojanised UnrealIRCd archive | Remove the affected binary/source build. Reinstall a verified release from an official source, verify its integrity, and restrict IRC service access. |
| Exposed DistCC daemon | Disable DistCC if it is not required. Otherwise, restrict access to trusted build hosts and enforce host/network firewall rules. |
| Clear-text or legacy services | Disable Telnet and other unnecessary legacy services. Disable SMBv1 if present and not required. Use encrypted alternatives such as SSH, SFTP, and HTTPS. |
| Exposed database services | Bind database listeners to localhost or approved internal interfaces where possible, restrict inbound access, and require strong authentication. |
| Unsupported operating system/software | Replace end-of-life systems and packages with supported releases and apply security updates. Confirm the exact OS release on the target before naming a distribution/version in the final report. |

## 📸 Evidence and Validation Notes

- Screenshots and command outputs in the task reports should be original evidence collected from the project lab.
- Each finding should include a concise explanation, affected service/port, CVE reference, impact, and remediation.
- Do not publish passwords, private keys, VPN configuration files, tokens, or other sensitive information.
- The host IP addresses shown above are private lab addresses. Replace them if the final evidence uses different addresses.
- Nmap OS detection is a fingerprint/estimate; describe `Linux 2.6.x` as an observed fingerprint rather than a confirmed distribution release unless you separately verified the OS on the target.

## ⚠️ Disclaimer

This project was completed for educational and academic purposes in a controlled virtual lab. Exploitation was limited to the intentionally vulnerable Metasploitable 2 machine under the stated lab scope. Do not scan, test, or exploit systems without explicit authorization. The public-domain reconnaissance was subject to the assignment's restrictions and was not used as an exploitation target.

## 👨‍💻 Author

**Sohel Rana**  
GitHub: [@sohelrana53](https://github.com/sohelrana53)

---

*Project: CEH-2602 Complete VAPT Assignment*  
*Environment: VMware | Kali Linux | Metasploitable 2*
