# Coursework-2-BOTSv3-Incident-Analysis-and-Presentation

# INTRODUCTION
<div align="justify">
A Security Operations Center (SOC) is a central unit responsible for continuously monitoring, analyzing, and improving an organization’s cybersecurity posture. Its main purpose is to prevent, detect, investigate, and respond to threats by transforming raw log data into actionable intelligence. SOC operations rely on specialized roles, structured processes, and technical tools such as Security Information and Event Management (SIEM) systems, intrusion detection platforms, and threat intelligence feeds to maintain situational awareness and manage security incidents effectively. This investigation mimics the lifecycle of an incident, progressing from initial cloud-based anomalies to granular host-level forensic analysis.
<br/><br/>
The Boss of the SOC (BOTSv3) dataset, created by Splunk, simulates a realistic security environment for training purposes. It focuses on a fictional brewing company, Frothly, and includes logs from network, endpoint, email, and cloud services such as AWS and Microsoft Azure. The dataset allows users to practice detecting anomalies, investigating incidents, and responding to threats using Splunk’s Search Processing Language (SPL).

### OBJECTIVES:
•	Identify legitimate IAM users accessing AWS services. <br/>
•	Detect API activity performed without Multi-Factor Authentication (MFA). <br/>
•	Analyze S3 bucket misconfigurations and identify responsible IAM users.<br/>
•	Determine the names of publicly accessible buckets and uploaded files.<br/>
•	Establish an asset inventory baseline to identify hardware configurations and detect OS version discrepancies (drift)<br/>

### SCOPE AND ASSUMPTIONS:
•	Focus on AWS cloud infrastructure and internal endpoints. <br/>
•	Logs are assumed accurate and reflective of SOC monitoring scenarios.<br/>
•	Analysis is limited to the BOTSv3 dataset; no real-world systems are affected.<br/>
•	Investigation demonstrates detection, analysis, and SOC-relevant response techniques.<br/>

</div> 

# SOC ROLES & INCIDENT HANDLING REFLECTION
<div  align="justify">
During the BOTSv3 exercise, the structure and responsibilities of a Security Operations Center (SOC) were highly relevant, simulating real-world cybersecurity monitoring, detection, and response. SOC operations are typically divided into multiple tiers. Tier 1, the alert analyst, focused on initial identification and validation of suspicious activity, such as AWS CloudTrail alerts, demonstrating the importance of rapid detection and escalation. Tier 2, the incident responder, conducted deeper investigation, analyzed attack patterns, correlated logs, and determined the scope of incidents, reflecting escalation from cloud alerts to host-level forensic analysis. Tier 3, including threat hunters and SOC engineers, managed complex incidents, performed detailed forensic analysis, and implemented long-term mitigations. This progression mirrors the “pivot” in professional SOCs, where alerts move from high-level monitoring to granular endpoint examination.
<br/><br/>
The exercise also highlighted the structured incident lifecycle consistent with frameworks like PIER (Preparation, Identification, Containment, Eradication, Recovery) and NIST SP 800-61. Preventive measures, such as secure configurations and access controls, reduced the risk of successful attacks. Detection relied on monitoring tools and alerts, while response involved containment and mitigation actions. Recovery included restoring normal operations and incorporating post-incident activity, such as identifying processor types or OS versions to update asset baselines and enhance future detection rules. Overall, BOTSv3 reinforced the criticality of clear tiered responsibilities, coordinated incident handling, and continuous improvement, demonstrating how SOCs maintain organizational cybersecurity resilience and adapt their detection strategies based on lessons learned.
</div>

