# EVALUATION PART ONE : PRELIMINARIES AND GENERAL INSTRUCTIONS

1. Clone Git repository on the station
2. Ensure that the `signature.txt` is identical to that of the `.vdi` file located.
3. As a precaution, you can duplicate the initial virutal machine in order to keep a copy.
4. Start the VM


# EVALUATION PART TWO : MANDATORY PART

## PROJECT OVERVIEW

#### Explain the basic fonctioning of the virtual machine.

#### Why did I choose Debian instead of CentOS?
  - Debian is recommanded for beginner. It's easier to install, configure and upgrade. It's stable and there is a lot of packages too.

#### What is a virtual machine?
  - A Virtual Machine (VM) is the **same thing than other physical computer** (laptop, smart phone or server). It has a CPU, memory, disks to store your files, and can connect to the internet if needed. While the parts that make up your computer (called hardware) are physical and tangible, VMs are often thought of as virtual computers or software-defined **computers within physical servers, existing only as code**.

#### Here are a few ways virtual machines are used/purpose :
  - Building and deploying apps to the cloud.
  - **Trying out a new operating system** (OS), including beta releases.
  - Spinning up a new environment to make it simpler and quicker for developers to run dev-test scenarios.
  - Backing up your existing OS.
  - **Accessing virus-infected data** or running an old application by installing an older OS.
  - **Running software or apps on operating systems that they weren't originally intended for**.
  - Pentesting or to **test applications in a safe sandboxed environment**. :)

#### How does a virtual machine work?
  - **Virtualization is the process of creating a software-based**, or "virtual" version of a computer, **with dedicated amounts of CPU, memory, and storage that are "borrowed" from a physical host** computer—such as your personal computer— and/or a remote server—such as a server in a cloud provider's datacenter. A virtual machine is a **computer file**, typically called an image, that behaves like an actual computer. It **can run in a window as a separate computing environment**, often to run a different operating system—or even to function as the user's entire computer experience—as is common on many people's work computers. The virtual machine **is partitioned from the rest of the system**, meaning that the software inside a VM **can't interfere with the host computer's primary operating system**.

#### What is the difference betweet aptitude and APT?
  - Aptitude and apt-get are two of the popular tools which **handle package management**. Both are capable of handling all kinds of activities on packages including **installation, removal, search, etc**. 
  - Aptitude is a high-level package manager
  - APT is lower-level package manager

#### What is AppArmor?
  - AppArmor ("Application Armor") is a **Linux kernel security module that allows the system administrator to restrict programs' capabilities with per-program profiles**. Profiles can allow capabilities like network access, raw socket access, and the permission to read, write, or execute files on matching paths.

#### What is SSH?
  - SSH or Secure Shell is a network communication protocol that enables two computers to communicate (c.f http or hypertext transfer protocol, which is the protocol used to transfer hypertext such as web pages) and share data. An inherent feature of ssh is that the communication between the two computers is encrypted meaning that it is suitable for use on insecure networks.
  - SSH is often used to "login" and perform operations on remote computers but it may also be used for transferring data.

#### How my script works (must display information all every 10 minutes)
|                                  |                                        |
| -------------------------------- | -------------------------------------- |
| `$ sudo crontab -e`              | To open cron inside text editor
| ################################ | ######################################
|


## SIMPLE SETUP

1. No graphical environment at launch.
2. A password will be requested before attempting to connect to this machine.
3. Connect with a user (must not be root).

4. Pay attention to the password chosen, it must follow the rules imposed :
  - .....
5. Check that the UFW service is started :
  - .....
6. Check that the SSH service is started :
  - .....
7. Check that the chosen operating system is Debian :
  - .....


## USER

1. Needs a user with the login of the evaluated student that belongs to "sudo" and "user42" groups.

2. Check password policy

  - How to do that ?
  - Create a new user and assign it a password respecting the subject rules.
 
#### How to create a new user: 
|                                  |                                        |
| -------------------------------- | -------------------------------------- |
| `$ sudo adduser USERNAME`        | Creates a new username
| `$ sudo chage -l USERNAME`       | Verify password expiration
| `$ sudo adduser USERNAME sudo`   | Assign user to sudo
| `$ sudo adduser USERNAME user42` | Assign user to user42

  - Explain how to set up the rules (should be one or two modified files).
