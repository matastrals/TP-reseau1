# TP2 : Ethernet, IP, et ARP

Dans ce TP on va approfondir trois protocoles, qu'on a survolé jusqu'alors :

- **IPv4** *(Internet Protocol Version 4)* : gestion des adresses IP
  - on va aussi parler d'ICMP, de DHCP, bref de tous les potes d'IP quoi !
- **Ethernet** : gestion des adresses MAC
- **ARP** *(Address Resolution Protocol)* : permet de trouver l'adresse MAC de quelqu'un sur notre réseau dont on connaît l'adresse IP

![Seventh Day](./pics/tcpip.jpg)

# Sommaire

- [TP2 : Ethernet, IP, et ARP](#tp2--ethernet-ip-et-arp)
- [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. Setup IP](#i-setup-ip)
- [II. ARP my bro](#ii-arp-my-bro)
- [II.5 Interlude hackerzz](#ii5-interlude-hackerzz)
- [III. DHCP you too my brooo](#iii-dhcp-you-too-my-brooo)

# 0. Prérequis

**Il vous faudra deux machines**, vous êtes libres :

- toujours possible de se connecter à deux avec un câble
- sinon, votre PC + une VM ça fait le taf, c'est pareil
  - je peux aider sur le setup, comme d'hab

> Je conseille à tous les gens qui n'ont pas de port RJ45 de go PC + VM pour faire vous-mêmes les manips, mais on fait au plus simple hein.

---

**Toutes les manipulations devront être effectuées depuis la ligne de commande.** Donc normalement, plus de screens.

**Pour Wireshark, c'est pareil,** NO SCREENS. La marche à suivre :

- vous capturez le trafic que vous avez à capturer
- vous stoppez la capture (bouton carré rouge en haut à gauche)
- vous sélectionnez les paquets/trames intéressants (CTRL + clic)
- File > Export Specified Packets...
- dans le menu qui s'ouvre, cochez en bas "Selected packets only"
- sauvegardez, ça produit un fichier `.pcapng` (qu'on appelle communément "un ptit PCAP frer") que vous livrerez dans le dépôt git

**Si vous voyez le p'tit pote 🦈 c'est qu'il y a un PCAP à produire et à mettre dans votre dépôt git de rendu.**

# I. Setup IP

Le lab, il vous faut deux machines : 

- les deux machines doivent être connectées physiquement
- vous devez choisir vous-mêmes les IPs à attribuer sur les interfaces réseau, les contraintes :
  - IPs privées (évidemment n_n)
  - dans un réseau qui peut contenir au moins 1000 adresses IP (il faut donc choisir un masque adapté)
  - oui c'est random, on s'exerce c'est tout, p'tit jog en se levant c:
  - le masque choisi doit être le plus grand possible (le plus proche de 32 possible) afin que le réseau soit le plus petit possible

🌞 **Mettez en place une configuration réseau fonctionnelle entre les deux machines**
(J'utilise deux machines à moi)
adresse réseau : 192.168.60.0
adresse broadcast : 192.168.63.255
```
PS C:\windows\system32> netsh interface ipv4 set address name="Ethernet" static 192.168.63.1 255.255.252.0

PS C:\windows\system32> ipconfig

Configuration IP de Windows


Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . : lan
   Adresse IPv6 de liaison locale. . . . .: fe80::59e:34fd:a8af:5dad%9
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.63.1
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Passerelle par défaut. . . . . . . . . :
```
2nd adresse IP : 192.168.63.2

🌞 **Prouvez que la connexion est fonctionnelle entre les deux machines**
```
PS C:\windows\system32> ping 192.168.63.2

Envoi d’une requête 'Ping'  192.168.63.2 avec 32 octets de données :
Réponse de 192.168.63.2 : octets=32 temps<1ms TTL=128
Réponse de 192.168.63.2 : octets=32 temps=1 ms TTL=128
Réponse de 192.168.63.2 : octets=32 temps<1ms TTL=128
Réponse de 192.168.63.2 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 192.168.63.2:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 1ms, Moyenne = 0ms
```

🌞 **Wireshark it**
C'est un ICMP de type 8 qui est un "echo request" pour la requête.
C'est un ICMP de type 0 qui est un "echo reply" pour la réponse.
![Ping Wireshark](./wireshark_screen_Ping_TP2.pcapng)

# II. ARP my bro

🌞 **Check the ARP table**
```
PS C:\windows\system32> arp -a

Interface : 192.168.63.1 --- 0x9
  Adresse Internet      Adresse physique      Type
  192.168.63.2          30-9c-23-49-c1-00     dynamique
  192.168.63.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  224.0.1.60            01-00-5e-00-01-3c     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```
🌞 **Manipuler la table ARP**
```
PS C:\windows\system32> arp -d
```

```
PS C:\windows\system32> arp -a

Interface : 192.168.63.1 --- 0x9
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.38.1 --- 0xd
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.1.98 --- 0xe
  Adresse Internet      Adresse physique      Type
  192.168.1.254         38-35-fb-cf-30-cc     dynamique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 192.168.23.1 --- 0x10
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
```
```
PS C:\windows\system32> ping 192.168.63.2

Envoi d’une requête 'Ping'  192.168.63.2 avec 32 octets de données :
Réponse de 192.168.63.2 : octets=32 temps=2 ms TTL=128
Réponse de 192.168.63.2 : octets=32 temps=1 ms TTL=128
Réponse de 192.168.63.2 : octets=32 temps=1 ms TTL=128
Réponse de 192.168.63.2 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 192.168.63.2:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 2ms, Moyenne = 1ms
```
```
PS C:\windows\system32> arp -a

Interface : 192.168.63.1 --- 0x9
  Adresse Internet      Adresse physique      Type
  192.168.63.2          30-9c-23-49-c1-00     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique
```
🌞 **Wireshark it**

![ARP wireshark](./arp_wireshark_screen.pcapng)

🦈 **PCAP qui contient les trames ARP**

# II.5 Interlude hackerzz

**Chose promise chose due, on va voir les bases de l'usurpation d'identité en réseau : on va parler d'*ARP poisoning*.**

> On peut aussi trouver *ARP cache poisoning* ou encore *ARP spoofing*, ça désigne la même chose.

Le principe est simple : on va "empoisonner" la table ARP de quelqu'un d'autre.  
Plus concrètement, on va essayer d'introduire des fausses informations dans la table ARP de quelqu'un d'autre.

Entre introduire des fausses infos et usurper l'identité de quelqu'un il n'y a qu'un pas hihi.

---

➜ **Le principe de l'attaque**

- on admet Alice, Bob et Eve, tous dans un LAN, chacun leur PC
- leur configuration IP est ok, tout va bien dans le meilleur des mondes
- **Eve 'lé pa jonti** *(ou juste un agent de la CIA)* : elle aimerait s'immiscer dans les conversations de Alice et Bob
  - pour ce faire, Eve va empoisonner la table ARP de Bob, pour se faire passer pour Alice
  - elle va aussi empoisonner la table ARP d'Alice, pour se faire passer pour Bob
  - ainsi, tous les messages que s'envoient Alice et Bob seront en réalité envoyés à Eve

➜ **La place de ARP dans tout ça**

- ARP est un principe de question -> réponse (broadcast -> *reply*)
- IL SE TROUVE qu'on peut envoyer des *reply* à quelqu'un qui n'a rien demandé :)
- il faut donc simplement envoyer :
  - une trame ARP reply à Alice qui dit "l'IP de Bob se trouve à la MAC de Eve" (IP B -> MAC E)
  - une trame ARP reply à Bob qui dit "l'IP de Alice se trouve à la MAC de Eve" (IP A -> MAC E)
- ha ouais, et pour être sûr que ça reste en place, il faut SPAM sa mum, genre 1 reply chacun toutes les secondes ou truc du genre
  - bah ui ! Sinon on risque que la table ARP d'Alice ou Bob se vide naturellement, et que l'échange ARP normal survienne
  - aussi, c'est un truc possible, mais pas normal dans cette utilisation là, donc des fois bon, ça chie, DONC ON SPAM

---

➜ J'peux vous aider à le mettre en place, mais **vous le faites uniquement dans un cadre privé, chez vous, ou avec des VMs**

➜ **Je vous conseille 3 machines Linux**, Alice Bob et Eve. La commande `[arping](https://sandilands.info/sgordon/arp-spoofing-on-wired-lan)` pourra vous carry : elle permet d'envoyer manuellement des trames ARP avec le contenu de votre choix.

GLHF.

# III. DHCP you too my brooo

*DHCP* pour *Dynamic Host Configuration Protocol* est notre p'tit pote qui nous file des IPs quand on arrive dans un réseau, parce que c'est chiant de le faire à la main :)

Quand on arrive dans un réseau, notre PC contacte un serveur DHCP, et récupère généralement 3 infos :

- **1.** une IP à utiliser
- **2.** l'adresse IP de la passerelle du réseau
- **3.** l'adresse d'un serveur DNS joignable depuis ce réseau

L'échange DHCP  entre un client et le serveur DHCP consiste en 4 trames : **DORA**, que je vous laisse chercher sur le web vous-mêmes : D

🌞 **Wireshark it**

[DORA screen wireshark](./dora_wireshark.pcapng)

1 : 10.33.17.35
2 : 10.33.19.254
3 : 8.8.8.8