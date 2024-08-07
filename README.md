# Zabbix-StarwindvSANMonitoring
## Overview
This repository provides a Zabbix Monitoring Templates for Starwind vSAN.
HTTP web requests are used to gather values. 
This template works without zabbix agent. Values are retrived by querying the api
## Known Issues
This template causes RAM Overflow at the moment. Be aware
## Disclaimer
This is NOT a offical template. I am NOT related to starwind other than being a (happy) customer.
Use this template at your own Risk.
Have fun with it (:
## Setup
1. Create a user `zbx` in the starwind web interface.
2. Download the template and import into zabbix
3. Create a host for each of your starwind nodes you want to monitor and link the template
4. Modify the macro `{$STWI.NODENAME}`. Enter Name of your Node. This is Case sensitive.
5. Modify the macro `{$STWI.PW}`. Enter password of created `zbx` user in cleartext.
6. Modify the macro `{$STWI.URL}`. Enter url of your node. Remember to put a `/` at the end

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
Get Sync Metrics for {#VOLUME}| Retrives Sync related Metrics. Does not hold data| Dependent Item| stwi.discover.SyncMetrics.get[{#VOLUME}]
Estimated Time till sync is finished on {#VOLUME} (in Seconds)| Gives you an estimate how many seconds till the sync is finished. This might not be 100% acruate| Dependent Item| stwi.discover.SyncMetrics.get.time[{#VOLUME}]
Not Synchronized Bytes on {#VOLUME}| Not Synced Bytes| Dependent Item|stwi.discover.SyncMetrics.get.notsyncedBytes[{#VOLUME}]
Synchronized Bytes on {#VOLUME}| Synced Bytes| Dependent Item| stwi.discover.SyncMetrics.get.syncedBytes[{#VOLUME}]
Synchronized Percents of {#VOLUME}| Shows how many bytes in percent are synced. 100% means starwind is synchronized. This Item makes for a good gauge item| Dependent Item|stwi.discover.SyncMetrics.get.Status[{#VOLUME}]
Sync Status for {#VOLUME}| Shows status of sync| Dependent Item|stwi.discover.SyncMetrics.get.Status[{#VOLUME}]
Availability of Virtual Disk {#VOLUME}| Shows if Disk is highly_available, simple or downgraded| Dependent Item| stwi.discover.getAvail[{#VOLUME}]
Get Metrics from {#VOLUME} of {#NODENAME}| Retrives volume metrics like status and availabilty. No data is stored| Dependent Item| stwi.discoverMetrics[{#VOLUME}]
Get Status from Virtual Disk {#VOLUME}|  Shows if disk is online, under maintenacne or ony other status| Dependent Item|stwi.discover.getStatus[{#VOLUME}]

## Triggers
Name|Description|Expression|Priority
--|--|--|--
Starwind API is unreachable |Triggers if the Starwind API is not reachable|last(/Starwind VSAN by HTTPv2/web.test.fail[Availability of API])<>0|High
Service {#TITLE} is not running| Service is not running|last(/Starwind VSAN by HTTPv2/stwi.getServices.Status[{#ID}])<>"running"|Warning
Installed version for Package {#NAME} is outdated| A new version of a package can be installed|last(/Starwind VSAN by HTTPv2/stwi.package.version[{#NAME}-{#COMPNAME}])<>last(/Starwind VSAN by HTTPv2/stwi.package.availversion[{#NAME}-{#COMPNAME}])|Information
Availability of Virtual Disk {#VOLUME} has degraded| Virtual Disk is degraded and likey not highly_avaliable anymore|last(/Starwind VSAN by HTTPv2/stwi.discover.getAvail[{#VOLUME}])<>"highly_available"|Warning
Status from Virtual Disk {#VOLUME} has changed| Status has changed| last(/Starwind VSAN by HTTPv2/stwi.discover.getStatus[{#VOLUME}])<>"online"| Warning

## References
Thank you Starwind for letting me publish this template. Special thanks go to Yaroslav. His dedication made this possible.

## Support 
I do not offer Support for any of my templates. I publish my templates so fellow Sysadmins have a starting point. 
If do need help however please open a issue and I will try to help best effort.

## Contribute
If want to make this template better please make a PR, if you need help please open an issue.
