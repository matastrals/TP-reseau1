# TP3 : On va router des trucs

## I. ARP

### 1. Echange ARP

- effectuer un ping d'une machine à l'autre
```
[matastral@john ~]$ ping 10.3.1.12
PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=0.655 ms
64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=0.440 ms
64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=0.283 ms
^C
--- 10.3.1.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2058ms
rtt min/avg/max/mdev = 0.283/0.459/0.655/0.152 ms
```
- observer les tables ARP des deux machines
```
[matastral@john ~]$ ip neigh show
10.3.1.12 dev enp0s8 lladdr 08:00:27:8c:71:79 STALE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:44 DELAY
```
```
[matastral@marcel ~]$ ip neigh show
10.3.1.11 dev enp0s8 lladdr 08:00:27:c5:dc:17 STALE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:44 DELAY
```
- repérer l'adresse MAC de john dans la table ARP de marcel et vice-versa

adresse mac de john : 10.3.1.11 dev enp0s8 lladdr 08:00:27:c5:dc:17 STALE
adresse mac de marcel : 10.3.1.12 dev enp0s8 lladdr 08:00:27:8c:71:79 STALE
- prouvez que l'info est correcte (que l'adresse MAC que vous voyez dans la table est bien celle de la machine correspondante)
  - une commande pour voir la MAC de marcel dans la table      ARP  de john
  - et une commande pour afficher la MAC de marcel, depuis    marcel
```
[matastral@john ~]$ ip neigh show 10.3.1.12
10.3.1.12 dev enp0s8 lladdr 08:00:27:8c:71:79 STALE
```
```
[matastral@marcel ~]$ ip neigh show 10.3.1.11
10.3.1.11 dev enp0s8 lladdr 08:00:27:c5:dc:17 STALE
```

### 2. Analyse de trames

- utilisez la commande `tcpdump` pour réaliser une capture de trame
```[matastral@marcel ~]$ sudo tcpdump -c 5
[sudo] password for matastral:
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on enp0s8, link-type EN10MB (Ethernet), snapshot length 262144 bytes
09:30:31.031558 IP 10.3.1.1.54230 > marcel.ssh: Flags [.], ack 733939781, win 8193, length 0
09:30:31.035295 IP marcel.ssh > 10.3.1.1.54230: Flags [P.], seq 1:61, ack 0, win 501, length 60
09:30:31.035519 IP marcel.ssh > 10.3.1.1.54230: Flags [P.], seq 61:97, ack 0, win 501, length 36
09:30:31.035553 IP marcel.ssh > 10.3.1.1.54230: Flags [P.], seq 97:205, ack 0, win 501, length 108
09:30:31.035680 IP 10.3.1.1.54230 > marcel.ssh: Flags [.], ack 97, win 8193, length 0
5 packets captured
20 packets received by filter
0 packets dropped by kernel
```
- videz vos tables ARP, sur les deux machines, puis effectuez un `ping`

🦈 **Capture réseau `tp3_arp.pcapng`** qui contient un ARP request et un ARP reply

## II. Routage

### 1. Mise en place du routage
```
[matastral@rooter ~]$ sudo firewall-cmd --add-masquerade
success
[matastral@rooter ~]$ sudo firewall-cmd --add-masquerade --permanent
success
```
- il faut taper une commande ip route add pour cela, voir mémo
```
[matastral@marcel ~]$ sudo ip route add 10.3.2.0/24 via 10.3.2.254 dev enp0s8
```
- il faut ajouter une seule route des deux côtés
```
[matastral@john ~]$ sudo ip route add 10.3.1.0/24 via 10.3.1.254 dev enp0s8
```
```
[matastral@john ~]$ ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=1.49 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=1.49 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=1.24 ms
^C
--- 10.3.2.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 1.238/1.405/1.489/0.118 ms````
```
- une fois les routes en place, vérifiez avec un ping que les deux machines peuvent se joindre
```
[matastral@marcel ~]$ ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=63 time=0.675 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=63 time=2.48 ms
64 bytes from 10.3.1.11: icmp_seq=3 ttl=63 time=1.43 ms
^C
--- 10.3.1.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2023ms
rtt min/avg/max/mdev = 0.675/1.529/2.483/0.741 ms
```
### 2. Analyse de trames

🌞**Analyse des échanges ARP**

- videz les tables ARP des trois noeuds
```
[matastral@marcel ~]$ sudo ip neigh flush all
```
```
[matastral@john ~]$ sudo ip neigh flush all
```
```
[matastral@rooter ~]$ sudo ip neigh flush all
```
- effectuez un `ping` de `john` vers `marcel`
  - le tcpdump doit être lancé sur la machine john
