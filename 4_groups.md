# 4. Groupes

## I. Create groups

|                       |                                |
| --------------------- | ------------------------------ |
| How to create a group | `$ sudo groupadd USERNAME`


## II. Add users to groups

|                                            |                                                      |
| ------------------------------------------ | ---------------------------------------------------- |
| 1. How to add user to a group              | `$ sudo adduser USERNAME  GROUP` <br>                                                                                                                                  B2bR groups : sudo, user42
| 2. Reboot for changes to take effect       | `$ reboot` for changes to take effect

<br>

|                                            |                                                      |
| ------------------------------------------ | ---------------------------------------------------- |
| How to check if user is added to the group |  `groups USERNAME GROUP` OR `$ getent group sudo`
| How to verify sudopowers                   | `$ sudo whoami`
| How to list any sudo privileges you have   | `$ sudo -l`
