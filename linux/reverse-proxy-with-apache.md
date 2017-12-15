<!-- TITLE: Reverse Proxy With Apache -->

# Integrating an Apache reverse proxy and Sharepoint
Recently I have installed an Apache reverse proxy to delegate external requests to a number of different MS SharePoint installations.
It was straightforward to set up and it worked quite well, with one problem -- The reverse proxy accepts HTTPS connections from clients, but all internal traffic to the SharePoint installations is over plain HTTP. At some point during the user signin process, the SharePoint server redirects to a new page using an absolute link beginning with "http://", since that is the only protocol of the request.
This results in the user being prompted to authenticate twice: once when they hit the HTTPS site, and then once again when their browser is redirected to the plain HTTP site. Worse still is that all secure traffic is taken back to an insecure protocol.
Ideally, it would reload using protocol-independent relative links, but the Windows engineer I was working with said this was not possible.
To fix this, I had to set up a simple RewriteRule for mod_rewrite that redirects all traffic to HTTPS for the virtual hosts listening for HTTP requests:


```apache_conf
RewriteEngine On
RewriteCond %{SERVER_PORT} !443
RewriteRule ^(.*)$ https://urlhere.com/$1 [R,L]
```


This fixed the double-authentication problem and has the added benefit of moving all traffic to a more secure protocol.
Here is the whole HTTP virtual host block for the reverse proxy to SharePoint:


```apache_conf
  ServerName urlhere.com

    Order Deny,Allow
    Allow from all

  ProxyPass / http://10.0.0.100/
  ProxyPassReverse / http://10.0.0.100/

  RewriteEngine On
  RewriteCond %{SERVER_PORT} !443
  RewriteRule ^(.*)$ https://urlhere.com/$1 [R,L]
```
