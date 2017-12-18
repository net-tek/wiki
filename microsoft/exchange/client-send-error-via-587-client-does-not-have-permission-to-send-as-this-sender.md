<!-- TITLE: Client Send Error Via 587 Client Does Not Have Permission To Send As This Sender -->


```powershell
Get-ReceiveConnector "Client EXCHANGE2010" | add-adpermission -user AU -extendedrights ms-Exch-SMTP-Accept-Authoritative-Domain-Sender
Add-ADPermission "Client EXCHANGE2010" –User AU –ExtendedRights ms-Exch-SMTP-Accept-Authoritative-Domain-Sender
Add-ADPermission "Client EXCHANGE2010" –User AU –ExtendedRights ms-Exch-SMTP-Accept-Any-Sender
```
