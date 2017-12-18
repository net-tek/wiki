<!-- TITLE: Exchange 2013 Configure External And Internal Url -->

http://www.mustbegeek.com/configure-external-and-internal-url-in-exchange-2013/

To be able to receive emails from users out on the Internet, various URLs must be properly configured in the Exchange server 2013. URL for **outlook web access, ActiveSync, autodiscover** and **outlook anywhere** virtual directories are the most important ones. In this post I will show how to configure External and Internal URL in Exchange 2013 for various virtual directories of Exchange Server 2013. After installing, configuring and creating user mailboxes in Exchange server, this is another very important task that must be done.

# Configure External and Internal URL in Exchange 2013

Before configuring the URLs, we need to plan the domain names that will be used to access the mail server. https://mail.mustbegeek.com/ is the domain name that will used from both internal and external network in our case. Similarly, https://mail.mustbegeek.com/ecp/ is the domain name that will be used by administrators to access EAC console. The mail.mustbegeek.com **CNAME recored** is added in both internal DNS server and public DNS server as well. It's easier for users if external and internal URL is same because they don't have to remember multiple domain names for same purpose. But you can also configure different internal and external URLs. Let's configure URLs for each services step by step.

Open **Exchange Admin Center**. Click **servers** on the features pane. Click **virtual directories** tab. This is where you configure most of the URL's of the virtual directories.

![0 Virtual Directories](/uploads/0-virtual-directories.png "0 Virtual Directories")

## Step 1: Outlook Web Access

Outlook web access virtual directory is the directory that users access while logging into their mailboxes. Double-click owa (Default Web Site) and change the URLs.

![1 Owa](/uploads/1-owa.png "1 Owa")

Now users will need to type, https://mail.mustbegeek.com/owa in their browser to access their mailboxes.

## Step 2: Exchange Control Panel (ecp) or Exchange Admin Center

**ECP** virtual directory is accessed by administrators to configure the Exchange server. Double-click **ecp (Default Web Site)** and configure the URLs.

![2 Ecp 1](/uploads/2-ecp-1.png "2 Ecp 1")

Now administrator needs to browse https://mail.mustbegeek.com/ecp to log in Exchange Admin Center.

## Step 3: ActiveSync

Exchange ActiveSync is used by mobile clients and devices to synchronize mails, contacts, calendar, etc. Double-click Microsoft-Server-ActiveSync (Default Web Site) and configure the URLs.

![3 Active Sync](/uploads/3-active-sync.png "3 Active Sync")

## Step 4: Offline Address Book (OAB)

**OAB** virtual directory is used to distribute offline address book for mailbox users. The address book is distributed to user’s office outlook application so that users can use the address book even when they are not connected to the Exchange server. Double-click OAB (Default Web Site) and configure the internal and external URLs.

![Oab](/uploads/oab.png "Oab")

## Step 5: Exchange Web Services (EWS)

Exchange Web Services allows for calender sharing and other various options provided by Exchange. To configure URLs, double-click EWS (Default Web Site).

![Ews](/uploads/ews.png "Ews")

## Step 6: Outlook Anywhere

Outlook Anywhere feature lets users out on the Internet to send and receive Exchange emails without requiring a VPN connection to the company network. Office outlook 2007. 2010 and 2013 is used to connect to Exchange server from the Internet. Click **servers** tab. Double-click the server and select outlook **anywhere** tab.

![4 Outlook Anywhere Setting](/uploads/4-outlook-anywhere-setting.png "4 Outlook Anywhere Setting")

Now configure the URLs as shown below and click **save**.

![4 1 Outlook Anywhere](/uploads/4-1-outlook-anywhere.png "4 1 Outlook Anywhere")

## Step 7: Auto Discover

AutoDiscover feature in Exchange 2013 let’s client application such as Office Outlook 2007, 2010 and 2013 to connect to Exchange server automatically. AutoDiscover feature automatically discovers the mailbox settings for user profile in Office Outlook application. AutoDiscover also works for supported mobile applications. In Exchange 2013, you can configure SCP for **AutoDiscover** service via Exchange Management Shell. The command below will update SCP (Service Connection Point) object. SCP is active directory object and is used by internal domain-joined clients to retrieve autodiscover URL.