```
[matastral@john ~]$ sudo tcpdump -i enp0s8 -w capturejm.pcapng
```
```
[matastral@john ~]$ ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=0.765 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=1.44 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=1.34 ms
^C
--- 10.3.2.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 0.765/1.179/1.435/0.295 ms
```
- essayez de déduire un peu les échanges ARP qui ont eu lieu
  - en regardant la capture et/ou les tables ARP de tout le monde
[arp_john_marcel](./arp_john_marcel.pcapng)
- répétez l'opération précédente (vider les tables, puis `ping`), en lançant `tcpdump` sur `marcel`
```
[matastral@marcel ~]$ sudo ip neigh flush all
```
```
[matastral@john ~]$ sudo ip neigh flush all
```
```
[matastral@rooter ~]$ sudo ip neigh flush all
```
```
[matastral@marcel ~]$ sudo tcpdump -i enp0s8 -w capturemj.pcapng
```
```
[matastral@marcel ~]$ ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=63 time=1.86 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=63 time=1.88 ms
^C
--- 10.3.1.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.864/1.871/1.879/0.007 ms
```
[arp_marcel_john](./arp_marcel_john.pcapng)
- **écrivez, dans l'ordre, les échanges ARP qui ont eu lieu, puis le ping et le pong, je veux TOUTES les trames** utiles pour l'échange

| ordre | type trame  | IP source | MAC source                  | IP destination | MAC destination             |
|-------|-------------|-----------|-----------------------------|----------------|-----------------------------|
| 1     | Requête ARP | x         | `marcel` `08:00:27:8c:71:79`| x              | Broadcast `FF:FF:FF:FF:FF`  |
| 2     | Réponse ARP | x         | Broadcast `FF:FF:FF:FF:FF`  | x              | `marcel` `08:00:27:8c:71:79`|
| ...   | ...         | ...       | ...                         |                |                             |
| 1     | Ping        | 10.3.2.12 | `marcel` `08:00:27:8c:71:79`| 10.3.1.11      | `john` `08:00:27:78:02:3d`  |
| 2     | Pong        | 10.3.1.11 | `john` `08:00:27:78:02:3d`  | 10.3.2.12      | `marcel` `08:00:27:8c:71:79`|

🦈 **Capture réseau `tp3_routage_marcel.pcapng`**

### 3. Accès internet

- ajoutez une carte NAT en 3ème inteface sur le `router` pour qu'il ait un accès internet
- ajoutez une route par défaut à `john` et `marcel`
  - vérifiez que vous avez accès internet avec un `ping`
  - le `ping` doit être vers une IP, PAS un nom de domaine
```
[matastral@marcel /]$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=53 time=23.4 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=53 time=24.5 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=53 time=30.3 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=53 time=24.3 ms
^C
--- 1.1.1.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 23.373/25.629/30.321/2.742 ms
```
- donnez leur aussi l'adresse d'un serveur DNS qu'ils peuvent utiliser
  - vérifiez que vous avez une résolution de noms qui fonctionne avec `dig`
  - puis avec un `ping` vers un nom de domaine
```
[matastral@marcel /]$ dig google.com

; <<>> DiG 9.16.23-RH <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11808
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             275     IN      A       142.250.179.78

;; Query time: 25 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Fri Oct 28 12:25:58 CEST 2022
;; MSG SIZE  rcvd: 55

[matastral@marcel /]$ ping google.com
PING google.com (142.250.74.238) 56(84) bytes of data.
64 bytes from par10s40-in-f14.1e100.net (142.250.74.238): icmp_seq=1 ttl=247 time=21.1 ms
64 bytes from par10s40-in-f14.1e100.net (142.250.74.238): icmp_seq=2 ttl=247 time=26.5 ms
64 bytes from par10s40-in-f14.1e100.net (142.250.74.238): icmp_seq=3 ttl=247 time=21.0 ms
^C
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 21.018/22.859/26.474/2.556 ms
```

- effectuez un `ping 8.8.8.8` depuis `john`
- capturez le ping depuis `john` avec `tcpdump`
[ping_internet](./ping_internet.pcapng)
- analysez un ping aller et le retour qui correspond et mettez dans un tableau :

