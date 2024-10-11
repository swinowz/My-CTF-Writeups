Impossible d'utiliser des commandes classiques (ls, cat etc )

Possible de faire de l'injection de commande

![](Images/Pasted%20image%2020240922114759.png)
Le flag est présent à la racine : 
![](Images/Pasted%20image%2020240922114817.png)

Python3 disponible : 
![](Images/Pasted%20image%2020240922114913.png)

Impossible d'utiliser python3, mais on peut utiliser /usr/bin/python3 ...
![](Images/Pasted%20image%2020240922115113.png)

On peut donc lire le fichier avec python : 

```
echo ; /usr/bin/python3 -c 'with open("flag.txt") as f: print(f.read())' 
```