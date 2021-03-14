---
title: NETPLAN sur Ubuntu
description: 
published: 1
date: 2021-03-14T20:09:12.070Z
tags: linux, réseau
editor: markdown
dateCreated: 2021-03-14T19:04:54.527Z
---

# CONFIGURATION RÉSEAU AVEC NETPLAN SUR UBUNTU

> Depuis la version 18.04 (Bionic Beaver) Ubuntu est passé à Netplan pour la configuration des interfaces réseau. Il s'agit d'un système de configuration basé sur YAML, qui simplifie le processus de configuration.
{.is-info}


## Fichiers de configuration

Ce nouvel outil remplace le fichier de configuration (`/etc/network /interfaces`) qui avait été précédemment utilisé pour configurer les interfaces réseau sur Ubuntu.

Les fichiers de configuration se trouvent maintenant sous la forme de fichiers YAML à l'emplacement `/etc/netplan/*.yaml`.

Assurez-vous de respecter les normes YAML lorsque vous modifiez le fichier. Une erreur de syntaxe peut engendrer une mauvaise lecture du fichier de configuration.

Un fichier` 01-netcfg.yaml` est utilisé pour configurer la première interface.

Vous trouverez ci-dessous la configuration par défaut pour une interface utilisant DHCP :

```
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0f0:
      dhcp4: yes
      
```

Vous pouvez voir ci-dessous une liste des options de configuration les plus courantes et une description de leur utilisation.

|Option|Exemple|Description|
|------|--------|----------|
|adresse|[192.168.1.2/24, 62.210.123.123/32]|Une liste d'adresses IP à affecter à une interface. Le format utilise la notation CIDR.
|gateway4|192.168.1.1|	L'adresse IP de votre passerelle IPv4 locale.
dhcp4|	true|	Définir si DHCP est activé pour IPv4 - true ou false
dhcp6|	true|	Définir si DHCP est activé pour IPv6 - true ou false


## Configuration d'une IP failover avec Netplan

Pour configurer une adresse IP failover, vous devez éditer le fichier `/etc/netplan/01-netcfg.yaml` et configurer une mise en réseau statique pour votre serveur.

Les adresses IP doivent être écrites avec leur notation CIDR. Le masque de réseau est:

- /24 pour l'adresse IP principale du serveur
- /32 pour chaque adresse IP failover

Votre fichier de configuration devrait ressembler à l'exemple suivant :

```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0f0:
      addresses: [163.172.123.123/24, 212.83.123.123/32]
      gateway4: 163.172.123.1
      nameservers:
        addresses: [ "62.210.16.6", "62.210.16.7" ]
```

Une fois le fichier édité et sauvegardé, recharger la configuration avec la commande suivante : `sudo netplan apply`
