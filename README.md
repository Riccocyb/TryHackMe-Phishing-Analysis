# Phishing Email Analysis - TryHackMe Lab

This repository documents the technical methodology and analytical mindset used to investigate a suspicious email during the "Phishing Emails in Action" room on TryHackMe.

## 🎯 Objective
The goal of this analysis was to dissect a suspected phishing email, verify the sender's authenticity, extract potential Indicators of Compromise (IoCs), and understand the attacker's underlying tactics.

---

## 🔍 Technical Investigation

### 1. Source Code & HTML Analysis
By inspecting the raw HTML source code of the email, I identified two critical social engineering and evasion techniques used by the attacker:

* **Hyperlink Discrepancy (Link Spoofing):** The display text was crafted to deceive the victim into believing it was a legitimate package tracking code (`Track your package: # LZ8942357486EN`). However, the actual destination URL (`href` attribute) pointed to a completely unrelated and suspicious external domain.
* **Tracking Pixel:** The attacker embedded a hidden image (`Tracking.png`). This technique triggers a silent request back to the attacker's server as soon as the victim opens the email, confirming that the email address is valid and active.

<img width="1676" height="842" alt="phishing" src="https://github.com/user-attachments/assets/c7d29762-e0b5-4c90-b792-c29acec07e84" />


---

### 2. Defanged URL & CyberChef Workflow
To document the malicious domain safely within this report without risking accidental clicks that could trigger malicious behavior, I used **CyberChef** to defang the URL.

* **Original Domain Found:** `devret.xyz`
* **Tool Used:** CyberChef
* **Operation Applied:** `Defang URL`
* **Secure Output (IoC):** `[REDACTED]`

By replacing the standard periods with brackets, the link becomes non-clickable, ensuring the safety of analysts and infrastructure reviewing this documentation.

<img width="1862" height="921" alt="cyberchef" src="https://github.com/user-attachments/assets/118680a0-5ab7-4325-803a-9e901dd949b8" />



---

## 🚨 Indicators of Compromise (IoCs)
* **Attacker Root Domain:** `[REDACTED]`

## 🧠 Key Takeaways
Through this lab, I practically demonstrated how to:
1. Analyze and interpret raw HTML structures in email headers and bodies.
2. Detect link spoofing and tracking mechanisms utilized in modern phishing campaigns.
3. Leverage standard industry tools like CyberChef to safely sanitize threat indicators.





# 🛡️ Digital Investigation & Sandbox Analysis Methodology

## 📌 Overview
This documentation outlines the technical workflow and methodology for conducting dynamic malware analysis within an interactive cloud-based sandbox environment (**ANY.RUN**). The focus of this session was to observe the runtime behavior, process execution tree, and network indicators of an untrusted document artifact mimicking a standard business file.

---

## 🛠️ Environment & Tools
* **Interactive Sandbox Platform:** Used for detonating the payload in an isolated, monitored environment to trace system-level modifications.
* **Process Monitor / Behavioral Engine:** Utilized to map the process hierarchy and detect suspicious system calls or application manipulation.

---

## 🔍 Behavioral Investigation Methodology

When an untrusted document (such as a weaponized PDF) is executed inside a controlled environment, the investigation is broken down into distinct analytical phases:

### Phase 1: Process Tree & Hierarchy Analysis
Malicious documents rarely operate within a single process. Instead, they leverage chain-of-execution tactics:
* **Initial Spawn:** The primary application (e.g., the PDF reader) is launched to open the file.
* **Child Processes:** The main application spawns sub-processes or secondary renderers. Security analysts monitor these to detect if the application is being used to launch external, unauthorized Windows features or binary code injection.

### Phase 2: Registry & System Fingerprinting (MITRE ATT&CK Triage)
Automated sandbox indicators track behavior that matches known adversary tactics:
* **T1012 - Query Registry:** Checking system certificates, trust settings, or configurations to evaluate the system environment.
* **T1518 - Software Discovery:** Searching for installed security applications or virtualization artifacts to determine if the payload is running inside an analyst's sandbox (anti-analysis technique).
* **T1082 - System Information Discovery:** Querying core properties like hostnames or architecture layouts.

---

## 🌐 Network Indicators & Infrastructure Mapping

Monitoring outbound traffic during detonation is critical for identifying potential Command and Control (C2) infrastructure or second-stage payload delivery.

### Network Traffic Auditing Structure

| Protocol | Source Process | Target Destination | Domain / Infrastructure | Security Classification |
| :--- | :--- | :--- | :--- | :--- |
| `TCP` | Legitimate Binary | External Host | Vendor Domain (e.g., Adobe/Microsoft) | Legitimate (CRL/Trust Verification) |
| `TCP / HTTP` | Suspicious Process | Remote IP | Unknown / Newly Registered Domain | High Risk (Potential Exfiltration/C2) |

*Analyst Note: It is critical to distinguish between benign, automated background traffic generated by the software (like checking for updates) and anomalous connections initiated by child processes.*

<img width="1492" height="742" alt="anyrun" src="https://github.com/user-attachments/assets/6912db48-26d2-4d91-ae29-14edaac5418c" />


## 💡 Key Defensive Security Takeaways

* **The Danger of `%TEMP%` Execution:** Malicious macros or PDF exploits frequently drop their active payloads into the local hidden directory (`\AppData\Local\Temp\`) to run outside standard installation paths. Implementing strict application control policies (e.g., AppLocker) to block unauthorized execution from temporary folders is a highly recommended defensive mitigation.
* **Living off the Land (LotL):** Sophisticated malware masquerades inside legitimate system processes to bypass traditional signature-based security. Security teams must rely on behavioral monitoring and Endpoint Detection and Response (EDR) telemetry to catch anomalies in process lineage (e.g., a PDF reader spawning an administrative utility).
