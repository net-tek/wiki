<!-- TITLE: Exchange 2010 Oab Regeneration -->


```powershell
Update-GlobalAddressList -identity “default global address list”
Get-GlobalAddressList | update-GlobalAddressList
Get-OfflineAddressBook | Update-OfflineAddressBook
Update-FileDistributionService -Identity Exchange-Server
```
