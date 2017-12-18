<!-- TITLE: Trouble Uninstalling Exchange 2003 -->

**Having Trouble Uninstalling Exchange...**

I've seen the problem I'm about to describe on the Exchange newsgroups a few times, and I've just been having an email conversation with someone who's had the same problem, so I thought it about time to blog this snippet.
Basically, you may find yourself in the situation one day where you need to decomission an Exchange server. OK, so you move all the mailboxes and public folders off of this server onto a new server and then run the setup program to remove the installation. However, you're greeted with the error detailed in MSKB 279202:
One or more users currently use this mailbox store. These users must be moved to a different mailbox store or be mail disabled before deleting this store.
ID no: c1034a7f
Exchange System Manager
So where are these users? You've moved all of them off this server. The person I was talking with followed the remaining instructions in 279202, which go on to show you how to use LDP to view the remaining mailboxes on the server. The problem was that the result of the LDP output showed that there were only 2 system mailboxes left on the server, which can be ignored.
A good way to try to find these missing users is to use good old Active Directory Users and Computers. Here's what to do:

1. Run ADUC.
2. Right-click your domain at the top, and choose **Find**.
3. Click the **Advanced** tab, and then choose **User** from the **Field** button.
4. From the list of attributes displayed, choose **Exchange Home Server**.
5. Set the **Condition** field to **Ends With** and then type your Exchange server name into the **Value** field. Click **Add** to add this value.
6. Now click the **Find** button, and hopefully you'll see the troublesome user listed in the results window. You should then be able to remove the Exchange attributes from these user accounts and proceed with the uninstall.