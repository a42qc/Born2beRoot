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
  ```
  • Expiration : 30 days
  • Minimum 2 days before be able to modify
  • Expiration warning : 7 days
  • Minimum 10 characters with a majuscule and a number. But not more than 3 consecutive identical characters.
  • Username is forbidden
  • At least 7 characters that aren't in the last password.
  • Same thing for root password
  • Don't forget to change existing passwords
  ```
  
5. Check that the UFW service is started :
    - `$ sudo ufw status`

6. Check that the SSH service is started :
    - `$ sudo systemctl status sshd`

7. Check that the chosen operating system is Debian :
    - `$ uname -o`


## USER

1. Needs a user with the login of the evaluated student that belongs to "sudo" and "user42" groups.
    - `$ groups USERNAME`
  
2. Check password policy
    - How to do that ? 
    - Create a new user and assign it a password respecting the subject rules.
      - `$ sudo adduser USERNAME` 
    - Explain how to set up the rules (should be one or two modified files).
      - [Password policy](https://github.com/a42qc/Born2beRoot/blob/master/1_password_policy.md)
      - `$ sudo chage -l USERNAME` Verify password expiration

3. Create a group named "evaluating", assign it to this user and check that this user belongs to the "evaluating" group.
    - `$ sudo groupadd evaluating`
    - `$ sudo adduser USERNAME evaluating` 
    - `$ groups USERNAME`
  
4. Explain the advantages of this password policy, as well as the advantages and disadvanteages of its implementation.
    - Advantages: Better security
    - Disadvanteages: Need to remember a newpassword every mounth, need to change password after modified the restrictions.


## HOSTNAME AND PARTITIONS

1. Check that the hostname of the machine is correctly formatted as follow : login42 (longin of the student evaluated)
    - `$ hostname`
2. Modify this hostname by replacing the login with yours, then restart the machine. If on restart, the hostname has not been updated, the evaluation stops here.
    - Modify hostname: `$ sudo vim /etc/hostname`
    - Modify hostname: `$ sudo vim /etc/hosts`
    - `$ reboot`
3. Restore the machine to the original hostname
    - See 2.
4. Check the partitions and compare to the bonus example.
    - `$ lsblk`
5. Explain how LVM works ans what it is all about.
   - device mapper framework that provides Logical Volume Manager for the linux kernel / gestionnaire de volumes logiques pour le noyau Linux. 
   - Les volumes logiques (LV): ce sont des «partitions virtuelles» (logiques parce qu'elles sont produites par un logiciel                                              sans forcément correspondre à une portion  d'un disque matériel. Les volumes logiques sont constitués d'étendues                                                    de blocs physiques réunis en un seul espace de stockage, et rendus lisibles par le système. On peut les utiliser                                                    comme des partitions ordinaires.


## SUDO

1. Check that the "sudo" program is properly installed on the VM.
    - `$ dpkg -l | grep sudo`
2. Assign new user to the "sudo" group.
    - `$ sudo adduser USERNAME sudo` 
3. Explain the value and operation of sudo with an exemple
    - is a program for Unix-like computer operating systems that enables users to run programs                                                                           with the security privileges of another user, by default the superuser.
    - `$ sudo COMMAND`
4. Show the implementation of the rules imposed by the subject
    - Verifify that the `/var/log/sudo` folder exist and has at least one file. 
    - Check the contents of the files in this folder. You should see a history of the commands used with sudo.
    - Try to run a command via sudo. See if the file(s) in the `/var/log/sudo` folder have been updated.


## UFW

1. Check that the "UFW" program is properly installed
    - `$ dpkg -l | grep UFW`
2. Check that it is working properly
    - `$ sudo ufw status`
3. Explain basically what UFW is and the value of using it.
    - Very simple and effective firewall
    - Firewall is a set of software filters that controls (allow or not open ports) incoming and outgoings traffic in your computer.                                     In simple words, it is a sort of wall between your computer and the outside world.
4. List the active rules in UFW. A rule must exist for port 4242.
    - See 2.
5. Assign a new rule to open port 8080. Check that this one has been added by listing the active rules.
    - `$ sudo ufw allow PORTNUMBER`
6. Delete this new rule
    - `$ sudo ufw delete NUMÉROduPORT` (deny, disable)


## SSH

1. Check that the SSH service is properly installed.
    - `$dpkg -l | grep ssh`
2. Check that it is working properly.
    - `$ sudo systemctl status sshd`
3. Explain basically what SSH is and the value of using it.
    - See Project Overview
4. Verify that the SSH service only uses port 4242.
    - See 2.
5. Make sure you cannot use SSH to log in with root
    - Use another terminal
    - `$ ssh root@IPADDRESS -p 4242`
7. Use SSH to log in with the new user.
    - `$ ssh NEWUSER@IPADDRESS -p 4242`


## Script Monitoring

1. Explain the operation of the script by displaying the code
2. What is cron?
    - cron est un programme qui permet aux utilisateurs des systèmes Unix d’exécuter automatiquement                                                                     des scripts, des commandes ou des logiciels à une date et une heure spécifiée à l’avance, ou selon un cycle défini à l’avance. 
3. How to set up the script so that it runs every 10 minutes when the server starts up.
    - `*/10 * * * * /root/monitoring.sh`
4. How to stop the script without modify the script, restart the VM and verify the script.
    - `$ sudo systemctl stop cron`(affect all processes and reset to start after reboot) OR add `#` before the process line inside cron.


# REFERENCES

- [PDF correction](https://github.com/sltcestloic/born2beroot_correction/blob/master/correction_born2beroot.pdf)
- [CentOS vs Debian](https://www.openlogic.com/blog/centos-vs-debian)
- [What is a VM](https://azure.microsoft.com/en-us/overview/what-is-a-virtual-machine/#how-do-work)
- [Difference between apt and aptitude](https://www.tecmint.com/difference-between-apt-and-aptitude/)
- [What is AppArmor](https://en.wikipedia.org/wiki/AppArmor)
- [What is SSH](https://www.ucl.ac.uk/isd/what-ssh-and-how-do-i-use-it)
- [What is LVM](https://wiki.archlinux.org/title/LVM_(Fran%C3%A7ais))
- [What is UFW](https://averagelinuxuser.com/linux-firewall/)
- [Guide UFW](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands-fr)
