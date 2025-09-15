# TP3 

## Variables du shell

Par exemple, la variable PS1 est la variable qui contient le prompt du shell. Quand on met, `PS1="$ "` on change la valeur du début de la ligne de notre shell.

Les variables du shell sont des variables d’environnement. Elles sont accessibles à tous les processus lancés par le shell. On peut les lister avec la commande `env` ou `printenv`. On peut également les lister avec la commande `set` qui liste également les variables internes du shell 

---

## Exercice 1 : Variables du shell

```bash
$ nom_fich=hello.c
$ echo nom_fich
$ echo $nom_fich
$ echo ${nom_fich}
$ touch $nom_fich
$ echo $nom_fichpp
$ echo ${nom_fich}pp
$ rm ${nom_fich}
```

Ici le `$` permet d'accéder au contenu de la variable. Si la variable n'existe pas elle ne print rien 

##### Attention
Si on met par erreur `nom_var =value`, il nous mettra une erreur car il va chercher le mot clé `nom_var`. Par contre à l'inverse, si on fait `nom_var= value`, la variable prend en valeur l'espace.

```bash
$ sujet=Alice verbe=aime cod=piscine //Crée les 3 variables
$ phrase="$sujet $verbe la $cod." //Crée la phrase avec les autres variables
$ echo $phrase //print la phrase
$ sujet=Bob verbe=mange cod=salade //On change les variables
$ echo $phrase //ca print la phrase mais avant les modications
$ echo "$sujet $verbe la $cod." //Print la phrase avec les modif
```

---
## Exercice 2 : Caractères spéciaux

- `<>` : Chevrons *(permettent une redicrection)*
- `()` : Regroupe des commandes et les execute dans un sous shell
- `$` : Développement de variables
- `\ ' " ` : Rendre les sens littéral des car spéciaux 
- `* ? ]` : Nom de chemin
- `#` : Commentaires
- `~` : tilde
- `%` : controle des taches


1)
```bash
$ echo a b #a b
$ echo a\ \ \ b #a   b
$ touch fichier\ vide #Crée un fichier "fichier vide"
$ rm fichier vide #Supprime les fichiers fichier et vide
$ rm fichier\ vide #Supp "fichier vide"
$ echo 3$canadiens #print 3 car la var 'canadiens' n'existe pas 
$ echo 3\$canadiens #Print "3$canadiens"
$ echo ; echo * #print rien puis print *(tous les dossiers)
$ echo \; echo \* #Print "; echo *"
$ echo "salut" #Print salut
$ echo \"salut\" #Print "salut"
$ echo 'salut' #Print salut
$ echo \'salut\' #Print 'salut'
$ echo \  #ouvre une ligne avec > (comme ci entrer un dossier)
$ echo \\  #Print \
```


Les caractères annulent le caractère spécial juste après

Un `\` seul lance une saisie

Si on veut écrire un `\`, on fait `\\`

---

## Exercice 3 : Inhibition de caractères spéciaux

```bash
$ touch 'ceci est un horrible nom de fichier'
$ rm -i ceci est un horrible nom de fichier
$ rm -i 'ceci est un horrible nom de fichier'
$ touch p; echo le caractère * est-il spécial ? et ?
$ echo 'le caractère * est-il spécial ? et ?'
$ echo 'en fait, même la fin de ligne<newline>est un caractère normal entre<newline>apostrophes'
$ echo 'le seul caractère spécial entre apostrophes n'est-il pas apostrophe ?'
```

Entre `''`, il n'y a pas de caractères spéciaux à part `*`, `;` et `?`


Ecrire : Un développement de variable (comme $var) peut-il s'inhiber; par exemple entre apostrophes ?
```bash 
echo 'Un developpement de variable (comme $var) peut-il s'inhiber\; par exemple entre apostrophes''
```

---
## Exercice 4 : 

```bash
$ echo "? * et [ sont utilisés pour le développement de chemins"
$ echo "~ provoque un développement du tilde"
$ echo " Entre \" , on peut aussi <newline>écrire sur plusieurs<newline> lignes"
$ nom=Alice
$ echo '$nom scripte en shell'
$ echo "$nom scripte en shell"
$ echo "\$nom scripte en shell"
$ echo "le chemin absolu du répertoire courant est `pwd`"
$ echo "le chemin absolu du répertoire courant est \`pwd\`"
$ echo "le chemin absolu du répertoire courant est $(pwd)"
$ echo "le chemin absolu du répertoire courant est \$(pwd)"
$ echo "Aussi sûr que 2 et 2 font $((2 + 2))"
$ echo "Aussi sûr que 2 et 2 font \$((2 + 2))"
$ echo "\\\\\"\$\`\*\'"
```

```bash
$ mavar="Alice<newline> et<newline>Bob"
$ echo $mavar font plein de choses
$ echo "$mavar font plein de choses"
```

Si on doit résumer, les `""` prennent en compte le `$` et le `` pour execute des commands. 

Pareil si on finit une commande " en oubliant ", ça nous permet d'écrire sur plusieurs lignes. 

et si on a une variables qui prend en compte des sauts de ligne, il faut le mettre entre "" pour que cela prenne effet.

---

## Exercice 5 : Extensions de chemin


`mv *.txt /files`
`mv *.png /imgs`

`rm -r dir`

---

## Exercice 6 : Extension accolade

```bash
$ echo {a,b,c,d} #Print a, b, c, d
$ echo {a..d} # a b c d
$ echo {a..d..2} #a c
$ echo {1,2,3,4,5,6,7,8,9} #1, 2, 3, 4, 5, 6, 7, 8, 9
$ echo {1..9} # 1 2 3 4 5 6 7 8 9
$ echo {1..9..2} # 1 3 5 7 9
```

Ici, quand on met une virgule, cela représente les différents éléments que l'on veut prendre. A l'inverse lorsqu'on met .., on a juste a indiqué les limites et ça écrit l'ensemble. On peut également indiquer un pas à suivre (+2, +3)

```bash
$ echo {a..d}* #a* b* c* d*
$ echo {a..d}.* #a.* b.* c.* d.*
$ echo {a..d}.txt #a.txt b.txt c.txt d.txt
```

Créer les fichiers en une seule ligne 

`touch file-{1..9}.txt`

---

## Exercice 7 : Substitution de commande simple 📚

```bash
$ date #Print la vraie date
$ echo date #Print date
$ echo $(date) #Print la vraie date
$ aujourdhui=$(date) #Variable avec la vraie date
$ echo $aujourdhui #print variables
$ echo "Nous sommes le $(date)"
```

```bash
$ prefix="Nous sommes le"
$ echo $prefix $(date)
$ echo $prefix $aujourdhui
$ echo ${prefix} ${aujourdhui}
$ phrase=${prefix} ${aujourdhui}
$ phrase="${prefix} ${aujourdhui}"
$ echo $phrase
$ echo "$phrase"
```

Les "" permettent de faire des chaines de car

**Différence** :
On met $(commande) lorsque commande est une commande pas un variable
On met ${commande} lorsque commande est une variable

---

## Exercice 8 : Substitution de commande imbriquée 📚📚📚

Rien d'intéressant 

---

## Exercice 9 : GCC

Tout est sur le TP, Rien d'intéressant