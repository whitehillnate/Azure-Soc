# Building a SOC + Honeynet in Azure ( with Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this hands on project, I have implemented a miniature honeynet within the Azure cloud environment. The honeynet collects logs from diverse sources and aggregates them into a centralized Log Analytics workspace. Microsoft Sentinel utilizes this workspace to construct attack maps, initiate alerts, and generate incident reports. To assess the security posture of the vulnerable environment, I measured specific security metrics over a 24-hour period. Subsequently, I implemented various security controls to fortify the environment, and once again measured the relevant metrics over another 24-hour period. The results are presented below, highlighting the impact of the security enhancements on the measured metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
The Architecture of my enviorment was wide open to any malicious actors trying to hack in. I turned off all of the firewalls within the Linux-Vm and Windows-VM. I also configured the Azure NSG to allow any inbound and outbound traffic, which resulted in many attempts. I also constructed a MySQl database within the Windows VM, but not malicious actor found it. 


## Architecture After Hardening / Security Controls
To harden the enviornment, I complied with the NIST 800-53 regulatory standards for securing an enviornement, my environment score went from a 38% to 80% after following their guidelines. I turned on the firewalls within the Virtual Machines, and within Azure I configured it so only my IpAddress would be able to connect for the inbound and outbound traffic. 


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

![Linux Syslog Auth Failures](![image](https://github.com/whitehillnate/Azure-Soc/assets/148999138/7740c48b-b651-4998-a1e4-32dc7b5d781d)

![Windows RDP/SMB Auth Failures](![image](https://github.com/whitehillnate/Azure-Soc/assets/148999138/f59739dd-d4d6-4671-8fcc-206a9efae5a1)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 12/6/2023 10:50:48
Stop Time 12/7/2023 10:50:48

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15430
| Syslog                   | 2340
| SecurityAlert            | 4
| SecurityIncident         | 209
| AzureNetworkAnalytics_CL | 1487



## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 12/7/2023 9:36:19
Stop Time 12/8/2023 9:36:19

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17
| Syslog                   | 280
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 3

![image](https://github.com/whitehillnate/Azure-Soc/assets/148999138/2e90fb82-9587-4a56-9deb-8c0916a7ff48)
![image](https://github.com/whitehillnate/Azure-Soc/assets/148999138/a5105e92-b2eb-4fd0-a446-39458b24c5cd)
![image](https://github.com/whitehillnate/Azure-Soc/assets/148999138/5bf1df39-cdbe-43cd-a383-5fe740179b17)





## Conclusion

In this project, a miniature honeynet was established in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to initiate alerts and generate incidents based on the collected logs. Subsequently, metrics were assessed in the insecure environment both before and after the implementation of security controls. Notably, the application of security measures led to a significant reduction in the number of security events and incidents, underscoring their efficacy.

It's important to recognize that if the network resources were extensively used by regular users, there might have been a potential for increased security events and alerts within the 24-hour period following the implementation of security controls.
