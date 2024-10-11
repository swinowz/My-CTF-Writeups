## TAGS : #WEB #COMMANDINJECTION

La page transforme le texte qu'on lui envoi en figlet
![[Pasted image 20240308113946.png]]

On essaye d'escape le champs pour detecter des failles ( xss ?? ) pas réussi.
On essaye simplement -texte- **; id** et on obtient le résultat suivant
![[Pasted image 20240308114155.png]]

On a donc une vulnérabilité pour injecter une commande, le challenge nous indique que le flag est à la racine du serveur 
Après plusieurs tests ( ls & cat ) on obtient le flag : 
![[Pasted image 20240308114500.png]]