# Coursework-2-BOTSv3-Incident-Analysis-and-Presentation

# INTRODUCTION
<div align="justify">
A Security Operations Center (SOC) is the core organizational function responsible for continuously monitoring, assessing, and improving an enterprise's security posture. Its primary purpose is to prevent, detect, analyze, and respond to cybersecurity threats. SOCs rely on a combination of specialized roles, well-defined processes, and technical infrastructures—such as Security Information and Event Management (SIEM) systems, intrusion detection tools, and threat intelligence platforms—to enable situational awareness and incident management. The ultimate goal is to minimize organizational risk by converting raw log data into actionable intelligence on potential intrusions. From the perspective of a security analyst within a SOC, these activities involve applying systematic intrusion analysis methodologies to identify, investigate, and mitigate threats effectively.  
<br/><br/>
The Boss of the SOC (BOTSv3) dataset is a publicly available, pre-indexed security dataset and Capture The Flag (CTF) platform created by Splunk. It is designed to train and test the skills of cybersecurity professionals, students, and enthusiasts in security analysis. The platform simulates a realistic security incident within a fictitious brewing company named "Frothly," providing a massive collection of logs from network, endpoint, email, and cloud service environments such as Amazon AWS and Microsoft Azure. Users must use Splunk's Search Processing Language (SPL) to analyze the data, identify anomalies, investigate the simulated attack, and practice real-world security response techniques.
<br/><br/>
The objective of this investigation is to utilize Splunk’s SPL on the BOTSv3 data sources, primarily focusing on aws:cloudtrail, aws:s3:accesslogs, and winhostmon, to systematically identify the initial stages of a breach targeting both AWS cloud infrastructure and a key endpoint asset. The investigation begins with a reconnaissance phase, aiming to identify legitimate IAM users accessing the AWS environment and determine the specific API activity field that can be used to alert on non-MFA (Multi-Factor Authentication) access, thereby highlighting a critical security weakness. Next, the investigation reconstructs a security incident involving AWS misconfiguration and potential data exfiltration. This involves identifying the event ID of the API call that publicly exposed an S3 bucket, determining the IAM user (“Bud”) responsible for the misconfiguration, and identifying the name of the S3 bucket and the text file uploaded to the publicly accessible bucket, suggesting unauthorized data staging or exfiltration. Lastly, the investigation seeks to characterize the internal environment using host monitoring data. This includes identifying the processor number used on the web servers and pinpointing the Fully Qualified Domain Name (FQDN) of the endpoint running a distinct Windows operating system edition, which could indicate a unique or vulnerable target host.
</div> 

# SOC ROLES & INCIDENT HANDLING REFLECTION
<div  align="justify">
During the BOTSv3 exercise, the structure and responsibilities of a Security Operations Center (SOC) were highly relevant, as the exercise simulated real-world cybersecurity monitoring, detection, and response scenarios. SOC operations are generally divided into multiple tiers, each with distinct responsibilities. Tier 1, or the alert analyst role, focused on monitoring system alerts, validating potential threats, and escalating genuine incidents. In the exercise, this mirrored the initial identification of suspicious activity and highlighted the importance of vigilance and accurate alert assessment. Tier 2, the incident responder role, was responsible for deeper investigation, analysing attack patterns, reviewing logs, and determining the scope of incidents. BOTSv3 reflected this through the investigation of alerts and proper classification of threats before remediation. Tier 3, comprising threat hunters and SOC engineers, dealt with complex incidents, performing forensic analysis and implementing long-term fixes. In the exercise, this was observed when advanced detection techniques and strategic responses were applied to contain and mitigate attacks.
<br/><br/>
The exercise also demonstrated how incident handling follows the structured lifecycle of prevention, detection, response, and recovery. Preventive measures, such as secure configurations and access controls, reduced the likelihood of successful attacks. Detection relied on monitoring tools and alert systems, which in BOTSv3 were simulated through suspicious traffic alerts and log anomalies, emphasising the need for rapid identification. The response phase involved containment, mitigation, and coordinated actions to prevent further compromise, while recovery focused on restoring systems to normal operation and learning from the incident to improve overall security posture. Overall, the BOTSv3 exercise reinforced the importance of a structured SOC approach, showing that clear tiered responsibilities and a coordinated incident lifecycle are essential for maintaining cybersecurity resilience.

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
1.	Splunk Enterprise Installation
Installing Splunk Enterprise on a Windows-based system provides a stable and supported environment for a SOC analyst workstation. Splunk is widely used in industry SOCs for centralising and correlating security logs, making it ideal for monitoring, detecting, and investigating incidents. Using the official Splunk website ensures software authenticity and mitigates the risk of compromised or malicious software entering the SOC environment.
2.	Administrator Account Configuration
Creating and configuring an administrator account enforces access control and accountability, which are critical in a SOC. Only authorised personnel can modify configurations, create indexes, or ingest data, preventing unauthorised changes that could compromise security monitoring and incident analysis.
3.	Universal Forwarder Installation
The Universal Forwarder was installed to simulate real-world SOC architecture, where logs are collected from distributed endpoints and servers and securely forwarded to a central SIEM platform. This ensures all relevant data is aggregated for monitoring and correlation, reflecting standard SOC operations.
4.	Dataset Placement and Ingestion
Extracting the BOTSv3 dataset into the Splunk apps directory allows Splunk to automatically recognise it as an app, enabling indexing, source type assignment, and access to pre-configured dashboards. This mirrors SOC practices where structured, centralised log data is necessary for timely threat detection and investigation.
5.	Service Restart and Validation
Restarting Splunk after dataset placement ensures the platform fully recognises the app and ingested logs. Validation through test searches confirms that data is correctly indexed, timestamps are accurate, and fields are available for correlation. This step reflects SOC best practices, where verified and reliable data is essential for operational readiness and accurate incident response.
</div>
>
