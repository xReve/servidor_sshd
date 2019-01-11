### **servidorldap:base**

**@edt ASIX M06-ASO Curs 2018-2019**


Serivodr ldap amb BDD edt.org. Servidor utilitzat al llarg de totes les pràctiques. No incorpora cap novetat


```
docker run --name ldap -h ldap --network ldapnet -p 389:389 -d servidorldap:base

```
