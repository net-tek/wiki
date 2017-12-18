<!-- TITLE: Offline Address Book Public Folder Database Still Points To Old Server -->

After migrating your Exchange server (Ive seen this in transition to exchange 2007 and 2010) the PublicFolderDatabase for your OfflineAddressBook is still pointing to the old servers public folder store.
When you run the get-OfflineAddressBook | fl command in a exchange management shell on your new server, you get a result like this:

![Galpublicfolderdb 1](/uploads/galpublicfolderdb-1.jpg "Galpublicfolderdb 1")

At Server you see the new servername and the PublicFolderDatabase is still pointing to your old server. Public folder replicas and offline address book generation server are already moved to the new server.
Solution: I found if you do the following steps you can change the PublicFolderDatabase.
First start adsiedit and browse to CN=Configuration, CN=Services, CN=Microsoft Exchange, CN=First Organization, CN=Address Lists Container, CN=Offline Address Lists and open the properties of CN=Default Offline Address List

![Galpublicfolderdb 2](/uploads/galpublicfolderdb-2.jpg "Galpublicfolderdb 2")

Look for the siteFolderServer attribute, here you will see the old public folder store. Choose clear and close with ok, now you may close adsiedit.
Now go to the exchange management console, Organization Configuration, Mailbox, Offline Address Book open the properties of the Default Offline Address List and go to the tab distribution.

![Galpublicfolderdb 3](/uploads/galpublicfolderdb-3.jpg "Galpublicfolderdb 3")

Uncheck Outlook version 2 and 3 at client support and Enable public folder distribution&. Make sure Web-based distribution is enabled. Choose apply and ok, then right click on Default Offline Address List and choose update. After that go back to properties and distribution and check Outlook client support version 2 and 3 and Enable public folder distibution&. Again choose apply and ok and right click and choose update.

When you go back to the exchange management shell and repeat get-OfflineAddressBook | fl you now will see the public folder store on your new server.