| ordre | type trame | IP source           | MAC source               | IP destination | MAC destination    |     |
|-------|------------|---------------------|--------------------------|----------------|--------------------|-----|
| 1     | ping       | `john` `10.3.1.11`  | `john` `08:00:27c5:dc:17`| `8.8.8.8`      | `08:00:27:d8:0b:de`|     |
| 2     | pong       | `8.8.8.8`                | `08:00:27:d8:0b:de`                      | `john` `10.3.1.11`            |`john` `08:00:27c5:dc:17`                |

## III. DHCP

On reprend la config précédente, et on ajoutera à la fin de cette partie une 4ème machine pour effectuer des tests.

| Machine  | `10.3.1.0/24`              | `10.3.2.0/24` |
|----------|----------------------------|---------------|
| `router` | `10.3.1.254`               | `10.3.2.254`  |
| `john`   | `10.3.1.11`                | no            |
| `bob`    | oui mais pas d'IP statique | no            |
| `marcel` | no                         | `10.3.2.12`   |

```schema
   john               router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └─┬─┘    └─────┘    └───┘    └─────┘
   dhcp        │
  ┌─────┐      │
  │     │      │
  │     ├──────┘
  └─────┘
```

### 1. Mise en place du serveur DHCP

🌞**Sur la machine `john`, vous installerez et configurerez un serveur DHCP** (go Google "rocky linux dhcp server").

- installation du serveur sur `john`
```sudo dnf install dhcp-server -y```
- créer une machine `bob`
- faites lui récupérer une IP en DHCP à l'aide de votre serveur
```
default-lease-time 900;
max-lease-time 10800;

authoritative;

subnet 10.3.1.0 netmask 255.255.255.0 {
range 10.3.1.12 10.3.1.50;
option routers 10.3.1.254;
option subnet-mask 255.255.255.0;
option domain-name-servers 1.1.1.1;
}
```
> Il est possible d'utilise la commande `dhclient` pour forcer à la main, depuis la ligne de commande, la demande d'une IP en DHCP, ou renouveler complètement l'échange DHCP (voir `dhclient -h` puis call me et/ou Google si besoin d'aide).

🌞**Améliorer la configuration du DHCP**

- ajoutez de la configuration à votre DHCP pour qu'il donne aux clients, en plus de leur IP :
- récupérez de nouveau une IP en DHCP sur `bob` pour tester :
```
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:97:15:f9 brd ff:ff:ff:ff:ff:ff
    inet 10.3.1.12/24 brd 10.3.1.255 scope global dynamic noprefixroute enp0s8
       valid_lft 556sec preferred_lft 556sec
    inet6 fe80::a00:27ff:fe97:15f9/64 scope link
       valid_lft forever preferred_lft forever
```
```
[matastral@bob ~]$ ping 10.3.1.254
PING 10.3.1.254 (10.3.1.254) 56(84) bytes of data.
64 bytes from 10.3.1.254: icmp_seq=1 ttl=64 time=0.678 ms
64 bytes from 10.3.1.254: icmp_seq=2 ttl=64 time=1.09 ms
^C
--- 10.3.1.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.678/0.884/1.090/0.206 ms
```

  - il doit avoir une route par défaut
```
[matastral@bob ~]$ ip route show
default via 10.3.1.254 dev enp0s8 proto dhcp src 10.3.1.12 metric 100
10.3.1.0/24 dev enp0s8 proto kernel scope link src 10.3.1.12 metric 100
```
  - il doit connaître l'adresse d'un serveur DNS pour avoir de la résolution de noms
```
[matastral@bob ~]$ dig google.com

; <<>> DiG 9.16.23-RH <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 61623
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             174     IN      A       142.250.74.238

;; Query time: 15 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Sun Nov 06 16:01:54 CET 2022
;; MSG SIZE  rcvd: 55
```
```
[matastral@bob ~]$ ping google.com
PING google.com (142.250.201.174) 56(84) bytes of data.
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=1 ttl=113 time=17.8 ms
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=2 ttl=113 time=14.1 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 14.118/15.934/17.750/1.816 ms
```
### 2. Analyse de trames

🌞**Analyse de trames**

- lancer une capture à l'aide de `tcpdump` afin de capturer un échange DHCP
```sudo tcpdump -i enp0s8 -w ip_dhcp.pcapng```
- demander une nouvelle IP afin de générer un échange DHCP
```[matastral@bob ~]$ sudo systemctl restart NetworkManager```
- exportez le fichier `.pcapng`
[ip dhcp](./ip_dhcp.pcapng)
🦈 **Capture réseau `tp3_dhcp.pcapng`**