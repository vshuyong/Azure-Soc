# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/vshuyong/Shuyong/blob/main/SOC%3AHoneynet.drawio.png)

## Introduction

In this project, I created a mini honeynet in Azure and integrated log sources from various resources into a Log Analytics workspace. Microsoft Sentinel utilized this workstation to build attack maps, trigger alerts, and create incidents. Initially, I measured the security metrics in the unsecured environment for 24 hours. After applying Security controls to harden environment, I measured the metrics again for another 24 hours. The results are shown below:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/vshuyong/Shuyong/blob/main/Architecture-Before-Hardening%3ASecurity-Controls.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel# Azure-Soc

- Initially, all resources were created and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls completely open. Additionally, all other resources were deployed with public endpoints accessible to the Internet, meaning Private Endpoints were not utilized.

For the "AFTER" metrics, Network Security Groups were strengthened by restricting ALL traffic except from my admin workstation. Additionally, all other resources were secured using their built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![mssql Auth Failures](https://github.com/vshuyong/Shuyong/blob/main/mssql-auth-fail.png)<br>
![Linux Syslog Auth Failures](https://github.com/vshuyong/Shuyong/blob/main/Screenshot%202024-06-29%20100418.png)<br>
![Windows RDP/SMB Auth Failures](https://github.com/vshuyong/Shuyong/blob/main/Windows-rdp-auth-fail.png)<br>

## Metrics Before Hardening / Security Controls

The table below shows the metrics I collected in my insecure environment for 24 hours:
Start Time 2024-06-03 09:00
Stop Time 2024-06-04 09:00

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 29471
| Syslog                   | 3040
| SecurityAlert            | 10
| SecurityIncident         | 548
| AzureNetworkAnalytics_CL | 643

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The table below shows the metrics i collected in my environment for another 24 hours after applying security controls:
Start Time 2024-06-04 14:15
Stop Time	2024-06-05 14:15

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 7778
| Syslog                   | 27
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and generate incidents based on the ingested logs. Metrics were collected in the insecure environment before applying security controls, and then again after the security measures were implemented. The results showed a significant reduction in Security events and incidents after the security controls were put in place, highlighting their effectiveness.

It is important to note that if the network resources had experienced heavy usage by regular users, there might have been an increase in Security events and alerts within the 24 hours following the implementation of Security controls.
