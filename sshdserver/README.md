# **SERVIDOR SSHD**

El servidor escolta en el port **1022**


Permet iniciar sessions remotes als comptes d’usuaris locals i de LDAP.

Es creen els homes de tots els usuaris en les sessions remotes.


APLICACIÓ EXEMPLES `ALLOWUSERS, pam_access.so, pam_list.so`


Fitxer /etc/pam.d/sshd

```
#%PAM-1.0
auth       substack     password-auth
auth	   required	pam_access.so
auth       required	pam_listfile.so onerr=succeed item=user sense=deny file=/opt/docker/users_listfile
auth       include      postlogin
account    required     pam_sepermit.so
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_mkhomedir.so
session    optional     pam_keyinit.so force revoke
session    include      password-auth
session    include      postlogin

```

Fitxer directives:
``` 
# DIRECTRIVA AllowUsers Exemple:

# Nomès es podran connectar via ssh els següents usuaris: (Per la resta d'usuaris la conexió serà rebutjada)

# AllowUsers pere marta anna exam01

```

Fitxer users_listfile: 

```
# Exemples per al modul pam_listfile.so

anna
pere

```

## COMPROVACIONS CREACIO HOME I RESTRICIONS USUARIS

**CONNEXIÓ USUARI LDAP amb localhost**

```

[root@sshd docker]# ssh pau@localhost -p 1022
The authenticity of host '[localhost]:1022 ([127.0.0.1]:1022)' can't be established.
ECDSA key fingerprint is SHA256:RAhQJ4lQ7IEdw8gvtY/SJz9JeFTgVrblJ+SGpCbTIRs.
ECDSA key fingerprint is MD5:01:13:2a:ae:33:fa:84:44:16:8b:7b:12:88:06:2c:dc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[localhost]:1022' (ECDSA) to the list of known hosts.
pau@localhost's password: 
Creating directory '/tmp/home/pau'.
[pau@sshd ~]$ pwd
/tmp/home/pau

```

**CONNEXIÓ USUARI LDAP desde fora el docker**

```

ssh marta@172.18.0.3 -p 1022
The authenticity of host '[172.18.0.3]:1022 ([172.18.0.3]:1022)' can't be established.
ECDSA key fingerprint is SHA256:RAhQJ4lQ7IEdw8gvtY/SJz9JeFTgVrblJ+SGpCbTIRs.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[172.18.0.3]:1022' (ECDSA) to the list of known hosts.
marta@172.18.0.3's password: 
Creating directory '/tmp/home/marta'.
[marta@sshd ~]$ pwd
/tmp/home/marta

```

**CONNEXIÓ USUARI local amb localhost**

```
ssh exam01@localhost -p 1022
exam01@localhost's password: 
[exam01@sshd ~]$ pwd
/home/exam01

```

**CONNEXIÓ AMB UN USUARI NO PERMÈS**

```
ssh anna@localhost -p 1022
anna@localhost's password: 
Permission denied, please try again.
anna@localhost's password: 
Permission denied, please try again.
anna@localhost's password: 
anna@localhost: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).

```




