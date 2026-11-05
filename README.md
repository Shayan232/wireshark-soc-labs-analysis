# wireshark-soc-labs-analysis

# 📡 Wireshark SOC Labs — Network Traffic Analysis & Threat Forensics

![Wireshark](https://img.shields.io/badge/Tool-Wireshark-1679A7?style=for-the-badge&logo=wireshark&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-NetworkWalks-blue?style=for-the-badge)
![Score](https://img.shields.io/badge/Score-100%25%20Outstanding-success?style=for-the-badge)

## 📌 Executive Summary
This repository documents the complete hands-on completion and detailed analysis of **Wireshark SOC Labs** hosted on **NetworkWalks**. The scenarios simulate real-world Security Operations Center (SOC) incident investigation techniques, covering packet capture analysis, network threat detection, protocol inspection, unencrypted credential recovery, and voice-over-IP (VoIP) forensics.

---

## 🛠️ Labs & Key Findings

### 🔹 Lab 3: Identify the Attacker IP & Port Scan Detection
* **Objective:** Detect network reconnaissance and determine open vs. closed target ports.
* **Filter Used:** `tcp.flags.syn == 1`
* **Attacker IP:** `192.168.10.66`
* **Target IP:** `192.168.10.20`
* **Key Findings:**
  * Attacker initiated connection attempts across 40 distinct destination ports using source port `52311`.
  * Open ports (`SYN, ACK` responses): `22, 80, 443` (3 total).
  * Closed ports (`RST, ACK` responses): 37 total.
* **Attack Classification:** Port Scanning (Reconnaissance).

---

### 🔹 Lab 4: Follow TCP Stream — Plain-Text Chat Inspection
* **Objective:** Reconstruct cleartext TCP communication streams.
* **Stream Analyzed:** `tcp.stream eq 5` (Port 5000)
* **Initiator IP:** `192.168.10.45`
* **Extracted Artifacts:**
  * **Staff Member:** Ali
  * **Inaccessible Resource:** Finance Drive
  * **Temporary Password Leaked:** `Tiger#4821`
  * **Guest Wi-Fi Password Leaked:** `BlueHarbor77`
  * **Payload Size:** 413 bytes

---

### 🔹 Lab 5: Follow TCP Stream Part 2 — Intranet & Secret Inspection
* **Objective:** Locate hidden metadata and cleartext file downloads across HTTP traffic streams.
* **Key Findings:**
  * Identified hidden comment inside HTML source of `/staff.html` (TCP Stream 7).
  * **Extracted Access Code:** `Falcon-9921`
  * **Backup Server IP:** `192.168.10.77`
  * **HTTP Status Code:** `200 OK`
  * **Downloaded File:** `Report.txt` (Authored by Nadia).

---

### 🔹 Lab 6: HTTP Plain-Text Credentials Sniffing
* **Objective:** Capture unencrypted authentication POST requests.
* **Protocol & Method:** HTTP / `POST` to `/login`
* **Client IP:** `192.168.10.45`
* **Captured Credentials:**
  * **Username:** `Farhan.k`
  * **Password:** `Mango#2026`
* **Location in Packet Details:** `HTML Form URL Encoded`
* **Remediation:** Enforce **HTTPS (TLS/SSL)** across all authentication pathways to prevent credential interception.

---

### 🔹 Lab 7: FTP Brute-Force Attack Catch
* **Objective:** Detect brute-force login attempts against an FTP server.
* **Target Server IP:** `192.168.10.20` | **Attacker IP:** `192.168.10.66`
* **Target Account:** `Admin`
* **Key Findings:**
  * Tracked 24 failed login attempts returning FTP Response Code `530` (*Login incorrect*).
  * Identified 1 successful login returning FTP Response Code `230` (*User logged in*).
  * **Cracked Password:** `Sunshine99`
* **Attack Classification:** Dictionary / Brute-Force Attack.

---

### 🔹 Lab 8: VoIP Call Replay & Audio Forensics
* **Objective:** Capture and reconstruct unencrypted voice traffic.
* **Control Protocol:** SIP (Session Initiation Protocol — `INVITE`, `Ringing`, `200 OK`)
* **Transport Protocol:** RTP (Real-Time Transport Protocol)
* **Voice Codec:** `PCMU` (G.711 mu-law)
* **Extracted Intel:**
  * **Caller Extension:** `101` (Ali Raza) $\rightarrow$ **Recipient:** Sara
  * **Exposed Credit Card (Last 4 digits):** `2394` | **Expiry:** `09/28`
* **Remediation:** Upgrade to **SRTP (Secure Real-time Transport Protocol)** and **SIPS** to encrypt audio payloads and call signaling.

---

## 🏆 Lab Completion Certificates

| Lab | Scenario | Result | Score |
| :--- | :--- | :---: | :---: |
| **Lab 3** | Identify the Attacker IP | Verified | 100% |
| **Lab 4** | Follow TCP Stream | Verified | 100% |
| **Lab 5** | Follow TCP Stream Part 2 | Verified | 100% |
| **Lab 6** | HTTP Plain-Text Credentials | Verified | 100% |
| **Lab 7** | Brute Force Catch | Verified | 100% |
| **Lab 8** | VoIP Call Replay | Verified | 100% |

---

## 🔒 Recommended Security Controls & Remediations
1. **Enforce Transport Security:** Upgrade HTTP endpoints to **HTTPS** and VoIP infrastructure to **SRTP** to mitigate eavesdropping and packet sniffing.
2. **Account Lockout Policies:** Implement rate limiting and automatic lockouts after multiple failed FTP/SSH login attempts to prevent brute-force attacks.
3. **Network Segmentation & Monitoring:** Deploy Intrusion Detection Systems (IDS) configured with alert signatures for rapid `SYN` scan bursts and high-frequency port probes.

---
*Completed as part of SOC Practical Training on [NetworkWalks](https://www.networkwalks.com).*
