<!-- TITLE: Exchange 2013 Configure External And Internal Url -->

http://www.mustbegeek.com/configure-external-and-internal-url-in-exchange-2013/

To be able to receive emails from users out on the Internet, various URLs must be properly configured in the Exchange server 2013. URL for **outlook web access, ActiveSync, autodiscover** and **outlook anywhere** virtual directories are the most important ones. In this post I will show how to configure External and Internal URL in Exchange 2013 for various virtual directories of Exchange Server 2013. After installing, configuring and creating user mailboxes in Exchange server, this is another very important task that must be done.

# Configure External and Internal URL in Exchange 2013

Before configuring the URLs, we need to plan the domain names that will be used to access the mail server. https://mail.mustbegeek.com/ is the domain name that will used from both internal and external network in our case. Similarly, https://mail.mustbegeek.com/ecp/ is the domain name that will be used by administrators to access EAC console. The mail.mustbegeek.com CNAME recored is added in both internal DNS server and public DNS server as well. It's easier for users if external and internal URL is same because they don't have to remember multiple domain names for same purpose. But you can also configure different internal and external URLs. Let's configure URLs for each services step by step.

Open Exchange Admin Center. Click servers on the features pane. Click virtual directories tab. This is where you configure most of the URL's of the virtual directories.
![0 Virtual Directories](/uploads/0-virtual-directories.png "0 Virtual Directories")