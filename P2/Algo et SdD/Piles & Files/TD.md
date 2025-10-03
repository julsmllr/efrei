# TD - Piles et Files

## Exercice 1

#### 1) pop() d'une t_list

```c
int pop(t_stacklist *stack){
    t_cell* curr = stack->head; 
    stack->head = curr->next; 
    int val = curr->value
    free(curr)
    return val;
}
```

#### 2) top() d'une t_list

```c
int top(t_stacklist stack){
    if (!isEmptyStack(&stack)){
        return stack->head->value; 
    }
    return -1
}
```

---

#### 1) pop() d'une pile sous forme de tableau

```c
int pop(t_stacklist *stack,){
    int val = stack->values[stack->size - 1]; 
    stack->size--;
    return val;
}
```

#### 2) top() d'une pile sous forme de tableau

```c
int top(t_stacktab stack){
    if (!isEmptyStack(stack->values)){
        return stack->values[stack->size - 1];
    }
    return -1;
}
```

---

## Exercice 2

On a les fonctions push(), pop(), top(), isEmptyStack()

On part d'une implémentation en liste

```c
typedef struct s_enigme{
    int num;
    char *pass;
    char *question;
    char *answer;
    char *reward;
    struct s_enigme* next;
}t_enigme;
```

`mygame` est une pile qui contient les `t_enigme`

#### 1) checkEscape

```c

int checkEscape(t_stack stack){
    while(!isEmptyStack(stack)){
        t_enigme actual = pop(&stack);
        if (actual->pass != top(stack)->reward){
            return 0
        }
    }
    return 1;
}

```
