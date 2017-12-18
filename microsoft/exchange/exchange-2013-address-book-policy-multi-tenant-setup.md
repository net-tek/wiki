<!-- TITLE: Exchange 2013 Address Book Policy Multi Tenant Setup -->

https://www.geekandi.com/2013/08/02/exchange-2013-multi-tenancy-step-by-step/


```powershell
New-ADOrganizationalUnit -Name Habitats
New-ADOrganizationalUnit -Name Tenant00001 -Path "OU=Habitats,DC=hosted,DC=exchange"
Set-ADForest -Identity hosted.exchange -UPNSuffixes @{add="zsmtp.net"}
New-AcceptedDomain -Name "Tenant00001" -DomainName zsmtp.net -DomainType:Authoritative
New-GlobalAddressList -Name "Tenant00001 – GAL" -ConditionalCustomAttribute1 "Tenant00001" -IncludedRecipients MailboxUsers -RecipientContainer "hosted.exchange/Habitats/Tenant00001"
New-AddressList -Name "Tenant00001 – All Rooms" -RecipientFilter "(CustomAttribute1 -eq 'Tenant00001') -and (RecipientDisplayType -eq 'ConferenceRoomMailbox')" -RecipientContainer "hosted.exchange/Habitats/Tenant00001"
New-AddressList -Name "Tenant00001 – All Users" -RecipientFilter "(CustomAttribute1 -eq 'Tenant00001') -and (ObjectClass -eq 'User')" -RecipientContainer "hosted.exchange/Habitats/Tenant00001"
New-AddressList -Name "Tenant00001 – All Contacts" -RecipientFilter "(CustomAttribute1 -eq 'Tenant00001') -and (ObjectClass -eq 'Contact')" -RecipientContainer "hosted.exchange/Habitats/Tenant00001"
New-AddressList -Name "Tenant00001 – All Groups" -RecipientFilter "(CustomAttribute1 -eq 'Tenant00001') -and (ObjectClass -eq 'Group')" -RecipientContainer "hosted.exchange/Habitats/Tenant00001"
New-OfflineAddressBook -Name "Tenant00001" -AddressLists "Tenant00001 – GAL"
New-EmailAddressPolicy -Name "Tenant00001 – EAP" -RecipientContainer "hosted.exchange/Habitats/Tenant00001" -IncludedRecipients "AllRecipients" -ConditionalCustomAttribute1 "Tenant00001" -EnabledEmailAddressTemplates "SMTP:%g.%s@zsmtp.net","smtp:%m@zsmtp.net" -EnabledPrimarySMTPAddressTemplate "SMTP:%g.%s@zsmtp.net"
Set-EmailAddressPolicy -Identity "Tenant00002 - EAP" -EnabledPrimarySMTPAddressTemplate "SMTP:%g.%s@zsmtp.org"
New-AddressBookPolicy -Name "Tenant00001" -AddressLists "Tenant00001 – All Users", "Tenant00001 – All Contacts", "Tenant00001 – All Groups" -GlobalAddressList "Tenant00001 – GAL" -OfflineAddressBook "Tenant00001" -RoomList "Tenant00001 – All Rooms"
New-Mailbox -Name 'Tenant00001 Conference Room 1' -Alias 'Tenant00001_conf1' -OrganizationalUnit 'hosted.exchange/Habitats/Tenant00001' -UserPrincipalName 'confroom1@zsmtp.net' -SamAccountName 'Tenant00001_conf1' -FirstName 'Conference' -LastName 'Room 1' -AddressBookPolicy 'Tenant00001' -Room
Set-Mailbox Tenant00001_conf1 -CustomAttribute1 'Tenant00001'
Set-CalendarProcessing -Identity Tenant00001_conf1 -AutomateProcessing AutoAccept -DeleteComments $true -AddOrganizerToSubject $true -AllowConflicts $false
$c = Get-Credential
New-Mailbox -Name 'Mike Horwath' -Alias 'tenant00001_mike' -OrganizationalUnit 'hosted.exchange/Habitats/Tenant00001' -UserPrincipalName 'mike@zsmtp.net' -SamAccountName 'tenant00001_mike' -FirstName 'Mike' -LastName 'Horwath' -Password $c.password -ResetPasswordOnNextLogon $false -AddressBookPolicy 'Tenant00001'
Set-Mailbox mike@zsmtp.net -CustomAttribute1 "Tenant00001"
```
