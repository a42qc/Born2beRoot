# Born2beRoot - Débuter en virtualisation 
Guide d'installation et de configuration <br/>
[PDF du projet en français](https://cdn.intra.42.fr/pdf/pdf/24446/fr.subject.pdf "lien vers le PDF") 

Création d'une machine virtuelle sous Debian 10.
Apprendre à utiliser la connection SSH, 

## Table des matières

[***Commandes Debian***](#commandesdebian)<br/>
[***Ressources***](#ressources)<br/>
[Tutoriels README.md](#tutoreadme)<br/>
[Turoriels Debian](#tutodebian)<br/>

## <a name="commandesdebian">Commandes Debian</a>
| Commandes    | Description |
| :----------: | ----------- |
touch NOMduFICHIER | Créer fichier
mkdir NOMduDOSSIER | Créer dossier
chmod PERMISSIONS NOMduFICHIERouDOSSIER | Permissions <br />[How to use the chmod command](https://www.howtogeek.com/437958/how-to-use-the-chmod-command-on-linux/ "howtogeek.com")<br /> [Wiki chmod](https://fr.wikipedia.org/wiki/Chmod "Wikipedia is nice")
chown UTILISATEURouGROUPE CIBLE| [How to use the chown command](https://linuxize.com/post/linux-chown-command/ "linuxize.com")
cd NOMduDOSSIER | Changer de dossier
cd .. | Changer dossier vers parent
cd ../DOSSIER | Changer de dossier vers dossier dans le même dossier parent
cd ~ (ou juste cd) | dossier racine
grep MOTquejeCHERCHE /dans/un/fichier | Effectuer une recherche
dpkg -l \| grep MonPAQUET | Vérifier si paquet installé 
mv SOURCE DESTINATION | Déplacer fichier/dossier
cp -r SOURCE DESTINATION | Copier fichier/dossier
rm | Supprimer fichier
rmdir | Supprimer dossier vide
rm -r | Supprimer dossier et son contenu
rm -rf | DANGER : deletes everything in its path, including files on hard drives or connected devices.
apt-get update && apt-get upgrade | Faire avant installation d'un paquet
apt-get install MonPAQUET | Installer paquet (-y ou The --yes flag simply answers yes to any questions that come up so the install won't hang waiting for someone to press a key. )
sudo apt-get --purge autoremove  PAQUETàSUPPRIMER | Supprimer un paquet et ses dépendances
reboot | Redémarrer VM 
poweroff | Éteindre VM
hostnamectl | 
uname -r | Version de Kernel
hostname -I | Adresse IP
ssh NOMdUTILISATEUR@ADRESSEIPduSERVEUR -p NUMduPORT | Connection SSH à patir d'un terminal

## <a name="ressources">Ressources</a>

### <a name="tutoreadme">Tutoriels README.md</a>

[Index de ressources](https://www.makeareadme.com/#more-documentation "makeareadme.com")<br/>
[Guide pour bien commencer avec Markdown](https://blog.wax-o.com/2014/04/tutoriel-un-guide-pour-bien-commencer-avec-markdown/ "blog.wax-o.com")<br/>
[Cheatsheet Markdown](https://github.com/tchapi/markdown-cheatsheet/blob/master/README.md#heading-1 "github.com")
[HTML tutorial](https://www.w3schools.com/html/ "w3schools.com")

### <a name="tutodebian">Tutoriels Debian</a>

[Comment supprimer des fichiers et des répertoires sous Linux?](https://geekflare.com/fr/remove-files-and-folder-examples/ "https://geekflare.com")<br/>
[Désinstaller proprement ses paquets sur sa distribution](https://linuxfr.org/wiki/desinstaller-proprement-ses-paquets-sur-sa-distribution "https://linuxfr.org")<br/>
[8 Risky Commands in Unix](https://www.proofpoint.com/us/blog/insider-threat-management/8-risky-commands-unix)



# Configuration de la VM

## 1. Politique de mot de passe fort
'''
Pour mettre en place une politique de mot de passe fort, il faudra remplir les conditions suivantes :

• Votre mot de passe devra expirer tous les 30 jours.

• Le nombre minimum de jours avant de pouvoir modifier un mot de passe sera configuré à 2.

• L’utilisateur devra recevoir un avertissement 7 jours avant que son mot de passe n’expire.

• Votre mot de passe sera de 10 caractères minimums dont une majuscule et un chiffre, et ne devra pas comporter plus de 3 caractères identiques consécutifs.

• Le mot de passe ne devra pas comporter le nom de l’utilisateur.

• Le mot de passe devra comporter au moins 7 caractères qui ne sont pas présents dans l’ancien mot de passe.

• Bien entendu votre mot de passe root devra suivre cette politique.

Après avoir mis en place vos fichiers de configuration, il faudra
changer tous les mots de passe des comptes présents sur la machine
virtuelle, compte root inclus.
'''

### Expiration (password age)
#### Méthode #1 login.defs
Note that the above-configured policy will only apply on the newly created users. To apply this policy to an existing user, use “chage” command.

1. Modifier fichier: 
    * 'sudo vim /etc/login.defs`

- Expiration mot de passe : 
      * `PASS_MAX_DAYS 30` 
- Nb min de jours avant de pouvoir modifier : 
      * `PASS_MIN_DAYS 2`
- Avertissement x jour avant expiration : 
      * `PASS_WARN_AGE 7`
[man login.defs](http://manpages.ubuntu.com/manpages/cosmic/fr/man5/login.defs.5.html#:~:text=Le%20fichier%20%2Fetc%2Flogin.,aura%20probablement%20des%20cons%C3%A9quences%20ind%C3%A9sirables "MANPAGES.UBUNTU.COM")

#### Méthode #2 chage - Expiration applicable sur utilisateurs existants
Note : To execute the chage command, you must be the owner of the account or have root privilege otherwise, you will not be able to view or modify the expiry policy. <br?>
<br/>
To use chage command, syntax is: <br/>
      * `$ chage [options] username`

##### To view the current password expiry/aging details, the command is :
   * `$ sudo chage –l USERNAME`
##### To configure the maximum No. of days after which a user should change the password.
   * `$ sudo chage -M <No./_of_days> <user_name>`
##### To configure the minimum No. of days required between the change of password.
   * `$ sudo chage -m <No._of_days> <user_name>`
##### To configure warning prior to password expiration :
   * `$ sudo chage -W <No._of_days> <user_name>`
exemple : chage -M 30 -m 2 -W 7 USERNAME


### Complexité (password strength) pam.d
#### I. Installer libpam-pwquality
   * `$ sudo apt install libpam-pwquality` (ou 'apt-get')
#### II. Vérifier que c'est bien installé : 
   * `dpkg -l | grep libpam-pwquality`
#### III. Appliquer la complexit/
   * `$ sudo vim /etc/pam.d/common-password`
     * ÉCRIRE SUR LA MÊME LIGNE QUE `password	requisite	pam_pwquality.so retry=3`
Ajouter les restrictions : | |
| :---: | --- |
`retry=3` | No. of consecutive times a user can enter an incorrect password.
`minlen=10` | Minimum length of password (10 caractères)
`maxrepeat=3` | To set a maximum of x consecutive identical characters
`difok=7` | No. of character that can be similar to the old password
`lcredit=` | Min No . of lowercase letters
`ucredit=-1` | Min No. of uppercase letters
`dcredit=-1` | Min No. of digits
`ocredit=` | Min No. of symbols
`reject_username` | Rejects the password containing the user name (oui)
`enforce_for_root` | Also **enforce the policy for the root user (oui)

~??? To check if the password contains the user name in some form (enabled if the value is not 0) usercheck=  <br/>
exemple : password        requisite	pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root <br/>
Now reboot the system to apply the changes in the password policy.


How to [enable and enforce secure password policies](https://linuxhint.com/secure_password_policies_ubuntu/)
How to [enforce password complexity](https://www.networkworld.com/article/2726217/how-to-enforce-password-complexity-on-linux.html)
How To Set [Password Policies](https://ostechnix.com/how-to-set-password-policies-in-linux/)
[login.defs](http://manpages.ubuntu.com/manpages/bionic/fr/man5/login.defs.5.html) - configuration de la suite des mots de passe cachés « shadow password »

### Tester et changer tous les mots de passe des utilisateurs, compte root compris.
#### To view the current password expiry/aging details
      * `sudo chage –l username`

#### Test the secure password policy
1. Run this command to add a user: `sudo useradd  testuser`
2. Then set a password: `$ sudo passwd testuser`
3. Now try to enter a password that does not include restrictions.

#### adding a complex password that meets the criteria defined by the password policy
      * `sudo passwd USERNAME`
      * ex: Sup3rP4ssw0rd! (non c'est pas ça pour vrai)

How to [enable and enforce secure password policies](https://linuxhint.com/secure_password_policies_ubuntu/)

## 2. Configuration stricte du groupe sudo

### I. Installation de sudo
1. Se connecter à l'utilisateur root
    `$ su -`
2. Update et upgrade
    `$ apt update && apt upgrade`
3. Installer sudo
    `$ apt install sudo`
4. Redémarrer
    `reboot`
5. Vérifier l’installation
    `dpkg -l | grep sudo`
6. Voir chapitre 6 pour créer utilisateurs et 4 pour ajouter utilisateurs au groupe sudo

### II. Install Vim (pas obligatoire)
- `apt install vim`
- `dpkg -l | grep vim`

### III. Configuration de sudo pour utilisateurs
1. `$ sudo visudo` <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; OU Modifier fichier sudoers.d:*** <br/>
      * `$ sudo vim /etc/sudoers.d/FileName'

#### L’authentification sudo limitée à 3 essais en cas de mot de passe erroné :
   * `Defaults        passwd_tries=3`
#### Message d'erreur en cas de mot de passe erroné :
   * `Defaults        badpass_message="MESSAGEdERREUR"`
#### Journal des commandes sudo :
   * `mkdir /var/log/sudo` <br/>
   * `touch /var/log/sudo/commandslog` or whatever you want
   * `Defaults        logfile="/var/log/sudo/commandslog"`
#### To archive all sudo inputs & outputs to a directory: 
   * `Defaults        log_input,log_output` <br/>
   * `Defaults        iolog_dir="/var/log/sudo"`
#### Activation du mode TTY :
   * `Defaults        requiretty`
#### Restreindre les paths utilisables par sudo :
   * `Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`

### IV. Running root-Privileged Commands
[Useful sudoers configurations](https://www.tecmint.com/sudoers-configurations-for-setting-sudo-in-linux/ "tecmint.com"), run root-privileged commands via prefix sudo
`$ sudo COMMAND`<br/>
By exemple : 
`$ sudo apt update`

## 3. Hostname - HOW TO FIND IP ADDRESS
### Trouver le nom d'hôte
   * `$ hostname`
### Configurer
1. Modifier nom : 
      * `$ sudo vim /etc/hostname`
2. Modifier nom : 
      * `$ sudo vim /etc/hosts`
      * `$ reboot`
### Login to your server: 
      * `ssh user@server-name.`
Become a root user: `sudo -s or su -` <br/>
Edit the file `/etc/hostname: vim /etc/hostname` <br/>
Edit the file `/etc/hosts: vim /etc/hosts` <br/>
Run command: `/etc/init.d/hostname. sh start` <br/>
[Change hostname](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/ "cyberciti.biz") command

## 4. Groupes
### I. Create group
      - 'sudo groupadd USERNAME'
### II. Ajouter utilisateurs à un groupe 
      - `sudo adduser USERNAME  GROUP`
            - group : sudo, user42
      - 'reboot' for changes to take effect
      - verify sudopowers via 'sudo -v' 'sudo -l' This will list any sudo privileges you have. 'sudo whoami'
      - Vérifier si l’utilisateur a bien été ajouté au groupe `$ getent group sudo` ('groups USERNAME GROUP')
(sudo groups USERNAME) mchampag comme utilisateur


## 5. Utilisateurs
### I. Créer un nouvel utilisateur
      * `$ sudo adduser <username>`
### II. Vérifier si l’utilisateur a bien été ajouté
      * `$ getent passwd USERNAME`
### III. Vérifier l’expiration le mot de passe de l'utilisateur nouvellement créé
      * `$ sudo chage -l USERNAME`

Last password change					: <last-password-change-date>
Password expires			                  : <last-password-change-date + PASS_MAX_DAYS>
Password inactive						: never
Account expires						: never
Minimum number of days between password change	: <PASS_MIN_DAYS>
Maximum number of days between password change	: <PASS_MAX_DAYS>
Number of days of warning before password expires	: <PASS_WARN_AGE>

[Supprimer effacer retirer un compte utilisateur](https://linuxhint.com/secure_password_policies_ubuntu/)
[Créer groupe sudo](https://linuxize.com/post/how-to-create-groups-in-linux/)

## 6. Service SSH, pare-feu et connection à distance
'''
Un service SSH sera actif sur le port 4242 uniquement. Pour des questions de sécurité, on ne devrait pas pouvoir se connecter par SSH avec l’utilisateur root.

L’utilisation de SSH sera testée durant la soutenance par la mise en
place d’un nouveau compte. Il faut par conséquent comprendre comment
fonctionne ce service.

Vous allez configurer votre système d’exploitation avec le pare-feu UFW et ainsi ne laisser ouvert que le port 4242.

Votre pare-feu devra être actif au lancement de votre machine
virtuelle. Pour CentOS, vous utiliserez UFW à la place du pare-feu de
base. Pour l’installer, vous allez devoir probablement utiliser DNF.

• Votre machine aura pour hostname votre login suivi de 42 (exemple : wil42). Vous serrez amenez à modifier ce hostname durant votre évaluation.

• Vous allez mettre en place une politique de mot de passe fort.

• Vous allez installer et configurer sudo selon une pratique stricte.

• Un utilisateur sera présent avec pour nom votre login en plus de l’utilisateur root.

• Cet utilisateur appartiendra aux groupes user42 et sudo.
'''

## Firewall : Installation, status et activation d’un port

### Installer pare-feu
1. `sudo apt update && apt upgrade` 
2. `sudo apt install ufw`
3. `reboot`
4. Vérifier si ufw est bien installé :
      * `dpkg -l | grep ufw`
### Statut du pare-feu et activation d'un port
   - `sudo ufw status`
   - `sudo ufw enable`
   - `sudo ufw allow NUMÉROduPORT` (ici, on veut 4242, 80(http) et 21(FTP))
   - `sudo ufw delete allow NUMÉROduPORT`      
      
## SSH

La connexion sécurisée à distance avec [SSH](https://openclassrooms.com/fr/courses/43538-reprenez-le-controle-a-laide-de-linux/41773-la-connexion-securisee-a-distance-avec-ssh)

### Installation openSSH-server
   * `$ sudo apt install openssh-server` <br/>
   * `sudo reboot`
#### Vérifier si SSH installé
   * `$ dpkg -l | grep ssh`


### Statut
- Vérifier si SSH est activé : 
      * `$ sudo systemctl status sshd`
- Démarrer SSH : 
      * `$ sudo systemctl start sshd`
- Redémarrer SSH : 
      * `$ sudo systemctl restart sshd`
- Arrêter SSH : 
      * `$ sudo systemctl stop sshd`

Installer et configurer un [serveur SSH](https://www.linuxtricks.fr/wiki/ssh-installer-et-configurer-un-serveur-ssh "linuxtricks.fr")

### Changer le port SSH et permissions utilisateur root
1. Modifier port SSH
      * Localiser fichier :
        * 'find / -name "sshd_config" 2>/dev/null'
2. `sudo vim /usr/share/openssh/sshd_config` et 'sudo vim /etc/ssh/sshd_config'
3. Rechercher Port 22 ou #Port 22
      * Port SSH par défaut : 22
4. Remplacer 22 par 4242 80 et 21 (un par ligne)
5. Reboot SSH `sudo systemctl restart sshd`
6. Vérifier ports ouverts `$ ss -tulw`

### Permission utilisateur root 
To disable SSH login as root irregardless of authentication mechanism
1. Localiser fichier :
      * `find / -name "sshd_config" 2>/dev/null`
2. `$ sudo vim /usr/share/openssh/sshd_config`
3. Rechercher: 
      * "PermitRootLogin" et le mettre à "no"
4. 'reboot'
Vérifier statut :
$ `service ssh status` (`$ systemctl status ssh`)

How to change [ssh port](https://www.cyberciti.biz/faq/howto-change-ssh-port-on-linux-or-unix-server/ "cyberciti.biz")<br/>
Check if [port is in use](https://www.linuxtricks.fr/wiki/ssh-installer-et-configurer-un-serveur-ssh "linuxtricks.fr") command

### Connection au server via SSH (connection à distance)
1. Obtenir l'adresse IP
      * `hostname -I`
2. Se connecter à partir d'ailleurs
      * Utiliser un second terminal
3. `ssh NOMd'UTILISATEUR@ADRESSEip -p NUMÉROduPORT`
      * exemple : `ssh audrey@10.12.231.216 -p 4242`
### Terminer un session SSH
   * `$ logout` (**ou** `$ exit`)



  
Rédiger des [scripts](https://debian-facile.org/doc:programmation:shells:debuter-avec-les-scripts-shell-bash "debian-facile.org") sous bash
Introduction aux [scripts](https://openclassrooms.com/fr/courses/43538-reprenez-le-controle-a-laide-de-linux/42867-introduction-aux-scripts-shell "openclassroom.com") shell

      
      
shasum Born2beRoot.vdi > signature.txt
cat signature.txt
a246db49c0c1abbbc6c8c164a8b2999a630bec36  Born2beRoot.vdi

