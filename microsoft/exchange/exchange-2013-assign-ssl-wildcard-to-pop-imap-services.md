<!-- TITLE: Exchange 2013 Assign Ssl Wildcard To Pop Imap Services -->

I recently added an Exchange 2013 server into our environment. While enabling services for the wildcard certificate I got this error:

This certificate with thumbprint 855951C368ECA4FF16A33D82298E81B3F001BDED and subject "*.blitzagency.com" cannot used for POP SSL/TLS connections because the subject is not a Fully Qualified Domain Name (FQDN). Use command Set-POPSettings to set X509CertificateName to the FQDN of the service.

**RESOLUTION**

Start the Exchange Management Shell


```powershell
Set-ImapSettings -X509CertificateName mail.blitzagency.com
Set-PopSettings -X509CertificateName mail.blitzagency.com
```


And then restart the IMAP and POP services.

Even when you do this and look at the wild certificate&#8217;s assigned services, you will not see IMAP and POP, but it works fine.