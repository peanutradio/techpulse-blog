---
categories:
- MS
- Fabric
date: '2026-08-31T20:00:00+00:00'
description: 'Bring private operational data into real-time analytics

  Real-time operational data is often generated in places that are intentionally isolated
  from the public '
draft: false
original_url: https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/From-factory-floor-to-Fabric-Securely-stream-private-data-with/ba-p/5362960
source: Microsoft Fabric Blog
tags: []
title: 'From factory floor to Fabric: Securely stream private data with Eventstream'
---

Bring private operational data into real-time analytics
Real-time operational data is often generated in places that are intentionally isolated from the public internet. Factory MQTT brokers, enterprise Kafka clusters, database change data capture sources, and messaging systems may live in an on-premises network, an Azure virtual network, or a private environment in another cloud.&nbsp;
Microsoft Fabric Eventstream virtual network and on-premises support gives streaming connectors a managed path to approved private sources. The source stays behind its network boundary while Eventstream retrieves events for transformation, routing, analytics, and action. Network teams continue to control connectivity, and data teams use a shared network reference in Fabric instead of exposing a public endpoint.&nbsp;
&nbsp;A manufacturing industry data scenario&nbsp;
Consider a manufacturing environment where sensors, machines, and vehicles publish events every second to an MQTT broker in the factory network. Operations teams want to&nbsp;monitor&nbsp;equipment&nbsp;health, detect production issues, and respond to changing conditions. The analytics team wants to route those events through&nbsp;Eventstream&nbsp;to&nbsp;Eventhouse&nbsp;and Lakehouse, then use them in real-time dashboards and alerts.&nbsp;
The MQTT broker is private by design. The security team doesn't permit inbound access from the public internet, and the broker has no publicly routable endpoint. The data team needs a connection model that reaches the broker through the organization's approved network path.&nbsp;
Figure: Eventstream cannot reach the private MQTT broker through the public internet.
A challenge shared across industries&nbsp;&nbsp;
The&nbsp;challenge is common across industries. Valuable operational sources often live in on-premises networks, Azure virtual networks, or private environments in other clouds because those systems were intentionally designed not to be publicly accessible.&nbsp;
Eventstream virtual network and on-premises support provide a secure, managed pathway for connecting those sources to Fabric. Network administrators retain control of the network boundary, while data engineers can use the approved streaming virtual network data gateway when creating Eventstream connections.&nbsp;
Use an Azure virtual network as the controlled bridge&nbsp;
Rather than making the MQTT broker public,&nbsp;data&nbsp;team&nbsp;partners with the networking team to use a customer-owned Azure virtual network as an intermediary bridge. This is the core of the&nbsp;Eventstream&nbsp;virtual network architecture.&nbsp;
The team registers the&nbsp;Microsoft.MessagingConnectors&nbsp;resource provider, creates an Azure virtual network, and prepares a&nbsp;subnet&nbsp;delegated to&nbsp;Microsoft.MessagingConnectors. Because the MQTT broker is in the factory network, the networking team connects that network to Azure through the organization’s private connectivity approach, such as VPN or Azure ExpressRoute.&nbsp;
Before moving forward, the team verifies that a client in the Azure virtual network can reach the MQTT broker. The broker&nbsp;remains&nbsp;private, but the Azure virtual network now has a secure route to it.&nbsp;
Enable&nbsp;Fabric permission to use the network&nbsp;
The private route now exists, but&nbsp;Eventstream&nbsp;still needs permission to use the Azure virtual network.&nbsp;Data&nbsp;team then works with Fabric admin to&nbsp;enable&nbsp;the workspace identity for the Fabric workspace that&nbsp;contains&nbsp;the&nbsp;Eventstream. The Azure administrator then grants the workspace&nbsp;identity&nbsp;the Network Contributor role on the Azure virtual network.&nbsp;
Think of this as authorizing fabric to use approved network&nbsp;path.&nbsp; The network team created the bridge. The role assignment authorizes the Fabric workspace to use the prepared Azure VNet for connector injection.&nbsp;
Bring&nbsp;the network reference into Fabric&nbsp;
Inside Fabric,&nbsp;data team&nbsp;creates a streaming virtual network data gateway from Manage connections and gateways. The gateway&nbsp;represents&nbsp;the Azure virtual network and subnet resources within Fabric and passes the network information needed for streaming connector virtual network injection.&nbsp;
The streaming virtual network data gateway is not a traditional gateway server. It is a Fabric reference to the Azure network resources already prepared by the networking team.&nbsp;
Figure: Creates the streaming virtual network data gateway and selects the prepared Azure VNet and delegated subnet.
Connect and publish the MQTT source&nbsp;
With the gateway available, data engineer returns to Eventstream and adds an MQTT source. During connection setup, select the streaming virtual network data gateway, supply the MQTT connection information and credentials, complete the source configuration, and publish the eventstream.&nbsp;
Figure: Select the approved gateway while creating the MQTT source connection
When the eventstream is published, Fabric injects the streaming connector into the delegated subnet. The connector makes an outbound connection through the private route to retrieve MQTT events. No inbound public endpoint is required on the factory broker. Eventstream can then transform and route the events to Fabric destinations.&nbsp;
Figure: Eventstream retrieves the telemetry through the managed private-network path and routes it to Fabric destinations.
Turning factory signals into real-time intelligence&nbsp;
Once the&nbsp;Eventstream&nbsp;is running, factory telemetry starts flowing into Fabric.&nbsp;Data engineering&nbsp;team can transform and route the events to&nbsp;Eventhouse&nbsp;for real-time analytics, persist data in Lakehouse, create operational dashboards, and configure alerts for important conditions.&nbsp;
The factory systems&nbsp;remain&nbsp;behind the company’s network boundary while the analytics team gains access to the operational signals needed for&nbsp;timely&nbsp;decisions.&nbsp;
Plan for operational boundaries&nbsp;
This connection model manages connector placement, but it&nbsp;doesn't&nbsp;replace the organization's network design. Teams still own private routing, DNS resolution,&nbsp;firewall&nbsp;rules, source credentials, certificates, and source availability. Capacity planning also matters because connector instances consume subnet IP addresses and can scale with source partitions.&nbsp;
The same model applies to many supported streaming sources, including Apache Kafka, Azure Service Bus, Amazon Kinesis Data Streams, Google Cloud Pub/Sub, HTTP, and several CDC connectors. Connector-specific authentication and network requirements still apply.
For prerequisites and the complete configuration sequence, follow the&nbsp;Eventstream streaming connector virtual network and on-premises support guide.
Next steps&nbsp;
Review the architecture with your networking and Fabric administrators, then&nbsp;identify&nbsp;one private streaming source for a controlled validation. Confirm network reachability before configuring&nbsp;Eventstream, document the delegated subnet and workspace permissions, and verify the complete event path after publishing.&nbsp;
If you find this blog helpful, please give it a&nbsp;thumbs-up!&nbsp;Have&nbsp;ideas for what&nbsp;you'd&nbsp;like to see next? Drop us a comment or reach out to&nbsp;askeventstreams@microsoft.com —&nbsp;we'd&nbsp;love to hear what real-time scenarios&nbsp;you're&nbsp;building and what topics&nbsp;you'd&nbsp;like us to cover in future posts. &nbsp;
To suggest improvements or additional scenarios, submit feedback through Fabric Ideas.
&nbsp;

---
*원문: [https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/From-factory-floor-to-Fabric-Securely-stream-private-data-with/ba-p/5362960](https://community.fabric.microsoft.com/t5/Fabric-Updates-Blog/From-factory-floor-to-Fabric-Securely-stream-private-data-with/ba-p/5362960)*
