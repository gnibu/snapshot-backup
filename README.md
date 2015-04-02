Snapshot-backup
===============

Acript to easily create snapshot backups with rsync

Create a file with the following content 

       #################
       #  Parameters   #
       #################
       
       DEST_USER=root
       DEST_HOST=localhost
       DEST_DIR=/backup/kimsufi
       MAIL_ADDRESS=you@example.com
       SOURCE_USER=root
       
       #options
       SSH_KEY=~backup/.ssh/id_backup-server
       VERBOSE=true
       
       ########################
       # do the real backup   #
       ########################
       
       backup /var var \
       --exclude /var/lock \
       --exclude /var/log \
       --exclude /var/cache \
       --exclude /var/lib/apt \
       --exclude /var/lib/dpkg \
       --exclude /var/cache/apt/archives
       
       backup /etc etc \
       --exclude fstab
       
       backup /root root
       
       backup /home home \
       --exclude backup 

Replace the parameters with ones suitable for your host.
Name the file after the host you want to backup samplehost.example.org
The user must have access ssh access to the source host or alternatively you cam specify an ssh key.

Then run the backup 

     snapshot samplehost.example.org

Place the line above in a cron
Backup will be launched every day and will keep snapshots from approx
000 001 002 003 004 005 010 015 020 025 030 060 090 120 150 180 days ago in the backup dir you specified

Advanced configuration
----------------------

ssh configuration: for more security,  you can add something like the line after before the key used in your .ssh/authorized_key2 file

    command="echo $SSH_ORIGINAL_COMMAND >> ~/ssh_commands.log ; echo $SSH_ORIGINAL_COMMAND | egrep -q '^(rsync --server --sender -vlogDtpr . /(var|etc|root|home)|date)$' &&  eval $SSH_ORIGINAL_COMMAND",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,from="snapshoting.host.org" 

