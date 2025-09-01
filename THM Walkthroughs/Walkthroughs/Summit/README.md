# TryHackMe — Summit Walkthrough
**Platform:** [TryHackMe — Summit](https://tryhackme.com)  
**Description:**  
This walkthrough covers the *Summit* room on TryHackMe, where the objective is to chase a simulated adversary up the Pyramid of Pain by detecting malware indicators and configuring defenses.

---

## Challenge Overview
PicoSecure is conducting a threat simulation and detection engineering engagement. The goal is to configure detection tools against a penetration tester running malware samples, following the **Pyramid of Pain** model.

Each task builds detection mechanisms (hashes, IPs, domains, registry events, Sigma rules, etc.) to increase adversary costs until they are stopped.

---

## Room Prerequisites
Recommended rooms before attempting:
- **The Pyramid of Pain**
- **MITRE ATT&CK**

---

## Flags

### Flag 1 — sample1.exe
**Objective:** Detect using the SHA256 hash.  

**Steps:**
1. Analyze `sample1.exe` in Malware Sandbox.  
2. Copy SHA256 hash.  
3. Add it to the Hash Blocklist.  

**Answer:**  
THM{f3cbf08151a11a6a331db9c6cf5f4fe4}

---

### Flag 2 — sample2.exe
**Objective:** Block network traffic by detecting malicious IP.  

**Steps:**
1. Analyze `sample2.exe` in Malware Sandbox.  
2. Identify suspicious IP: `154.35.10.113`.  
3. Block it via Firewall Manager (Egress → Destination IP).  

**Answer:**  
THM{2ff48a3421a938b388418be273f4806d}

---

### Flag 3 — sample3.exe
**Objective:** Block malware domain.  

**Steps:**
1. Analyze `sample3.exe` in Malware Sandbox.  
2. Identify malicious domain: `emudyn.bresonicz.info`.  
3. Add it to DNS Filter → Deny rule.  

**Answer:**  
THM{4eca9e2f61a19ecd5df34c788e7dce16}

---

### Flag 4 — sample4.exe
**Objective:** Detect registry modification disabling Defender.  

**Steps:**
1. Analyze `sample4.exe` behavior.  
2. Registry modification found:  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection
Name: DisableRealtimeMonitoring
Value: 1
3. Create Sigma rule (Sysmon Event Logs → Registry Modifications).  
4. Map to MITRE ATT&CK `Defense Evasion (TA0005)`.  

**Answer:**  
THM{c956f455fc076aea829799c0876ee399}

---

### Flag 5 — sample5.exe
**Objective:** Detect beaconing traffic pattern.  

**Steps:**
1. Analyze `outgoing_connections.log`.  
2. Malicious pattern found:  
   - Destination IP: `51.102.10.19`  
   - Port: `443`  
   - Packet size: `97 bytes`  
   - Interval: every 30 min  
3. Create Sigma rule for detection.  

**Answer:**  
THM{46b21c4410e47dc5729ceadef0fc722e}

---

### Final Flag
**Objective:** Detect data exfiltration attempt.  

**Steps:**
1. Analyze `commands.log`.  
2. Malware creates file in `%temp%\exfiltr8.log`.  
3. Create Sigma rule:  
   - File Creation/Modification  
   - Path: `%temp%`  
   - File: `exfiltr8.log`  
   - ATT&CK: `Exfiltration (TA0010)`  

**Answer:**  
THM{c8951b2ad24bbcbac60c16cf2c83d92c}

---

## ✅ Conclusion
This room simulates real-world malware detection and demonstrates how defenders can escalate adversary costs by detecting indicators across the **Pyramid of Pain**:
- Hashes → IPs → Domains → Registry → Behavior → Exfiltration.  
