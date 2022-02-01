## 1. Sudo strict configuration

### I. Installation

|                                                                |                                    |
| -------------------------------------------------------------- | ---------------------------------- |
| 1. Connect as sudo                                             | `$ su -`
| 2. Install the package                                         | `$ apt install sudo -y`
| 3. Create sudo directory                                       | `$ mkdir /var/log/sudo`
| 4. Create log file for sudoers commands                        | `$ touch commandslog`
| 3. Reboot                                                      | `$ reboot`
| 4. Check if installed correctly                                | `$ dpkg -l \| grep sudo`
|    How to use sudo-privilege                                   | `$ sudo COMMAND`
|    See chapter 4 ([create user](https://github.com/a42qc/Born2beRoot/blob/master/4_users.md)) and 3 ([add users to groups](https://github.com/a42qc/Born2beRoot/blob/master/3_groups.md))     |



### II. Sudo configuration for users

|                                   |                                       |
| --------------------------------- | ------------------------------------- |
| With command                      | `$ sudo visudo` 
| OR modify sudoers.d file          | `$ sudo vim /etc/sudoers.d/FileName`


<br>

|                                                     |                                                         |
| --------------------------------------------------- | ------------------------------------------------------- |
| Limited tryies when badpassword                     | `Defaults        passwd_tries=3`
| Error message when badpassword                      | `Defaults        badpass_message="MESSAGEdERREUR"`
| Sudo command log                                    | `Defaults        logfile="/var/log/sudo/commandslog"`
| To archive all sudo inputs & outputs to a directory | `Defaults        log_input,log_output` <br>                                                                                                                         `Defaults        iolog_dir="/var/log/sudo"`
| Activation du mode TTY                              | `Defaults        requiretty` 
| Path restrictions                                   | `Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`

<br>

requiertty : use sudo only inside a session

# Documentation 

- [Useful sudoers configurations](https://www.tecmint.com/sudoers-configurations-for-setting-sudo-in-linux/ "tecmint.com")
