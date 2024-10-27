# 3---Installation-d-un-serveur-DHCP-sur-Linux

1) Sur Virtual Box, monter une VM Ubuntu et mettre la carte réseau de la VM en "Réseau interne" et choisir un nom de réseau
2) Il faut installer les paquets du DHCP server, en root :
```console
# apt install isc-dhcp-server
```  
3) Editer le fichier de configuration du serveur :
```console
$ sudo nano /etc/default/isc-dhcp-server
```
Et y ajouter
```bash
option domain-name "tecmint.lan";  
option domain-name-servers ns1.tecmint.lan, ns2.tecmint.lan;  
default-lease-time 3600;  
max-lease-time 7200;  
authoritative;  
```
Puis
```bash
subnet 192.168.10.0 netmask 255.255.255.0 {
        option routers                  192.168.10.1;
        option subnet-mask              255.255.255.0;
        option domain-search            "tecmint.lan";
        option domain-name-servers      192.168.10.1;
        range   192.168.10.10   192.168.10.100;
        range   192.168.10.110   192.168.10.200;
}
```
