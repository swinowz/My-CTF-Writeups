Lancement avec Ghidra, lecture de la fonction principale : 

Sur celui-ci on voit la condition de victoire ( ici le premier if, on peut vérifier en double cliquant sur le syscall à la fin qui nous montre l'adresse de "victory" sur ghidra)
```c

/* WARNING: Control flow encountered bad instruction data */

void processEntry entry(void)

{
  size_t sVar1;
  char local_28;
  char local_27;
  char local_26;
  char local_25;
  char local_24;
  char local_23;
  char local_22;
  char local_21;
  char local_20;
  char local_1f;
  char local_1e;
  char local_1d;
  char local_1c;
  
  syscall();
  syscall();
  sVar1 = _strlen(&local_28);
  if (((((sVar1 == 0xe) && (local_27 == 't')) && (local_21 == 'r')) &&
      ((((local_1e == '1' && (local_1d == 's')) &&
        ((local_23 == 'n' && ((local_24 == '1' && (local_26 == 'u')))))) && (local_28 == '5')))) &&
     ((((local_1f == 'n' && (local_1c == '3')) && (local_20 == '0')) &&
      ((local_25 == 'p' && (local_22 == 't')))))) {
    syscall();
  }
  else {
    syscall();
  }
  syscall();
                    /* WARNING: Bad instruction - Truncating control flow here */
  halt_baddata();
}

```

On remet dans l'ordre et on obtient le flag 
flag : 404CTF{5tup1ntr0n1s3}