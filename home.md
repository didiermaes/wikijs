---
title: page test
description: Une page test pour vérifier le paramétrage
published: 1
date: 2021-03-14T18:40:47.699Z
tags: test
editor: markdown
dateCreated: 2021-03-12T22:22:07.311Z
---

# Pilote Macvlan pour Docker

Avec Macvlan plutôt que d'avoir à gérer un pont pour chaque réseau Docker, les conteneurs sont connectés directement à une interface telle que par exemple "eth0" qui attache le conteneur au même domaine de diffusion que l'interface parent. 

Exemple, si un hôte eth0 est sur le réseau **192.168.1.0/24** avec une passerelle **192.168.1.1** un réseau Macvlan Docker pourra démarrer des conteneurs sur les adresses **192.168.1.2 - 192.168.1.254.** Les conteneurs utilisent le même réseau que le parent spécifié lors de la création.

> Les conteneurs Macvlan sont faciles à dépanner. L'adresse MAC et IP réelle du conteneur est reliée au réseau en amont, ce qui permet aux opérateurs de suivre facilement une application. Les outils existants de gestion et de surveillance du réseau sous-jacent restent possibles.
{.is-info}


## Exemples


Les réseaux de pilotes Macvlan sont connectés à une interface hôte Docker parent. 

Tout conteneur Macvlan partageant le même sous-réseau peut communiquer via IP avec tout autre conteneur du même sous-réseau sans passerelle.


Dans l'exemple suivant, l'hôte eth0 docker a une adresse IP sur le réseau **172.16.86.0/24** et une passerelle par défaut de **172.16.86.1.**

La passerelle est un routeur externe avec une adresse de 1**72.16.86.1.**

Une adresse IP n'est pas requise sur l'interface hôte Docker eth0 en bridge-mode, elle doit simplement être sur le bon réseau en amont pour être transmise par un routeur.

![macvlan_bridge_simple[1].png](/macvlan_bridge_simple[1].png){.align-center}


Créez le réseau macvlan et exécutez quelques conteneurs qui y sont attachés :

```
# Macvlan  (-o macvlan_mode= Defaults to Bridge mode if not specified)
docker network create -d macvlan \
    --subnet=172.16.86.0/24 \
    --gateway=172.16.86.1  \
    -o parent=eth0 pub_net

# Run a container on the new network specifying the --ip address.
docker  run --net=pub_net --ip=172.16.86.10 -itd alpine /bin/sh

# Start a second container and ping the first
docker  run --net=pub_net -it --rm alpine /bin/sh
ping -c 4 172.16.86.10
```

Jetez un œil à l'adresse IP et à la table de routage des conteneurs :

```
ip a show eth0
    eth0@if3: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UNKNOWN
    link/ether 46:b2:6b:26:2f:69 brd ff:ff:ff:ff:ff:ff
    inet 172.16.86.2/24 scope global eth0

ip route
    default via 172.16.86.1 dev eth0
    172.16.86.0/24 dev eth0  src 172.16.86.2

# NOTE: the containers can NOT ping the underlying host interfaces as
# they are intentionally filtered by Linux for additional isolation.
# In this case the containers cannot ping the -o parent=172.16.86.250
```

Les utilisateurs peuvent spécifier explicitement le mode bridge avec l'option mode `-o macvlan_mode=bridge` ou en laissant l'option par défaut (bridge).

Bien que l'eth0 interface n'est pas besoin d'avoir une adresse IP, il n'est pas rare d'avoir une adresse IP sur l'interface. Aussi, des adresses peuvent être exclues de l'obtention d'une adresse à partir de l'IPAM intégré par défaut à l'aide de l'argument `--aux-address=x.x.x.x`. Cela mettra en liste noire l'adresse spécifiée.

Ce qui suit est un exemple :

```
docker network create -d macvlan \
    --subnet=172.16.86.0/24 \
    --gateway=172.16.86.1  \
    --aux-address="exclude_host=172.16.86.250" \
    -o parent=eth0 pub_net
```

Une autre option pour spécifier la plage d'adresses utilisables par défaut consiste à utiliser `--ip-range=argument`. Cela demande au pilote d'allouer des adresses de conteneur à partir de la plage spécifique, plutôt que de la plage avec `--subnet=argument`.

Le réseau crée dans l'exemple suivant, alloue des adresses à partir de **192.168.32.128** et incrémente n+1 :

```
docker network create -d macvlan  \
    --subnet=192.168.32.0/24  \
    --ip-range=192.168.32.128/25 \
    --gateway=192.168.32.254  \
    -o parent=eth0 macnet32

# Start a container and verify the address is 192.168.32.128
docker run --net=macnet32 -it --rm alpine /bin/sh
```

[Documentation Docker](https://docs.docker.com/network/bridge/)
