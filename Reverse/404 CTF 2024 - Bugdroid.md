Décompilation de l'apk : 
`apktool d fichier.apk`

On ouvre sur vscode le ficheir de sortie
On trouve un premier message qui nous indique la suite pour trouver le message : 
`/home/anthony/Desktop/chall/Bugdroid_Fight_-_Part_1/smali_classes2/com/example/reverseintro/Utils.smali`

On trouve : 
`const-string v0, "_m3S5ag3!"`

Si on arrive pas à déduire le début du message, on cherche un autre string avec une autre partie du message 
`const-string v4, "Br4v0_tU_as_`

On peux déduire le reste du flag : 
404CTF{Br4v0_tU_as_tr0uv3_m0N_m3S5ag3!}