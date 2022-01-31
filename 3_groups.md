# 3. Groupes

## I. Create groups
 
|                                            |                                                      |
| ------------------------------------------ | ---------------------------------------------------- |
| How to create a group                      | `$ sudo groupadd GROUPNAME`
| How to delete a group                      | `$ sudo groupdel GROUPNAME`
| How to check if a group exists             | `$ getent group | grep GROUPNAME`


## II. Add users to groups

|                                            |                                                      |
| ------------------------------------------ | ---------------------------------------------------- |
| 1. How to add user to a group              | `$ sudo adduser USERNAME  GROUPNAME` <br>                                                                                                                                  B2bR groups : sudo, user42
| 2. Reboot for changes to take effect       | `$ reboot` for changes to take effect
| How to check if user is added to the group |  `groups USERNAME GROUP` OR `$ getent group sudo`
| How to list any sudo privileges you have   | `$ sudo -l`
| How to verify sudopowers works             | example: `$ sudo whoami`
