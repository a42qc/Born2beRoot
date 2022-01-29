# 8. BONUS : Wordpress lighttpd, MariaDB et PHP

## Tâches
• Mettre en place un site web WordPress fonctionnel avec, comme services, lighttpd, MariaDB et PHP.

## Définitions paquets à installer

| Paquets       | Définitions                                                                                                                   |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| WordPress     | Système de gestion de contenu opensource (SGC ou content management system (CMS). Permet de créer et gérer différents                                             types de sites Web sans avoir à les coder soi-même.
| lighttpd      | Logiciel libre de serveur web (HTTP) sécurisé, rapide et flexible. Lighttpd is a free, open-source and high-speed webserver                                       specially designed for speed-critical environments. 
| MariaDB       | Système de gestion de base de données. Fork communautaire de MySQL.
| PHP           | Hypertext Preprocessor est un langage de programmation libre, principalement utilisé pour produire des pages Web dynamiques                                       (et applications web) via un serveur HTTP. Langage impératif orienté objet. 
| Let’s Encrypt | (optionnel) free SSL to secure your web server.

## Configurations des services (bonus)

### MariaDB : database server installation and configuration 

1. Install MariaDB server and client
```bash
$ sudo apt install mariadb-server mariadb-client -y
```

2. Start service, activate it at startup and check status
```bash
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
$ susdo systemctl status mariadb
```

3. Secure the database
```bash
$ sudo mysql_secure_installation
```
| Questions                              | Answer                                                    |
| -------------------------------------- | --------------------------------------------------------- |
| Enter current password for root :      | PRESS ENTER
| Set root password?                     | YES  <br>                                                                                                                                                            (Setting the root password ensures that nobody can log into the MariaDB root user without the proper authorisation.)
| Remove anonymous users?                | YES <br>                                                                                                                                                            (By default, an anonymous user allow anyone to log into MariaDB without having to gave a user account created for them.                                              This intended only for testing. Remove them before moving into a production environment.)                           
| Disallow root login remotely?          | YES <br>                                                                                                                                                            (Root should only be allowed to connect from 'localhost. <br>                                                                                                        This ensures that someone cannont guess at the root password from the network.')
| Remove test database and access to it? | YES <br>                                                                                                                                                            (Database named 'test' that anyone can access. Should be remove before moving into a production environment.)
| Reload privilege tables now?           | YES <br>                                                                                                                                                            (Reloading the privilege tables will ensure that all changes made so far will take effect immediately.)

4. Access to MYSQL shell to create a database for the Wordpress web site.
```bash
$ sudo mysql
```
- Choose a database name
```SQL
MariaDB [(none)]> CREATE DATABASE wordpressB2BR;
```
- Database list
```SQL
MariaDB [(none)]> SHOW DATABASES;
```
- Create a user
```
MariaDB [(none)]> CREATE USER 'wordpressB2BR'@'localhost' IDENTIFIED BY 'Sup3rCh4ts!';
```
- Give all rights
```SQL
MariaDB [(none)]> GRANT ALL PRIVILEGES ON wordpressB2BR.* TO 'wordpressB2BR'@'localhost';
MariaDB [(none)]> FLUSH PRIVILEGES;
```
- Leave MYSQL
```SQL
MariaDB [(none)]> quit
```
 
### lighttpd : installation and activation

1.  Install `apt install lighttpd -y`
2.  Start Lighttpd service and enable it to start after system reboot
```
systemctl start lighttpd
systemctl enable lighttpd
systemctl status lighttpd (check status)
```

### PHP : installation

```bash
$ sudo apt install php-cgi php-mysql -y
```

### WORDPRESS : intall and configuration
```bash
$ sudo apt install w3m -y

    w3m http://wordpress.org => download latest.tar.gz

    sudo tar -xyzf latest.tar.gz => sudo cp -r wordpress/* /var/www/html => sudo rm latest.tar.gz => sudo rm -rf /var/www/html/wordpress

    cd /var/www/html => sudo cp wp-config-sample.php wp-config.php => sudo wp-config.php

define( 'DB_NAME', 'database_name_here' );^M define( 'DB_USER', 'username_here' );^M define( 'DB_PASSWORD', 'password_here' );^M

    sudo lighty-enable-mod fastcgi

    sudo lighty-enable-mod fastcgi-php

    sudo service lighttpd force-reload
```
23 - intall and set ftp (sftp)

    sudo apt install vsftpd

    sudo ufw allow 21

    cd /etc => vi vsftpd.conf

write_enable=YES

    sudo mkdir /home//ftp

    sudo mkdir /home//ftp/files

    udo chown nobody:nogroup /home//ftp

    sudo chmod a-w /home//ftp




## Installation et configuration lighttpd, php et mariadb
<a href ="https://www.howtoforge.com/how-to-install-lighttpd-with-php-and-mariadb-on-debian-10/">lien du tuto</a>
0.  Update our system `apt update -y && apt upgrade -y`






aller
se connecter au serveur
ftp://IPADDRESS
username et mot de passe
Puis acces au dossiers etr fichiers

## Installation et configuration Wordpress
<a href="https://www.osradar.com/install-wordpress-with-lighttpd-debian-10/">lien du tuto</a>

## Documentation
- [Base de données](https://fr.wikipedia.org/wiki/Base_de_donn%C3%A9es) — Wikipédia
- [Serveur web](https://fr.wikipedia.org/wiki/Serveur_web) — Wikipédia
- [Installer WordPress sur Debian](https://wiki.debian.org/WordPress)
- [Désinstaller proprement ses paquets](https://linuxfr.org/wiki/desinstaller-proprement-ses-paquets-sur-sa-distribution#toc-suppression-des-d%C3%A9pendances-avec-apt---purge-autoremove)
- [How to Install Lighttpd with PHP, MariaDB](https://www.howtoforge.com/how-to-install-lighttpd-with-php-and-mariadb-on-debian-10/)

certbot certonly --webroot -w /var/www/html/ -d www.example.com
dpkg -l | 
d/sinstallation

