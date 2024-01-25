# Level 03
Le répertoire de ce niveau contient un exécutable appartenant à flag03.

Quand on ouvre l'exécutable en question sur Ghidra, on voit qu'il exécute la commande `echo exploit me`

On va donc exploiter la variable d'environement `PATH` pour lui faire exécuter `getflag`: on copie `/bin/getflag` dans `/tmp/echo`, et on set `PATH` à `/tmp`.

On peut ensuite lancer l'exécutable qui va nous donner le résultat de `getflag`: `qi0maab88jeaj46qoumi7maus`