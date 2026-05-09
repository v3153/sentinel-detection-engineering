# Detection Engineering Methodology

This document outlines the detection lifecycle followed in this repository.

---

## Detection Lifecycle

```
Threat Intelligence → Hypothesis → Rule Development → Testing → Tuning → Production
        ↑                                                                      |
        └──────────────────── Continuous Review ──────────────────────────────┘
```

### 1. Threat Intelligence Input
- MITRE ATT&CK technique analysis
- Threat actor TTPs (from ISACs, threat reports, CTI feeds)
- Incident post-mortems and purple team findings

### 2. Hypothesis Formation
Every detection starts with a hypothesis:
> *"If an attacker performs [technique], they will leave [observable evidence] in [data source]."*

### 3. Rule Development Standards
All rules must include:
- MITRE ATT&CK tactic and technique mapping
- Severity classification (High / Medium / Low / Informational)
- Documented false positive scenarios
- Tuning parameters (exposed as `let` variables at the top)
- Performance-conscious query design (time bounds, selective joins)

### 4. Testing Checklist
- [ ] Tested against synthetic/lab data
- [ ] Validated against known-good activity (false positive check)
- [ ] Performance reviewed (query runtime < 30s on 7-day window)
- [ ] Peer reviewed via GitHub PR

### 5. Tuning in Production
- Monitor alert volume weekly for first 30 days
- Document suppression decisions with rationale
- Re-evaluate after major environment changes

---

## KQL Best Practices

| Practice | Reason |
|----------|--------|
| Always bound queries with `TimeGenerated > ago(Xd)` | Prevents full table scans |
| Use `let` for configurable thresholds | Enables environment-specific tuning |
| Project only needed columns | Reduces data transfer and cost |
| Prefer `summarize` over raw event streaming | Reduces alert volume |
| Use `make_set()` for sample data in alerts | Preserves context without duplication |

---

## Severity Classification

| Severity | Meaning | Expected Response Time |
|----------|---------|----------------------|
| High | Direct indicator of compromise or imminent threat | 15 minutes |
| Medium | Suspicious activity requiring investigation | 1 hour |
| Low | Informational / weak signal, investigate in batch | 24 hours |
| Informational | Audit/compliance only, no active response needed | Weekly review |

---

## MITRE ATT&CK Coverage Approach

Detection priority follows a **crown jewel + kill chain** model:

1. **Initial Access** — Detect adversary entry (phishing, valid accounts, public exploits)
2. **Credential Access** — Detect credential theft before lateral movement
3. **Lateral Movement** — Detect internal spread before objectives are reached
4. **Exfiltration** — Detect data leaving the environment

This prioritizes earlier-stage detection to reduce dwell time.
