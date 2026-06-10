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
