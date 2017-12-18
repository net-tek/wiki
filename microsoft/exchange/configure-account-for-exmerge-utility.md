<!-- TITLE: Configure Account For Exmerge Utility -->

# This article discusses how to configure an account to use the Microsoft Exchange...
This article discusses how to configure an account to use the Microsoft Exchange Server Mailbox Merge (ExMerge) utility in Microsoft Exchange 2000 Server and in Microsoft Exchange Server 2003. 

The account that is used to run the ExMerge utility must have the Send As permission and the Receive As permission on the mailbox store that the ExMerge utility will be used on. By default, the following permissions are assigned when you install Exchange:

* In Exchange 2000, built-in administrator accounts and built-in administrative groups inherit the Deny permission for both the Send As and the Receive As permissions.
* In Exchange 2003, built-in administrator accounts and built-in administrative groups inherit the Allow permission together with the Deny permission for the Send As permission and for the Receive As permission.

**Note** Some permissions take precedence over others. Typically, the Deny permission overrides the Allow permission. However, inherited Deny permissions do not prevent access to an object if the object has an explicit Allow permission entry. Explicit permissions take precedence over inherited permissions, even inherited Deny permissions.
To use the ExMerge utility without being restricted by these inherited permissions, we recommend that you create a new security group, add members to the group, and then grant permissions to the security group on the Exchange mailbox store. In Exchange 2003, you must use a security group to override inherited Send As permissions and Receive As permissions. In Exchange 2000, you can apply explicit permissions to an individual account to override inherited Send As and Receive As permissions. However, we recommend that you use a security group to apply permissions in both versions of Exchange.

## To create a new security group and to add accounts to this group, follow these steps
To create a new security group and to add accounts to this group, follow these steps:
1. Click Start, point to Programs, point to Microsoft Exchange, and then click Active Directory Users and Computers.
2. Expand your domain name.
3. Right-click the organizational unit or the container where you want to create the new security group, point to New, and then click Group.
4. In the Group name box, type a name for the group.
5. Under Group type, make sure that the Security option is selected.
6. Click Next two times, and then click Finish.
7. Right-click the group that you created, and then click Properties.
8. Click the Members tab, and then click Add.
9. In the Select Users, Contacts, or Computers dialog box, add the accounts or the groups that you want to use to run the ExMerge utility, and then click OK two times.

Note If you are running Exchange on a Microsoft Windows Server 2003-based computer, the list of accounts and groups does not appear in the Select Users, Contacts, or Computers dialog box. To locate the user account or the security group, follow these steps:
1.     In the Select Users, Contacts, or Computers dialog box, click Advanced, and then click Find Now.
2.     Locate and then click the account or the security group in the Search results list, and then click OK.
10.  Delegate the Exchange View Only Administrator role of the related administrative group to the security group that you created:
1.     On the Exchange computer, click Start, point to Programs, point to Microsoft Exchange, and then click System Manager.
2.     If administrative groups are enabled, expand Administrative Groups, and then expand your administrative group.
3.     Right-click the related administrative group, and then click Delegate control.
4.     In Exchange Administration Delegation Wizard, click Next, and then click Add.
5.     Click the security group that you just created in the Group box, click Exchange View Only Administrator in the Role box, and then click OK.
6.     Click Next, and then click Finish.
To grant mailbox permissions to the security group that you created, follow these steps:
1.     On the Exchange computer, click Start, point to Programs, point to Microsoft Exchange, and then click System Manager.
2.     If administrative groups are enabled, expand Administrative Groups, and then expand your administrative group.
3.     Expand Servers, expand your Exchange computer, and then expand the storage group that contains the mailbox store that you want to use with the ExMerge utility. For example, expand First Storage Group.
4.     Right-click the mailbox store that you want to use with the ExMerge utility, and then click Properties.
5.     Click the Security tab.
6.     Click Add.
7.     In the Select Users, Contacts, or Computers dialog box, add the security group that you created earlier, and then click OK two times.

When you add the security group, the Allow permission check box is automatically selected for all permissions on the mailbox store. This includes the Receive As and the Send As permissions that are required for the accounts that will use the ExMerge utility to access all mailboxes in the mailbox store.
For more information about how Exchange 2000 and Exchange 2003 use permissions to control access to objects in the Exchange store, visit the following Microsoft Web site to download the "Working with Store Permissions in Microsoft Exchange 2000 and 2003" document:
http://www.microsoft.com/downloads/details.aspx?familyid=2ae266f0-16b7-40d7-94d9-c8be0e968a57&displaylang=en
For more information about the ExMerge utility, click the following article number to view the article in the Microsoft Knowledge Base:
174197  Microsoft Exchange Mailbox Merge program (Exmerge.exe) information
For more information, click the following article number to view the article in the Microsoft Knowledge Base:
288446  Error message: "Error encountered getting mailbox information from the private information store database on server"
If the user runs the ExMerge utility without administrative privileges on the computer that is running the ExMerge utility, the user must be granted "Create Global Object" rights. For more information, click the following article number to view the article in the Microsoft Knowledge Base:
821546  Overview of the "Impersonate a Client After Authentication" and the "Create Global Objects" security settings
Additionally, the user must be granted full control to the MAPI profile. For more information, click the following article number to view the article in the Microsoft Knowledge Base:
288446  Error message: "Error encountered getting mailbox information from the private information store database on server"