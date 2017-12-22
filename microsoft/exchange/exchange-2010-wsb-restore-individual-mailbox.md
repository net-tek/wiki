<!-- TITLE: Exchange 2010 Wsb Restore Individual Mailbox -->

# Restoring an Exchange Server 2010 Mailbox Database to an Alternate Location
 
The first step is to perform a restore of the mailbox server backup, redirecting the restore to an alternate location on the server.  Start by launching Windows Server Backup, and then in the Actions pane click on Recover to start the Recovery Wizard.
Choose the location of the backup you wish to restore from, and click Next to continue.
Choose the backup date and time to restore from, and click Next to continue.
For Exchange Server 2010 mailbox database restores choose Applications as the Recovery Type.  Click Next to continue.
Choose Exchange as the application to recover, and check the box to not perform a roll forward of the database.  Click Next to continue.
**This step is very important**.  Select **Recover to another location** and enter the path to a folder that is different to the location of the live mailbox database or log files.  Click Next to continue.
At the confirmation screen if you are happy with the selections you’ve made click on Recover to start the restore.
When the restore has completed successfully you can close the Recovery Wizard.
On the Exchange Server 2010 mailbox server where the database files were restored open Windows Explorer and look at the folder where the restored files are located.  Notice how the recovery process has created the original folder structure for the data under the D:\Recovery folder.
Those restored paths are important for the next steps.

## Bringing the Restored Database to a Clean Shutdown State with ESEUtil

The restored database file will be in a state known as “dirty shutdown”.  You can confirm this by running the following ESEUtil command, specifying the path to the restored .edb file on your server.


```powershell
[PS] D:\>eseutil /mh 'D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb

Extensible Storage Engine Utilities for Microsoft(R) Exchange Server
Version 14.01
Copyright (C) Microsoft Corporation. All Rights Reserved.

Initiating FILE DUMP mode...
         Database: D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb

DATABASE HEADER:
Checksum Information:
Expected Checksum: 0x1d0abd9b
  Actual Checksum: 0x1d0abd9b

Fields:
        File Type: Database
         Checksum: 0x1d0abd9b
   Format ulMagic: 0x89abcdef
   Engine ulMagic: 0x89abcdef
 Format ulVersion: 0x620,17
 Engine ulVersion: 0x620,17
Created ulVersion: 0x620,17
     DB Signature: Create time:12/03/2010 21:20:08 Rand:715244149 Computer:
         cbDbPage: 32768
           dbtime: 21550 (0x542e)
            State: Dirty Shutdown
     Log Required: 143-143 (0x8f-0x8f)
```

Next check the state of the log files with the following ESEUtil command, specifying the path to the restored log files. Note the end of the path is the log file prefix, in this case “E00″.


```powershell
[PS] D:\>eseutil /ml D:\Recovery\E_\Logs\EX201\E00

Extensible Storage Engine Utilities for Microsoft(R) Exchange Server
Version 14.01
Copyright (C) Microsoft Corporation. All Rights Reserved.

Initiating FILE DUMP mode...

Verifying log files...
     Base name: E00

      Log file: D:\Recovery\E_\Logs\EX201\E000000007B.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000007C.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000007D.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000007E.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000007F.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000080.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000081.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000082.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000083.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000084.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000085.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000086.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000087.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000088.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000089.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000008A.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000008B.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000008C.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000008D.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000008E.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E000000008F.log - OK
      Log file: D:\Recovery\E_\Logs\EX201\E0000000090.log - OK

No damaged log files were found.

Operation completed successfully in 0.922 seconds.
```

Now we can run ESEUtil in recovery mode to bring the database into a clean shutdown state.


```powershell
[PS] D:\>eseutil /r E00 /i /l D:\Recovery\E_\Logs\EX201 /d 'D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb'

Extensible Storage Engine Utilities for Microsoft(R) Exchange Server
Version 14.01
Copyright (C) Microsoft Corporation. All Rights Reserved.

Initiating RECOVERY mode...
    Logfile base name: E00
            Log files: D:\Recovery\E_\Logs\EX201
         System files:
   Database Directory: D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb

Performing soft recovery...
                      Restore Status (% complete)

          0    10   20   30   40   50   60   70   80   90  100
          |----|----|----|----|----|----|----|----|----|----|
          ...................................................

Operation completed successfully in 0.985 seconds.
```

Now run ESEUtil to check the database state again.


```powershell
[PS] D:\>eseutil /mh 'D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb' | findstr "State:"
            State: Clean Shutdown
```

Note: if the database is still in a dirty shutdown state you can try a repair using ESEUtil /p instead.


```powershell
[PS] D:\>eseutil /p 'D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb'

Extensible Storage Engine Utilities for Microsoft(R) Exchange Server
Version 14.01
Copyright (C) Microsoft Corporation. All Rights Reserved.

Initiating REPAIR mode...
        Database: .\Mailbox Database EX2 01.edb
  Temp. Database: TEMPREPAIR1492.EDB

Checking database integrity.

The database is not up-to-date. This operation may find that
this database is corrupt because data from the log files has
yet to be placed in the database.

To ensure the database is up-to-date please use the 'Recovery' operation.

                     Scanning Status (% complete)

          0    10   20   30   40   50   60   70   80   90  100
          |----|----|----|----|----|----|----|----|----|----|
          ...................................................

Integrity check successful.

Note:
  It is recommended that you immediately perform a full backup
  of this database. If you restore a backup made before the
  repair, the database will be rolled back to the state
  it was in at the time of that backup.

Operation completed successfully in 3.547 seconds.
```

