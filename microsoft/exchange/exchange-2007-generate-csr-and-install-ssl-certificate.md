<!-- TITLE: Exchange 2007 Generate Csr And Install Ssl Certificate -->

CSR generate tool : https://www.digicert.com/easy-csr/exchange2007.htm

```powershell
Import-ExchangeCertificate -Path C:\mydomain.cer
Enable-ExchangeCertificate -Thumbprint [paste thumbprint here] -Services "SMTP,IMAP, POP, IIS"
Remove-ExchangeCertificate -Thumbprint [paste old expired cert thumbprint here]
```