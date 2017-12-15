<!-- TITLE: Reset Admin Password Fortigate Forti Mail Forti Analyzer Etc -->

Resetting a lost Fortigate Admin Password
If you have lost the admin password for a Fortigate you can reset it if you have physical access to the box.

**Heads up: You have to type the userid and password within a 15 seconds of the login prompt first appearing. If you take too much time you should reboot the firewall again.**
Connect the console cable to the Fortigate and fire up your favorite terminal emulator
Reboot the firewall unit.
At the console login prompt, type in "maintainer" as the userid.
Type in bcpbFGTxxxxxxxxxxxxx as the password. xxxxxxxxxxxxx will be the S/N of the Fortigate. The serial number is case sensitive so for example you should use FGT60B, not FGT60b. If that does NOT work try bcpbxxxxxxxxxxxxx as the password.
After logging in, change the admin password:


```text
config system admin
edit admin
set password 
next
end
```
