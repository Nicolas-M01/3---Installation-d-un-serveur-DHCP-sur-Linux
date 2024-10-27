# 3---Installation-d-un-serveur-DHCP-sur-Linux

1) Sur Virtual Box, monter 3 VMs:
* 1 Ubuntu en serveur
* 1 Debian pour l'IP static
* 1 XXXXXXX en DHCP
et mettre les cartes réseaux des VM en "Réseau interne" et choisir un nom de réseau pour les 3 VMs
### Configurer le Serveur
2) Il faut installer les paquets du DHCP serveur, en root :
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


### Configuration pour la machine en IP statique
1) Editer le ficher de config des interfaces réseau
```console
# nano /etc/network/interfaces
```
2) Modifier pour obtenir :
```bash
auto eth0
iface eth0 inet static 
  address 192.168.0.100
  netmask 255.255.255.0
  gateway 192.168.0.1
  dns-nameservers 4.4.4.4
  dns-nameservers 8.8.8.8
```
