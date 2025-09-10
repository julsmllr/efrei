# TP2 - Système de fichiers et permissions

## Exercice 1 : ID + psw 

`id` : uid=1001(student) gid=1001(student) groupes=1001(student),27(sudo)
`id root` : uid=0(root) gid=0(root) groupes=0(root)

En passant en paramètre le profil que l'on veut

4) `cat /etc/passwd`  (il prend tous les profils disponiblent)

Il nous permet de voir le dossier racine du profil. 




## Exo 2 : Permissions

```bash 
$ls -l 
total 28
drwxr-xr-x 1 student student   88 sept.  8 14:17 Desktop
drwxr-xr-x 2 student student    6 sept.  8 13:57 Documents
drwxrwxr-x 2 student student    6 sept.  8 14:17 dossier_ex2
-rw-r--r-- 1 student student 4170 mars  26 00:44 example.1hub-3m.mar
-rw-r--r-- 1 student student 4496 mars  26 00:44 example.1sw-3m.mar
-rw-r--r-- 1 student student 9999 mars  26 00:44 example.1sw-4m-1rt-1gw-1m.mar
-rw-rw-r-- 1 student student    0 sept.  8 14:18 fichier_ex2
drwxr-xr-x 2 student student    6 sept.  8 13:57 Images
drwxr-xr-x 2 student student    6 sept.  8 13:57 Modèles
drwxr-xr-x 2 student student    6 sept.  8 13:57 Musique
drwxr-xr-x 2 student student    6 sept.  8 13:57 Public
drwxrwxr-x 2 student student    6 sept.  8 13:57 public_html
drwxr-xr-x 2 student student    6 sept.  8 13:57 Téléchargements
drwxr-xr-x 2 student student    6 sept.  8 13:57 Vidéos
$ ls -d
.
```
On remarque que les dossiers possèdent un d avant les métadonnées.


### 2)
Desktop


### 4)
```bash
$ ls -ld /etc/passwd : rw- rw- r-- (110 110 100 = 6 6 4)
$ ls -l /home/student : drwxr-xr-x (111 101 101 = 8 5 5), etc... pour tous les fichiers de student
```

### Ex 3 : chmod

```bash
$ chmod a= f : modifie le fichier f pour que toutes les permissions = vide
$ chmod o+rw f; modifie les perm de other en ajout de r w
$ chmod u=o f; modifie les perm de user = à celle de other
$ chmod o-wx f; : modifie les perm de other en retirant w et x
$ chmod g+u f; modifie les perm de g en ajoutant celle de u 
$ chmod a+x,g-w f; modifie les perm de tous en ajout de x et en retirant w à group
```

`chmod 644 f` : modifie les perm en suivant l'octale (donc 6 puis 4 puis 4 = 110 010 010 = rw- -w- -w-)


Execution tous, lecture et écriture pour proprio
```bash
$ chmod a=x, u+rw f
```

ou 

`chmod 711 f`

Lecture + Execution tous, personne n'écrit

```bash
$ chmod a+wx, a-r f
```
ou 

`chmod 333 f`

Tous pour tous, Pas d'écriture pour other
```bash
$ chmod a=rwx, o-r f
```

ou 

`chmod 773 f`

Lecture + Ecriture pour user, Ex pour groupe et rien pour other
```bash
$ chmod a= f, u+rw, g+x
```

ou 

`chmod 610 f`

## Exercice 4 : Permissions aux fichiers normaux 

```bash
$ mkdir ex4
$ touch f g
$ chmod u-r f
$ chmod u-w g
``` 

### 3) Test `cat`

`cat f` : Autorisation non accordée
`cat g` : fichier vide

### 4) Fichier g inmodifiable car lecture seule

### 5) Copie de fichier
`cp f h` : Ne peut pas copie car lecture pas autorisée
`cp g h` : ça marche

Le contenu de h est vide (comme g) et les permissions sont les mêmes que g

### 6) 

```bash
$ echo "toto" >> f
$ chmod u+w f
$ cat f
toto
``` 

On a bien toto à la fin du fichier (logique vu qu'on peut écrire dedans)

### 7)
`rm g` : demande une confirmation avant de supprimer le fichier
`rm -f g` : force la suppression (-f)

## Exercice 5 : Permission de dossiers

Les permissions changent : 
- r : Possibilité de lister le contenu
- w : Possibilité de modifier le contenu
- x : Possibilité de d'ouvrir le dossier


### 1)
```bash
$ mkdir rep
$ touch rep/a rep/b
``` 

### 2)
```bash
$ chmod a= rep
``` 

Toutes les commandes de cd, ls, touch, cat, rm ne sont pas autorisée à être exécutée. 

### 3) 

```bash
$ chmod a=r rep
``` 

Seul la commande ls rep marche. Même cat rep/a ne marche pas car le fichier dcp a pris les perms du fichier (qui n'avait rien) et mtn on a changé les perms du dossier



### 4)

###5)

```bash
$ chmod a=w rep
``` 

Bizarrement plus rien ne marche (même le touch et le rm VRMT BIZARRE)

## Exercice 6

### 1)

```bash
echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
```

Peut être les chemins de départ possible

### 2) 
cela a rajouté un chemin jusq'au dossier bin dans notre home

### 3) 
Chemin absolu de cat : /usr/bin/cat
Chemin absolu de rm : /usr/bin/rm

### 4) 

Quand je rm fich, ça me le cat 

### 5)

```bash 
$ type rm 
rm est haché (/home/student/bin/rm)
```

### 6) 

Après retrait de perm w, je ne peux plus "rm" (en réalité cat)


### 12)

PATH prend en paramètres les chemins avec toutes les commandes internes disponibles

## Exercice 8

### 1)

`umask` renvoie 0002 


Avec mask par défaut : 
```bash
$ ls -ld rep f 
-rw-rw-r-- 1 student student 0 sept.  8 16:23 f
drwxrwxr-x 2 student student 6 sept.  8 16:23 rep
```

mask = 121
```bash
$ls -ld rep f 
-rw-r--rw- 1 student student 0 sept.  8 16:24 f
drw-r-xrw- 2 student student 6 sept.  8 16:24 rep
```

mask = 666
```bash
$ ls -ld rep f 
---------- 1 student student 0 sept.  8 16:26 f
d--x--x--x 2 student student 6 sept.  8 16:26 rep
```

### 2) Explications

Le umask se soustrait aux perms de base. S'il vaut 1, on retire x, 2 on retire w, 3, on retire w et x, etc...

