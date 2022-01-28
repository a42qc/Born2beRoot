# 5. Service SSH, pare-feu et connection à distance

```
Un service SSH sera actif sur le port 4242 uniquement. Pour des questions de sécurité, on ne devrait pas pouvoir 
se connecter par SSH avec l’utilisateur root.

L’utilisation de SSH sera testée durant la soutenance par la mise en
place d’un nouveau compte. Il faut par conséquent comprendre comment
fonctionne ce service.

Vous allez configurer votre système d’exploitation avec le pare-feu UFW et ainsi ne laisser ouvert que le port 4242.

Votre pare-feu devra être actif au lancement de votre machine
virtuelle. Pour CentOS, vous utiliserez UFW à la place du pare-feu de
base. Pour l’installer, vous allez devoir probablement utiliser DNF.

  • Votre machine aura pour hostname votre login suivi de 42 (exemple : wil42). Vous serrez amenez à modifier 
  ce hostname durant votre évaluation.

  • Vous allez mettre en place une politique de mot de passe fort.

  • Vous allez installer et configurer sudo selon une pratique stricte.

  • Un utilisateur sera présent avec pour nom votre login en plus de l’utilisateur root.

  • Cet utilisateur appartiendra aux groupes user42 et sudo.
```

## Firewall : Installation, status and port activation

### Installation ufw

|                                 |                                            |
| ------------------------------- | ------------------------------------------ |
| 1. Update and upgrade           | `$ sudo apt update -y && apt upgrade -y` 
| 2. Install ufw                  | `$ sudo apt install ufw -y`
| 3. Reboot the system            | `$ reboot`
| 4. Check if correctly installed | `dpkg -l \| grep ufw`


### Firewall status and port activation

|                                 |                                            |
| ------------------------------- | ------------------------------------------ |
| Check status                    | `sudo ufw status`
| Enable the firewall             | `sudo ufw enable`
| Activate a port                 | `sudo ufw allow PORTNUMBER` <br>                                                                                                                                  For B2bR : 4242, 80(http) et 21(FTP)
| Disable a port                  | `sudo ufw deny NUMÉROduPORT`      
      

## SSH : Installation, status and port configuration

### Installation openSSH-server

|                                  |                                                |
| -------------------------------- | ---------------------------------------------- |
| 1. Install openssh               | `$ sudo apt install ssh -y` (opensshd_server?)
| 2. Reboot the system             | `$ sudo reboot`
| 3. Check if correctly installed  | `$ dpkg -l \| grep ssh`


### Status and activation

|                                    |                                            |
| ---------------------------------- | ------------------------------------------ |
| Check if SSH is activated          | `$ sudo systemctl status sshd`
| Start SSH                          | `$ sudo systemctl start sshd`
| Restart SSH                        | `$ sudo systemctl restart sshd`
| Stop SSH                           | `$ sudo systemctl stop sshd`

### Modify SSH port et root user permission

|                                                        |                                            |
| ------------------------------------------------------ | ------------------------------------------ |
| 1. Localise config files                               | `find / -name "sshd_config" 2>/dev/null`
| 2. Modify the config files                             | `sudo vim /usr/share/openssh/sshd_config` <br>                                                                                                                      `sudo vim /etc/ssh/sshd_config`
| 3. Search `Port 22` or `#Port 22` (default SSH port)
| 4. Replace `22` by `4242`
| 5. Reboot SSH `sudo systemctl restart sshd`
| 6. Check open ports                                    | `$ ss -tulw`

### Permission utilisateur root

To disable SSH login as root irregardless of authentication mechanism

|                                               |                                                        |
| --------------------------------------------- | ------------------------------------------------------ |
| 1. Localise sshd_config file                  | `find / -name "sshd_config" 2>/dev/null`
| 2. Modify sshd_config file                    | `$ sudo vim /usr/share/openssh/sshd_config`
| 3. Find `PermitRootLogin` and set it to `no`  |
| 4. 'reboot'                                   |
| 5. Check status                               | `$ systemctl status ssh` OR (`$ service ssh status` )


### Connection au server via SSH (connection à distance)

|                                              |                                                          |
| -------------------------------------------- | -------------------------------------------------------- |
| 1. Get IP address                            | `hostname -I`
| 2. Connect to the server by another terminal | `$ ssh NOMd'UTILISATEUR@ADRESSEip -p NUMÉROduPORT` <br>                                                                                                            exemple : `ssh audrey@10.12.231.216 -p 4242`
| Finish a SSH session                         | `$ logout` OR ( `$ exit`)


# Documentation

- La connexion sécurisée à distance avec [SSH](https://openclassrooms.com/fr/courses/43538-reprenez-le-controle-a-laide-de-linux/41773-la-connexion-securisee-a-distance-avec-ssh)
- Installer et configurer un [serveur SSH](https://www.linuxtricks.fr/wiki/ssh-installer-et-configurer-un-serveur-ssh "linuxtricks.fr")
- How to change [ssh port](https://www.cyberciti.biz/faq/howto-change-ssh-port-on-linux-or-unix-server/ "cyberciti.biz")
- Check if [port is in use](https://www.linuxtricks.fr/wiki/ssh-installer-et-configurer-un-serveur-ssh "linuxtricks.fr") command

