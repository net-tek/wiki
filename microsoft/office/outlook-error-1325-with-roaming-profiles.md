<!-- TITLE: Outlook Error 1325 With Roaming Profiles -->

http://answers.microsoft.com/en-us/windows/forum/windows_7-files/windows-installer-error-1325-documents-is-not-a/d70411d0-017d-41f0-b5b4-0861b6ca9955

I think I found the solution for this one Mark, as I just encountered the same problem in our environment and here is what I did to resolve it:
 
- Create proper top-level permissions at the root of the folder redirection (eg \\servername\mydocredirfolder) : Grant Authenticated Users the following permissions and apply it to "this folder only" ALLOW : List folders/read data, read permissions, read attributes, read extended attributes, create folders / append data
 
I believe that the MSI is trying to read the free disk space from the server but it can't because it's trying to access the folder above where your my documents redirection is sitting.
 
Good luck!