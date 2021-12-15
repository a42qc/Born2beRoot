# 1. Installation de VirtualBox et Debian

1. Télécharger VirtualBox à partir du "Centre de gestion des logiciels"
2. Installer Debian à partir de l'iso dans "sgoinfre".
      
# 2. Créer les partitions à partir de l’exemple du bonus
![bonus-partitions.png](https://i.postimg.cc/dVyPKrT7/bonus-partitions.png)

## I. Création disque dur virtuel
Tools -> New
      - Name : Born2beRoot
      - Machine Folder : the/path/I/want (/sgoinfre/perso/username/folder)
      - Type : Linux
      - Version : Debian (64-bit)
      - Memory size : 1024 MB
      - Create a virtual hard disk now
      - VDI (VirtualBow Disk Image)
      - Dynamically allocated
      - 30.80 GB (even if inGio)

### II. Properties
Settings
1. Scale Factor : Max

Storage
1. Contrôleur : IDE -> click on empty
2. Optical Drive : IDE Secondary Device 0 
3. Click on blue disk -> Choose a disk file... -> find ISO Debian

Audio
1. Uncheck Enable Audio

Network
1. Attached to : bridged Adapter
2. Name : en0 : Ethernet

### III. Debian installation and configuration 
1. Start VM
2. DO NOT INSTALL THE GRAPHICAL INTERFACE
3. Install
   - Language : English
   - Country : Canada
   - Keymap : Canadian Multilingual
   - Hostname : username42
   - Root password : UTh1nkImStup1d?
   - Full name of the new user : your name dude
   - Username for your account : same thing or something else
   - Password for the new user : Y0l0Sh!t
   - Time zone : Eastern
 
4. Patition disk
   - Manual
   
   - SCSI3 (0,0,0) (sda) - 33,1 GB ATA VBOX HARDDISK
      - Create new empty partition table : YES
      - pri/log 33,1 GB FREE SPACE
   
   - Create a new partition (sda1)
      - New partition size : 525M
      - Type : Primary
      - Location : Beginning
      - Use as : Ext4 journaling file system
      - Mount point : /boot - static files of the boot loader
      - Done setting up the partition
   
   - pri/log 32,5 GB FREE SPACE
      - Create a new partition (sda5)
      - Size : max
      - type : Logical
      - Use as : Ext 4 journaling file system
      - Mount point : do not mount it
      - Done setting up the partition

   - Configure encrypted volumes
      - Write the changes to disk and configure encrypted volumes : YES
      - Create encrypted volumes
      - Select /dev/sda5 with the SPACEBAR and confirme with ENTER
      - Done setting up the partition
      - Finish
      - Erase the data on SCSI3 (0,0,0), partition #5 (sda) : YES
      - Wait... WAIIiiIIIIIIIiiT....... wait it's overwriting with random shit to prevent meta-information leaks from the encrypted volume.

   - Configure the Logical Volume Manager
      - Write the changes to disks and configure LVM : YES
      - Create volume group
      - Volume group name : LVMGroup
      - Select /dev/mapper/sda5_crypt with the SPACEBAR and confime with ENTER
      - Create logical volume
      - LVMGroup (32526MB)
      - Check the next table :
      - finish partitioning and write changes to disk
      - Write changes : YES

| Volume & size | use as : | Mont point : | 
| :----------: | :----------: | :----------: | 
root 10,75G (7% de 10G) | Ext4 journaling file system | Mount point : / - the root file system
swap 2,5G (7% de 2,3G) | swap area [SWAP] | |
home 5,4G (7% de 5G) | Ext4 journaling file system | /home - user home directory 
var 3,25G (7% de 3G) | Ext4 journaling file system | /var - variable data 
srv 3,25G (7% de 3G) | Ext4 journaling file system | /srv - data for services provided by this system 
tmp 3,25G (7% de 3G) | Ext4 journaling file system | /tmp - temporary files 
var-log (un seul tiret) max (finish) | Ext4 journaling file system | ENTER MANUALLY -> /var/log 

5. Last shit
   - Configure the package manager
      - Scan another CD or DVD : NO
      - Debian archive mirror : deb.debian.org
      - HTTP proxy information : BLANK FOR NONE
      - wait...
      - Participate in the package usage survey : NO

   - Software selection
      - ***Ne RIEN cocher***<br/>
         ÇA VEUT DIRE PÈSE PAS SUR ENTER!!!!!<br/>
         Utiliser “space” pour sélectionner ou désélectionner<br/>

   - GRUB boot loader
      - Install the GRUB boot loader to the master boot record : YES
      - Device for boot loader installation : /dev/sda
      - Continue


### IV. Vérifier partitionnage

1. Start the VM
2. Use 'lsblk' command inside the terminal


# Documentation :

- [Vidéo](https://www.youtube.com/watch?v=2w-2MX5QrQw) démonstrative de comment partitionner le disque dur virtuel<br/>
- Commande alternative : [mkpart](https://docs.fedoraproject.org/en-US/quick-docs/creating-a-disk-partition-in-linux/)<br/>
