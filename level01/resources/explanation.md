# Level 01

Pendant le level 00, on avais déjà remarqué en faisant des recherches basiques qu'il y avait quelque chose d'anormal dans le fichier `/etc/passwd`: le compte flag01 a quelque chose de plus que les autres

```flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash```


En faisant [des recherches](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/) sur le format du fichier passwd, on a vu qu'il s'agit du mot de passe du compte hashé.

Après quelques recherches supplémentaires, on a appris que la manière la plus efficace de cracker un mot de passe hashé (surtout en ne connaissant pas à coup sur l'algorithme de hashage utilisé) est d'utiliser un tool de bruteforce.

On a donc utilisé [John the ripper](https://github.com/openwall/john/tree/bleeding-jumbo) avec une [word list basique](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Software/john-the-ripper.txt)

```sh
lbertran@z1r10p1 ~/work/snow-crash/level01/resources/john-the-ripper                                                            
> $ ./john --wordlist=../wordlist.txt ../decode.txt                                                                                                                           
Using default input encoding: UTF-8
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 256/256 AVX2])
Cracked 1 password hash (is in ./john.pot), use "--show"
No password hashes left to crack (see FAQ)
                                                                                                                                                                                          
lbertran@z1r10p1 ~/work/snow-crash/level01/resources/john-the-ripper                                                            
> $ cat ./john.pot                                                                                                                                                   
42hDRfypTqqnw:abcdefg
```

On trouve `abcdefg` qui est donc le mot de passe du compte `flag01`