3. Create a group named "evaluating", assign it to this user and check that this user belongs to the "evaluating" group.
4. Explain the advantages of this password policy, as well as the advantages and disadvanteages of its implementation.


## HOSTNAME AND PARTITIONS

1. Check that the hostname of the machine is correctly formatted as follow : login42 (longin of the student evaluated)
  - `$ hostname`
2. Modify this hostname by replacing the login with yours, then restart the machine. If on restart, the hostname has not been updated, the evaluation stops here.

3. Restore the machine to the original hostname

4. Check the partitions and compare to the bonus example.
  - `$ lsblk`
5. Explain how LVM works ans what it is all about.


## SUDO

1. Check that the "sudo" program is properly installed on the VM.

2. Assign new user to the "sudo" group.

3. Explain the value and operation of sudo with an exemple

4. Show the implementation of the rules imposed by the subject
  - Verifify that the `/var/log/sudo` folder exist and has at least one file. 
  - Check the contents of the files in this folder. You should see a history of the commands used with sudo.
  - Try to run a command via sudo. See if the file(s) in the `/var/log/sudo` folder have been updated.


## UFW

1. Check that the "UFW" program is properly installed

2. Check that it is working properly

3. Explain basically what UFW is and the value of using it.

4. List the active rules in UFW. A rule must exist for port 4242.

5. Ass a new rule to open port 8080. Check that this one has been added by listing the active rules.

6. Delete this new rule


## SSH

1. Check that the SSH service is properly intalled.

2. Check that it is working properly.

3. Explain basically what SSH is and the value of using it.

4. Verify that the SSH service only uses port 4242.

5. Make sure you cannot use SSH to log in with root

7. Use SSH to log in with the new user.


##





|                                          |                                                             |
| ---------------------------------------- | ----------------------------------------------------------- |
| 1. `$ lsblk`                             | Check partitions
| 2. `$ sudo aa-status`                    | AppArmor status
| 3. `$ getent group sudo`                 | sudo group users
| 4. `$ getent group user42`               | user42 group users
| 5. `$ sudo service ssh status`           | ssh status, yep
| 6. `$ sudo ufw status`                   | ufw status
| 7. `$ ssh username@ipadress -p 4242`     | connect to VM from your host (physical) machine via SSH
| 8. `$ nano /etc/sudoers.d/<filename>`    | yes, sudo config file. You can $ ls /etc/sudoers.d first
| 9. `$ nano /etc/login.defs`              | password expire policy
| 10. `$ nano /etc/pam.d/common-password`  | password policy
| 11. `$ sudo crontab -l`                  | cron schedule
| 12. `$sudo nano /etc/hostname`           | change hostname



Where is sudo logs in /var/log/sudo?
[$cd /var/log/sudo/00/00 && ls]
You will see a lot of directories with names like 01 2B 9S 4D etc. They contain the logs we need.
[$ sudo apt update]
[$ ls]
Now you see that we have a new directory here.
[$ cd <nameofnewdirectory> && ls]
[$ cat log] <- Input log
[$ cat ttyout] <- Output log

How to add and remove port 8080 in UFW?
[$ sudo ufw allow 8080] <- allow
[$ sudo ufw status] <- check
[$ sudo ufw deny 8080] <- deny (yes yes)

How to run script every 30 seconds?
[$ sudo crontab -e]
Remove or commit previous cron "schedule" and add next lines in crontab file
|*************************************************|
| */1 * * * * /path/to/monitoring.sh              |
| */1 * * * * sleep 30s && /path/to/monitoring.sh |
|*************************************************|
To stop script running on boot you just need to remove or commit
|********************************|
| @reboot /path/to/monitoring.sh |
|********************************|
line in crontab file.


# REFERENCES

- [CentOS vs Debian](https://www.openlogic.com/blog/centos-vs-debian)
- [What is a VM](https://azure.microsoft.com/en-us/overview/what-is-a-virtual-machine/#how-do-work)
- [Difference between apt and aptitude](https://www.tecmint.com/difference-between-apt-and-aptitude/)
- [What is AppArmor](https://en.wikipedia.org/wiki/AppArmor)
- [What is SSH](https://www.ucl.ac.uk/isd/what-ssh-and-how-do-i-use-it)
