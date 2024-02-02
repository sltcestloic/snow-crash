Après avoir vérifié à beaucoup d'endroits, il n'y a aucune piste pour résoudre le level14.

On essaye donc de s'intérésser à l'executable `getflag` directement.

En l'ouvrant avec le debugger gdb, et en l'executant le programme se ferme directement.

On met un breakpoint sur la fonction main et on relance le programme en utilisant la commande `layout` afin d'observer le fichier source de manière plus poussée.

On identifie plusieurs appels à `fwrite` et `fputs`, un appel à la fonction `getuid`, un appel à une fonction `ptrace`, et surtout plusieurs appels à une fonction `ft_des`.

Après plusieurs tentatives infructueuses (dont de multiples segmentation faults) on arrive à identifier la partie du programme qui renvoie la valeur de get flag en mettant un deuxième breakpoint à l'appel de `ptrace`, puis en faisant un jump légèrement avant le dernier appel de la fonction `ft_des` (sur les opérations qui mettent les arguments de la fonction sur la stack) :

Opérations préliminaires :
```bash
level14@SnowCrash:~$ gdb /bin/getflag
GNU gdb (Ubuntu/Linaro 7.4-2012.04-0ubuntu2.1) 7.4-2012.04
Copyright (C) 2012 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "i686-linux-gnu".
For bug reporting instructions, please see:
<http://bugs.launchpad.net/gdb-linaro/>...
Reading symbols from /bin/getflag...(no debugging symbols found)...done.
(gdb) b main
Breakpoint 1 at 0x804894a
(gdb) b *0x8048989
Breakpoint 2 at 0x8048989
(gdb) run
Starting program: /bin/getflag 

Breakpoint 1, 0x0804894a in main ()
(gdb) lay next
```
Deuxième breakpoint et jump :
```bash
(gdb) b *0x8048989
Breakpoint 2 at 0x8048989
(gdb) n
Single stepping until exit from function main,
which has no line number information.

Breakpoint 2, 0x08048989 in main ()
(gdb) jump *0x8048de5
Continuing at 0x8048de5.
7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
(gdb) ior 1 (process 3298) exited normally]
```
On test le flag obtenu et il marche correctement :

```bash
level14@SnowCrash:~$ su flag14
Password: 
Congratulation. Type getflag to get the key and send it to me the owner of this livecd :)
flag14@SnowCrash:~$ getflag
Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
```
