# Level 00

Ne voyant aucun fichier dans le dossier de l'user du level, on a décidé de chercher tous les fichiers appartenant à flag00 (l'user qui peut exécuter getflag)

```sh
level00@SnowCrash:~$ find / -user flag00 2>/dev/null
/usr/sbin/john
/rofs/usr/sbin/john
```

Les deux fichiers contiennent la même chaine de caractères: `cdiiddwpgswtgt`

On a essayé de se connecter à flag00 en utilisant ce string, ça ne 
fonctionne pas.<br>
On a donc cherché des sites qui proposent des algo de décryptage pour voir si on trouvait quelque chose qui ressemble à un mot, une phrase, ou un autre indice pour trouver le flag.

Sur https://dencode.com/, on n'a trouvé aucun résultat intéressant.<br>
Sur https://www.dcode.fr/rot-cipher, on à vu que le rot-12 donne: `nottoohardhere`

On a donc essayé de se conneter à flag00 en utilisant `nottoohardhere` en tant que mot de passe, et ça fonctionne ! On peut désormais utiliser `getflag` et passer au niveau suivant.