# 🛡️ BlueHammer, RedSun & UnDefend  
### Advanced Exploitation of Microsoft Defender (2026)

![Status](https://img.shields.io/badge/status-research-blue)
![Threat Level](https://img.shields.io/badge/threat-critical-red)
![Platform](https://img.shields.io/badge/platform-Windows-lightgrey)
![MITRE](https://img.shields.io/badge/MITRE-Privilege%20Escalation-orange)
![License](https://img.shields.io/badge/license-Internal%20Use-yellow)

---

## 📌 Overview

This repository provides a technical breakdown of three advanced exploitation techniques targeting **Microsoft Defender**:

- **BlueHammer** → Privileged File Read → Credential Theft  
- **RedSun** → Privileged File Write → SYSTEM Execution  
- **UnDefend** → Defender Disablement (No Privilege Required)

All techniques were observed in **real-world intrusions** and demonstrate how legitimate Windows components can be chained for exploitation.

---

## 📚 Table of Contents

- [Overview](#-overview)
- [Attack Summary](#-attack-summary)
- [BlueHammer](#-bluehammer)
- [RedSun](#-redsun)
- [UnDefend](#-undefend)
- [Attack Chain Mapping](#-attack-chain-mapping)
- [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
- [Detection Opportunities](#-detection-opportunities)
- [References](#-references)

---

## ⚠️ Attack Summary

| Technique   | Impact                     | Privilege Needed | Outcome                     |
|------------|--------------------------|-----------------|-----------------------------|
| BlueHammer | Read sensitive registry   | Low             | NTLM Hash Extraction        |
| RedSun     | Write to System32         | Low             | SYSTEM Execution            |
| UnDefend   | Block AV updates          | None            | Defender Blindness          |

---

## 🔵 BlueHammer

### 🎯 Objective
Extract **NTLM hashes** from protected registry hives using Defender behavior.

### ⚙️ Key Techniques
- Volume Shadow Copy abuse
- Cloud Files API manipulation
- Opportunistic locking (OPLOCK)
- Defender update workflow hijack

### 🔗 Attack Flow
1. Detect pending Defender update  
2. Trigger Defender scan using EICAR  
3. Create Volume Shadow Copy  
4. Freeze Defender via Cloud Filter API  
5. Extract:
