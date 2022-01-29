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

1. MariaDB : database server installation and configuration 
    - 






## Installation et configuration lighttpd, php et mariadb
<a href ="https://www.howtoforge.com/how-to-install-lighttpd-with-php-and-mariadb-on-debian-10/">lien du tuto</a>
0.  Update our system `apt update -y && apt upgrade -y`

### lighttpd
1.  Install `apt install lighttpd -y`
2.  Start Lighttpd service and enable it to start after system reboot
```
systemctl start lighttpd
systemctl enable lighttpd
systemctl status lighttpd (check status)
```


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

