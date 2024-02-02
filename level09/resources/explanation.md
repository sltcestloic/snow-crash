On a un executable `level09` et un fichier `token`. 
On peut cat le fichier token :
`f4kmm6p|=�p�n��DB�Du{��`
 
Lancer l'executable sur le fichier nous donne cette chaine de caractères :
`tpmhr`

La création d'un fichier test dans le dossier /tmp/ avec une chaine de caractères normaux ("abc", "a", "c", "abcdefgh") et l'execution de level09 sur ce fichier donne toujours le même résultat :
`/uos3ykz|`

Après plusieurs tests, il devient apparent que le programme ne traite pas un fichier mais directement la chaîne de caractères données.

Après ce test :
`aaaaaaaa` => `abcdefgh`
Il semble que le programme change le la chaîne de caractères en ajoutant la valeur de la position à la valeur du caractère.

L'utilisation d'un éditeur hexadécimal permet de montrer la chaîne de caractères plus en détails :
`f4kmm6p|=épénâéDBâDu{îë`

Une tentative de décodage du token en enlevant l'index à chaque caractère donne ce résultat :
`f3iji1ju5浲dⱮ^ޭ�۪�-*֥�(X]П�͜�`
Qui ne marche cependant pas pour débloquer l'accès au flag ni au level10. Il est possible qu'il faille adopter une approche différente pour certains des caractères spéciaux.

Plusieurs tentatives ont mené à différents résultats non fonctionnels :
`f3iji1ju5￴f￲b￰￯52￬2bg￨￧`
`f3iji1ju5ôfòbðï52ì2bgèç`
`f3iji1ju5xueuatr41p1afitp`

Probablement la conversion la plus fidèle après comparaison des valeurs hexadécimales :
`f3iji1ju5yfwbvt52r2bgws`
Il manque seulement les caractères DEL qui servent probablement à autre chose quand dans un des levels précédents.
Supprimer les caractères avant les DEL f3iji1ju5fwbvt52r2bws ne marche pas.

En ajoutant les caractères DEL on obtient :
`f3iji1ju5yuevaus41q1afiuq`
Qui est le password pour l'utilisateur flag09 !

getflag
`s5cAJpM8ev6XHw998pRWG728z`