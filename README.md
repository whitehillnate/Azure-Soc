# Building a SOC + Honeynet in Azure ( with Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I have implemented a miniature honeynet within the Azure cloud environment. The honeynet collects logs from diverse sources and aggregates them into a centralized Log Analytics workspace. Microsoft Sentinel utilizes this workspace to construct attack maps, initiate alerts, and generate incident reports. To assess the security posture of the vulnerable environment, I measured specific security metrics over a 24-hour period. Subsequently, I implemented various security controls to fortify the environment, and once again measured the relevant metrics over another 24-hour period. The results are presented below, highlighting the impact of the security enhancements on the measured metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel (Note: my time and experience with Azure was excellent, KQL's syntax is a bit different from SQL, but overall went really smooth)


Prior to implementing changes, all resources were deployed with direct exposure to the internet. The Virtual Machines had wide-open Network Security Groups and built-in firewalls, and other resources were configured with public endpoints accessible to the internet—rendering the use of Private Endpoints unnecessary.

In the "AFTER" metrics phase, Network Security Groups were strengthened by restricting ALL traffic except for my admin workstation. Additionally, all other resources were safeguarded by their respective built-in firewalls, coupled with the implementation of Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](![image](https://github.com/whitehillnate/Azure-Soc/assets/148999138/54df7057-f104-42e9-8ec5-af68dfe365ad)
)<br>
![Linux Syslog Auth Failures](![image](https://github.com/whitehillnate/Azure-Soc/assets/148999138/7740c48b-b651-4998-a1e4-32dc7b5d781d)

![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
