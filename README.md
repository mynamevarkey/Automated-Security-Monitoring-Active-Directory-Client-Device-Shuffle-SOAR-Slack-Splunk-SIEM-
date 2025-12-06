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

## 🥇 Step 1: Deploying Cloud Infrastructure

The first step of this project was to set up the cloud environment using Vulture Cloud.  
I deployed **three virtual machines** inside the same VPC (Virtual Private Cloud) to ensure secure internal communication:

- **Windows Server (Domain Controller)** – Used for configuring Active Directory.  
- **Windows Client Machine** – Joined to the AD domain for log generation and authentication testing.  
- **Ubuntu Server (Splunk Instance)** – Used to install and run Splunk for log ingestion and alerting.

I then configured the **VPC networking**, ensuring all machines were able to communicate internally with private IP addresses.  
Next, I applied **firewall rules** that restricted access so only my IP address could reach the machines externally.  
This provided a secure and isolated environment for the SOC simulation.

## 🥈 Step 2: Active Directory Configuration and User Setup

After deploying the cloud machines, I configured the Windows Server as a Domain Controller and completed the Active Directory setup.  
Using **Active Directory Users and Computers (ADUC)**, I created two domain users to simulate a real organizational environment.

I added a user named **Tony** and enabled **Remote Desktop access** for him so he could log in to the domain-joined client machine.  
This step allowed me to test authentication, user logon behavior, and generate Windows event logs required for Splunk monitoring.


## 🥉 Step 3: Setting Up the Splunk Server and Log Forwarding

Next, I deployed the Ubuntu machine that would act as my **Splunk Server**. After hosting the server on Vulture Cloud, I installed Splunk Enterprise on Ubuntu and configured it to receive logs from the Windows environment. Splunk is used here because it acts as the **central SIEM platform**, allowing me to collect, index, search, and analyze security events generated inside the Active Directory environment. This makes it possible to detect unauthorized access, login failures, and other suspicious activities.

To ensure that the Splunk server receives real security event logs, I installed the **Splunk Universal Forwarder** on the Windows client machine and domaincontroller machine. The forwarder is required because Windows systems do not send logs by default; the forwarder securely transmits Windows Event Logs (such as security, system, and authentication logs) directly to the Splunk server in real time. This log flow is essential for building alerts and detecting abnormal behavior inside the network.

Once the setup was complete, I configured Splunk to listen on the required port and verified that the Windows logs were being ingested. Finally, I hosted and accessed the **Splunk Web GUI**, which allowed me to monitor incoming logs, create detection rules, and visualize user activity within the Active Directory environment.

## 🏁 Step 4: Verifying Log Ingestion, Access Testing, and Monitoring in Splunk

After configuring the Splunk Forwarders on both the client machine and the Domain Controller, I verified that log forwarding was working correctly by checking the connection status on the Splunk server. Both systems successfully established communication with the Splunk instance running on the Ubuntu server. Once the Splunk service was started, I accessed the Splunk Web GUI, which allowed me to begin monitoring all incoming logs in real time. My first step inside Splunk was to create a dedicated index to store Windows event data.

Initially, the RDP settings on the Windows client machine were restricted to allow only my IP address to connect. To simulate a real authentication scenario, I changed the RDP rule to allow connections from anyone. After updating this policy, I tested remote login by using my laptop to connect to the Windows client machine via **Windows Remote Desktop Connection**, logging in as the user **Tony** that I had created earlier in Active Directory. This triggered an event that appeared inside the Splunk SIEM dashboard, showing a successful remote connection coming from an external IP, which Splunk correctly identified as a potentially unauthorized access.

To analyze this behavior further, I used a specialized Splunk query that filtered out all internal traffic and highlighted only IP addresses originating from outside my network range (specifically excluding addresses within the /40-range boundary). This made it easy to detect suspicious or unexpected login sources and confirmed that the SIEM setup was working exactly as intended.With that query, I saved the alert, and now it’s easier to access the unauthorized successful logins

## 🧩 Step 5: Integrating Shuffle SOAR, Automating Splunk Alerts, and Enabling SOC Notifications

In the final stage of the project, I connected my Splunk SIEM to **Shuffle SOAR** to automate incident handling. Inside Shuffle, I configured a workflow that receives alerts directly from Splunk whenever a suspicious or unauthorized login event occurs. Once the alert is ingested, the workflow parses the event data and forwards a detailed notification to Slack. This message contains the username, source IP address, timestamp, and the type of access attempt, allowing the SOC team to see real-time incidents inside their chat environment and respond immediately.

After enabling Slack alerting, I added a second automated action: sending a private email alert to the SOC analyst's mailbox (such as Gmail or any other email service). This email asks whether the user should be blocked or investigated further, creating a simple escalation and approval workflow. To support this capability, I connected Shuffle with my **Active Directory** environment and configured actions that allow Shuffle to disable or block a user account if necessary. This step simulates how modern SOAR platforms automate decision-making and response actions, turning the entire setup into a functioning mini-SOC environment where alerts → notifications → decisions → automated responses flow seamlessly.


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
