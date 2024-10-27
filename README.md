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
3) Editer le fichier /etc/default/isc-dhcp-server pour que DHCPD ecoute sur enp0s3 :
 ```bash
INTERFACES = "enp0s3"
 ```


4) Editer le fichier de configuration du serveur :
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
}
```
5)
Ajouter les informations pour la machine statique, en indiqant son IP fixe et son adresse MAC :
```bash
host debian-node{
 hardware ethernet 08:00:27:54:6d:d2;
 fixed-address 172.20.0.10;
}
```

### Configuration pour la machine en IP statique
1) Editer le ficher de config des interfaces réseau
```console
# nano /etc/network/interfaces
```
2) Modifier pour obtenir :
```bash
auto enp0s3
iface enp0s3 inet static 
  address 172.20.0.10
  netmask 255.255.255.0
  gateway 172.20.0.1
```
3) Sauvegarder et éditer le fichier /etc/resolv.conf
```bash
# nano /etc/resolv.conf
nameserver 8.8.8.8 

```
4) Redémarrer
```bash
# /etc/init.d/network restart  [On SysVinit]
# systemctl restart network    [On SystemD]
```
