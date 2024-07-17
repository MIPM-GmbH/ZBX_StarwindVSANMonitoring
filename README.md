# Zabbix-ArubaVirtualController
## Overview
This repository provides a Zabbix Monitoring Templates for {APPLICATION}.
{INTERFACE} is used to gather values.
## Setup
-TODO- Explain How to Set up
## Discovery rules
Name|Description|Type|Key and additional info
--|--|--|--
Example| Example Description | Agent Item | example.discover

## Items collected
Name|Description|Type|Key and additional info
--|--|--|--
Example Name| Example Description of this item| Agent| exampleItem.example

## Triggers
Name|Description|Expression|Priority
--|--|--|--
Example is down for more than 5 Minutes- {#EXAMPLE}|Status is higer than 1 for 5 Minutes|avg(/EXAMPLE/aiAPStatus.[{#EXAMPLE}],5m)=2|Warning
## References
-TODO-

## Contribute
If want to make this template better please make a PR, if you need help please open an issue.
I will do my best to assist.
