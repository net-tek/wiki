<!-- TITLE: Activate Exchange 2007 2010 Owa Audit -->

Throw this command in Powershell
```powershell
Set-EventLogLevel "MSExchangeIS\9000 Private\Logons" -Level Low
```

You will get these events :
Event ID 1016 log\app