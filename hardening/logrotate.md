## Logrotate
Software bugs might result in log spamming, and eventually a denial-of-service.

+ Append `maxsize 20M` to `/etc/logrotate.conf`. `maxsize` tells logrotate that it can rotate either when the size limit has been reached OR when an appropriate time has passed.

+ Logrotate has a daily cron job by default. Make it hourly:
`sudo mv /etc/cron.daily/logrotate /etc/cron.hourly/`
