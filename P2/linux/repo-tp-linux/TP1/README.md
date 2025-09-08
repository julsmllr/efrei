# TP1 - Introduction Linux

On commence toujours par ouvrir un cmd et on tape 
```bash
PS1="$ "
```

## 1er Exercice  

### Quelques commandes
`$ date` : Donne la date + heure

`$ cal` : Ouvre le calendrier du mois

`$ cal 3 2022` : Ouvre le calendrier du mois mis en argument (ici 03/22)

`$ who` : pas de réponse du terminal (la suite non plus car la commande c'est whoami) | *La vraie commande est `whoami`*

`$ uname` : Donne le nom de la distribution de l'OS

`$ uname -m -r` : Donne la version de l'OS avec le socket du processeur + bits d'encodage (86 bits)

`$ uname -mrs` : Donne le nom de la distribution complète (uname + uname -m -r)

`$ echo`: print en console (en supprimant les espaces en trop)

### Raccourcis

-  Les fleches permettent de parcourir l'historique des commandes saisie | *ou `CTRL+P` et `CTRL+N`* 
-  `CTRL + L` (ou `cls`) :  clear le shell

- `CTRL + U` : Supprime le contenu de la ligne

- `CTRL + D` : Ferme le shell ou tout process si rien n'est fait dessus

### Remarques

L'historique entre la fermeture du shell et son réouverture ne se clear pas

---
## 2e Exercice 

### Nouvelles commandes
- `pwd` : Donne le chemin absolu de notre localisation
- `cd ..` : revenir au dossier précédent
- `cd` : fait revenir au home
- `cd /` : va à la racine / du disque
 - `ls` : liste le contenu du dossier

- `cat` : affiche le contenu (on peut mettre plusieurs fichiers un après l'autre, ça les affiche à la suite)
- `wc` (word count) : Donne nb ligne, nb mots, nb car


### Dossiers importants

- `share/man` : On dirait des langues disponibles (fr, en, el, da, es, ...)

- `usr/bin` : librairies de la session

### Comprendre le `~` 

Le symbole `~` désigne le **home** de la session. 

*Par exemple : un utilisateur aura comme dossier par défaut `home/utilisateur`, `~` désignera ce chemin*

Donc :

- `echo ~` renvoie le `pwd` initial de la session
- `cd ~` fait revenir au home

---

## 3e Exercice

### Création de dossiers

On utilise `mkdir`
*Exemples :*

- `mkdir abeilles tp_shell/tp1 ~/arbres` : Il crée 3 dossiers "Abeilles" à la base, "TP1" dans "tp_shell" et "Arbres" à la racine aussi avec `~`

- `mkdir -p vivant/plante/fleur tp_shell/tp1/exos/ex1` | Ici `-p` permet de créer tous les dossiers intermédiaires. (Si on veut créer fleur, il faut d'abord créer plante)


### Chemin relatif et absolu

Un chemin absolu est un chemin qui marchera tout le temps. Relatif, il dépend du dossier dans lequel on se situe

*Exemple du TP :*

- Chemin relatif : `tp_shell/tp1`, `abeilles` 
- Chemin Absolu : `~/arbres` *(car ~ correspond à `home/student/`)*

### Suppression de dossiers
On utilise `rmdir`

> Attention le dossier sélectionner doit être vide


### Création de fichiers vides
On utilise `touch`

### MOVE de fichiers
On utilise `mv <fichier à bouger> <nouvelle destination + nouveau nom>`

- `mv : move + renommer` *(exemple : `mv arbres/hello.c arbres/bonjour.c`)*
- `mv arbres/hello.c arbres/bonjour.c` : renomme
- `mv abeilles arbres vivant/` : déplace `abeilles` et `arbres` dans `vivant/`
- `mv bidule vivant` : déplace `bidule` dans `vivant/`
- `mv vivant vie` : `vie` n'existe pas donc vivant est renommé

### COPY de fichiers/Dossiers

On utilise `cp <fichier à copier> <vers où copier>`

- `cp vie/arbres/bonjour.c salut.c` : Créer une copie de `bonjour.c` dans `/home/student` nommé `salut.c`

- `cp salut.c vie/abeilles/truc.txt copies` : copie `salut` + `truc` dans `copies` (fichier puis dossier)

**Et pour copier des dossiers ?**

On utilise le paramètres `-R` 

- `cp vie/bidule tp_shell copies` : Il y a une erreur, tp_shell est un dossier
- `cp -R vie/bidule tp_shell copies` : copie `bidule` + dossier `tp_shell` dans `copies`
- `cp -R vie copie_vie` : copie de `vie` dans un nouveau dossier `copie_vie` *(vu que copie_vie n'existe pas)* 


### REMOVE de fichiers

On utilise `rm <fichier à supprimer>`

- `rm vie/bidule` : supprime `bidule`
- `rm copies` : pas possible copies est un dossier

**Même raisonnement que précédemment pour les dossiers**

- `rm -r copies` : supprime le dossier copies
- `rm -R copie_vie` : supprime le dossier copies_vie
- `rm i- vie/arbres/bonjour.c vie/abeilles/truc.txt` : supprime les fichier `bonjour` et `truc` avec une demande de confirmation (`-i`)

---

## Exercice 4 
```bash
$ mkdir Mail Rapport Web
$ touch Rapport/rapport.txt Web/index.html
$ mkdir Rapport/Docs
$ mkdir Rapport/Docs/Afaire Rapport/Docs/Fait
$ cd ~/Rapport/Docs/Afaire
$ cd ../Fait
$ cp ../rapport.txt ./
$ mv rapport.txt rapport_copie.txt
$ cd ~/Rapport
$ cat ../Web/index.html
$ ls ../Web
$ cd ~
$ rm -R Mail Rapport Web
```

---
## Exercice 5 
Il existe plusieurs type de commande : 
- externes : besoin d'un librairie
- internes : présentes dans l'OS de base
- fonctions du shell : commandes définies par le user
- alias : raccourcis pour des commandes du user


### Comment connaître le type d'une commande ? 
```bash
type <commande>
```

- `type cd` : primitive
- `type touch` : haché (externe et présent dans le /bin)
- `type mv` : haché (externe et présent dans le /bin)
- `type cp` : haché (externe et présent dans le /bin)
- `type rm` : haché (externe et présent dans le /bin)
- `type ls` : alias (externe et présent dans le /bin)

Plupart des programmes dans le usr/bin ?

---

## Exercice 6

commande `man` : manuel des commandes externes

commande `help` : manuel des commandes internes

#### Doc pour `man ls` 

ls -a : all
ls -l : Pour utiliser un format de listing long avec tous les droits

rm -f : force le delete

touch edit timestamps
