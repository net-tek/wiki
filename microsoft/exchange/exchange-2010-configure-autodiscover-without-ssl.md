<!-- TITLE: Exchange 2010 Configure Autodiscover Without Ssl -->

http://support.microsoft.com/kb/940881

In your external DNS zone, remove any HOST (A) or CNAME records for the Autodiscover service.
Use the following parameters to create a new SRV record:
Service: _autodiscover
Protocol: _tcp
Port Number: 443
Host: mail.contoso.com