## Creating an Exchange Server 2010 Recovery Database
The next stage of the recovery process is creating the Recovery Database.  Launch the Exchange Management Shell.  Run the **New-MailboxDatabase** cmdlet with the following parameters:

* Recovery:$true (specifies that the database will be a Recovery Database)
* EdbFilePath (the path to the restored mailbox database file)
* LogFolderPath (the path to be used for transaction log files, which must be an empty folder)
* Server (the server that the recovery is being performed on)
 
In this example the following command is run.


```powershell
[PS] D:\>New-MailboxDatabase RecoveryDB -Server EX2 -Recovery:$true -EdbFilePath 'D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb' -LogFolderPath 'D:\Recovery\E_\Logs\EX201-RecoveryDB'

WARNING: Recovery database 'RecoveryDB' was created using existing file
D:\Recovery\D_\Data\EX201\Mailbox Database EX2 01.edb. The database
must be brought into a clean shutdown state before it can be mounted.

Name                           Server          Recovery        ReplicationType
----                           ------          --------        ---------------
RecoveryDB                     EX2             True            None
```

Note the warning about the database not being in a clean shutdown state. Since we’ve already brought the database to a clean shutdown state we can now mount the recovery database.


```powershell
[PS] D:\>Mount-Database RecoveryDB
```

## Restoring Mailbox Items from a Recovery Database

With the recovery database mounted we can now proceed with mailbox item restores.  You can see the available items to restore by looking at the mailbox statistics for the recovery database.


```powershell
[PS] D:\>Get-MailboxStatistics -Database RecoveryDB

DisplayName               ItemCount    StorageLimitStatus
-----------               ---------    ------------------
Alex Heyne                11                   BelowLimit
SystemMailbox{f13446dd... 1                    BelowLimit
```

To restore all mailbox items into a sub-folder of the existing mailbox so that they can be inspected use the following command.


```powershell
[PS] D:\>Restore-Mailbox -Identity "Alex Heyne" -RecoveryDatabase RecoveryDB -RecoveryMailbox "Alex Heyne" -TargetFolder Restore

Confirm
Are you sure you want to perform this action?
Recovering mailbox content from mailbox 'Alex Heyne' in the recovery database 'RecoveryDB' to the mailbox for 'Alex
Heyne (Alex.Heyne@exchangeserverpro.net)'. This operation may take a long time to complete.
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): y
```

The restored items will now be visible in the mailbox.

## Restoring Individual Mailbox Folders

Here you can see that I am using the -IncludeFolders parameter to specify that only data from the Inbox should be restored from the mailbox in the recovery database:


```powershell
New-MailboxRestoreRequest -SourceDatabase RecoveryDB -SourceStoreMailbox administrator -TargetMailbox administrator -IncludeFolders '#Inbox#'
```

## Restoring to an Alternate Mailbox

By default, the New-MailboxRestoreRequest cmdlet looks for a matching LegacyExchangeDN on the source and destination mailbox, or checks to see that an X500 proxy address on the target mailbox corresponds to the LegacyExchangeDN on the source mailbox. This ensures that you do not accidentially restore mailbox data to the wrong location. If you need to restore data to an alternate mailbox, you can use the -**AllowLegacyDNMismatch** switch parameter:


```powershell
New-MailboxRestoreRequest -SourceDatabase RecoveryDB -SourceStoreMailbox 'Mike Pfeiffer' -TargetMailbox administrator -TargetRootFolder Restore -AllowLegacyDNMismatch
```

In this example, I am restoring the data from my mailbox in the recovery database to a sub-folder of the administrator mailbox called Restore.
Be careful when restoring to alternate mailboxes. If you omit the -**TargetRootFolder parameter**, the data will be restored and merged into the existing folders in the target mailbox. On the other hand, that might be exactly what you want — if so, just remove the -TargetRootFolder parameter.


# Troubleshooting

## Dealing with Multiple Mailboxes with the same DisplayName
 
It's possible that a Recovery database will have multiple mailboxes with the same display name. This can happen if there were one or more disconnected versions of a mailbox, in addition to an active mailbox, in the same database during the time of the back up. In this case, you can use the **MailboxGuid** value to identify the source mailbox when doing a restore. Consider the following:


```powershell
Get-MailboxStatistics -Database RecoveryDB | ?{$_.DisplayName -like 'Isabel*'} | fl DisplayName,MailboxGuid,DisconnectDate
```

Here you can see that there are two mailboxes with the same display name in the Recovery database. One has a DisconnectDate defined, meaning it is a disconnected mailbox, and the other one does not which means it was the active mailbox in the database at the time of the backup. If you run into a scenario where there are multiple mailboxes in a database with the same display name, use the above command to determine the **MailboxGuid** of each mailbox. You can then use this value to identify the correct mailbox when performing a restore.


```powershell
New-MailboxRestoreRequest -SourceDatabase RecoveryDB -SourceStoreMailbox 4a1d2118-b8cc-456c-9fd9-cd9af1f549d0 -TargetMailbox ihill
```
