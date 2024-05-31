# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which Microsoft Sentinel then uses to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measured metrics for another 24 hours, and then showed the results below. The metrics we will show are:

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
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the Internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet—aka, there was no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic except my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![(before)-windows-rdp-auth-fail](https://github.com/molson20/Azure-SOC/assets/145723812/4957e225-7c79-48ac-865b-82d12cbf4b56)

![(before)-linux-ssh-auth-fail](https://github.com/molson20/Azure-SOC/assets/145723812/efc1fc47-757b-4bbd-b945-852d73fd8d95)

![(before)-nsg-malicious-allowed-in](https://github.com/molson20/Azure-SOC/assets/145723812/a13274f4-e567-4351-99ea-21aee01de2ff)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 2024-05-27 12:54:42

Stop Time 2024-05-28 12:54:42

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 49474
| Syslog                   | 5320
| SecurityAlert            | 6
| SecurityIncident         | 375
| AzureNetworkAnalytics_CL | 2177

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 2024-05-30 21:06:54

Stop Time	2024-05-31 21:06:54

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17500
| Syslog                   | 8
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel triggered alerts and created incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied and after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.
If regular users heavily utilized the resources within the network, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
