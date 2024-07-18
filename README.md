# Zabbix-StarwindvSANMonitoring
## Overview
This repository provides a Zabbix Monitoring Templates for {APPLICATION}.
{INTERFACE} is used to gather values.
## Setup
-TODO- Explain How to Set up
## Discovery rules
Name|Description|Type|Key and additional info
--|--|--|--
Discover Services| Discovers Systemd services via the API. Creates Item and trigger for each discoverd Service. Only services in `{$SERVICES_REGEX_ALLOW}` will be discoverd | Dependent Item | stwi.discover.services
Discover Update Components| Turns out the way Starwind Web interface deals with updates is that there are two diffrent compnentens that are updadeted seperatly. I figured to make it somewhat future proof I will create a discovery rule for it. It be overkill though.|Dependent Item| stwi.discoverUpdate.Components
Get Disk Metrics| Discovers Disc Metrics| Dependent Item| stwi.discover.DisksMetrics
Starwind Discover Sync Performance Values| Discovers a bunch of Sync related values| stwi.discover.SyncMetrics
Starwind Discover Virtual Disk Status| Discovers Status of Virtual Disks| stwi.discover



## Items collected
Name|Description|Type|Key and additional info
--|--|--|--
starwind vSAN Update State| Retrives information about Starwind Update Status and starts update search. This is why this item needs a big timeout| Script Item| stwi.updateState
starwind vSAN request Virtual Disk Metrics|Collects a wide number of Virutal Disk Metrics | Script Item| stwi.reqMetrics
starwind vSAN request Host information| Collects a information about the host| Script Item|stwi.getHost
starwind systemd services| Retrives information about systemd services | Script Item| stwi.services
Service {#TITLE} Data| Retrives Information about service. This Item does not store Data| Dependent Item| stwi.getService[{#ID}]
Service {#TITLE} status| Status of service| Dependent Item| stwi.getServices.Status[{#ID}]
Update information for package {#NAME}| Retrives Update Informations for package. Does not store any Data| Dependent Item| stwi.getUpdateData[{#NAME}-{#COMPNAME}]
Installed version for Package {#NAME}| Version Number of installed package| Dependent Item| stwi.package.version[{#NAME}-{#COMPNAME}]
Available version for Package {#NAME}| Version Number of available package. If this Number is higher than the installed Version Number a update is available| Dependent Item| stwi.package.availversion[{#NAME}-{#COMPNAME}]
Get Disk Metrics for {#NODEID}| Retrives Disk Metric for your Starwind Node. Does not store Data| Script Item| stwi.discover.DisksMetrics.getMetrics[{#NODEID}]
Read Bytes for {#NODENAME}| Read Bytes| Dependent Item| stwi.discover.DisksMetrics.getRead_bytes[{#NODEID}]
Write Bytes for {#NODENAME}| Write Bytes| Dependent item| stwi.discover.DisksMetrics.getWrite_bytes[{#NODEID}]
Get Sync Metrics for {#VOLUME}| Retrives Sync related Metrics. Does not hold data| Dependent Item 
## Triggers
Name|Description|Expression|Priority
--|--|--|--
Starwind API is unreachable |Triggers if the Starwind API is not reachable|last(/Starwind VSAN by HTTPv2/web.test.fail[Availability of API])<>0|High
Service {#TITLE} is not running| Service is not running|last(/Starwind VSAN by HTTPv2/stwi.getServices.Status[{#ID}])<>"running"|Warning
Installed version for Package {#NAME} is outdated| A new version of a package can be installed|last(/Starwind VSAN by HTTPv2/stwi.package.version[{#NAME}-{#COMPNAME}])<>last(/Starwind VSAN by HTTPv2/stwi.package.availversion[{#NAME}-{#COMPNAME}])|Information


## References
-TODO-

## Contribute
If want to make this template better please make a PR, if you need help please open an issue.
I will do my best to assist.
