# TP4 Canaux standards et redirections | Processus et tâches | Signaux

## Canaux standards et redirections

La plupart des commandes écrivent en *sortie standard* (elles sont connectées)

On peut pour autant rediriger la sortie vers un fichier gra^ce `>`

```bash
$ ls ~ # affiche le contenu du répertoire personnel sur la sortie standard
$ ls ~ > list_files.txt # redirige la sortie standard vers le fichier list_files
```

Si le fichier n'existe pas, il sera créer. 
La redirection écrase le fichier.

---

## Exercice 1

- `>` : Mettre le résultat dans un fichier (en écrasant)
- `>>` : Rajoute le résultat dans un fichier (à la fin)

*(Je ne sais pas à quoi sert le 1 dans `1>` et `1>>`)*


```bash
ls > list_files.txt; cat list_files.txt
```

ça note ls dans le fichier `list_files.txt`

---

## Exercice 2

Lister tous les fichiers de `usr/include/` mais que finissant par `.h`
`ls /usr/include/*.h > include_files.txt`

2)
Ecrire le nombre de fichiers
```bash
echo "Il y a $(wc -l include_files.txt) fichiers .h dans le répertoire /usr/include" >> include_files.txt
```

le `-f` permet de n'avoir que le nombre de ligne 

---

## Exercice 3

1. Ecrire dans le fichier
```bash
$ mkdir dir; echo "Hello World !" > dir/file-1.txt
```

2. Copie du fichier + pas de 
```bash
cp dir/file-1.txt dir/file-2.txt
chmod a= dir/file-2.txt
```

3. Lancement de commande 
```bash
cat file-1.txt file-2.txt file-3.txt
```

Le file-1 s'affiche
file-2 n'a pas la permission de cat
file-3 n'existe pas

4. Mettre résultat dans un fichier

```bash
cat file-1.txt file-2.txt file-3.txt > result.txt
```

En console ça met les erreurs (le file-2 et file-3)
et result.txt a uniquement pris file-1

5.

```bash
$ cat file-1.txt file-2.txt file-3.txt 1> result.txt 2> error.txt
```

error prend les erreurs (grâce aux canaux 2)

---

On peut avoir le même raisonnement pour la lecture avec des flèches `<`
*(Il faut que le fichier soit créé et est les permissions)*

---

## Exercice 4 - Retour sur la commande cat

1. Quand cat ne prend pas d'argument elle lance une saisie, puis cat s'il s'agit d'un fichier ou juste echo. 

2. 

```bash
$ cat
hello<newline>
#print (hello)
world<newline>
#print (wolrd)
C-d # appuyer sur la touche Ctrl et la touche d en même temps
```
Combien d'argument la commande cat a-t-elle reçu ? Qu'a-t-elle affiché ? Pourquoi ?

3. 
Testez ensuite la commande suivante:

```bash
$ cat > catout.txt
hello<newline>
world<newline>
C-d # appuyer sur la touche Ctrl et la touche d en même temps
cat catout.txt
```

 Que contient-il ? Pourquoi ?


Tapez enfin la commande suivante :
```bash
$ cat < catout.txt
$ cat 0< catout.txt
```

Combien d'argument la commande cat a-t-elle reçu ? Qu'a-t-elle affiché ?
Quelle est la différence entre < et 0< ?
*Aucune différence*

---

## Exercice 5 - Compter les entêtes

1. 

```bash
wc -l < include_files.txt
```

On a en sortie le nombre de ligne dans le fichier includes_files.txt

2. 

```bash
wc -l < include_files.txt >> includes_files.txt
```

ça rajoute le résultat à la fin du fichier

3. 
```bash
ls /usr/include/*.h > include_files.txt
echo "Il y a $(wc -l < include_files.txt) fichiers .h dans le répertoire /usr/include" >> include_files.txt
```

---

# Les pipes

Le pipe permet de connecter la sortie standard à l'entrée standard de la commande suivante

`cm1 | cm2` : la sortie standard de cm1 prend l'entrée standard de cm2

---

## Exercice 6

```bash
$ ls /usr/include/*.h | wc -l
```

1.
Ici par exemple l'entrée de `wc -l` prend en entrée la sortie de `ls /usr/include/*.h` et écrit la sortie dans le terminal

2. 
```bash
$ echo "Il y a $(ls /usr/include/*.h | wc -l) fichiers .h dans le répertoire /usr/include"
```

3. 

```bash
wc -l $(ls /usr/include/*.h)
```

On a une liste de avec le nombre pour chaque fichier .h dans /usr/include

---

# Processus


##### 1. Commande `sleep 10`

Lorsque l'on exécute :

```bash
sleep 10
```

