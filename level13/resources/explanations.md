On a un executable `level13` qui semble vérifier l'UID avant de lancer son contenu.

On a l'UID 2013 mais le programme s'attend à l'UID 4242.

Nos recherches nous ont menés à [une solution](https://axcheron.github.io/playing-with-ld_preload/) qui consisterait à exploiter LD_PRELOAD.

On crée un fichier `exploit.c` dont le but est de remplacer la fonction de la libc `getuid` pour renvoyer l'UID voulu :
```c
int getuid() {
    return 4242;
}
```

On crée un .so avec ce .c :
```bash
level13@SnowCrash:/tmp$ gcc /tmp/exploit.c -o /tmp/exploit.so -fPIC -shared -ldl
```

L'avantage d'un `shared object` c'est qu'il peut être linked pendant le runtime.

Puis on export notre variable d'environnement `LD_PRELOAD` :
```bash
export LD_PRELOAD="/tmp/exploit.so"
```

Ça ne change rien à l'exécution classique de `level13`, mais une fois lancé avec gdb (comme vu dans l'article cité précédemment), on arrive à lancer le programme avec notre shared library !

```bash
level13@SnowCrash:~$ gdb level13
GNU gdb (Ubuntu/Linaro 7.4-2012.04-0ubuntu2.1) 7.4-2012.04
Copyright (C) 2012 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "i686-linux-gnu".
For bug reporting instructions, please see:
<http://bugs.launchpad.net/gdb-linaro/>...
Reading symbols from /home/user/level13/level13...(no debugging symbols found)...done.
(gdb) r
Starting program: /home/user/level13/level13 
your token is 2A31L79asukciNyi8uppkEuSx
```
