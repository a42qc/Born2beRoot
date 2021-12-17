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










#


  
Rédiger des [scripts](https://debian-facile.org/doc:programmation:shells:debuter-avec-les-scripts-shell-bash "debian-facile.org") sous bash
Introduction aux [scripts](https://openclassrooms.com/fr/courses/43538-reprenez-le-controle-a-laide-de-linux/42867-introduction-aux-scripts-shell "openclassroom.com") shell

      
      
shasum Born2beRoot.vdi > signature.txt
cat signature.txt
a246db49c0c1abbbc6c8c164a8b2999a630bec36  Born2beRoot.vdi

