<!-- TITLE: Exchange 2010 Configure Internet Mail Flow -->

# Configure Internet Mail Flow Directly Through a Hub Transport Server

Applies to: Exchange Server 2010 SP1

You can use the EMC or the Shell to configure an Internet-facing Hub Transport server. To establish Internet mail flow directly through a Hub Transport server, you create a Send connector that routes e-mail to the Internet. Also, you modify the configuration of the default Receive connector to accept e-mail from the Internet. In this scenario, the Microsoft Exchange Server 2010 Hub Transport server can be reached directly through the Internet. We don't recommend this topology because it increases security risks by exposing to the Internet the Exchange 2010 server and all roles installed on that server. We recommend that you implement a perimeter network-based SMTP gateway, such as the Edge Transport server, instead.

If you decide to configure Internet mail flow directly through your Hub Transport server, you may want to install the anti-spam agents on your Hub Transport servers. For more information, see Enable Anti-Spam Functionality on a Hub Transport Server.

Looking for other management tasks related to managing message routing? Check out Managing Message Routing.
 
## Prerequisites 

Register MX resource records for all accepted domains in a public domain name system (DNS) server. Consult the documentation of your DNS provider for information about how to register MX records for your domain. Detailed procedures about how to complete this step are outside the scope of this topic.

 Configure network gateways to allow SMTP traffic to and from the Hub Transport server. Consult the documentation for your network routers and firewalls for information about how to route SMTP traffic to and from the Hub Transport server. Detailed procedures about how to complete this step are outside the scope of this topic.
 
**Step 1:** Create a Send connector on the Hub Transport server to send e-mail to the Internet 
You need to be assigned permissions before you can perform this procedure. To see what permissions you need, see the "Send connectors" entry in the Transport Permissions topic. 

**Use the EMC to create the Send connector** 

1. Expand Organization Configuration, click Hub Transport, and then in the action pane, click New Send connector.
2. On the New Send Connector wizard Introduction page, in the Name field, type a unique name for the connector. From the Select the intended use for this Send connector drop-down list, select Internet, and then click Next.
3. On the Address space page, click Add. In the SMTP Address Space dialog box, type `*` in the Address field, and then click OK. Click Next.
4. On the Network settings page, select Use Domain Name System (DNS) to route mail automatically. Select the Use External DNS Lookup settings check box. Click Next. 

Note: 

For more information about how to configure external DNS lookup settings, see Configure Hub Transport Server Properties. 

5. On the Source Server page, click Add. In the Select Hub Transport and subscribed Edge Transport servers dialog box, select one or more Hub Transport servers in your organization, click OK, and then click Next.
6. On the New Connector page, click New, and then on the Completion page, click Finish.

**Use the Shell to create the Send connector** 
This example creates a Send connector that's used by the Hub Transport server HubA to send e-mail to the Internet.
 

```powershell
New-SendConnector -Name "Internet" -Usage Internet -AddressSpaces "*" -SourceTransportServers "HubA" -DNSRoutingEnabled:$true -UseExternalDNSServersEnabled:$true
```


For detailed syntax and parameter information, see New-SendConnector.
 
**Step 2:** Modify the default Receive connector to allow anonymous connections 
You need to be assigned permissions before you can perform this procedure. To see what permissions you need, see the "Receive connectors" entry in the Transport Permissions topic.
 
**Use the EMC to configure the Receive connector **

1. Expand Server Configuration, click Hub Transport, and in the work pane under the Receive Connectors tab, select the Default <Server Name> connector. In the action pane, click Properties.
2. In <Connector> Properties, select the Permissions tab.
3. Select Anonymous Users to add anonymous permissions. Click OK.

**Use the Shell to configure the Receive connector **

This example modifies the default Receive connector on the Hub Transport server HubA to allow anonymous connections.
 

```powershell
Set-ReceiveConnector "HubA\Default HubA" -PermissionGroups AnonymousUsers,ExchangeUsers,ExchangeServers,ExchangeLegacyServers
```
