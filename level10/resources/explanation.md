On a un executable level10 et un fichier token inaccessible.
Quand on lance l'executable sans arguments on a le message suivant :
```bash
./level10 file host
    sends file to host if you have access to it
```

En utilisant ghidra sur l'executable, on peut mieux comprendre son fonctionnement.
On note que le programme va d'abord tester si l'utilisateur a les droits d'accès au fichier (argument 2) à transférer, avec la fonction access() :
```c
  ...
  iVar2 = access(argv[1],4);
  if (iVar2 == 0) {
    printf("Connecting to %s:6969 .. ",__cp);
    fflush(stdout);
    iVar2 = socket(2,1,0);
  ...
```
L'utilisation d'un lien symbolique ne marche pas puisque la fonction access() déréférence la cible avant de procéder à ses vérifications. 
Il devrait être possible de tromper le programme en le lançant sur un fichier auquel on a accès, puis en le remplaçant avec un lien symbolique sur le token en cours d'execution, comme ont peut le lire dans le man de la fonction access.

Ce type de faille est une race condition. Pour la résoudre, il faut lancer le programme à duper et effectuer des opérations de modifications au court de ce lancement.

Après plusieurs tentatives qui n'ont pas permis de résoudre le problème de timing, on a pensé à lancer le programme en boucle avec un script dans un shell, à lancer un autre script de modification de la cible en boucle dans un autre shell, pendant qu'un dernier script lance netcat en boucle en écoute sur localhost:6969 pour récupérer l'output du programme.

Les différents scripts à exécuter sont dans les ressources.

On obtient un token toutes les 2-3 executions du programme !
```woupa2yuojeeaaed06riuj63c```

Ce token nous permet de nous connecter avec l'utilisateur `flag10` puis de faire `getflag` pour obtenir le flag :
```feulo4b72j7edeahuete3no7c```
