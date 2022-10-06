# TP1 - Premier pas réseau

# I. Exploration locale en solo

## 1. Affichage d'informations sur la pile TCP/IP locale

**🌞 Affichez les infos des cartes réseau de votre PC**

```PS C:\Users\matas> ipconfig /all```
- nom, adresse MAC et adresse IP de l'interface WiFi
Suffixe DNS propre à la connexion. . . :
Description. . . . . . . . . . . . . . : Intel(R) Wi-Fi 6 AX201 160MHz
   Adresse physique . . . . . . . . . . . : D4-54-8B-A5-2C-DD
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::110c:f184:586f:7a10%15(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.16.151(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : lundi 3 octobre 2022 08:46:04
   Bail expirant. . . . . . . . . . . . . : mardi 4 octobre 2022 08:46:04
   Passerelle par défaut. . . . . . . . . : 10.33.19.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254
   IAID DHCPv6 . . . . . . . . . . . : 181687435
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-29-9C-D5-05-C0-18-03-88-4E-B8
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       8.8.4.4
                                       1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
   
- nom, adresse MAC et adresse IP de l'interface Ethernet

Statut du média. . . . . . . . . . . . : Média déconnecté
Suffixe DNS propre à la connexion. . . : lan
 Description. . . . . . . . . . . . . . : Realtek Gaming GbE Family Controller
 Adresse physique . . . . . . . . . . . : C0-18-03-88-4E-B8
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
  
Je suis pas connécté via ethernet.

**🌞 Affichez votre gateway**

```PS C:\Users\matas> ipconfig /all```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wi-Fi 6 AX201 160MHz
   Adresse physique . . . . . . . . . . . : D4-54-8B-A5-2C-DD
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::110c:f184:586f:7a10%15(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.16.151(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : lundi 3 octobre 2022 08:46:04
   Bail expirant. . . . . . . . . . . . . : mardi 4 octobre 2022 08:46:04
   Passerelle par défaut. . . . . . . . . : 10.33.19.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254
   IAID DHCPv6 . . . . . . . . . . . : 181687435
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-29-9C-D5-05-C0-18-03-88-4E-B8
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       8.8.4.4
                                       1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé

**🌞 Déterminer la MAC de la passerelle**

```PS C:\Users\matas> arp -a```
Interface : 10.33.16.151 --- 0xf
  Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  10.33.19.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

### En graphique (GUI : Graphical User Interface)


**🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)**

![](https://i.imgur.com/QozxhVu.png)


## 2. Modifications des informations

### A. Modification d'adresse IP (part 1)  

🌞 Utilisez l'interface graphique de votre OS pour **changer d'adresse IP** :

```PS C:\Users\matas> ipconfig```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::110c:f184:586f:7a10%15
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.16.151
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Passerelle par défaut. . . . . . . . . : 10.33.19.254

🌞 **Il est possible que vous perdiez l'accès internet.** Que ce soit le cas ou non, expliquez pourquoi c'est possible de perdre son accès internet en faisant cette opération.

Si on se connecte avec la même adresse IP, le routeur enverra des info qu'au premier connécté avec cette adresse.


# II. Exploration locale en duo

## 1. Prérequis

## 2. Câblage

## Création du réseau (oupa)

## 3. Modification d'adresse IP

🌞 **Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau**

![](https://i.imgur.com/cOO4DvU.png)

🌞 **Vérifier à l'aide d'une commande que votre IP a bien été changée**

``` PS C:\Users\b> ipconfig /all
Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . : digitechnic.local
   Description. . . . . . . . . . . . . . : Intel(R) Ethernet Controller (3) I225-V
   Adresse physique . . . . . . . . . . . : B0-25-AA-47-C7-A4
   DHCP activé. . . . . . . . . . . . . . : Non
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::142a:7980:caa9:8be4%10(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.10.10.250(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
   IAID DHCPv6 . . . . . . . . . . . : 632300970
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2A-5A-6F-F9-B0-25-AA-47-C7-A4
   Serveurs DNS. . .  . . . . . . . . . . : 192.168.100.90
                                       192.168.100.72
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
   ```

🌞 **Vérifier que les deux machines se joignent**

```
PS C:\Users\b> ping  10.10.10.251
Envoi d’une requête 'Ping'  10.10.10.251 avec 32 octets de données :
Réponse de 10.10.10.251 : octets=32 temps=84 ms TTL=128
Réponse de 10.10.10.251 : octets=32 temps=2 ms TTL=128
Réponse de 10.10.10.251 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.251 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 10.10.10.251:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 84ms, Moyenne = 22ms
```

🌞 **Déterminer l'adresse MAC de votre correspondant**

```
PS C:\Users\b> arp -a
Interface : 10.10.10.250 --- 0xa
  Adresse Internet      Adresse physique      Type
  10.10.10.251          c0-18-03-59-30-32     dynamique
  10.10.10.255          ff-ff-ff-ff-ff-ff     statique
  169.254.225.125       c0-18-03-59-30-32     dynamique
  224.0.0.0             01-00-5e-00-00-00     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```


## 4. Utilisation d'un des deux comme gateway

🌞**Tester l'accès internet**

```
PS C:\Users\b> ping 1.1.1.1

Envoi d’une requête 'Ping'  1.1.1.1 avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=22 ms TTL=54
Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=54
Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=54
Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=54

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 21ms, Maximum = 22ms, Moyenne = 21ms
```

🌞 **Prouver que la connexion Internet passe bien par l'autre PC**

```PS C:\Users\b> tracert 192.168.137.1```

Détermination de l’itinéraire vers Milanese [192.168.137.1]
avec un maximum de 30 sauts :

  1     1 ms     1 ms     1 ms  Milanese [192.168.137.1]

Itinéraire déterminé.

## 5. Petit chat privé

🌞 **sur le PC *serveur*** avec par exemple l'IP 192.168.1.1
```
PS C:\Users\b\netcat-win32-1.11\netcat-1.11> .\nc.exe -l -p 9999
coucou
yes
ça marche
lets goooooooooo
allez lol
gggggg
```
🌞 **sur le PC *client*** avec par exemple l'IP 192.168.1.2
```
PS C:\Users\b\netcat-win32-1.11\netcat-1.11> .\nc.exe 192.168.137.1 8888

okaay
couc
```
🌞 **Visualiser la connexion en cours**
```
PS C:\Windows\system32> netstat -a -n -b
 Impossible d’obtenir les informations de propriétaire
  TCP    192.168.137.2:9999     192.168.137.1:64049    ESTABLISHED
 [nc.exe]
```
🌞 **Pour aller un peu plus loin**
```
PS C:\Windows\system32> netstat -a -n -b | Select-String 9999

  TCP    192.168.137.2:9999     0.0.0.0:0              LISTENING
```
## 6. Firewall

Toujours par 2.

Le but est de configurer votre firewall plutôt que de le désactiver

🌞 **Activez et configurez votre firewall**
```
PS C:\Users\b\netcat-win32-1.11\netcat-1.11> .\nc.exe -l -p 9999

SALOUTE
le firewall est activé et ca marche
```
# III. Manipulations d'autres outils/protocoles côté client

## 1. DHCP

🌞**Exploration du DHCP, depuis votre PC**
```
PS C:\Users\matas> ipconfig /all

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wi-Fi 6 AX201 160MHz
   Adresse physique . . . . . . . . . . . : D4-54-8B-A5-2C-DD
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::110c:f184:586f:7a10%15(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.17.35(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : jeudi 6 octobre 2022 11:36:55
   Bail expirant. . . . . . . . . . . . . : vendredi 7 octobre 2022 11:36:55
   Passerelle par défaut. . . . . . . . . : 10.33.19.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254
   IAID DHCPv6 . . . . . . . . . . . : 181687435
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-29-9C-D5-05-C0-18-03-88-4E-B8
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
## 2. DNS

🌞** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**
```
PS C:\Users\matas> ipconfig /all

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Intel(R) Wi-Fi 6 AX201 160MHz
   Adresse physique . . . . . . . . . . . : D4-54-8B-A5-2C-DD
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::110c:f184:586f:7a10%15(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.17.35(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : jeudi 6 octobre 2022 11:36:55
   Bail expirant. . . . . . . . . . . . . : vendredi 7 octobre 2022 11:36:55
   Passerelle par défaut. . . . . . . . . : 10.33.19.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254
   IAID DHCPv6 . . . . . . . . . . . : 181687435
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-29-9C-D5-05-C0-18-03-88-4E-B8
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
🌞 Utiliser, en ligne de commande l'outil `nslookup` (Windows, MacOS) ou `dig` (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

PS C:\Users\matas> nslookup google.com
```
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:80e::200e
          216.58.209.238
```
PS C:\Users\matas> nslookup ynov.com
```
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:ae9
          2606:4700:20::681a:be9
          2606:4700:20::ac43:4ae2
          104.26.10.233
          104.26.11.233
          172.67.74.226
```
Cela affiche le nom du domaine et ses adresses IP.

PS C:\Users\matas> nslookup 231.34.113.12
```
Serveur :   dns.google
Address:  8.8.8.8

*** dns.google ne parvient pas à trouver 231.34.113.12 : Non-existent domain
```
PS C:\Users\matas> nslookup 78.34.2.17
```
Serveur :   dns.google
Address:  8.8.8.8

Nom :    cable-78-34-2-17.nc.de
Address:  78.34.2.17
```
L'adresse 231.34.113.12 n'est atribué a aucun domaine.
L'adresse 78.34.2.17 est attribué au domaine du nom de cable-78-34-2-17.nc.de.

# IV. Wireshark

🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

![](https://i.imgur.com/FUxB4Zs.png)
![](https://i.imgur.com/51ewR4o.png)

![](https://i.imgur.com/zq0HGNn.png)

# Bilan

**Vu pendant le TP :**

- visualisation de vos interfaces réseau (en GUI et en CLI)
- extraction des informations IP
  - adresse IP et masque
  - calcul autour de IP : adresse de réseau, etc.
- connaissances autour de/aperçu de :
  - un outil de diagnostic simple : `ping`
  - un outil de scan réseau : `nmap`
  - un outil qui permet d'établir des connexions "simples" (on y reviendra) : `netcat`
  - un outil pour faire des requêtes DNS : `nslookup` ou `dig`
  - un outil d'analyse de trafic : `wireshark`
- manipulation simple de vos firewalls

**Conclusion :**

- Pour permettre à un ordinateur d'être connecté en réseau, il lui faut **une liaison physique** (par câble ou par *WiFi*).  
- Pour réceptionner ce lien physique, l'ordinateur a besoin d'**une carte réseau**. La carte réseau porte une adresse MAC  
- **Pour être membre d'un réseau particulier, une carte réseau peut porter une adresse IP.**
Si deux ordinateurs reliés physiquement possèdent une adresse IP dans le même réseau, alors ils peuvent communiquer.  
- **Un ordintateur qui possède plusieurs cartes réseau** peut réceptionner du trafic sur l'une d'entre elles, et le balancer sur l'autre, servant ainsi de "pivot". Cet ordinateur **est appelé routeur**.
- Il existe dans la plupart des réseaux, certains équipements ayant un rôle particulier :
  - un équipement appelé *passerelle*. C'est un routeur, et il nous permet de sortir du réseau actuel, pour en joindre un autre, comme Internet par exemple
  - un équipement qui agit comme **serveur DNS** : il nous permet de connaître les IP derrière des noms de domaine
  - un équipement qui agit comme **serveur DHCP** : il donne automatiquement des IP aux clients qui rejoigne le réseau
  - **chez vous, c'est votre Box qui fait les trois :)**