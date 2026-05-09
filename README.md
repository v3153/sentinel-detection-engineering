# 🛡️ Sentinel Detection Engineering

> Production-grade KQL detection rules and threat hunting queries for Microsoft Sentinel — built by a Detection Engineer with hands-on SOC experience.

[![Sentinel](https://img.shields.io/badge/Microsoft%20Sentinel-Compatible-0078D4?logo=microsoft-azure)](https://azure.microsoft.com/en-us/products/microsoft-sentinel)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Detections](https://img.shields.io/badge/Detection%20Rules-20%2B-brightgreen)](detections/)

---

## 👤 About This Repository

This repo contains **20+ battle-tested KQL detection rules** used in a real SOC environment, organized by MITRE ATT&CK tactic. Each rule includes:

- Documented detection logic and rationale
- MITRE ATT&CK mapping
- Severity classification
- Known false positive guidance
- Tuning recommendations (exposed as `let` variables)

**My background:**
- Security Engineer @ Toyota Tsusho Systems India
- Microsoft Certified: Security Operations Analyst (SC-200)
- CVE-2024-9495 discoverer
- Certified Blue Team Professional (CBTP)

📖 Detection engineering write-ups: [Medium](https://medium.com)

---

## 📁 Repository Structure

```
sentinel-detection-engineering/
├── detections/                   # Scheduled analytics rules (KQL)
│   ├── identity/                 # Account compromise, auth anomalies
│   ├── endpoint/                 # Process, script, and file activity
│   ├── network/                  # DNS, firewall, proxy anomalies
│   ├── cloud/                    # Azure AD, M365, resource abuse
│   └── lateral-movement/         # Internal spread techniques
├── hunting-queries/              # Proactive threat hunting KQL
│   └── identity/
└── docs/                         # Detection methodology & notes
```

---

## 🔍 Detection Rules

### Identity & Access
| Rule | MITRE Tactic | Severity | Description |
|------|-------------|----------|-------------|
| [Impossible Travel](detections/identity/impossible-travel.kql) | Initial Access (T1078) | High | Sign-ins from geographically impossible locations |
| [Password Spray](detections/identity/password-spray.kql) | Credential Access (T1110.003) | High | Low-and-slow password spray against many accounts |

### Endpoint
| Rule | MITRE Tactic | Severity | Description |
|------|-------------|----------|-------------|
| [Suspicious PowerShell](detections/endpoint/suspicious-powershell.kql) | Execution (T1059.001) | Medium | Encoded commands, download cradles, AMSI bypass |

### Cloud
| Rule | MITRE Tactic | Severity | Description |
|------|-------------|----------|-------------|
| [Anomalous Azure Sign-in](detections/cloud/anomalous-azure-signin.kql) | Initial Access (T1078.004) | High | New-country sign-ins and legacy auth (MFA bypass) |

### Network
| Rule | MITRE Tactic | Severity | Description |
|------|-------------|----------|-------------|
| [DNS Tunneling](detections/network/dns-tunneling.kql) | Exfiltration (T1048.003) | Medium | High-entropy subdomains and abnormal query volumes |

### Lateral Movement
| Rule | MITRE Tactic | Severity | Description |
|------|-------------|----------|-------------|
| [PsExec Detection](detections/lateral-movement/psexec-detection.kql) | Lateral Movement (T1569.002) | High | PSEXESVC service creation and remote execution |

---

## 🏹 Hunting Queries

| Query | MITRE | Use Case |
|-------|-------|----------|
| [Dormant Account Activation](hunting-queries/identity/dormant-account-activation.kql) | T1078 | Re-activated stale accounts — account takeover indicator |
| [Privileged Account Enumeration](hunting-queries/identity/privileged-account-enumeration.kql) | T1069.002, T1087.002 | LDAP/AD recon targeting admin groups |

---

## 🗺️ MITRE ATT&CK Coverage

| Tactic | Techniques Covered |
|--------|--------------------|
| Initial Access | T1078, T1078.004 |
| Credential Access | T1110.003 |
| Execution | T1059.001 |
| Lateral Movement | T1569.002 |
| Exfiltration | T1048.003 |
| Discovery | T1069.002, T1087.002 |

---

## 📐 Detection Methodology

All rules follow a structured development process. See [docs/detection-methodology.md](docs/detection-methodology.md) for the full lifecycle, KQL best practices, and severity classification guide.

---

## 🚀 Usage

Each `.kql` file is self-contained and ready to paste into:
- **Microsoft Sentinel** → Analytics → New Scheduled Query Rule
- **Microsoft Sentinel** → Hunting → New Query

Tunable parameters are exposed as `let` variables at the top of each file.

---

## 📜 License

MIT — free to use, adapt, and build on. Attribution appreciated.

---

*Built from real detections written in a production SOC environment. Connect with me on [LinkedIn](https://linkedin.com).*
