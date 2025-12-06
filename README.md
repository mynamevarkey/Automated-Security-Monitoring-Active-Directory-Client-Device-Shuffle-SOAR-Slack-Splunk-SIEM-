# Automated-Security-Monitoring-Active-Directory-Client-Device-Shuffle-SOAR-Slack-Splunk-SIEM-


## Objective
The objective of this project is to understand how Active Directory works, explore basic virtual machine networking, and deploy a Splunk server to collect and analyze security logs. The project also aims to build an automated alerting workflow that notifies SOC analysts about unauthorized access attempts, including the user details, IP address, and timestamp. Additionally, Slack and Shuffle SOAR were integrated to automate real-time notifications and enhance incident response efficiency.

## Highlights
- Designed and deployed a fully instrumented detection lab
- Ingested and normalized logs into a SIEM for analysis
- Generated test telemetry to emulate real-world adversary behavior
- Built detections, validated findings, and iterated on tuning

## Skills Learned
- Active Directory Fundamentals
- Virtual Machine Networking Basics
- SIEM (Splunk) Setup & Log Ingestion
- SOAR Automation (Shuffle)
- Slack Security Alert Integration
- Incident Detection & Response Basics
- Hands-on Cybersecurity Lab Building

## 🛠️ Tools Used

- **Vulture Cloud**  
  Used to host all machines (Windows Server + Client) and manage cloud networking for the lab environment.

- **Active Directory (Windows Server)**  
  Set up the Domain Controller, managed users/groups, enforced authentication policies, and enabled audit logging.

- **Windows Client Machine**  
  Joined to the AD domain and generated security event logs for monitoring.

- **Splunk Enterprise (SIEM)**  
  Ingested Windows logs, created detection rules, and triggered alerts for unauthorized access attempts.

- **Shuffle SOAR**  
  Built automated workflows to process Splunk alerts and forward notifications.

- **Slack**  
  Used as the alerting channel for SOC notifications (user details, IP address, timestamp, and event info).

## 🔄 Lab Workflow

1. **Deploy Environment on Vulture Cloud**  
   - Create Windows Server (AD) and Windows Client machines.  
   - Configure cloud networking so both systems communicate internally.

2. **Set Up Active Directory**  
   - Promote Windows Server to a Domain Controller.  
   - Create users, groups, and organizational units.  
   - Enable audit policies for login events and security logs.

3. **Join Windows Client to the Domain**  
   - Connect the client machine to the AD domain.  
   - Verify authentication works for domain users.  
   - Ensure Windows Event Logs are being generated.

4. **Install and Configure Splunk Enterprise**  
   - Set up Splunk on the server.  
   - Add data inputs for WinEventLog (Security, System, Application).  
   - Configure log forwarding from the client and server.

5. **Create Splunk Detection Rules**  
   - Build alerts for unauthorized login attempts, failed logins, or account misuse.  
   - Enable webhook/HTTP alert actions to send events to Shuffle SOAR.

6. **Build Automation in Shuffle SOAR**  
   - Create a workflow that receives Splunk alerts.  
   - Parse alert data (user, IP, timestamp, event ID).  
   - Format a clear SOC notification message.

7. **Integrate Slack for SOC Notifications**  
   - Configure Slack webhook inside Shuffle.  
   - Send formatted alerts directly to SOC channels in real time.

8. **Test End-to-End Alert Flow**  
   - Generate a failed login or unauthorized access attempt.  
   - Confirm Splunk detects it → sends alert → Shuffle automates → Slack receives the notification.

9. **Review and Validate Results**  
   - Ensure accuracy of user details, IP address, and log timestamps.  
   - Validate the workflow reflects real SOC alerting processes.




## Screenshots and Evidence
Drag and drop screenshots here or host them on Imgur and reference them with `![Title](img_url)`.

For each screenshot, include a short explanation:
- What the screenshot shows
- Why it matters (context or detection objective)
- Any notable indicators or outcomes

Example:

*Ref 1: Network Diagram*
- Overview of lab components and data flows
- Highlights log sources, forwarding paths, and SIEM ingestion
- Identifies where detections are applied in the pipeline

*Ref 2: SIEM Dashboard*
- Authentication trends, failed logon spikes, and off-hours activity
- Visualizations used to spot anomalies and pivot into investigations

*Ref 3: Detection Rule*
- Correlation search for suspicious logon types (e.g., Type 3, 10)
- Thresholds, exclusions, and rationale behind rule design

*Ref 4: Packet Capture*
- Wireshark snapshot of anomalous traffic
- Protocol details, endpoints, and potential IOC highlights

## Example Detections
- Brute-force and password spraying indicators (failed logon bursts)
- Suspicious logon patterns (remote/interactive off-hours)
- Privilege escalation signals (admin group membership changes)
- Account lockout anomalies correlated with source hosts
- Endpoint process anomalies (with Sysmon) mapped to MITRE ATT&CK

## 📘 Lessons Learned

- Gained a strong understanding of how **Active Directory** works, including domain setup, authentication, and audit logging.  
- Learned how to deploy and manage machines using **Vulture Cloud**, including networking and connectivity configuration.  
- Understood how Windows systems generate **security logs** and how these logs flow into a SIEM.  
- Learned to install, configure, and manage **Splunk Enterprise** for log ingestion and alert creation.  
- Developed the ability to build **detection rules** for unauthorized access and suspicious activity.  
- Learned how **SOAR automation** works by creating workflows in Shuffle to process SIEM alerts.  
- Integrated **Slack** to deliver real-time SOC notifications with user, IP, and timestamp details.  
- Gained practical insight into how a **SOC operates**, including log analysis, alerting, and automated response.  
- Improved troubleshooting skills across cloud networking, SIEM ingestion, and SOAR workflows.  
- Learned end-to-end implementation of a **mini SOC environment**, connecting AD → Splunk → Shuffle → Slack.



## 🚀 Next Steps

- Expand detection rules in Splunk to cover more security events such as privilege escalation, lateral movement, and password spraying.
- Integrate additional enrichment tools (e.g., VirusTotal, AbuseIPDB) into Shuffle SOAR workflows.
- Automate response actions such as disabling compromised accounts or blocking suspicious IP addresses.
- Add an email notification layer alongside Slack for multi-channel alerting.
- Deploy a dashboard in Splunk for real-time SOC monitoring and incident trends.
- Explore integrating other SIEM/SOAR platforms like Wazuh, Elastic, or Cortex XSOAR.
- Improve cloud architecture by adding subnets, firewalls, and better segmentation within Vulture Cloud.
- Document all configurations and export workflows for reproducibility.
- Scale the project into a full mini-SOC environment with more hosts and attack simulations.



## Credits
Designed and implemented by Varkey as part of a hands-on learning journey to strengthen detection engineering and security operations skills.MyDfir videos helped me throughout this process
