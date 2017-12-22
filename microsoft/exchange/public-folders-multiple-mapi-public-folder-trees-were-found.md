<!-- TITLE: Public Folders Multiple Mapi Public Folder Trees Were Found -->

Try the following steps.
Open the ADSIEDIT.msc

Configuration [<domainController>.contoso.com] 
CN=Configuration,DC=contoso,DC=com 
CN=Services 
CN=Microsoft Exchange 
CN=<OrganizationName> 
CN=Administrative Groups 
CN=<AdministrativeGroupName> 
CN=Folder Hierarchies 

In the details pane, right-click CN=<PublicFoldersTree>, and then click Properties. 
Important: 
On the Attribute Editor tab, click msExchPFTreeType, and then click Edit. 
Change the Value Back to 1.



-----


I know that you have reverted this change following the technet article.
But the article says that if you have multiple CN=<PublicFoldersTree> under the Folder Hierarchies.
I am sure that you have only one Public Folder Tree under the Folder Hieararchies.


-----



Now do the following.
In ADSIEDIT navigate to

Configuration [<domainController>.contoso.com] 
CN=Configuration,DC=contoso,DC=com 
CN=Services 
CN=Microsoft Exchange 

Highlight the Microsoft Exchange Container.
Check on the Right Hand Pane.
See if you see any entry named CN = Public Folders
Delete that off.
Then restart the Microsoft Exchange System Attendant Service.
I am also posting a Screen Shot for the Same.