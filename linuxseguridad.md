---
title: "LinuxSeguridad"
tags: ""
---

### [Politica de contraseñas](https://www.ochobitshacenunbyte.com/2019/03/26/crear-politicas-de-contrasenas-en-linux/) 

change atribbutes on /etc/security/pwquality.conf


### [Tiempo de expiracion](https://www.osradar.com/how-to-change-the-default-sudo-timeout/)

add 

```
Defaults env_reset,timestamp_timeout=1
```
to the /etc/sudoers (timeout is in minutes)

### [Chage](https://www.thegeekstuff.com/2009/04/chage-linux-password-expiration-and-aging/)

```
apt-get install chage
##to set it from now
chage -M number-of-days username
##to set it static
chage -E "2009-05-31" dhinesh
##to force locking on expired
chage -I 10 dhinesh
```

### [ACL](https://www.thegeekdiary.com/how-to-disable-a-specific-command-for-a-specific-user-in-linux/)
to find file

```
which command

```
to list properties

```
getfacl /bin/mkdir

```
to set control rule

```
/bin/setfacl -m u:john:--- /bin/mkdir
```

### IPTABLES

basicamente un firewall(muro de contension de trafico en la red)
![image.png](https://boostnote.io/api/teams/1R0f4yHjo/files/5fe3b81d09ac197b6a664d06b4bf1c54598e00a14df3577c35adcf63b4b4f328-image.png)

listar reglas

```
ipables -L
```

eliminar reglas

```
iptables -X
```
para modificar

```
iptables -P (INPUT/OUTPUD/FORWARD)(DROP/ACCEPT)
```

reglas basicas de red especificando puerto

```
iptables -A INPUT -i lo -p udp -s 127.0.0.1 -d 127.0.1.1 --sport 40000:65535 --dport 53 -m limit --limit 50/s -j ACCEPT
iptables -A INPUT -i lo -p udp -d 127.0.0.1 -s 127.0.1.1 --sport 40000:65535 --dport 53 -m limit --limit 50/s -j ACCEPT
```

reglas basicas de red orientadas a relacion

```
iptables -A INPUT -p udp --sport 67 --dport 68 -m state --state RELATED,ENTABLISHED -j ACCEPT
iptables -A INPUT -p udp --sport 53 -m limit --limit 50/s -j ACCEPT
```

reglas basicas de internet 

```
iptables -A INPUT -p tcp -m multiport --sport 80,443,8080 -m state --state ENTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p tcp -m multiport --sports 443,80 -m state --state NEW,ENTABLISHED -m limit --limit 50/s -j ACCEPT
# iptables -A INPUT -p icmp -s 0/0 -j ACCEPT

```

reglas especiales

```
iptables -A INPUT -p tcp --destination-port 8000 -j ACCEPT
```

[generar reglas de iptables](https://www.perturb.org/content/iptables-rules.html)
