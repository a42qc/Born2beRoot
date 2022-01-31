# 4. Users

|                               |                                                                         |
| ----------------------------- | ----------------------------------------------------------------------- |
| 1. Create a new user          | `$ sudo adduser USERNAME`
| 2. Check if user is added     | `$ getent passwd USERNAME`
| 3. Check password expiration  | `$ sudo chage -l USERNAME`
| Delete a user and all entries | `$ deluser USERNAME`
| Modify an user                | [See examples here](https://www.tecmint.com/usermod-command-examples/)
| How to connect to an user     | `$ su USERNAME`
| How to list users             | `$ less /etc/passwd` (less instead of cat because it does pages)

```
EXAMPLE OF chage -l USERNAME

Last password change : <last-password-change-date>
Password expires : <last-password-change-date + PASS_MAX_DAYS>
Password inactive : never
Account expires : never
Minimum number of days between password change : <PASS_MIN_DAYS>
Maximum number of days between password change : <PASS_MAX_DAYS>
Number of days of warning before password expires : <PASS_WARN_AGE>
```

# Documentation

- [How to delete an user](https://www.cyberciti.biz/faq/linux-remove-user-command/)
- [How to enable and enforce secure password policies on Ubuntu](https://linuxhint.com/secure_password_policies_ubuntu/)
- [How to create groups](https://linuxize.com/post/how-to-create-groups-in-linux/)
- [How to modify an user](https://www.tecmint.com/usermod-command-examples/)
- [How to list users](https://linuxize.com/post/how-to-list-users-in-linux/)
