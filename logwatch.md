Create/Edit `/etc/logwatch/conf/logwatch.conf`, put and replace `USERNAME@HOSTNAME`:
```
Output = mail
Format = html
MailTo = USERNAME@HOSTNAME
MailFrom = Logwatch
Range = yesterday
Detail = 1
Service = All
```
A daily cron job should also have been created when logwatch was installed.

If you want to read local mails in GUI client (Evolution, Thunderbird), you might need a cron job to fetch mail from the mail spool in `/var` to `mbox` in `$HOME` if using `firejail` (at least for Evolution). You can also tweak firejail profile, but instead of making overall config less secure, take your own mail to the home dir. To do so, edit your user's crontab by `crontab -e` and add the following (replace `USERNAME`):
```
# To fetch mail from mail spool to mbox
# For reading with Evolution under Firejail sandbox
# Suppress output as it interferes with emptying the mailbox
@hourly /home/USERNAME/cron/fetchmail > /dev/null 2>&1
```
Note that you need to use **absolute paths both in crontabs and cronjob scripts.** And the sample cron script would be like (replace `USERNAME`):
```shell
#!/bin/bash

MYUSERNAME="USERNAME"

# Exit script as soon as a command fails.
set -o errexit

# Check if the mail spool is empty
if [ -s /var/mail/$MYUSERNAME ]
then
    cat /var/mail/$MYUSERNAME | tee -a /home/$MYUSERNAME/mbox
    # Add a newline at the end
    echo "" >> /home/$MYUSERNAME/mbox
    # Empty the mail spool
    cat /dev/null > /var/mail/$MYUSERNAME
else
    exit
fi
```
