# 2. Politique de mot de passe fort

```
Pour mettre en place une politique de mot de passe fort, il faudra remplir les conditions suivantes :

  • Votre mot de passe devra expirer tous les 30 jours.

  • Le nombre minimum de jours avant de pouvoir modifier un mot de passe sera configuré à 2.

  • L’utilisateur devra recevoir un avertissement 7 jours avant que son mot de passe n’expire.

  • Votre mot de passe sera de 10 caractères minimums dont une majuscule et un chiffre, 
    et ne devra pas comporter plus de 3 caractères identiques consécutifs.

  • Le mot de passe ne devra pas comporter le nom de l’utilisateur.

  • Le mot de passe devra comporter au moins 7 caractères qui ne sont pas présents dans l’ancien mot de passe.

  • Bien entendu votre mot de passe root devra suivre cette politique.

Après avoir mis en place vos fichiers de configuration, il faudra changer tous les mots de passe des comptes présents 
sur la machine virtuelle, compte root inclus.
```

## I. Expiration (password age)

### Method #1 login.defs

Note that the above-configured policy will only apply on the newly created users. <br>
To apply this policy to an existing user, use “chage” command.

1. Modify the file login.defs 'sudo vim /etc/login.defs

|                                           |                       |
| ----------------------------------------: | --------------------- |
| Expiration mot de passe :                 | `PASS_MAX_DAYS 30` 
| Nb of days before being able to modify :  | `PASS_MIN_DAYS 2`
| Avertissement x jour avant expiration :   | `PASS_WARN_AGE 7`


### Method #2 chage - Expiration applicable sur utilisateurs existants

Note : To execute the chage command, you must be the owner of the account or have root privilege otherwise, you will not be able to view or modify the expiry policy. <br?>

|                                                      |                                                       |
| ---------------------------------------------------- | ----------------------------------------------------- |
| To use chage command, syntax is:                     | `$ chage [options] username`
| To view the current password expiry/aging details :  | `$ sudo chage –l USERNAME`
| To configure the maximum No. of days after which a user should change the password. | `$ sudo chage -M <No./_of_days> <user_name>`
| To configure the minimum No. of days required between the change of password.       | `$ sudo chage -m <No._of_days> <user_name>`
| To configure warning prior to password expiration :  | `$ sudo chage -W <No._of_days> <user_name>` <br>                                                                                                                    exemple : chage -M 30 -m 2 -W 7 -d 2021-08-10 USERNAME(or root) (-d do not change the actual password) 

## II. Complexité (password strength) pam.d

|                                        |                                                                               |
| -------------------------------------- | ----------------------------------------------------------------------------- |
| 1. Installer libpam-pwquality          | `$ sudo apt install libpam-pwquality -y`
| 2. Vérifier que c'est bien installé :  | `dpkg -l \| grep libpam-pwquality`
| 3. Appliquer la complexité             | `$ sudo vim /etc/pam.d/common-password` <br>                                                                                                                        ÉCRIRE SUR LA MÊME LIGNE QUE `password	requisite	pam_pwquality.so retry=3`

<br>

| Ajouter les restrictions : |                                                                    |
| -------------------------: | ------------------------------------------------------------------ |
| `retry=3`                  | No. of consecutive times a user can enter an incorrect password.
| `minlen=10`                | Minimum length of password (10 caractères)
| `maxrepeat=3`              | To set a maximum of x consecutive identical characters
| `difok=7`                  | No. of character that can be similar to the old password
| `lcredit=`                 | Min No . of lowercase letters
| `ucredit=-1`               | Min No. of uppercase letters
| `dcredit=-1`               | Min No. of digits
| `ocredit=`                 | Min No. of symbols
| `reject_username`          | Rejects the password containing the user name (oui)
| `enforce_for_root`         | Also enforce the policy for the root user (oui)

~??? To check if the password contains the user name in some form (enabled if the value is not 0) usercheck=  <br>
exemple : password        requisite	pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root <br>
Now reboot the system to apply the changes in the password policy.

## Tester et changer tous les mots de passe des utilisateurs, compte root compris.

|                                                   |                                                                                   |
| ------------------------------------------------- | --------------------------------------------------------------------------------- |
| To view the current password expiry/aging details | `sudo chage –l username`
| Test the secure password policy                   | 1. Run this command to add a user: `sudo useradd  testuser` <br>                                                                                                     2. Then set a password: `$ sudo passwd testuser` <br>                                                                                                               3. Now try to enter a password that does not include restrictions.
| adding a complex password that meets the criteria defined by the password policy | `sudo passwd USERNAME` <br>                                                                                                                                         ex: Sup3rP4ssw0rd! (non c'est pas ça pour vrai)

# Documentation

- [man login.defs](http://manpages.ubuntu.com/manpages/cosmic/fr/man5/login.defs.5.html#:~:text=Le%20fichier%20%2Fetc%2Flogin.,aura%20probablement%20des%20cons%C3%A9quences%20ind%C3%A9sirables "MANPAGES.UBUNTU.COM")
- How to [enable and enforce secure password policies](https://linuxhint.com/secure_password_policies_ubuntu/)
- How to [enforce password complexity](https://www.networkworld.com/article/2726217/how-to-enforce-password-complexity-on-linux.html)
- How To Set [Password Policies](https://ostechnix.com/how-to-set-password-policies-in-linux/)
- [login.defs](http://manpages.ubuntu.com/manpages/bionic/fr/man5/login.defs.5.html) - configuration de la suite des mots de passe cachés « shadow password »
- How to [enable and enforce secure password policies](https://linuxhint.com/secure_password_policies_ubuntu/)

