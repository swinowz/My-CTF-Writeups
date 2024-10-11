TAGS : #WEB #GIT 
https://medium.com/stolabs/git-exposed-how-to-identify-and-exploit-62df3c165c37

Gobuster :
![[Pasted image 20240315155845.png]]

.git exposé 
On récup le fichier .git 
```
wget --mirror -I .git http://ctf.hackinprovence.fr:33378/.git/
```

On se déplace dans le fichier téléchargé 
* git status 
![[Pasted image 20240315160044.png]]
* git restore .  --> restaurer les ficheirs supprimés 
* git log -- voir les commits passés 
![[Pasted image 20240315160132.png]]

Un commit nommé "What is this ?" --> sus donc on vérifie
* git show --id--
![[Pasted image 20240315160216.png]]

