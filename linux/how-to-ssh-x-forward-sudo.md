<!-- TITLE: How To Ssh X Forward Sudo -->
<!-- SUBTITLE: A quick summary of How To Ssh X Forward Sudo -->

First you will need a working X11 environment on your local system. There are a number of different options available depending on your OS.

Once you have your local X11 environment working and tested you’re ready to proceed x forwarding sudo.


```text
you@local$ ssh -XC server
 
you@server$ xauth list
[output]
 
you@server$ sudo su - otheruser
 
otheruser@server$ xauth add [paste output from "xauth list"]
 
otheruser@server$ xterm (or other X application)
```
