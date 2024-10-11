## TAGS : #WEB 

Scan basique, deux ports ouverts ( 22,80 )

Site WEB simple 
![[Pasted image 20240307094832.png]]

Utilisation de gobuster pour vérifier si le site contient des fichiers spécifiques :
`gobuster dir -u http://10.10.109.201/ -w /usr/share/wordlists/dirb/common.txt -t 30`

On trouve le dossier /r on recommence puis on finit par trouver /r/a/b/b/i/t/
![[Pasted image 20240307131540.png]]

On trouve des identifiants sur le code source : 
```
alice:HowDothTheLittleCrocodileImproveHisShiningTail
```

![[Pasted image 20240307131610.png]]