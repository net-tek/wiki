<!-- TITLE: Backup Linux To Dropbox -->
<!-- SUBTITLE: A quick summary of how to Backup Linux To Dropbox -->

# Using Linux and Dropbox as a remote backup solution
How to create a free remote backup solution using Linux and Dropbox.
In my previous post I wrote about how to backup websites and databases using the command line in Linux.
In this post I am going to show how to extend this setup with Dropbox to make it a full fledged remote backup solution and it's free! (at least for the first 2 GB )

## Obtaining an account from Dropbox
You might want to create a Dropbox account only for taking backups if you don&#8217;t want all your documents and photos synchronized to your Linux server.
Go to www.dropbox.com to create your account.

## Dropbox installation
The installation instructions below are a slightly modified version from the Dropbox community’s to make it as easy as possible.
Start by logging in to your Linux server as the user you want to assign Dropbox to. In this example we will use root:

`$ sudo su`

Change to your home directory:

`$ cd ~`

Download Dropbox.
Stable 32-bit:

`$ wget -O dropbox.tar.gz "http://www.dropbox.com/download/?plat=lnx.x86"`

Or stable 64-bit:

`$ wget -O dropbox.tar.gz "http://www.dropbox.com/download/?plat=lnx.x86_64"`

Extract:

`$ tar -xvzf dropbox.tar.gz`


Run Dropbox:

`$ ~/.dropbox-dist/dropbox`

You should see output like this:

`This client is not linked to any account...`
`Please visit https://www.dropbox.com/cli_link?host_id=7d44a557aa58f285f2da0x67334d02c1 to link this machine.`

Go to the URL given; you should see a success message at the top of your screen.
**Important:** Dropbox will create a ~/Dropbox folder and start synchronizing when you do this. Make sure you've logged in to the correct Dropbox account at www.dropbox.com before going to the URL.
Exit Dropbox by pressing CTRL+C.