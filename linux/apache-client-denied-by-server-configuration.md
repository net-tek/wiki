<!-- TITLE: Apache Client Denied By Server Configuration -->
<!-- SUBTITLE: A quick summary of Apache Client Denied By Server Configuration -->

This error means that the access to the directory on the hard disk was denied by an Apache configuration. It could be that access was denied due to an explicit deny directive or due to an attempt to access a folder that is outside of the DocumentRoot. It can also happen when you are proxying and there's no access configured for the proxied location. And it is the default response to a PUT request. 

These are some reasons for this entry to be recorded in your ErrorLog: 

*The default Apache config includes Deny from all in the block the DocumentRoot - this must be changed to allow access! 
*If you change the DocumentRoot, you will need to change the block referring the old root, to the refer to the new root 
*You need a block for every folder outside of your DocumentRoot, i.e. your cgi-bin folder. 
*You need a or block for every Alias. 
*You need a or block for your proxy 