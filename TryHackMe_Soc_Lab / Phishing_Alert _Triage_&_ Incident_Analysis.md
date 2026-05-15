# 🛡️ SOC Investigation: Phishing & Incident Response Analysis
**Analyst:** Pang Xing Thong (TheGentlePotato)  
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

<table width="100%">
  <tr>
    <td width="50%"><b>Email Lure</b></td>
    <td width="50%"><b>Splunk Verification</b></td>
  </tr>
  <tr>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8814.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8814_Splunk.jpg" width="100%"></td>
  </tr>
</table>

---

## 🚨 Alert 8815 & 8816: Inbound Phishing & Execution Chain
**Verdict:** ⚠️ True Positive (TP) - Blocked

### 🔍 Analysis & Evidence
This case displayed a classic "Attack Chain." Alert 8815 caught the email delivery, while Alert 8816 caught the user actually clicking the malicious link.

1. **Delivery (8815):** A fake Amazon delivery notification using an urgent lure.
2. **Execution (8816):** The user searched for "payroll systems" and clicked a malicious `bit.ly` link.
3. **Defense:** The corporate firewall successfully intercepted the request.

#### Phase 1: Email Delivery (Alert 8815)
<table width="100%">
  <tr>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8815.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8815_Splunk.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8815_VirusTotal.jpg" width="100%"></td>
  </tr>
</table>

#### Phase 2: User Interaction (Alert 8816)
<table width="100%">
  <tr>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8816.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8816_Splunk.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8816_VirusTotal.jpg" width="100%"></td>
  </tr>
</table>

---

## 🚨 Alert 8817: Typosquatting & Successful Breach (Microsoft)
**Verdict:** 🔥 True Positive (TP) - **CRITICAL**

### 🔍 Analysis & Evidence
The attacker utilized **Typosquatting** (`m1crosoftsupport.co`) to trick the user. The firewall action was **"ALLOWED,"** indicating a high probability of credential theft.

<table width="100%">
  <tr>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8817.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8817_Splunk.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8817_VirusTotal.jpg" width="100%"></td>
  </tr>
</table>

---

## 🎓 Technical Lessons Learned

### 1. SIEM Proficiency (Splunk)
* **Log Correlation:** Mastered pivoting from a single Alert ID to broader traffic logs.
* **Action Analysis:** Learned to identify "Allowed" vs "Blocked" logs to assess breach success.

### 2. Threat Intelligence (VirusTotal)
* **Domain Reputation:** Used VT to identify typosquatted domains and shortened URLs.
* **Consensus:** Leveraged community scores to verify malicious landing pages.

### 3. Reporting Standards
| Metric | False Positive (FP) | True Positive (TP) |
| :--- | :--- | :--- |
| **Focus** | Business context & validation. | Impact, Kill-Chain stage, & IOCs. |
| **Action** | Alert tuning & whitelisting. | Escalation, Host isolation, & Reset. |

---
