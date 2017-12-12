<!-- TITLE: Apache Client Denied By Server Configuration -->
<!-- SUBTITLE: A quick summary of Apache Client Denied By Server Configuration -->

This error means that the access to the directory on the hard disk was denied by an Apache configuration. It could be that access was denied due to an explicit deny directive or due to an attempt to access a folder that is outside of the DocumentRoot. It can also happen when you are proxying and there's no access configured for the proxied location. And it is the default response to a PUT request. 

These are some reasons for this entry to be recorded in your ErrorLog: 

* The default Apache config includes Deny from all in the block the DocumentRoot - this must be changed to allow access! 
* If you change the DocumentRoot, you will need to change the block referring the old root, to the refer to the new root 
* You need a block for every folder outside of your DocumentRoot, i.e. your cgi-bin folder. 
* You need a or block for every Alias. 
* You need a or block for your proxy 


To fix this problem, look at the line in your ErrorLog, to find out which folder it is trying to access. 
If a block already exists for that folder, make sure it is set to allow access as necessary. If not, add a block to your Apache configuration file, allowing access as required. See the example below for folder /usr/local/awstats/htdocs. 


```apache_conf
<Directory /usr/local/awstats/htdocs>
    Order allow,deny
    Allow from all
</Directory>
```

This directory block will allow Apache to serve files from this location, in response to an incoming request. This assumes either you have an Alias set up somewhere for serving content from this directory or, less likely, that your DocumentRoot is /usr/local or /usr/local/awstats. 


```apache_conf
ProxyPass /foo http://internal.foo.com:8900/
ProxyPassReverse /foo http://internal.foo.com:8900/
<Location /foo>
    Order allow,deny
    Allow from all
</Location>
```

This Location block will allow Apache to proxy content for /foo. This Location block is only needed if there is earlier Proxy or Location block denying access to this resource. Some Linux distributions like Debian put Proxy block with "Deny from all" in their default mod_proxy configuration. 

Example


```text
[Fri Jan 16 15:00:42 2009] [error] [client ::1] client denied by server configuration: /var/www/phpmyadmin/
```

**Adding "Allow from 127.0.0.0/255.0.0.0 ::1/128" to the ACL, will prevent the apache internal process from erroring.**


