## Appimages
+ Place them under `/usr/local/bin/`
+ Make them executable and owned by root.
+ Create desktop files, with firejail in mind, if applicable.

## Custom Scripts
+ If it is to be executed only by root, place under `/usr/local/sbin/`. Make them executable and owned by root. Set permissions to `754` or `750`.
+ If it is to be executed by the root and the other users, place them under `/usr/local/bin/`. Make them executable and owned by root. Set permissions to `755`.
