# 8. BONUS : Wordpress lighttpd, MariaDB et PHP

## Tâches
📌 Mettre en place un site web WordPress fonctionnel avec les services lighttpd, MariaDB et PHP.

## Définitions paquets à installer

| Paquets       | 📔 Définitions 📔                                                                                                            |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| WordPress     | Système de gestion de contenu opensource (SGC ou content management system (CMS). Permet de créer et gérer différents                                               types de sites Web sans avoir à les coder soi-même.
| lighttpd      | Logiciel libre de serveur web (HTTP) sécurisé, rapide et flexible. Lighttpd is a free, open-source and high-speed webserver                                         specially designed for speed-critical environments. 
| MariaDB       | Système de gestion de base de données. Fork communautaire de MySQL.
| PHP           | Hypertext Preprocessor est un langage de programmation libre, principalement utilisé pour produire des pages Web dynamiques                                         (et applications web) via un serveur HTTP. Langage impératif orienté objet. 
| Let’s Encrypt | (optionnel) free SSL to secure your web server.

## Configurations des services (bonus)

### ✏️ MariaDB : database server installation and configuration 

#### 1. Install MariaDB server and client
```bash
$ sudo apt install mariadb-server mariadb-client -y
```

#### 2. Start service, activate it at startup and check status
```bash
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
$ susdo systemctl status mariadb
```

#### 3. Secure the database
```bash
$ sudo mysql_secure_installation
```
| Questions                              | Answer                                                                                 |
| -------------------------------------- | -------------------------------------------------------------------------------------- |
| Enter current password for root :      | PRESS ENTER
| Set root password?                     | NO
| | (Setting the root password ensures that nobody can log into the MariaDB root user without the proper authorisation.)
| Remove anonymous users?                | YES
| | (By default, an anonymous user allow anyone to log into MariaDB without having to gave a user account created for them. <br>                                        This intended only for testing. Remove them before moving into a production environment.)                           
| Disallow root login remotely?          | YES
| | (Root should only be allowed to connect from 'localhost. <br>                                                                                                        This ensures that someone cannont guess at the root password from the network.')
| Remove test database and access to it? | YES
| | (Database named 'test' that anyone can access. Should be remove before moving into a production environment.)
| Reload privilege tables now?           | YES
| | (Reloading the privilege tables will ensure that all changes made so far will take effect immediately.)

#### 4. Access to MYSQL shell to create a database for the Wordpress web site.
```bash
$ sudo mysql
```
##### 4.1 Choose a database name
```SQL
MariaDB [(none)]> CREATE DATABASE wordpress;
```
💡 Database list
```SQL
MariaDB [(none)]> SHOW DATABASES;
```
##### 4.2 Create a user
```
MariaDB [(none)]> CREATE USER 'mchampag'@'localhost' IDENTIFIED BY 'Sup3rCh4ts!';
```
##### 4.3 Give all rights to the user
💡`GRANT type_of_permission ON database_name.table_name TO 'username'@'localhost';`
```SQL
MariaDB [(none)]> GRANT ALL PRIVILEGES ON wordpress.* TO 'mchampag'@'localhost';
MariaDB [(none)]> FLUSH PRIVILEGES;
```
##### 4.4 Leave MYSQL
```SQL
MariaDB [(none)]> quit
```
 
### ✏️ lighttpd : web server installation and activation

#### 1. Installation
```bash
apt install lighttpd -y
```
#### 2.  Start Lighttpd service and enable it to start after system reboot
```
systemctl start lighttpd
systemctl enable lighttpd
```
💡`systemctl status lighttpd`


### ✏️ PHP : installation and configuration
#### 1. Installation
```bash
$ sudo apt install php php-cli php-common php-fpm php-mysql -y
```

#### 2. Puis, sans être essentiel dans le même fichier de configuration, l'emplacement par defaut du repertoire racine des fichiers qui compose les pages web est /var/www/html . Pour modifier la racine des fichiers web, éditer la ligne server.document-root comme ceci:
```bash
$ sudo vim /etc/lighttpd/lighttpd.conf

Search
server.document-root = "/var/www/html"

Change it for
server.document-root = "/var/www"
```
Cela permettra de placer le dossier wordpress (qui contiendra les fichier de la page web) a cette emplacement /var/www.

Le fichier de configuration principal est placés ici: /etc/lighttpd/lighttpd.conf et les autres fichiers de configuration sont placés ici: /etc/lighttpd/conf-available et quand certain fichier sont activés, des lien symboliques sont placé dans le repertoire /etc/lighttpd/conf-enable.

#### 3. Modify '/etc/php/7.3/fpm/php.ini' file to activate lighttpd PHP support. (I DIDN'T NEED TO DO THAT)
```bash
$ sudo vim /etc/php/7.3/fpm/php.ini

Decomment that line
cgi.fix_pathinfo=1
```

#### 4. By default, PHP-FPM listen on socket UNIX `/var/run/php7-fpm.sock`. Change it for TCP socket.
```bash
$ sudo vim `/etc/php/7.3/fpm/pool.d/www.conf` 

listen = 127.0.0.1:9000
```

#### 5. Activate FastCGI module support inside lighttpd
```bash
$ vim /etc/lighttpd/conf-available/15-fastcgi-php.conf

Search thoses lines
"bin-path" => "/usr/bin/php-cgi",
"socket" => "/var/run/lighttpd/php.socket",

Change it for
"host" => "127.0.0.1",
"port" => "9000",
```

#### 6. Activate FastCGI and FastCGI-PHP modules
Configure and restart Lighttpd
```bash
$ sudo lighty-enable-mod fastcgi
$ sudo lighty-enable-mod fastcgi-php
```

#### 7. Restart PHP and lighttpd
💡 How to found PHP version `php -v` -> php7.3
```
$ sudo systemctl restart php7.3-fpm
$ sudo systemctl restart lighttpd
```

💡`sudo systemctl status php7.3-fpm`

### ✏️ WORDPRESS : installation and configuration

1. Download
```bash
cd /var/www
sudo wget https://fr.wordpress.org/latest-fr_FR.tar.gz
```
💡Or dowload it like that:
#### 1. Install w3m (teminal web browser) and download Wordpress
```bash
$ sudo apt install w3m -y
```
💡 Make sure you are in the right directory. <br>
Here, I want to download it inside '/var/www' directory.
```bash
$ w3m http://wordpress.org

-> Get Wordpress
-> Download .tar.gz
(Download) Save file to: wordpress-5.9.tar.gz
-> Ok
```

2. Decompress
```bash
$ sudo tar -xf latest-fr_FR.tar.gz
$ rm latest-fer_FR.tar.gz
```

3. Change owner and rights
```bash
sudo chown -R www-data:www-data wordpress
sudo chmod -R 755 wordpress
```
💡 `ls -la` to verify changes

#### 4. Change the Wordpress configuration file's name
```bash
$ cd /wordpress 
$ mv wp-config-sample.php wp-config.php
```

5. Configure 
```bash
$ vim wp-config.php

define( 'DB_NAME', 'wordpress' );

/** Utilisateur de la base de données MySQL. */
define( 'DB_USER', 'mchampag' );

/** Mot de passe de la base de données MySQL. */
define( 'DB_PASSWORD', 'Sup3rCh4ts!' )
```

#### 1. Install w3m (teminal web browser)
```bash
$ sudo apt install w3m -y
```
#### 2. Dowload wordpress

💡 Make sure you are in the right directory. <br>
Here, I want to download it inside '/home/USERNAME' directory.

```bash
$ w3m http://wordpress.org

-> Get Wordpress
-> Download .tar.gz
(Download) Save file to: wordpress-5.9.tar.gz
-> Ok
```

#### 3. Decompress the directory with tar
```bash
$ sudo tar -xvf wordpress-5.9.tar.gz
```

💡`tar -xvf` means `tar --extract --verbose --file=FILENAME`

#### 4. Copy the directory inside `var/www/html`
```bash
$ sudo mv wordpress /var/www/html
$ sudo rm latest.tar.gz
$ sudo rm -r /var/www/html/wordpress IF YOU WANT TO DELETE THE WORDPRESS DIRECTORY
```

#### 5. Copy the Wordpress configuration file 
```bash
$ cd /var/www/html
$ sudo cp wordpress/wp-config-sample.php wp-config.php
$ sudo chmod 700 wp-config.php
```
💡 `700` -> `-rwx-------`

#### 6. Configure Wordpress database
```bash
$ sudo vim wp-config.php
```
```
define( 'DB_NAME', 'database_name_here' ); -> wordpress
define( 'DB_USER', 'username_here' ); -> mchampag
define( 'DB_PASSWORD', 'password_here' ); -> Sup3rCh4ts!
```
❓ Why the database password is in clear? Same as inside MariaDB...

7. Installation 
Inside a web browser write this : IP_ADDRESS/wordpress/wp-admin (http://10.12.232.35/wordpress/wp-admin/install.php)
<br>
- Titre du site : Born2beRoot
- Identifiant : mchampag
- Mot de passe : Sup3rCh4ts!
- Vote e-mail : audrey.42qc@gmail.com
- I check -> Demander aux moteurs de recherche de ne pas indexer ce site

![Installation Worpress inside a browser](https://i.imgur.com/YDQZe9Y.png)

8. Web site creation

![Tableau de bord Wordpress](https://i.imgur.com/XBoGdsR.png)

### ✏️ FTP : installation and configuration

#### 1. Install `vsftpd`
```bash
$ sudo apt install vsftpd -y
```

#### 2. Allow FTP port 21
```bash
$ sudo ufw allow 21
```

#### 3. Configure vsftpd
```bash
$ cd /etc
$ vim vsftpd.conf

Uncomment this to enable any form of FTP write command.
write_enable=YES
```

#### 4. Create `ftp` directory and files directory
```bash
cd /home/USERNAME
$ sudo mkdir ftp
$ sudo mkdir ftp/files
```

#### 5. Change `ftp` directory rights
```bash
$ sudo chown nobody:nogroup ftp
$ sudo chmod a-w ftp
```
💡 `a-w` -> `dr-xr-xr-x`

### ✏️ Pour configurer wordpress, accéder depuis un naviguateur address_ip/wordpress/wp-admin et au site lui meme adresse_ip/wordpress

### ✏️ How to use FTP service

#### 1. Click on `aller`
#### 2. Click on `Se connecter au serveur...`
#### 3. `ftp://IPADDRESS`
#### 4. `username` et `password`
#### 5. Now, we have access to directories and files

## 📚 Documentation 📚
- [Base de données](https://fr.wikipedia.org/wiki/Base_de_donn%C3%A9es) — Wikipédia
- [Serveur web](https://fr.wikipedia.org/wiki/Serveur_web) — Wikipédia
- [Installer WordPress sur Debian](https://wiki.debian.org/WordPress)
- [Désinstaller proprement ses paquets](https://linuxfr.org/wiki/desinstaller-proprement-ses-paquets-sur-sa-distribution#toc-suppression-des-d%C3%A9pendances-avec-apt---purge-autoremove)
- [How to Install Lighttpd with PHP, MariaDB](https://www.howtoforge.com/how-to-install-lighttpd-with-php-and-mariadb-on-debian-10/)
- <a href="https://www.osradar.com/install-wordpress-with-lighttpd-debian-10/">Installation et configuration Wordpress</a>

certbot certonly --webroot -w /var/www/html/ -d www.example.com
dpkg -l | 
d/sinstallation

