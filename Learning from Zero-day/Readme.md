# 🛡️ BlueHammer, RedSun & UnDefend  
### Advanced Exploitation of Microsoft Defender (2025)

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
```

SAM
SYSTEM
SECURITY

```
6. Reconstruct boot key & decrypt hashes  
7. Escalate to SYSTEM  

### 💥 Impact
- Full credential compromise
- Persistent privilege escalation
- Stealthy execution via legitimate processes

---

## 🔴 RedSun

### 🎯 Objective
Achieve **privileged file write → SYSTEM execution**

### ⚙️ Root Cause
Missing **reparse point validation** in:
```

MpSvc.dll

```

### 🔗 Attack Flow
1. Drop EICAR payload  
2. Trigger Defender remediation  
3. Lock file using OPLOCK  
4. Replace path with junction → `C:\Windows\System32`  
5. Defender writes payload as SYSTEM  
6. Execute via COM:
```

CLSID {50d185b9-fff3-4656-92c7-e4018da4361d}

```

### 💥 Impact
- Direct SYSTEM shell
- No exploit needed beyond logic flaw
- Fully trusted write operation

---

## 🟡 UnDefend

### 🎯 Objective
Disable Defender updates **without privilege escalation**

### ⚙️ Techniques Used

#### 1. Backup Lock
- Locks:
```

mpavbase.vdm
mpavbase.lkg

```

#### 2. Update Race Condition
- Blocks Defender read access during update

#### 3. Service Restart Hook
- Monitors `WinDefend` restart
- Locks definitions immediately

#### 4. MRT Lockout
- Blocks:
```

C:\Windows\System32\MRT

```

### 💥 Impact
- Defender updates fail silently
- No alerts triggered
- Endpoint remains unprotected

---

## 🔗 Attack Chain Mapping

```

Initial Access (VPN compromise)
↓
Execution (EICAR trigger)
↓
Defense Evasion (UnDefend)
↓
Privilege Escalation (RedSun / BlueHammer)
↓
Credential Access (BlueHammer)
↓
Persistence (SYSTEM context)

```

---

## 🧠 MITRE ATT&CK Mapping

| Technique                  | ID        |
|--------------------------|----------|
| Privilege Escalation     | T1068    |
| Credential Dumping       | T1003    |
| Defense Evasion          | T1562    |
| File/Directory Abuse     | T1106    |
| Exploitation for Priv    | T1068    |

---

## 🔍 Detection Opportunities

### 🛑 High-Fidelity Signals

- Defender writing to:
```

C:\Windows\System32\

```
- Access to:
```

\Device\HarddiskVolumeShadowCopy*

```
- Suspicious Cloud Files API usage:
- `CfRegisterSyncRoot`
- `CfCreatePlaceholders`

### 🧩 Behavioral Indicators

- EICAR-triggered scan followed by SYSTEM activity  
- Named pipe communication:
```

\.\pipe\REDSUN

```
- OPLOCK usage patterns  
- Unexpected COM activation chains  

---

## 📎 References

- Huntress Labs Research  
- Microsoft Defender Internals  
- Windows Cloud Files API Documentation  

---

## ⚠️ Disclaimer

This repository is intended for:
- Security research  
- Detection engineering  
- Defensive use only  

Do **not** use these techniques in unauthorized environments.

---

## 👤 Author

**Nayan**  
Cybersecurity & Cloud Security Architect  

