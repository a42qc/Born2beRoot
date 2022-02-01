# 1. Installation de VirtualBox et Debian

1. Télécharger VirtualBox à partir du "Centre de gestion des logiciels"
2. Installer Debian à partir de l'iso dans "sgoinfre".


# 2. Créer les partitions à partir de l’exemple du bonus
![bonus-partitions.png](https://i.postimg.cc/dVyPKrT7/bonus-partitions.png)


## I. Création disque dur virtuel

|                   |                                                   |
| ----------------: | ------------------------------------------------- |
| Tools -> New      |                                                   |
| Name :            | Born2beRoot                                       |
| Machine Folder :  | the/path/I/want (/sgoinfre/perso/username/folder) |
| Type :            | Linux                                             |
| Version :         | Debian (64-bit)                                   |
| Memory size :     | 1024 MB                                           |
|                   | Create a virtual hard disk now                    |
|                   | VDI (VirtualBow Disk Image)                       |
|                   | Dynamically allocated                             |
|                   | 30.80 GB (even if inGio)                          |


## II. Properties

|           |                                                                    |
| --------: | ------------------------------------------------------------------ |
| General   | -> Advanced <br>                                                                                                                                                   Shared Clipboard AND Drag'n'Drop: Bidirectional (active copy/past)
| Display   | 1. Scale Factor : Max                                              |
| Storage   | 1. Contrôleur : IDE -> click on empty <br>                                                                                                                         2. Optical Drive : IDE Secondary Device 0 <br>                                                                                                                     3. Click on blue disk -> Choose a disk file... -> find ISO Debian  |
| Audio     | 1. Uncheck Enable Audio                                            |
| Network   | 1. Attached to : bridged Adapter <br>                                                                                                                               2. Name : en0 : Ethernet                                           |


## III. Debian installation and configuration 

1. Start VM
2. DO NOT INSTALL THE GRAPHICAL INTERFACE
3. Install

|                              |                               |
| ---------------------------: | ----------------------------- |
| Language :                   | English
| Country :                    | Canada
| Keymap :                     | US english
| Hostname :                   | username42
| Root password :              | UTh1nkImStup1d?
| Full name of the new user :  | your name dude
| Username for your account :  | same thing or something else
| Password for the new user :  | Y0l0Sh!t
| Time zone :                  | Eastern
 

4. Patition disk

|                                                  |                                                                           |
| -----------------------------------------------: | ------------------------------------------------------------------------- |
| Manual                                           |                                                                           | 
| SCSI3 (0,0,0) (sda) - 33,1 GB ATA VBOX HARDDISK  | 1. Create new empty partition table : YES <br>                                                                                                                      2. pri/log 33,1 GB FREE SPACE
| Create a new partition (sda1)                    | 1. New partition size : 525M <br>                                                                                                                                    2. Type : Primary <br>                                                                                                                                              3. Location : Beginning <br>                                                                                                                                        4. Use as : Ext4 journaling file system <br>                                                                                                                        5. Mount point : /boot - static files of the boot loader <br>                                                                                                        6. Done setting up the partition
| pri/log 32,5 GB FREE SPACE                       | 1. Create a new partition (sda5) <br>                                                                                                                                2. Size : max <br>                                                                                                                                                  3. type : Logical <br>                                                                                                                                              4. Use as : Physical volume for encryption <br>                                                                                                                      5. Done setting up the partition
| Configure encrypted volumes                      | 1. Write the changes to disk and configure encrypted volumes : YES <br>                                                                                              2. Create encrypted volumes <br>                                                                                                                                    3. Select /dev/sda5 with the SPACEBAR and confirme with ENTER <br>                                                                                                  4. Done setting up the partition <br>                                                                                                                                5. Finish <br>                                                                                                                                                      6. Erase the data on SCSI3 (0,0,0), partition #5 (sda) : YES <br>                                                                                                    7. Wait... it's overwriting with random shit to prevent meta-information leaks from the encrypted volume. <br>                                                      8. Passphrase
| Configure the Logical Volume Manager             | 1. Write the changes to disks and configure LVM : YES Create volume group <br>                                                                                      2. Volume group name : LVMGroup <br>                                                                                                                                3. Select /dev/mapper/sda5_crypt with the SPACEBAR and confime with ENTER <br>                                                                                      4. Create logical volume <br>                                                                                                                                        5. LVMGroup (32526MB) <br>                                                                                                                                          6. Check the next table -> <br>                                                                                                                                      7. finish partitioning and write changes to disk <br>                                                                                                                8. Write changes : YES

<br>

| Volume & size                        | use as :                    | Mont point :                                      | 
| :----------------------------------: | :-------------------------: | :-----------------------------------------------: | 
| root 10,75G (7% de 10G)              | Ext4 journaling file system | Mount point : / - the root file system
| swap 2,5G (7% de 2,3G)               | swap area [SWAP]            |                                                   |
| home 5,4G (7% de 5G)                 | Ext4 journaling file system | /home - user home directory 
| var 3,25G (7% de 3G)                 | Ext4 journaling file system | /var - variable data 
| srv 3,25G (7% de 3G)                 | Ext4 journaling file system | /srv - data for services provided by this system 
| tmp 3,25G (7% de 3G)                 | Ext4 journaling file system | /tmp - temporary files 
| var-log (un seul tiret) max (finish) | Ext4 journaling file system | ENTER MANUALLY -> /var/log 


5. Last step

|                                |                                                                                              |
| -----------------------------: | -------------------------------------------------------------------------------------------- |
| Configure the package manager  | 1. Scan another CD or DVD : NO <br>                                                                                                                                  2. Debian archive mirror : deb.debian.org <br>                                                                                                                      3. HTTP proxy information : BLANK FOR NONE <br>                                                                                                                    4. wait... <br>                                                                                                                                                    5. Participate in the package usage survey : NO
| Software selection             | ***Ne RIEN cocher SAUF standard system utilities*** (optional, can be installed later) <br>                                                                       Don't press ENTER, use SPACEBAR for selection and deselection! <br>                                                               
| GRUB boot loader               | 1. Install the GRUB boot loader to the master boot record : YES <br>                                                                                                2. Device for boot loader installation : /dev/sda <br>                                                                                                              3. Continue


### IV. Update, check partions and install programs such as Vim and Git

1. Start the VM
2. `$ su -`
3. `$ sudo apt update -y && apt upgrade -y`
4. `$ lsblk` to see if partitions are the same as ask
5. `$ sudo apt install vim` (Text editor very useful!)
6. `$ sudo apt install git` (Use https inside the VM to clone a repo)

### Check if install correctly

`$ dpkg -l | grep PAQUAGE`

### Change the keybord language

1. `$ sudo dpkg-reconfigure keyboard-configuration`
2. Macbookpro
3. US english Macbook
4. Reboot

### Tips regarding copy and paste

It's better to use an external terminal and a [SSH connection](https://github.com/a42qc/Born2beRoot/blob/master/5_UFW_SSH.md).

### Instantané (snapshots)

1. Click on "..."
2. Click on "Instantanés"
3. Click on "Prendre un instantané"
4. Click write on the snapshot desire to start at this moment.

# Documentation :

- [Vidéo démonstrative de comment partitionner le disque dur virtuel](https://www.youtube.com/watch?v=2w-2MX5QrQw)
- [Commande alternative de partionnage: mkpart](https://docs.fedoraproject.org/en-US/quick-docs/creating-a-disk-partition-in-linux/)
- [Comment supprimer des fichiers et des répertoires sous Linux?](https://geekflare.com/fr/remove-files-and-folder-examples/ "https://geekflare.com")
- [Désinstaller proprement ses paquets sur sa distribution](https://linuxfr.org/wiki/desinstaller-proprement-ses-paquets-sur-sa-distribution "https://linuxfr.org")
- [8 Risky Commands in Unix](https://www.proofpoint.com/us/blog/insider-threat-management/8-risky-commands-unix)
