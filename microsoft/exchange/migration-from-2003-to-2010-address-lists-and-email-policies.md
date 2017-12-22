<!-- TITLE: Migration From 2003 To 2010 Address Lists And Email Policies -->


```powershell
Get-EmailAddressPolicy | where {$_.RecipientFilterType –eq “Legacy”} | Set-EmailAddressPolicy –IncludedRecipients AllRecipients
Set-AddressList “All Users” –IncludedRecipients MailboxUsers
Set-AddressList “All Groups” –IncludedRecipients Mailgroups
Set-AddressList “All Contacts” –IncludedRecipients MailContacts
Set-AddressList "Public Folders" -RecipientFilter { RecipientType -eq 'PublicFolder' }
Set-GlobalAddressList "Default Global Address List" -RecipientFilter {(Alias -ne $null -and (ObjectClass -eq 'user' -or ObjectClass -eq 'contact' -or ObjectClass -eq 'msExchSystemMailbox' -or ObjectClass -eq 'msExchDynamicDistributionList' -or ObjectClass -eq 'group' -or ObjectClass -eq 'publicFolder'))}
```
