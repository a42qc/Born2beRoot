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
| Questions                              | Answer                                                    |
| -------------------------------------- | --------------------------------------------------------- |
| Enter current password for root :      | PRESS ENTER
| Set root password?                     | YES
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
MariaDB [(none)]> CREATE DATABASE wordpressB2BR;
```
##### 4.2 Database list
```SQL
MariaDB [(none)]> SHOW DATABASES;
```
##### 4.3 Create a user
```
MariaDB [(none)]> CREATE USER 'wordpressB2BR'@'localhost' IDENTIFIED BY 'Sup3rCh4ts!';
```
##### 4.4 Give all rights
```SQL
MariaDB [(none)]> GRANT ALL PRIVILEGES ON wordpressB2BR.* TO 'wordpressB2BR'@'localhost';
MariaDB [(none)]> FLUSH PRIVILEGES;
```
##### 4.5 Leave MYSQL
```SQL
MariaDB [(none)]> quit
```
 
### ✏️ lighttpd : installation and activation

#### 1. Install 
```bash
apt install lighttpd -y
```
#### 2.  Start Lighttpd service and enable it to start after system reboot
```
systemctl start lighttpd
systemctl enable lighttpd
systemctl status lighttpd (check status)
```

### ✏️ PHP : installation
```bash
$ sudo apt install php-cgi php-mysql -y
```

### ✏️ WORDPRESS : installation and configuration

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
$ sudo chmod 710 wp-config.php
```
💡 `700` -> `-rwx-------`

#### 6. Configure Wordpress database
```bash
$ sudo vim wp-config.php
```
```
define( 'DB_NAME', 'database_name_here' ); -> wordpressB2BR
define( 'DB_USER', 'username_here' ); -> wordpressB2BR
define( 'DB_PASSWORD', 'password_here' ); -> Sup3rCh4ts!
```
❓ Why the database password is in clear? Same as inside MariaDB...

#### 7. Configure and restart Lighttpd
```bash
$ sudo lighty-enable-mod fastcgi
$ sudo lighty-enable-mod fastcgi-php
$ sudo service lighttpd force-reload
```
💡The debian Policy Manual also explains the different parameters:
```
    start
    start the service,

    stop
    stop the service,

    restart
    stop and restart the service if it's already running, otherwise start the service

    reload
    cause the configuration of the service to be reloaded without actually stopping and restarting the service,

    force-reload
    cause the configuration to be reloaded if the service supports this, otherwise restart the service.
```

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

