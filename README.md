# 🛡️ Sentinel Detection Engineering

> Production-grade KQL detection rules and threat hunting queries for Microsoft Sentinel — built by a Detection Engineer with hands-on SOC experience.

[![Sentinel](https://img.shields.io/badge/Microsoft%20Sentinel-Compatible-0078D4?logo=microsoft-azure)](https://azure.microsoft.com/en-us/products/microsoft-sentinel)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Detections](https://img.shields.io/badge/Detection%20Rules-7-blue)](detections/)
[![Hunting](https://img.shields.io/badge/Hunting%20Queries-5-brightgreen)](hunting-queries/)

---

## 👤 About This Repository

This repo contains **production KQL detection rules and threat hunting queries** used in a real SOC environment, organized by MITRE ATT&CK tactic. Each rule includes:

- Documented detection logic and rationale
- MITRE ATT&CK mapping
- Severity classification
- Known false positive guidance
- Tuning recommendations (exposed as `let` variables)


📖 Detection engineering write-ups: [Medium](https://medium.com/@patelvidhi4288)

---

## 📁 Repository Structure

```
sentinel-detection-engineering/
├── detections/                        # Scheduled analytics rules (KQL)
│   ├── identity/                      # Account compromise, auth anomalies
│   ├── endpoint/                      # Process, script, and file activity
│   ├── network/                       # DNS, firewall, VPN, proxy anomalies
│   ├── cloud/                         # Azure AD, M365, resource abuse
│   └── lateral-movement/              # Internal spread techniques
├── hunting-queries/                   # Proactive threat hunting KQL
│   ├── identity/                      # Account-based hunting
│   ├── endpoint/                      # Host and process-based hunting
│   └── network/                       # Network traffic hunting
└── docs/                              # Detection methodology & notes
```

---

## 🔍 Detection Rules

### Identity & Access
| Rule | MITRE | Severity | Description |
|------|-------|----------|-------------|
| [Impossible Travel](detections/identity/impossible-travel.kql) | T1078 | High | Sign-ins from geographically impossible locations |
| [Password Spray](detections/identity/password-spray.kql) | T1110.003 | High | Low-and-slow password spray against many accounts |

### Endpoint
| Rule | MITRE | Severity | Description |
|------|-------|----------|-------------|
| [Suspicious PowerShell](detections/endpoint/suspicious-powershell.kql) | T1059.001 | Medium | Encoded commands, download cradles, AMSI bypass |

### Cloud
| Rule | MITRE | Severity | Description |
|------|-------|----------|-------------|
| [Anomalous Azure Sign-in](detections/cloud/anomalous-azure-signin.kql) | T1078.004 | High | New-country sign-ins and legacy auth (MFA bypass) |

### Network
| Rule | MITRE | Severity | Description |
|------|-------|----------|-------------|
| [DNS Tunneling](detections/network/dns-tunneling.kql) | T1048.003 | Medium | High-entropy subdomains and abnormal query volumes |
| [VPN Internal Scanning](detections/network/vpn-internal-scanning.kql) | T1046 | High | VPN-authenticated sources scanning internal hosts/ports |
| [TOR Node Communication](detections/network/tor-node-communication.kql) | T1090.003 | High | Allowed traffic to/from live TOR exit and relay nodes |

### Lateral Movement
| Rule | MITRE | Severity | Description |
|------|-------|----------|-------------|
| [PsExec Detection](detections/lateral-movement/psexec-detection.kql) | T1569.002 | High | PSEXESVC service creation and remote execution |

---

## 🏹 Hunting Queries


### Endpoint
| Query | MITRE | Description |
|-------|-------|-------------|
| [SMB Share High-Frequency Access](hunting-queries/endpoint/smb-share-high-frequency.kql) | T1021.002 | Abnormal SMB share access — ransomware or data staging |
| [Shadow Copy & Recovery Manipulation](hunting-queries/endpoint/shadow-copy-manipulation.kql) | T1490 | VSS deletion and boot recovery tampering — ransomware precursor |

### Network
| Query | MITRE | Description |
|-------|-------|-------------|
| [Risky Base64 Commands in URL](hunting-queries/network/risky-base64-in-url.kql) | T1071 | Base64-encoded OS commands in web requests — webshell C2 |
| [New Destination IP](hunting-queries/network/new-destination-ip.kql) | T1071, TA0043 | Outbound connections to IPs not seen in 30-day baseline |
| [Base64-Encoded IPv4 in URL](hunting-queries/network/base64-encoded-ipv4-in-url.kql) | T1071 | Obfuscated C2 IPs encoded in request URLs |

---

## 🗺️ MITRE ATT&CK Coverage

| Tactic | Techniques |
|--------|-----------|
| Reconnaissance | TA0043, T1595 |
| Initial Access | T1078, T1078.004 |
| Execution | T1059.001 |
| Discovery | T1046, T1082, T1482, T1069.002, T1087.002 |
| Lateral Movement | T1021.002, T1569.002 |
| Credential Access | T1110.003 |
| Command and Control | T1071, T1090.003 |
| Exfiltration | T1048.003 |
| Impact | T1490 |

---

## 📐 Detection Methodology

All rules follow a structured development process. See [docs/detection-methodology.md](docs/detection-methodology.md) for the full lifecycle, KQL best practices, and severity classification guide.

---

## 🚀 Usage

Each `.kql` file is self-contained and ready to paste into:
- **Detections** → Microsoft Sentinel → Analytics → New Scheduled Query Rule
- **Hunting Queries** → Microsoft Sentinel → Hunting → New Query

Tunable parameters are exposed as `let` variables at the top of each file.

---

## 📜 License

MIT — free to use, adapt, and build on. Attribution appreciated.

---

*Built from real detections written in a production SOC environment. Connect with me on [LinkedIn](https://linkedin.com).*
