# TP4 : TCP, UDP et services réseau

Dans ce TP on va explorer un peu les protocoles TCP et UDP. 

**La première partie est détente**, vous explorez TCP et UDP un peu, en vous servant de votre PC.

La seconde partie se déroule en environnement virtuel, avec des VMs. Les VMs vont nous permettre en place des services réseau, qui reposent sur TCP et UDP.  
**Le but est donc de commencer à mettre les mains de plus en plus du côté administration, et pas simple client.**

Dans cette seconde partie, vous étudierez donc :

- le protocole SSH (contrôle de machine à distance)
- le protocole DNS (résolution de noms)
  - essentiel au fonctionnement des réseaux modernes


# Sommaire

- [TP4 : TCP, UDP et services réseau](#tp4--tcp-udp-et-services-réseau)
- [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. First steps](#i-first-steps)
- [II. Mise en place](#ii-mise-en-place)
  - [1. SSH](#1-ssh)
  - [2. Routage](#2-routage)
- [III. DNS](#iii-dns)
  - [1. Présentation](#1-présentation)
  - [2. Setup](#2-setup)
  - [3. Test](#3-test)

# 0. Prérequis

➜ Pour ce TP, on va se servir de VMs Rocky Linux. On va en faire plusieurs, n'hésitez pas à diminuer la RAM (512Mo ou 1Go devraient suffire). Vous pouvez redescendre la mémoire vidéo aussi.  

➜ Si vous voyez un 🦈 c'est qu'il y a un PCAP à produire et à mettre dans votre dépôt git de rendu

➜ **L'emoji 🖥️ indique une VM à créer**. Pour chaque VM, vous déroulerez la checklist suivante :

- [x] Créer la machine (avec une carte host-only)
- [ ] Définir une IP statique à la VM
- [ ] Donner un hostname à la machine
- [ ] Vérifier que l'accès SSH fonctionnel
- [ ] Vérifier que le firewall est actif
- [ ] Remplir votre fichier `hosts`, celui de votre PC, pour accéder au VM avec un nom
- [ ] Dès que le routeur est en place, n'oubliez pas d'ajouter une route par défaut aux autres VM pour qu'elles aient internet

> Toutes les commandes pour réaliser ces opérations sont dans [le mémo Rocky](../../cours/memo/rocky_network.md). Aucune de ces étapes ne doit figurer dan le rendu, c'est juste la mise en place de votre environnement de travail.

# I. First steps

🌞 **Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP**

- avec Wireshark, on va faire les chirurgiens réseau
- déterminez, pour chaque application :
  - IP et port du serveur auquel vous vous connectez
  - le port local que vous ouvrez pour vous connecter

Ouvert : Leage of legends
```
PS C:\Users\matas> netstat -b -n -p udp -a
  UDP    0.0.0.0:53662          *:*
 [League of Legends.exe]
```
[LOL Wireshark](./LOL_ports.pcapng)

Ouvert : discord
```
PS C:\Users\matas> netstat -b -n -p udp -a
  UDP    0.0.0.0:59328          *:*
 [Discord.exe]
```
[Discord Wireshark](./Discord_ports.pcapng)

Ouvert : Minecraft
```
PS C:\Users\matas> netstat -b -n
  TCP    10.33.16.152:54125     209.222.114.2:25565    ESTABLISHED
 [javaw.exe]
```
[Minecraft Wireshark](./Minecraft_ports.pcapng)


Ouvert : Brawlhalla
```
PS C:\Users\matas> netstat -b -n -p udp -a
  UDP    0.0.0.0:65282          *:*
 [System]
```
[Brawlhalla Wireshark](./Brawlhalla_ports.pcapng)


Ouvert : Twitch (sur un stream)
```
PS C:\Users\matas> netstat -b -n
  TCP    10.33.16.152:54639     52.223.195.81:443      ESTABLISHED
 [chrome.exe]
 ```
[Twitch Wireshark](./Twitch_stream.pcapng)

# II. Mise en place

## 1. SSH

🖥️ **Machine `node1.tp4.b1`**

🌞 **Examinez le trafic dans Wireshark**
```
matas@Mata_milo MINGW64 ~
$ netstat -n -b

Connexions actives

  Proto  Adresse locale         Adresse distante       ▒tat
  TCP    10.4.1.1:53605         10.4.1.11:22           ESTABLISHED
 [ssh.exe]
```
[ssh Wireshark](./ssh.pcapng)

## 2. Routage

🖥️ **Machine `router.tp4.b1`**

# III. DNS

## 1. Présentation

## 2. Setup

🖥️ **Machine `dns-server.tp4.b1`**

🌞 **Dans le rendu, je veux**

```
[matastral@dns /]$ sudo cat etc/named.conf
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; any; };
        allow-query-cache {localhost; any; };

        /*
         - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
         - If you are building a RECURSIVE (caching) DNS server, you need to enable
           recursion.
         - If your recursive DNS server has a public IP address, you MUST enable access
           control to limit queries to your legitimate users. Failing to do so will
           cause your server to become part of large scale DNS amplification

           attacks. Implementing BCP38 within your network would greatly
           reduce such attack surface
        */
        recursion yes;

        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "tp4.b1" IN {
     type master;
     file "tp4.b1.db";
     allow-update { none; };
     allow-query {any; };
};

zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp4.b1.rev";
     allow-update { none; };
     allow-query { any; };
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```
```
[matastral@dns /]$ sudo cat var/named/tp4.b1.db
$ sudo cat /var/named/tp4.b1.db

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1 (
        2019061800 ;Serial
        3600 ;Refresh
        1800 ;Retry
        604800 ;Expire
        86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

; Enregistrement DNS pour faire correspondre des noms à des IPs
dns-server IN A 10.4.1.201
node1      IN A 10.4.2.11
```
```
[matastral@dns var]$ sudo cat named/tp4.b1.rev
$ sudo cat /var/named/tp4.b1.rev

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1 (
        2019061800 ;Serial
        3600 ;Refresh
        1800 ;Retry
        604800 ;Expire
        86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

; Reverse lookup for Name Server
201 IN PTR dns-server.tp4.b1.
11 IN PTR node1.tp4.b1
```
```
[matastral@dns ~]$ sudo systemctl status named
[sudo] password for matastral:
● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; vendor preset: disabl>
     Active: active (running) since Sun 2022-11-27 20:54:50 CET; 53s ago
    Process: 676 ExecStartPre=/bin/bash -c if [ ! "$DISABLE_ZONE_CHECKING" == "yes" ]; the>
    Process: 692 ExecStart=/usr/sbin/named -u named -c ${NAMEDCONF} $OPTIONS (code=exited,>
   Main PID: 696 (named)
      Tasks: 5 (limit: 5905)
     Memory: 24.4M
        CPU: 60ms
     CGroup: /system.slice/named.service
             └─696 /usr/sbin/named -u named -c /etc/named.conf

Nov 27 20:54:50 dns named[696]: zone localhost/IN: loaded serial 0
Nov 27 20:54:50 dns named[696]: all zones loaded
Nov 27 20:54:50 dns systemd[1]: Started Berkeley Internet Name Domain (DNS).
Nov 27 20:54:50 dns named[696]: running
Nov 27 20:54:50 dns named[696]: network unreachable resolving './NS/IN': 199.9.14.201#53
Nov 27 20:54:50 dns named[696]: network unreachable resolving './DNSKEY/IN': 198.41.0.4#53
Nov 27 20:54:50 dns named[696]: network unreachable resolving './NS/IN': 198.41.0.4#53
Nov 27 20:54:50 dns named[696]: resolver priming query complete
Nov 27 20:54:50 dns named[696]: managed-keys-zone: Unable to fetch DNSKEY set '.': failure
Nov 27 20:54:51 dns named[696]: listening on IPv4 interface enp0s8, 10.4.1.201#53
```
```
[matastral@dns ~]$ ss -listen

(...)

LISTEN         0              10                        10.4.1.201:53                        0.0.0.0:*            uid:25 ino:19038 sk:6f cgroup:/system.slice/named.service <->
         cubic cwnd:10
```

🌞 **Ouvrez le bon port dans le firewall**

```
[matastral@dns /]$ sudo firewall-cmd --add-port=53/tcp --permanent
[sudo] password for matastral:
success
```
```
[matastral@dns /]$ sudo firewall-cmd --reload
success
```


## 3. Test

🌞 **Sur la machine `node1.tp4.b1`**

- configurez la machine pour qu'elle utilise votre serveur DNS quand elle a besoin de résoudre des noms
- assurez vous que vous pouvez :
  - résoudre des noms comme `node1.tp4.b1` et `dns-server.tp4.b1`
  - mais aussi des noms comme `www.google.com`

🌞 **Sur votre PC**

- utilisez une commande pour résoudre le nom `node1.tp4.b1` en utilisant `10.4.1.201` comme serveur DNS

> Le fait que votre serveur DNS puisse résoudre un nom comme `www.google.com`, ça s'appelle la récursivité et c'est activé avec la ligne `recursion yes;` dans le fichier de conf.

🦈 **Capture d'une requête DNS vers le nom `node1.tp4.b1` ainsi que la réponse**