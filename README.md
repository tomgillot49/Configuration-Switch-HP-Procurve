 Admin Switch HP Procurve 2610

---

---

Configuration Réseau : 

![hp02533_01.jpg](hp02533_01.jpg)

**Switch-Tom01 :** 

- **VLAN 1** → Non actif
- **VLAN 10 → LAN_Serveur** ⇒ 172.16.12.1/24
    - Ports 1 à 10
    - Trunk port 48
- **VLAN 20 → LAN_Client** ⇒ 192.168.12.1/24
    - Ports 20 à 29
    - Trunk port 48

**Switch-Tom02 :** 

- **VLAN 1** → Non actif
- **VLAN 10 → LAN_Serveur** ⇒ 172.16.12.1/24
    - Ports 1 à 10
    - Trunk port 48
- **VLAN 20 → LAN_Client** ⇒ 192.168.12.1/24
    - Ports 20 à 29
    - Trunk port 48

# I. Connexion Avec Cable Console

Sur votre ordinateur, ouvrir PuTTY et le Gestionnaire de Périphériques en même temps.

Dans le Gestionnaire de Périphériques, cherchez sur quel port est branché votre câble. Ensuite, ouvrir Putty et se connecter "Serial" en lui donnant votre numéro de port.

Une fois sur votre interface Putty, vous devriez voir apparaître les informations du switch (Nom…).

## A. Commandes de bases

Se connecter et modifier la configuration : 

```c
Switch> enable
Switch# configure terminal "Entrer en mode configuration"
Switch(config)# exit "quitter les différents modes"
Switch# 
```

Pour renommer un switch : 

```c
Switch(config)# hostname Switch-Tom
Switch-Tom(config)
```

Pour voir une configuration existante :

```c
Switch# show running-config 
```

Pour mettre un message d’accueil :

```c
Switch(config)# banner motd #
	Bienvenue sur le Switch de Tom
	#
```

Pour enregistrer la configuration : 

```c
Switch(config)# write memory
```

Création d’un utilisateur : 

```c
Switch(config)# password manager user tom
New password for manager: ******* (tom)
Please retype new password for manager: ******* (tom)

```

Pour vérifier le statut des interfaces Ethernet : 

```c
Switch# show interfaces brief
```

Pour attribuer une passerelle par défaut : 

```c
Switch(config)# ip default-gateway 192.168.1.254 255.255.255.0
```

# II. Supprimer un configuration existante

Pour supprimer une configuration existante : 

```c
Switch# erase startup-config
"Si il reste un mot de passe et que vous le connaissez"
Switch# conf t
Switch (config)# no password manager
```

# III. Configuration de VLANs

A SAVOIR : 

- Tagged = Mode TRUNK chez CISCO
- Untagged = Mode ACCESS chez CISCO

Pour créer un VLAN : 

```c
Switch> en
Switch# conf t
Switch(config)# VLAN 10
Switch(vlan-10)# name LAN_Serveur
Switch(vlan-10)# VLAN 20
Switch(vlan-20)# name LAN_Client
Switch(vlan-20)# exit
Switch(config)#

```

Attribuer une adresse IP à un VLAN : 

```c
Switch> en
Switch# conf t
Switch(config)# VLAN 10
Switch(vlan-10)# ip address 172.16.12.1 255.255.255.0

Switch(vlan-10)# VLAN 20
Switch(vlan-20)# ip address 192.168.12.1 255.255.255.0
```

Pour attribuer un VLAN à un ports ou à un groupe de ports : 

```c
Switch(config)# vlan 10
Switch(vlan-10)# untagged ethernet 1-10 "pour un groupe de ports"
Switch(vlan-10)# untagged ethernet 15 "pour un seul port"
Switch(config)# tagged ethernet 48 "pour faire un trunk sur le port 48"
```

Pour supprimer un port ou un groupe de ports d’un VLAN :

*Exemple : Ports 1 à 10 sur le VLAN 20* 

```c
Switch(config)# VLAN 1
Switch(vlan-1)# untagged ethernet 1-10 "Met les ports 1 à 10 sur le VLAN natif"
Switch(vlan-1)# exit
Switch(config)# ex
Switch# show running-config "Affiche la configuration"
```

Configuration du mode DHCP sur les VLANS :

```c

```

# IV. Connexion SSH

Pour activer la connexion SSH, il vous faut obligatoirement une adresse IP sur votre PC qui est reliée à l'adresse IP du VLAN qui est mis sur votre interface.

Pour savoir sur quel VLAN est votre interface Ethernet, il vous suffit :

```c
Switch# show vlan 20 "Montre les interfaces qui sont connectées sur le VLAN 20"
```

Pour créer une connexion ssh : 

```c
Switch(config)# "créer un utilisateur avec un mot de passe"
Switch(config)# crypto key generate ssh bits 1024
Switch(config)# ip ssh
Switch(config)# show ip ssh

```

Ensuite, il faut ouvrir une console d'administration comme PuTTY et se connecter en SSH avec l'adresse IP du VLAN ainsi que votre nom d'utilisateur et votre mot de passe que vous avez créé un peu plus haut.

# V. Liaisons Trunk

Pour faire une liaison trunk entre deux switch, il faut que nous définissions 1 port sur chaque switch qui va permettre de faire passer les VLAN sur l'autre switch. 

```c
"Sur le Switch 1"
Switch> en
Switch# conf t
Switch(config)# interface ethernet 48
Switch(eth-48)# name Liaison_Trunk
Switch(eth-48)# exit
Switch(config)# vlan 10
Switch(vlan-10)# tagged ethernet 48
```

```c
"Sur le Switch 2"
Switch> en
Switch# conf t
Switch(config)# interface ethernet 48
Switch(eth-48)# name Liaison_Trunk
Switch(eth-48)# exit
Switch(config)# vlan 10
Switch(vlan-10)# tagged ethernet 48

```

# VI. Routage avec un switch PROCURVE 2610

**⚠️ Il faut désactiver le Wi-Fi pour que le routage Inter-VLANs se fasse ⚠️**

```c
Switch> en
Switch# conf t
Switch(config)# "Créer les VLANs + mettre les @IP + untagged les ports"
Switch(config)# ip routing "Activer le routage"
Switch(config)# show ip route "Vérifier que le routage ce fait bien"
```

Normalement, après cela, on peut établir des connexions (ping) entre les VLAN. 

Exemple : 

- Les PCs du VLAN 10 ping les PCs du VLAN 20
- Les PCs du VLAN 20 ping les PCs du VLAN 10dmin Switch HP Procurve 2610 184291169444807d8cb5da873ad7eb03.md…]()
