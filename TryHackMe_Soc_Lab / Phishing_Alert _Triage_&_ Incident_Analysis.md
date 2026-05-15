# 🛡️ SOC Investigation: Phishing & Incident Response Analysis
**Analyst:** TheGentlePotato  
**Specialization:** Cybersecurity (Blue Team Operations)  
**Lab Platform:** TryHackMe SOC Level 1  

---

## 📋 Executive Summary
This project demonstrates a full-lifecycle investigation of multiple security alerts within a simulated Security Operations Center (SOC). By correlating **Splunk (SIEM)** logs with **VirusTotal (Threat Intel)**, I triaged events to distinguish between legitimate business processes and active cyber threats. 

The investigation resulted in:
* **1 False Positive (FP)** identified through internal log correlation.
* **1 Blocked True Positive (TP)** involving SEO poisoning and shortened URLs.
* **1 Critical True Positive (TP)** involving a successful credential harvesting attempt via typosquatting.

---

## 🚨 Alert 8814: Suspicious Inbound Email (HR Onboarding)
**Verdict:** ✅ False Positive (FP)

### 🔍 Analysis & Evidence
The SIEM flagged an email from `onboarding@hrconnex.thm`. While external domains often trigger phishing alerts, a deep dive into Splunk revealed that this was a legitimate business exception.

* **Investigation:** I queried Splunk for internal IT communications.
* **Findings:** Discovered an announcement confirming **HRConnex** as a new authorized partner.

![Alert 8814 Email](TryHackMe_Soc_Lab%20/%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8814.jpg)
![Alert 8814 Splunk Verification](TryHackMe_Soc_Lab%20/%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8814.jpg)

---

## 🚨 Alert 8815 & 8816: Inbound Phishing & Execution Chain
**Verdict:** ⚠️ True Positive (TP) - Blocked

### 🔍 Analysis & Evidence
This case displayed a classic "Attack Chain." Alert 8815 caught the email delivery, while Alert 8816 caught the user actually clicking the malicious link.

1.  **Delivery (8815):** A fake Amazon delivery notification using an urgent lure.
2.  **Execution (8816):** The user searched for "payroll systems" and clicked a malicious `bit.ly` link (SEO Poisoning).
3.  **Defense:** The corporate firewall successfully intercepted the request.

#### Phase 1: Email Delivery (8815)
![Alert 8815 Email](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8815.jpg)
![Alert 8815 Splunk Search](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8815_Splunk.jpg)
![Alert 8815 VirusTotal Check](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8815_VirusTotal.jpg)

#### Phase 2: User Interaction (8816)
![Alert 8816 Web Alert](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8816.jpg)
![Alert 8816 Splunk Traffic Log](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8816_Splunk.jpg)
![Alert 8816 VirusTotal Result](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8816_VirusTotal.jpg)

---

## 🚨 Alert 8817: Typosquatting & Successful Breach (Microsoft)
**Verdict:** 🔥 True Positive (TP) - **CRITICAL**

### 🔍 Analysis & Evidence
This was a high-severity incident. The attacker utilized **Typosquatting** (`m1crosoftsupport.co`) to trick the user into thinking the email was a security alert from Microsoft.

* **The Gap:** Unlike the previous cases, the firewall action was **"ALLOWED."** * **Impact:** The user reached the malicious destination, indicating a high probability of credential theft.

![Alert 8817 Email Lure](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8817.jpg)
![Alert 8817 Splunk Allowed Traffic](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8817_Splunk.jpg)
![Alert 8817 VirusTotal Malicious Flag](TryHackMe_Soc_Lab/images/Phishing_Alert_Triage_&_Incident_Response/Alert_8817_VirusTotal.jpg)

---

## 🎓 Technical Lessons Learned

### 1. SIEM Proficiency (Splunk)
I learned that an alert is only a starting point. By using **Splunk**, I mastered:
* **Pivoting:** Moving from a suspicious email address to searching for associated network traffic (IPs/URLs).
* **Action Analysis:** Differentiating between "Blocked" (threat neutralized) and "Allowed" (active breach) logs.

### 2. Threat Intelligence Integration (VirusTotal)
I utilized **VirusTotal** to provide objective proof for my verdicts:
* **Domain Reputation:** Identifying malicious shortened URLs and typosquatted domains.
* **Community Consensus:** Leveraging vendor analysis to confirm phishing landing pages.

### 3. Reporting & Classification
I developed a standardized method for explaining results based on the verdict:

| Metric | False Positive (FP) | True Positive (TP) |
| :--- | :--- | :--- |
| **Primary Goal** | Minimize "Alert Fatigue" by identifying legitimate traffic. | Mitigate damage and identify the "Kill Chain" stage. |
| **Documentation** | Focus on the business context (e.g., authorized partners). | Focus on IOCs (IPs/URLs) and remediation steps. |
| **Action** | White-list or tune the alert rules. | Escalate for password resets and host isolation. |

---
