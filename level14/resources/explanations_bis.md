Alternativement, il existe un autre moyen d'exploiter `getflag` pour obtenir n'importe quel flag.

En observant le code sur ghidra, on peut voir que chaque flag est simplement une chaine de charactères encryptée que l'exécutable décrypte avec la fonction `ft_des`

![Extrait ghidra de getflag](https://cdn.discordapp.com/attachments/804402242014806057/1205062838930907216/Screenshot_from_2024-02-08_09-08-02.png?ex=65d70139&is=65c48c39&hm=bc0361999048c2875572a3ec218fb14e8ac822830092261c06639a6dc26a099e&)

On peut donc simplement récupérer la fonction `ft_des` dans l'exécutable, regler les quelques erreurs de compilations et compiler un exécutable qui appelle `ft_des` avec le flag du level 14 encrypté.

```bash
lbertran@z1r10p1 ~/work                                                                                                                                                       
> $ ./a.out                                                                                                                                                                                         
7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
```

Et voila, on peut aussi utiliser cette méthode avec n'importe quel flag des autres niveaux.