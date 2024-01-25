# Level 02
Le répertoire de ce niveau contient un fichier `.pcap`, il s'agit d'un fichier de captures de packets.

On peut donc ouvrir le fichier en question avec wireshark et faire follow TCP stream sur un des packets, on trouve la saisie de mot de passe suivante (en lisant en UTF-8): `ft_wandrNDRelL0L`

Les caractères carrés sont des DEL, il suffit donc d'effacer le nombre de caractères correspondants pour obtenir le mot de passe de flag02: `ft_waNDReL0L`