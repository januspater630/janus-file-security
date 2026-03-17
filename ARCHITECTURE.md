# Janus File Security System – Architecture Overview

## 1. System Overview
Janus is a local-first file security system that combines **strong per-file encryption** with **runtime adaptive protection**.  
It is designed to increase the cost of unauthorized access, reduce accidental data exposure, and provide auditability of sensitive operations.

---

## 2. Core Modules

| Module | Responsibilities |
|--------|-----------------|
| **KeyManager** | Derives master and per-file keys using PBKDF2 (password + device fingerprint) and HKDF. Stores master key securely. |
| **Workspace** | Manages encrypted file storage (AES-256-GCM). Each file has a unique key and encrypted metadata. |
| **AttackDetector** | Monitors abnormal behaviors (failed logins, debuggers, VM detection) and calculates a dynamic threat level (0–1). |
| **EnergyShield** | Introduces dynamic delays based on threat level to increase attack cost exponentially. |
| **Fuse** | Automatically switches system state (normal / degraded / hibernating / self-destruct) based on threat assessment. |
| **TraceChain** | Maintains a hashed, append-only log of sensitive operations for audit and post-event analysis. |
| **ConfigManager** | Manages encrypted configuration files. |
| **License** | Binds system to a specific device (MAC + hostname fingerprint) and validates on startup. |
| **Trial** | Manages 365-day trial period, records first run time, and locks system after expiration. |

---

## 3. Runtime Threat-Adaptive Loop

1. **Operation Monitoring**  
   - All interactions are monitored by `AttackDetector`.  

2. **Threat Assessment**  
   - Threat level is calculated dynamically (0 = safe, 1 = high risk).  

3. **Adaptive Response**  
   - `EnergyShield` adds operation delays.  
   - `Fuse` triggers state changes if thresholds are exceeded.  
   - `TraceChain` logs all sensitive operations.

4. **Outcome**  
   - Unauthorized attempts are slowed or blocked.  
   - Legitimate use remains smooth under normal conditions.

---

## 4. Security Principles

- **Per-file encryption** ensures compromise of one file does not affect others.  
- **Device-bound keys** prevent decryption on unauthorized machines.  
- **Runtime adaptive protection** increases attack cost without preventing legitimate use.  
- **Auditability** allows tracking and analysis of security events.

---

## 5. Future Roadmap

- **v9.0 (Enterprise)**: Remote authorization, multi-user support, key rotation.  
- **v10+ (Hardened Core)**: Native implementation (Rust/C++), memory encryption, side-channel attack mitigation.

---

## 6. Usage Scenarios

- Personal file protection  
- Small team document security  
- Offline, local-first data storage  
- Demonstration of adaptive security concepts
