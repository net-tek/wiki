<!-- TITLE: Mail Enabled Public Folder Recipient Not Found -->

So, the decommissioning in this case was troublesome. The setup exited halfway with an error and after that the setup could not remove all of Exchange, because it could not find the items to remove. Even the setup.log couldn't help me any further. Eventually I decided to manually remove Exchange 2003. The server itself would also be decommissioned; any leftovers on the server would be resolved eventually. 

The new environment worked without any problems after that. But around 24 hours later, the administrators noticed that the mail enabled Public Folders couldn't receive any mail and the sender would get the following NDR: 

**554 5.6.0 STOREDRV.Deliver.Exception:ObjectNotFoundException;
Failed to process message due to a permanent exception with message The Active Directory user wasn't found.**

**Solution**
As stated in the comments in both first posts, the resolution was to remove the (empty) Server container via ADSIedit. In my case it was the server container of the administrative group recently containing the Exchange 2003 server. In my case the location was: 

CN=Servers,CN=First Administrative Group,CN=Administrative Groups,CN=First Organization,CN=Microsoft Exchange,CN=Services,CN=Configuration,
DC=domain,DC=local 

I assume that the failed setup may have had a role in this, although I still find it curious that an empty container could have such a detriment effect on mail enabled Public Folders. 

Possibly clues to the underlying reasons can be found in the 24 hour frame and actions taken by the setup of perhaps Exchange 2003 or specific behavior of Exchange 2010 regarding recipients. I have never experienced this issue with Exchange 2007. 

**Conclusion** 
Check the above mentioned container with ADSIedit after you decommissioned your last Exchange 2000/2003 server. Remove the empty Server container (at you own risk and be sure to have a backup !!).