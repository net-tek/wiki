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

It will extract to `.dropbox-dist`


Run Dropbox:

`$ ~/.dropbox-dist/dropbox`

You should see output like this:

`This client is not linked to any account...`
`Please visit https://www.dropbox.com/cli_link?host_id=7d44a557aa58f285f2da0x67334d02c1 to link this machine.`

Go to the URL given; you should see a success message at the top of your screen.
**Important:** Dropbox will create a ~/Dropbox folder and start synchronizing when you do this. Make sure you've logged in to the correct Dropbox account at www.dropbox.com before going to the URL.
Exit Dropbox by pressing `CTRL+D`

## Installing Dropbox as a service
The following is a modified single-user version of Drazenko D.’s Dropbox daemon script.
Start up your favorite editor, creating `/etc/init.d/dropbox` :

`$ nano /etc/init.d/dropbox`

Insert the following script:


```sh
start() {
    echo "Starting dropbox..."
    start-stop-daemon -b -o -c root -S -x /root/.dropbox-dist/dropbox
}

stop() {
    echo "Stopping dropbox..."
    start-stop-daemon -o -c root -K -x /root/.dropbox-dist/dropbox
}

status() {
        dbpid=$(pgrep -u root dropbox)
        if [ -z $dbpid ] ; then
            echo "dropbox not running."
        else
            echo "dropbox running."
        fi
}

case "" in
  start)
    start
    ;;

  stop)
    stop
    ;;

  restart|reload|force-reload)
    stop
    start
    ;;

  status)
    status
    ;;

  *)
    echo "Usage: /etc/init.d/dropbox {start|stop|reload|force-reload|restart|status}"
    exit 1

esac

exit 0
```

And save and exit the editor.
Set up execute permissions for the script:

`$ chmod +x /etc/init.d/dropbox`

Set the script to load at startup:

`$ update-rc.d dropbox defaults`

Run the script to start Dropbox:

`$ /etc/init.d/dropbox start`

Make sure Dropbox is running:

`$ /etc/init.d/dropbox status`

And you're good to go . Dropbox will now run as a background service when you start your server.

## Backing up to Dropbox
After installing Dropbox, you can use the backup script from my previous post and backup to the Dropbox instead. Like this:

`$ /var/scripts/backup.sh -d ~/Dropbox/backup/lassebunk/daily -s lassebunk -m lassebunk`

Or, you can manually backup files by copying them to the Dropbox folder:

`$ cp myveryimportantfile.tar.gz ~/Dropbox`

## Conclusion
I hope you found this post helpful when creating your own remote backup solution. If you did, please let me know in the comments how you use it .
