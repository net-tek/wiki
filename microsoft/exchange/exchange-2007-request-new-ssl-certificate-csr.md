<!-- TITLE: Exchange 2007 Request New Ssl Certificate Csr -->


```powershell
New-ExchangeCertificate –generaterequest –subjectname "O=My Corporation Inc, OU=Internet Sales, C=US, S=California, L=Los Angeles, CN=exchange.mydomain.com" –privatekeyexportable:1 -keysize 2048 –path c:\certrequest.txt

Import-ExchangeCertificate -path c:\cert.cer

Get-ExchangeCertificate

Enable-ExchangeCertificate -Services IMAP, POP, UM, IIS, SMTP -thumbprint 896B74B25F7EBF330C93E56DA2A76CFC6A7
```
