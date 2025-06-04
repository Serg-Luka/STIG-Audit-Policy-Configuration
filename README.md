# 🛡 Audit Policy Configuration

In this lab, I will perform a compliance scan using Tenable's vulnerability management tool on a Windows 10 Pro virtual machine. The focus of this lab is on Audit Policy Configuration. I’ll specifically examine audit settings related to system security, such as logging of user logon/logoff events, file share access, and PowerShell activity. These audit policies are critical for tracking user actions and detecting potential security incidents.

After identifying any audit policy configurations that do not meet compliance standards, I will apply the necessary changes to enable or improve auditing. Finally, I will re-run the scan to verify that all audit policies are properly configured and compliant.

# 🌟 Table of Contents 🌟

- [🧰 **Tools Used**](#tools-used)
- [1. 🔍 **Configuring Compliance Scan**](#configuring-compliance-scan)
- [2. 📊 **Analysing Scan Results**](#analysing-scan-results)
- [3. 🔐 **Security Control WN10-CC-000327 Overview**](#security-control-wn10-cc-000327-overview)
  - [3.1 Remediating WN10-CC-000327](#remediating-wn10-cc-000327)
  - [3.2 Confirming WN10-00-000090 Remediation](#confirming-wn10-00-000090-remediation)
- [4. 📁 **Security Control WN10-AU-000082 Overview**](#security-control-wn10-au-000082-overview)
  - [4.1 Remediating WN10-AU-000082](#remediating-wn10-au-000082)
  - [4.2 Confirming WN10-AC-000035 Remediation](#confirming-wn10-ac-000035-remediation)
- [5. 🔑 **Security Control WN10-AU-000560 Overview**](#security-control-wn10-au-000560-overview)
  - [5.1 Remediating WN10-AU-000560](#remediating-wn10-au-000560)
  - [5.2 Confirming WN10-AU-000560 Remediation](#confirming-wn10-au-000560-remediation)
- [6. 🏁 **Lab Conclusion and Lessons Learned**](#lab-conclusion-and-lessons-learned)
  - [6.1 Conclusion](#conclusion)
  - [6.2 Lessons Learned](#lessons-learned)

<a id="tools-used"></a>
## 🧰 Tools Used

• Tenable Vulnerability Management<br>
• Windows 10 Pro VM (Azure)<br>
• Group Policy Editor<br>
• PowerShell<br>

<a id="configuring-compliance-scan"></a>
## 🔍 1.Configuring Compliance Scan

<img src="https://i.imgur.com/5Ri6rix.png">

I set up an Advanced Network Scan in Tenable, specifying the private IP of my Windows 10 Pro VM as the target. I selected the internal scanner and supplied valid Windows credentials to allow authenticated scanning. In the Compliance tab, I turned on policy compliance checks, and under Plugins, I enabled the “Windows Compliance Checks” plugin. This setup enabled the scan to effectively evaluate the system’s security and configuration against defined policy standards.

<a id="analysing-scan-results"></a>
## 📊 2.Analysing Scan Results

<img src="https://i.imgur.com/GYpwg99.png">

The scan identified 145 failed checks, 15 warnings, and 102 passed items related to audit policy settings on the Windows 10 Pro VM.

Since this was a freshly deployed Windows 10 VM on Azure, the high number of failed checks reflects the default out-of-the-box configuration, which typically lacks customised security and audit policies.

The failures spanned various audit settings, including critical areas like PowerShell transcription, object access auditing, and logon/logoff event tracking. For example, these key compliance checks failed:

•	**WN10-CC-000327** – PowerShell Transcription must be enabled on Windows 10<br>
•	**WN10-AU-000082** – Audit Object Access - File Share successes<br>
•	**WN10-AU-000560** – Audit other Logon/Logoff Events successes<br>

These audit policies are essential for capturing detailed activity logs that support security monitoring and incident investigations.

For this lab, I concentrated on these audit-related compliance checks to strengthen the system’s logging and monitoring capabilities, ensuring better visibility into user actions and system events.

<a id="security-control-wn10-cc-000327-overview"></a>
## 🔐 3.Security Control WN10-CC-000327 Overview

<img src="https://i.imgur.com/6HlHyzu.png">

**What is WN10-CC-000327?**
It’s a security rule that requires PowerShell Transcription to be enabled on Windows 10 systems.

**Why remediate?**
Without PowerShell Transcription enabled, there’s no detailed record of the commands and scripts executed in PowerShell. This makes it harder to identify configuration errors, troubleshoot problems, or investigate security breaches. Enabling transcription ensures a comprehensive audit trail is kept, which can reveal suspicious or malicious activity — especially if malware uses PowerShell to run harmful commands.

**Real-world example:**
PowerShell is often exploited by attackers to run scripts stealthily. In many cyberattacks, detailed logs from PowerShell Transcription have been crucial in uncovering how malware operated on a compromised system. Without these logs, it’s much harder to detect and analyse such attacks, delaying response and increasing damage.

**This compliance control applies to the following key frameworks:**

•	**NIST 800-171** – Audit logging and monitoring (3.3.1, 3.3.2)<br>
•	**NIST 800-53** – Audit record management (AU-3)<br>
•	**PCI-DSS** – Logging and audit trails (10.3)<br>
•	**ISO/IEC 27001:2022** – Audit logging (A.5.28)<br>
•	**HIPAA** – Audit controls for ePHI (164.312(b))<br>
•	**GDPR** – Security of processing (Article 32.1.b)<br>
•	**DISA STIG (Windows 10)** – System log configuration and monitoring<br>

---

<a id="remediating-wn10-cc-000327"></a>
### 3.1 Remediating WN10-CC-000327

<img src="https://i.imgur.com/j8s9l0J.png">

I enabled the Group Policy “Turn on PowerShell Transcription” under Windows PowerShell to log all PowerShell commands and scripts executed on the system. I also enabled “Include invocation headers” to capture extra details like user info and command timestamps. Additionally, I configured the transcript output directory to a secure, central log server to protect the logs from user access or tampering. This configuration ensures comprehensive auditing for security monitoring and investigation purposes.

Setting the PowerShell transcription output to a central, secure directory is important because it collects all logs in one place. This makes it much easier for tools like Splunk to access and analyse the data for suspicious activity. Having a dedicated location helps protect logs from being deleted or tampered with by users, improving overall security monitoring and incident response.

---

<a id="confirming-wn10-00-000090-remediation"></a>
### 3.2 Confirming WN10-00-000090 Remediation

<img src="https://i.imgur.com/f3b1GlB.png">

After running another compliance scan, I confirmed that WN10-00-000090 has been successfully implemented and passed the check.

<a id="security-control-wn10-au-000082-overview"></a>
## 📁 4. Security Control WN10-AU-000082 Overview

<img src="https://i.imgur.com/IK948gB.png">

**What is WN10-AU-000082?**
It’s a security rule that requires Windows 10 systems to audit Object Access - File Share successes.

**Why remediate?**
Without this audit setting enabled, there’s no visibility into who is accessing file shares on the system. This makes it difficult to spot unauthorised access, detect lateral movement by attackers, or investigate data exfiltration. Enabling this setting ensures that successful file share access attempts are logged, providing a useful audit trail for both security and troubleshooting purposes.

**Real-world example:**
Attackers often move between systems by accessing administrative shares like C$. If auditing for file share access isn’t enabled, this activity can go completely unnoticed. In real incidents, these logs have helped trace how attackers accessed sensitive folders or transferred stolen data across the network. Having this logging in place makes it much easier to detect and investigate unauthorised file access.

**This compliance control applies to the following key frameworks:**

•	**NIST 800-171** – Audit log review and monitoring (3.3.1, 3.3.2)<br>
•	**NIST 800-53** – Audit log review processes (AU-12c)<br>
•	**PCI-DSS** – Log review and retention (10.1)<br>
•	**ISO/IEC 27001:2022** – Logging and monitoring (A.8.15, A.12.4.1)<br>
•	**HIPAA** – Audit controls and security processes (164.312(b), 164.306(a)(1))<br>
•	**GDPR** – Security of processing (Article 32.1.b)<br>
•	**DISA STIG (Windows 10)** – Log review requirements (WN10-AU-000082)<br>
•	**NIST CSF (v1.1 & 2.0)** – Continuous monitoring and audit log review (DE.CM-1, DE.CM-3, PR.PT-1)<br>

---

<a id="remediating-wn10-au-000082"></a>
### 4.1 Remediating WN10-AU-000082

<img src="https://i.imgur.com/pAnzHzM.png">

To remediate WN10-AU-000082, I enabled auditing for successful file share access events. Using the Local Group Policy Editor, I navigated to Advanced Audit Policy Configuration > Object Access and set 'Audit File Share' to audit Success events. This ensures that any successful access to shared folders is logged in the Security event log, helping track user activity on shared resources and supporting incident investigation if needed.

---

<a id="confirming-wn10-ac-000035-remediation"></a>
### 4.2	Confirming WN10-AC-000035 Remediation

<img src="https://i.imgur.com/FsSNelT.png">

After running another compliance scan, I confirmed that WN10-AU-000082 has been successfully implemented and passed the check.

<a id="security-control-wn10-au-000560-overview"></a>
## 🔑 5. Security Control WN10-AU-000560 Overview

<img src="https://i.imgur.com/QfcxuDV.png">

**What is WN10-AU-000560?**
It’s a security rule that says: “Windows 10 must be configured to audit Other Logon/Logoff Event successes.”

**Why remediate?**
Without this setting, important logon-related events may go unrecorded, making it harder to detect unauthorised access or investigate suspicious activity. Enabling auditing for these events ensures better visibility into user sessions and system behaviour, helping identify patterns that could indicate misuse or compromise.

**Real-world example:**
In many real-world incidents, attackers gain access through valid credentials and operate under the radar. By auditing other logon/logoff events—such as remote logons or network logoffs—security teams can spot unusual activity, like logons outside business hours or from unexpected locations. These logs are critical for threat hunting and post-incident analysis.

**This compliance control applies to the following key frameworks:**

•	**NIST 800-171** – Audit logging requirements (3.3.1, 3.3.2)<br>
•	**NIST 800-53** – Audit record creation and management (AU-3)<br>
•	**PCI-DSS** – Logging of user activities and events (10.3 series)<br>
•	**ISO/IEC 27001:2022** – Audit logging and access (A.5.28, A.8.15)<br>
•	**HIPAA** – Security controls and audit capabilities (164.312(b), 164.306(a)(1))<br>
•	**GDPR** – Security of processing (Article 32.1.b)<br>
•	**DISA STIG (Windows 10)** – Logging configuration and auditing (WN10-AU-000560)<br>
•	**NIST CSF (v1.1 & 2.0)** – Protective technologies and log generation (PR.PT-1, PR.PS-04)<br>

---

<a id="remediating-wn10-au-000560"></a>
### 5.1 Remediating WN10-AU-000560

<img src="https://i.imgur.com/M7CwVi3.png">

To remediate WN10-AU-000560, I enabled auditing for “Other Logon/Logoff Events” via the Local Group Policy Editor. I navigated to Computer Configuration > Windows Settings > Security Settings > Advanced Audit Policy Configuration > System Audit Policies > Logon/Logoff, then opened “Audit Other Logon/Logoff Events”. I selected “Configure the following audit events” and checked both Success and Failure.

This ensures the system logs key user activity, such as when users lock or unlock their workstations, reconnect to remote sessions, or experience logon failures. These events can help detect suspicious behaviour or signs of an attack, like repeated failed logins or unusual session activity.

---

<a id="confirming-wn10-au-000560-remediation"></a>
### 5.2	Confirming WN10-AU-000560 Remediation

<img src="https://i.imgur.com/UjUuUvo.png">

After running another compliance scan, I confirmed that WN10-AU-000560 has been successfully implemented and passed the check.

<a id="lab-conclusion-and-lessons-learned"></a>
## 🏁 6. Lab Conclusion and Lessons Learned

<a id="conclusion"></a>
### 6.1 Conclusion

This lab successfully demonstrated the process of conducting a compliance scan using Tenable's vulnerability management tool on a Windows 10 Pro virtual machine, with a focus on audit policy configuration. By identifying and remediating key compliance failures, such as WN10-CC-000327 (PowerShell Transcription), WN10-AU-000082 (Audit Object Access - File Share), and WN10-AU-000560 (Audit Other Logon/Logoff Events), the system’s auditing capabilities were significantly enhanced. The remediation steps ensured that critical activities, including PowerShell commands, file share access, and logon/logoff events, are now logged, improving visibility into user actions and system events. Subsequent scans confirmed that the applied changes successfully addressed the identified compliance issues, bringing the system into alignment with security best practices. This exercise underscored the importance of robust audit policies in strengthening security monitoring and supporting incident investigation.

---

<a id="lessons-learned"></a>
### 6.2 Lessons Learned

•	**Importance of Default Configuration Awareness:** The high number of failed checks on the freshly deployed Windows 10 VM highlighted that default configurations often lack the necessary audit policies for robust security. Organisations must proactively customise these settings to meet compliance and security requirements.<br>

•	**Critical Role of Auditing in Security:** Enabling audit policies, such as PowerShell Transcription and file share access logging, is essential for creating detailed audit trails. These logs are invaluable for detecting suspicious activity, troubleshooting issues, and investigating security incidents.<br>


•	**Centralised Log Management:** Configuring PowerShell transcription to output to a secure, central log server demonstrated the value of centralised logging. This approach not only protects logs from tampering but also streamlines analysis using tools like Splunk, enhancing incident response capabilities.<br>

•	**Granular Auditing for Threat Detection:** Auditing specific events, such as logon/logoff activities and file share access, provides granular visibility into system behaviour. This is critical for identifying unauthorised access, lateral movement, or other malicious activities in real-world scenarios.<br>

•	**Iterative Compliance Process:** The lab reinforced the importance of an iterative approach to compliance. Running initial scans, applying remediations, and re-scanning to verify changes ensures that systems remain compliant and secure over time.<br>

•	**Real-World Relevance:** The remediation of audit policies directly addresses common attack vectors, such as PowerShell-based attacks and unauthorised file access. Understanding and implementing these controls can significantly reduce the risk of undetected compromises<br>


This lab provided practical experience in identifying, remediating, and verifying audit policy configurations, equipping me with the skills to enhance system security and compliance in real-world environments.