# INSTALLATION & DATA PREPARATION
<div  align="justify">
Splunk Enterprise was installed using the official Splunk website to ensure software authenticity and security. An account was first created on the Splunk platform to enable access to licensed downloads. Then, Splunk Enterprise was downloaded and installed on a Windows-based system to simulate a SOC analyst workstation environment. During installation, an administrator account was configured to support secure access and accountability, which aligns with SOC operational practices. Successful installation was verified by accessing the Splunk web interface and confirming that the Splunk service was running correctly.
<p align="center">
  <img src="PICTURE/Splunk_Platform_Registration" width="600">
  <br>
  <strong>Figure 1:</strong> Splunk Platform Registration
</p>
<p align="center">
   <img src="PICTURE/image.png"  width="600">
  <br>
  <strong>Figure 2:</strong> Splunk Enterprise Installation Package
</p>
<p align="center">
   <img src="PICTURE/Screenshot 2025-12-17 215348.png" width="600">
  <br>
  <strong>Figure 3:</strong> Splunk Web Interface
</p>
 
Additionally, the Splunk Universal Forwarder was downloaded and installed as a core SOC data collection component, allowing logs from endpoint and server systems to be securely forwarded to the central SIEM platform. 
<p align="center">
   <img src="PICTURE/Screenshot 2025-12-17 211722.png" width="600">
  <br>
  <strong>Figure 4:</strong> Splunk Universal Forwarder Downloader
</p>
After downloading the BOTSv3 dataset from the official Splunk GitHub repository (https://github.com/splunk/botsv3), the data was extracted and placed into the Splunk apps directory at (C:\Program Files\Splunk\etc\apps). Following extraction, the Splunk Enterprise service was restarted via the Server Controls panel. Dataset validation was then performed by executing test searches using “index=botsv3” to confirm that the data was successfully ingested. 
<p align="center">
   <img src="PICTURE/Screenshot 2025-12-17 211913.png" width="600">
  <br>
  <strong>Figure 5:</strong> Download Dataset from GitHub Repository
</p>
<p align="center">
   <img src="PICTURE/Screenshot 2025-12-17 225406.png" width="600">
  <br>
  <strong>Figure 6:</strong> Extract Data Into C Drive
</p>
<p align="center">
   <img src="PICTURE/Screenshot 2025-12-17 215942.png" width="600">
  <br>
  <strong>Figure 7:</strong> Restart Splunk via Server Control Panel
</p>
<p align="center">
   <img src="PICTURE/Screenshot 2025-12-17 220354.png" width="600">
  <br>
  <strong>Figure 8:</strong> Dataset Validation
</p>

### JUSTIFICATION OF SETUP CHOICES
1.	Splunk Enterprise Installation:
Installing Splunk Enterprise on a Windows-based system provides a stable and supported environment for a SOC analyst workstation. Splunk is widely used in industry SOCs for centralising and correlating security logs, making it ideal for monitoring, detecting, and investigating incidents. Using the official Splunk website ensures software authenticity and mitigates the risk of compromised or malicious software entering the SOC environment.
2.	Administrator Account Configuration:
Creating and configuring an administrator account enforces access control and accountability, which are critical in a SOC. Only authorised personnel can modify configurations, create indexes, or ingest data, preventing unauthorised changes that could compromise security monitoring and incident analysis.
3.	Universal Forwarder Installation:
The Universal Forwarder was installed to simulate real-world SOC architecture, where logs are collected from distributed endpoints and servers and securely forwarded to a central SIEM platform. This ensures all relevant data is aggregated for monitoring and correlation, reflecting standard SOC operations.
4.	Dataset Placement and Ingestion:
Extracting the BOTSv3 dataset into the Splunk apps directory allows Splunk to automatically recognise it as an app, enabling indexing, source type assignment, and access to pre-configured dashboards. This mirrors SOC practices where structured, centralised log data is necessary for timely threat detection and investigation.
5.	Service Restart and Validation:
Restarting Splunk after dataset placement ensures the platform fully recognises the app and ingested logs. Validation through test searches confirms that data is correctly indexed, timestamps are accurate, and fields are available for correlation. This step reflects SOC best practices, where verified and reliable data is essential for operational readiness and accurate incident response.
</div>
