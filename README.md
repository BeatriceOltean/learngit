<properties
	pageTitle="IoT Hub - IP Filter | Microsoft Azure"
	description="Follow this tutorial to learn how to blacklist or whitelist certain IP addresses for Azure IoT Hub."
	services="iot-hub"
	authors="fsautomata"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="BeatriceOltean"/>

# IP Filter

## Introduction

Security is an important aspect of an IoT solution based on Azure IoT Hub and sometimes customers need to blacklist or whitelist certain IP addresses. The IP Filter feature enables them to configure rules for rejecting or accepting traffic from specific IPv4 addresses.

There are two case scenarios when closing the IoT Hub endpoint for certain IP addresses is useful: 
- When IoT Hub is designed to receive traffic only from an allowed range of IP addresses and reject everything else. A practical example is using the IoT Hub with [Azure Express Route] to create private connections between IoT Hub and on premises infrastructure.
- Another use case for IP filter is to reject traffic from IP addresses that have been identified as suspicious by the IoT Hub administrator.

The IP filter rules are applied at the IoT Hub service level meaning any time a device is connecting on any supported protocols (currently AMQP, MQTT, AMQP/WS, HTTP/1). 
Any application from an IP address that matches a rejecting IP rule in the IoT Hub will receive an unauthorized 401 status code and description without specific mention of the IP rule in the message.

## Default state
By default the IP Filter grid is empty meaning that all IP addresses are accepted. This is the equivalent of having a rule that accepts 0.0.0.0/0 IP range. 

![ip-filter-default.png]
 
## Adding or editing an IP filter rules for IoT Hub

Adding an IP filter rule will prompt the user to introduce the following values: 

- Define a **IP filter rule name** that must be unique, case insensitive, alphanumeric string up to 128 characters long. Only ASCII 7-bit alphanumeric chars + {'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';',  '''} are accepted
- Select a specific **action** for rejecting or accepting the IP rule. 
- Define one **individual IPv4** or one **CIDR-notation mask**. For example, the 192.168.100.0/22 represents the 1024 IPv4 addresses from 192.168.100.0 to 192.168.103.255. 

![ip-filter-add-rule.png]

After creating and saving the rule the user will be prompted with an alert notifying that the update is in progress

![ip-filter-save-new-rule.png]

Add option will be disabled after reaching the **maximum of 10 IP filter rules**.

User can edit an existing rule by double clicking on the row containing the rule. 

## Deleting IP rule
Deleting rules is possible by selecting one or more rules in the grid and using the Delete icon in the menu. 

![ip-filter-delete-rule.png]

## IP rule evaluation 

The rules are applied in order; the first rule that matches the IP decides the action. 
For example if the user wants to accept the 192.168.100.0/22 and reject everything else the grid would have a first rule that accepts the 192.168.100.0/22 followed by a rule that rejects all addresses 0.0.0.0/0. By adding a last rule that rejects 0.0.0.0/0 the user changes the default behavior to blacklist. 

Changing the priority of the IP rule is possible by clicking on the vertical dots at the beginning of the rule followed by drag and drop to the desired order in the grid.
The ordered changes are preserved by clicking Save command.

![ip-filter-rule-order.png]


<!-- Images -->
[20]: ./media/ip-filter-default.png
[21]: ./media/ip-filter-add-rule.png
[23]: ./media/ip-filter-save-new-rule.png
[24]: ./media/ip-filter-delete-rule.png
[25]: ./media/p-filter-rule-order.png 


<!-- Links -->



[IoT Hub Developer Guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services
