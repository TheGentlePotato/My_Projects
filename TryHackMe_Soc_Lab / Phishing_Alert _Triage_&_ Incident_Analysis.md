# 🛡️ SOC Investigation: Phishing & Incident Response Analysis
**Analyst:** Pang Xing Thong (TheGentlePotato)  
**Lab Platform:** TryHackMe SOC Level 1  

---

## 📋 Executive Summary
This project demonstrates a full-lifecycle investigation of multiple security alerts within a simulated Security Operations Center (SOC). I utilized a "Defense-in-Depth" approach, correlating **Splunk (SIEM)** logs with **VirusTotal (Threat Intel)** to distinguish between legitimate business processes and active cyber threats. 

The investigation focused on the **Cyber Kill Chain**, resulting in the identification of a False Positive, a successfully blocked attack, and a critical credential breach.

---

## 🚨 Alert 8814: Suspicious Inbound Email (HR Onboarding)
**Verdict:** ✅ False Positive (FP)

### 🔍 Triage Logic: How I proved it was a False Alarm
* **The Tool:** Splunk (Search Processing Language).
* **The Process:** I queried the sender domain `hrconnex.thm` against all internal company communications.
* **The Result:** I found a mass internal email from the IT Department confirming that **HRConnex** is an authorized third-party partner.
* **Final Decision:** Although the link was external, it was a **legitimate business process**.

<table width="100%">
  <tr>
    <td width="50%"><b>Phishing Lure Evidence</b></td>
    <td width="50%"><b>Internal Verification (Splunk)</b></td>
  </tr>
  <tr>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8814.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8814_Splunk.jpg" width="100%"></td>
  </tr>
</table>

---

## 🚨 Alert 8815 & 8816: The Phishing Chain (Amazon Lure)
**Verdict:** ⚠️ True Positive (TP) - **Blocked**

### 🔍 Triage Logic: How I proved it was a True Threat
* **The Tool:** VirusTotal & Splunk Traffic Logs.
* **The Process:** 1. **Email (8815):** Identified the sender as `amazon.biz` (a known spoofed domain). 
    2. **Traffic (8816):** Tracked the user's web request to a shortened `bit.ly` link. 
* **The Result:** VirusTotal returned a **14/70 Malicious Score** for the URL. Splunk logs showed the Firewall action as **"DENIED"**, meaning the connection was successfully dropped.
* **Final Decision:** Confirmed Phishing attempt; threat **neutralized** by automated perimeter security.

<table width="100%">
  <tr>
    <td><b>Email Delivery (8815)</b></td>
    <td><b>Splunk Traffic Log (8816)</b></td>
    <td><b>VirusTotal Verdict</b></td>
  </tr>
  <tr>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8815.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8816_Splunk.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8816_VirusTotal.jpg" width="100%"></td>
  </tr>
</table>

---

## 🚨 Alert 8817: Typosquatting & Successful Breach (Microsoft)
**Verdict:** 🔥 True Positive (TP) - **CRITICAL INCIDENT**

### 🔍 Triage Logic: How I proved it was a Breach
* **The Tool:** Manual Domain Inspection & Splunk HTTP Analysis.
* **The Process:** 1. **Visual Check:** Noticed **Typosquatting** (`m1crosoftsupport.co`)—using the number '1' to trick the user's eye and bypass simple spam filters.
    2. **Traffic Check:** Filtered Splunk for the destination IP `45.145.10.131`.
* **The Result:** The Splunk log showed the action was **"ALLOWED"** with an HTTP **Status 200** (Success).
* **Final Decision:** This was a **successful compromise**. The user interacted with the page, requiring immediate account lockdown and password reset.

<table width="100%">
  <tr>
    <td><b>Typosquatted Sender</b></td>
    <td><b>Allowed Connection (Splunk)</b></td>
    <td><b>Malicious Domain Check</b></td>
  </tr>
  <tr>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8817.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8817_Splunk.jpg" width="100%"></td>
    <td><img src="%20Pictures%20/Phishing_Alert_Triage_%26_Incident_Response/Alert_8817_VirusTotal.jpg" width="100%"></td>
  </tr>
</table>

---

## 🎓 Technical Lessons Learned

### 1. Determining Alarm Validity (TP vs. FP)
* **False Positives:** I learned to verify external domains against internal IT announcements. Just because a link is external doesn't mean it is malicious.
* **True Positives:** I learned to spot technical indicators like Typosquatting and use VirusTotal as a "second opinion" to confirm malicious intent.

### 2. Utilizing SIEM (Splunk)
* I mastered the **"Action"** field. Seeing "Blocked" means the system worked; seeing "Allowed" means I need to start the incident response process immediately.

### 3. Professional Reporting
* I learned that a good report includes the **Tool Used**, the **Result Found**, and the **Final Verdict**. This ensures that the next analyst in the chain has all the facts to take action.

---
