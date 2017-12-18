<!-- TITLE: Exchange 2007 2010 Point All Path To Same Certificate -->


```powershell
Set-ClientAccessServer -Identity SERVER-EXCH -AutodiscoverServiceInternalUri https://mail.yourmail.com/autodiscover/autodiscover.xml
Set-WebServicesVirtualDirectory -Identity "SERVER-EXCH\EWS (Default Web Site)" -InternalUrl https://mail.yourmail.com/ews/exchange.asmx
Set-OABVirtualDirectory -Identity "SERVER-EXCH\oab (Default Web Site)" -InternalUrl https://mail.yourmail.com/oab
Set-UMVirtualDirectory -Identity "SERVER-EXCH\unifiedmessaging (Default Web Site)" –InternalUrl https://mail.yourmail.com/unifiedmessaging/service.asmx
```


In IIS, Application Pools, Do “Recycle” on MSExchangeAutodiscoverAppPool