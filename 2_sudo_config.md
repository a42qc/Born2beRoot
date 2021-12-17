## 2. Sudo strict configuration

### I. Installation

|                                   |                                    |
| --------------------------------- | ---------------------------------- |
| 1. How to connect as sudo         | `$ su -`
| 2. Update and upgrade             | `$ apt update && apt upgrade`
| 3. How to install                 | `$ apt install sudo`
| 4. Reboot                         | `$ reboot`
| 5. Check if installed correctly   | `$ dpkg -l \| grep sudo`
| 6. How to use sudo-privilege      | `$ sudo COMMAND`
| 7. See chapter 6 (create user) and 4 (add users to groups)


### II. Vim installation

|                                   |                                    |
| --------------------------------- | ---------------------------------- |
| 1. How to install                    | `$ apt install vim`
| 2. Check if installed correctly      | `$ dpkg -l \| grep vim`

### III. Sudo configuration for users

|                                   |                                    |
| --------------------------------- | ---------------------------------- |
|                                   | `$ sudo visudo` 
| OR  modify sudoers.d file         | `$ sudo vim /etc/sudoers.d/FileName`


<br>

|                                   |                                    |
| --------------------------------- | ---------------------------------- |
| Limited tryies when badpassword   | `Defaults        passwd_tries=3`
| Error message when badpassword    | `Defaults        badpass_message="MESSAGEdERREUR"`
| Sudo command log                  | `mkdir /var/log/sudo` <br>                                                                                                                                         `touch /var/log/sudo/commandslog` or whatever you want <br>                                                                                                       `Defaults        logfile="/var/log/sudo/commandslog"`
| To archive all sudo inputs & outputs to a directory | `Defaults        log_input,log_output` <br>                                                                                                                        `Defaults        iolog_dir="/var/log/sudo"`
| Activation du mode TTY            | `Defaults        requiretty`
| Path restrictions                 | `Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`


# Documentation 

- [Useful sudoers configurations](https://www.tecmint.com/sudoers-configurations-for-setting-sudo-in-linux/ "tecmint.com")
