# SERVIDOR SSHD
## @edt ASIX M06-ASO Curs 2018-2019

CREACIO SERVIDOR LDAP + SSHD AMB DOCKER

**2 dockers que estan connectats entre si en la red LDAPNET**
El servidor LDAP s'anomena **ldap**
El servidor SSHD s'anomena **sshd**

### IMATGES

**eescriba/serversshd:base** Servidor SSHD (antigament era el HOST pam) que conte usuaris locals i autentica usuaris contra servidor LDAP.

**eescriba/servidorldap:base** Servidor LDAP amb usuaris i groups.


### EXECUCIÓ
**SSHD**
docker run --rm -it --privileged --name sshd -h sshd --network ldapnet eescriba/serversshd:base

**LDAP**
docker run --name ldap -h ldap --network ldapnet -p 389:389 -d eescriba/servidorldap:base 






