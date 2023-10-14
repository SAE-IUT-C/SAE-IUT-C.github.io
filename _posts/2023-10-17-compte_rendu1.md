---
title: Mise en place de la SAE3.ROM.03
date: 2023-10-14
categories: [SAE,SAE3.ROM.03]
tags: [SAE,SAE3.ROM.03,Compte rendu]     
---

# SAE 303 GRP 5 Etapes Vincent Enzo Noah

## 1. Objectifs

L'objectif de cette SAE est de monter nos compétences en administration Windows Server. Pour effectuer ceci, nous devons mettre en place une architecture réseau d'une entreprise qui possédant 3 sites distincs reliés par un tunnel IPSec.

## 2. Topologie

![](/assets/img/SAE3_ROM_03/Compte_rendu_1/Capture1.PNG)

L'adressage du site est le suivant:

### Site A

| Nom du réseau | Adresse de sous-réseau | Masque de sous-réseau |
|:-------------:|:----------------------:|:---------------------:|
|      dmzA     |       10.0.5.0/24      |     255.255.255.0     |
|   ressourceA  |       10.1.5.0/24      |     255.255.255.0     |
|    clientA    |       10.2.5.0/24      |     255.255.255.0     |

|    Nom de l'hôte   | Nom de l'interface | Nom du sous-réseau | Adresse IP de sous-réseau | Masque de sous-réseau |
|:------------------:|:------------------:|:------------------:|:-------------------------:|:---------------------:|
|       WinDMZA      |      ethernet      |        dmzA        |        10.0.5.1/24        |     255.255.255.0     |
|     WinClientA     |      ethernet      |       clientA      |        10.2.5.1/24        |     255.255.255.0     |
| PDCA.principal5.rt |      ethernet      |     ressourceA     |       10.1.5.128/24       |     255.255.255.0     |
|    StormshieldA    |         out        |     Réseau Home    |      192.168.1.56/24      |     255.255.255.0     |
|                    |         dmz        |        dmzA        |       10.0.5.254/24       |     255.255.255.0     |
|                    |         in         |     ressourceA     |       10.1.5.254/24       |     255.255.255.0     |
|                    |         in         |       clientA      |       10.2.5.254/24       |     255.255.255.0     |

### Site B

| Nom du réseau | Adresse de sous-réseau | Masque de sous-réseau |
|:-------------:|:----------------------:|:---------------------:|
|      dmzB     |       10.4.5.0/24      |     255.255.255.0     |
|   ressourceB  |       10.5.5.0/24      |     255.255.255.0     |
|    clientB    |       10.6.5.0/24      |     255.255.255.0     |

|    Nom de l'hôte   | Nom de l'interface | Nom du sous-réseau | Adresse IP de sous-réseau | Masque de sous-réseau |
|:------------------:|:------------------:|:------------------:|:-------------------------:|:---------------------:|
|       WinDMZB      |      ethernet      |        dmzA        |        10.4.5.1/24        |     255.255.255.0     |
|     WinClientB     |      ethernet      |       clientB      |        10.6.5.1/24        |     255.255.255.0     |
| SDCB.principal5.rt |      ethernet      |     ressourceB     |       10.5.5.128/24       |     255.255.255.0     |
|    StormshieldB    |         out        |     Réseau Home    |      192.168.1.54/24      |     255.255.255.0     |
|                    |         dmz        |        dmzB        |       10.4.5.254/24       |     255.255.255.0     |
|                    |         in         |     ressourceB     |       10.5.5.254/24       |     255.255.255.0     |
|                    |         in         |       clientB      |       10.6.5.254/24       |     255.255.255.0     |

### Site C

| Nom du réseau | Adresse de sous-réseau | Masque de sous-réseau |
|:-------------:|:----------------------:|:---------------------:|
|      dmzC     |       10.7.5.0/24      |     255.255.255.0     |
|   ressourceC  |       10.8.5.0/24      |     255.255.255.0     |
|    clientC    |       10.9.5.0/24      |     255.255.255.0     |

|         Nom de l'hôte        | Nom de l'interface | Nom du sous-réseau | Adresse IP de sous-réseau | Masque de sous-réseau |
|:----------------------------:|:------------------:|:------------------:|:-------------------------:|:---------------------:|
|            WinDMZC           |      ethernet      |        dmzC        |        10.7.5.1/24        |     255.255.255.0     |
|         WinRessourceC        |      ethernet      |     ressourceC     |        10.8.5.1/24        |     255.255.255.0     |
| SDCC.tertiaire.principal5.rt |      ethernet      |     ressourceC     |       10.8.5.128/24       |     255.255.255.0     |
|         StormshieldC         |         out        |     Réseau Home    |      192.168.1.55/24      |     255.255.255.0     |
|                              |         dmz        |        dmzC        |       10.7.5.254/24       |     255.255.255.0     |
|                              |         in         |     ressourceC     |       10.8.5.254/24       |     255.255.255.0     |
|                              |         in         |       clientC      |       10.9.5.254/24       |     255.255.255.0     |

### 3 Organisation du travail:
Seveur A: Nono
Serveur B: Vince
Serveur C: Enzo


### 4. Site A

Pour commencer, je me connecte sur le pare-feu en me connectant sur l'adaptateur réseau 2 de la VM.

![](/assets/img/SAE3_ROM_03/Compte_rendu_1/Capture2.PNG)

Après m'avoir attribué une IP dans le réseau 10.0.0.0/24, je me connecte sur l'interface web du firewall.

![](/assets/img/SAE3_ROM_03/Compte_rendu_1/Capture3.PNG)

