# TD1

## Structures de base : 

```c
struct s_cell {
    int value;
    struct s_cell *next;
};

typedef struct s_cell t_cell;

typedef struct s_list {
    t_cell *head;
} t_list;

```

---

## Exercice 3

Chercher une valeur dans une liste : 
```c
int searchValue(t_list myList, int valueToSearch) {
    t_cell* currCell = myList.head;

    while (currCell->value != valueToSearch && currCell != NULL) {
        currCell->next;
    }

    if (currCell == NULL) {
        return 0;
    }else {
        return 1;
    }
}
```


Renvoie avec l'adresse de la cellule : 

```c
t_cell* searchValue(t_list myList, int valueToSearch) {
    t_cell* currCell = myList.head;

    while (currCell->value != valueToSearch && currCell != NULL) {
        currCell->next;
    }
    return currCell;
}
```


Recherche en récursif : 

```c
t_cell* searchValueRec(t_cell* cell, int valueToSearch) {
    if (cell != NULL) {
        if (cell->value == valueToSearch) {
            return cell;
        }else {
            searchValueRec(cell->next, valueToSearch);
        }
    }
    return NULL;

}

t_cell* searchValueListRec(t_list list, int valueToSearch) {
    return searchValueRec(list.head, valueToSearch);
}

```

---



## Exercice 4

1) La fonction ne marche pas car on free en avançant sauf que l'on supprime d'abord la première cellule qui nous redirigeait vers la 2e. Mais vu qu'elle est supprimée, on y a plus accès


Destroy List : 

```c

void destroyList(t_list mylist){

    t_cell *curr ;
    t_cell *currNext;
    curr = mylist.head ;
    currNext = curr->next;
    while (currNext != NULL){
        free(curr);
        curr = currNext;
        currNext = curr->next;
    }
}
```

Destroy List Recursif : 

```c
void destroyCellRec(t_cell* cell){
    if (cell->next != NULL) {
        destroyCellRec(cell->next);
    }
    free(cell);
}

void destroyListRec(t_list myList) {
    destroyCellRec(myList.head);
    myList.head = NULL;
}

```


---


## Exercice 5

1) Non, myList n'est pas modifié. Il y a que les value des cells qui sont modif

FliVal : 
```c
void flipVals(t_list myList) {
    t_cell* currCell = myList.head;

    while (currCell != NULL) {
        if (currCell->value % 2 == 0) {
            currCell->value++;
        }else {
            currCell->value--;
        }
    }
}
```

---


## Exercice 6 : 

Delete Value : 

```c
t_cell* searchValuePrev(t_list myList, int valueToSearch) {
    t_cell* currCell = myList.head;
    t_cell* prevCell = NULL;

    while (currCell->value != valueToSearch && currCell != NULL) {
        prevCell = currCell;
        currCell = currCell->next;
    }
    return currCell, prevCell;
}

void deleteVal(t_list *myList, int valueToDelete) {
    t_cell* cellToDelete;
    t_cell* prevCell;

    cellToDelete, prevCell = searchValue(*myList, valueToDelete);

    if (myList->head == cellToDelete) {
        myList->head = cellToDelete->next;
    }else {

    }
}
```

---

## Exercice 7 

Destroy All Value : 

```c
void deleteAllValFromlist(t_list* myList, int valueToDelete) {
    if (myList != NULL && myList->head != NULL) {
        while (searchValueListRec(*myList, valueToDelete) != NULL) {
            deleteVal(myList, valueToDelete);
        }
    }
}
```