👉 Le terminal **se fige pendant 10 secondes**, puis on récupère la main. Pendant ce temps, un processus `sleep` est actif et visible avec la commande `ps`.

##### 2. Test des commandes

. a) `ps`

Affiche la liste des processus rattachés au terminal :

```
PID   TTY      TIME     CMD
1234  pts/1    00:00:00 bash
5678  pts/1    00:00:00 ps
```

* `bash` : le shell (toujours présent).
* `ps` : la commande en cours d'exécution.

.b) `sleep 240`

Exécute une pause de **240 secondes** (4 minutes). 👉 Pendant ce temps, le terminal est bloqué.


.c) `Ctrl+Z`

* `Ctrl+Z` suspend le processus courant et le place en arrière-plan.
* Le terminal redevient disponible.

Avec `ps`, on observe que `sleep` est toujours présent mais avec l'état **"T" (stopped)**.

.d) `fg %1`

* `fg` = *foreground*.
* La commande **ramène un job en avant-plan**.
* Ici `%1` correspond au numéro de tâche indiqué après avoir fait `Ctrl+Z`.

👉 Le processus `sleep 240` reprend son exécution.

.e) `Ctrl+C`

* `Ctrl+C` envoie un signal **SIGINT** au processus au premier plan.

👉 Le processus est immédiatement **terminé** et disparaît de la liste de `ps`.

## 3. Réponses aux questions

* **Que fait `Ctrl+Z` ?** 👉 Suspend le processus en cours et le met en arrière-plan.

* **Que fait `Ctrl+C` ?** 👉 Tue le processus en cours au premier plan.

* **Peut-on taper des commandes entre `sleep 240` et `Ctrl+Z` ?** 👉 Non, car tant que `sleep` est au premier plan, le terminal est bloqué. Il faut d'abord le suspendre (`Ctrl+Z`) pour reprendre la main.

* **Que fait `fg %1` ?** 👉 Ramène le job numéro 1 en avant-plan. 👉 Plus généralement, `fg` reprend un processus suspendu et le relance au premier plan.

* **Que remarque-t-on sur le PID de `sleep` ?** 
  - 👉 Le PID **reste identique** tant que le processus n'est pas tué.
  - 👉 Si on le suspend (`Ctrl+Z`) puis le reprend (`fg`), le PID ne change pas.
  - 👉 Si on le tue (`Ctrl+C`) et qu'on relance un nouveau `sleep`, il aura un **nouveau PID**.

##### 4. Programme C

Voici un programme en C qui incrémente une variable `i` indéfiniment et affiche sa valeur à chaque multiple de 100, avec une pause d'une seconde :

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    int i = 0;
    while (1) {
        i++;
        if (i % 100 == 0) {
            printf("i = %d\n", i);
            sleep(1);       // pause d'une seconde
        }
    }
    return 0;
}
```

##### Compilation et exécution

```bash
gcc prog.c -o prog
./prog
```

##### Exemple de sortie

```
i = 100
i = 200
i = 300
...
```

---

## Envoyer des signaux à un processus


Un signal est une instruction permettant à un processus de changer l'état de ce dernier (`bg`, `fg`, ...)

La principale est `kill + PID`

Plusieurs options : 
- `SIGINT` : kill le proc
- `SIGTSTP` : met en pause
- `SIGCONT` : remet le proc en exécution
- `SIGTERM` : Il demande l'arrêt du proc (mais bien arrêté)
- `SIGKILL` : Provoque un arrêt brutal 

---

## Exercice 8 

1. Tapez `kill -L`

`SIGINT` : 2
`SIGTSTP` : 20
`SIGCONT` : 18
`SIGTERM` : 15
`SIGKILL` : 9

2. 
```bash
$ ./compteur 1 & # ce sera notre processus 1
$ ./compteur 2 & # ce sera notre processus 2
$ ./compteur 3 & # ce sera notre processus 3
$ jobs -p # notez les PID des processus, mais ils ont dû être affichés à l'écran lors de leur lancement 
$ kill -SIGTSTP <PID du processus 1> #Met en pause le p1 
$ jobs
$ kill -SIGINT %2 #Kill le proc2
$ jobs # Montre qu'il a été kill
$ jobs # oui une deuxième fois #Supprime le 2e proc
$ kill -SIGCONT %1 #~Remet en marche le p1
$ jobs
$ kill -s SIGTERM <PID processus 1> #Complète le P1
$ jobs
$ kill -9 <PID du processus 3> # KILL le p3
$ jobs
$ jobs # pour voir disparaître la tâche [3]
```

On peut donc 2 manières d'utiliser le kill soit avec le `-SIG...` ou avec la valeur `-9 pour -SIGKILL`