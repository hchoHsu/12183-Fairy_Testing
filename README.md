# 12183-Fairy_Testing
## still RE
nthu oj solution


```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node{
    int value;   // 0 or 1
    char token;  // symbol
    struct node *left, *right, *parent;
}Node;

int n, m, fn;
Node *root, **val;

void  renew_the_tree(Node*);
void  freeTree(Node*);
Node* buildTree();
Node* makeNode(char);

void Fairy_Testing(){
    scanf("%d %d", &n, &m);
    val = (Node** )malloc((n+1)*sizeof(Node*));
    scanf("\n");
    root = buildTree();

    while(m--){
        scanf("%d", &fn);
        renew_the_tree(val[fn]);
        printf("%d\n", root->value);
    }

    free(val);
    freeTree(root);
    return;
}
int main(){
    int T; scanf("%d", &T);
    while(T--){
        Fairy_Testing();
    }
    return 0;
}

Node* makeNode(char c){
    Node *np = (Node* )malloc(sizeof(Node));
    np->value = 0; np->token = c;
    np->parent = np->left = np->right = NULL;
    return np;
}
Node* buildTree(){
    char cIn; scanf("%c", &cIn);
    Node *np;
    if(cIn == '&' || cIn == '|'){
        np = makeNode(cIn);
        np->left = buildTree();
        np->left->parent = np;
        np->right = buildTree();
        np->right->parent = np;
    }
    else if(cIn == '['){
        scanf("%c", &cIn);
        np = makeNode(cIn);
        val[cIn - '0'] = np;
        scanf("%c", &cIn);
        if(cIn != ']'){ printf("Error([x])\n"); return NULL;}
    }
    else if(cIn == '\n') return NULL;

    return np;
}

void renew_the_tree(Node* np)
{
    if(np->left == NULL && np->right == NULL){
        np->value = (np->value == 1) ? 0 : 1;
    }
    else{
        int org = np->value;
        if(np->token == '&'){
            np->value = (np->left->value && np->right->value);
        }
        else if(np->token == '|'){
            np->value = (np->left->value || np->right->value);
        }
        else {
            printf("Error(renew)\n"); return;
        }

        if(org == np->value) return;
    }

    if(np->parent == NULL) return;
    else renew_the_tree(np->parent);
    
    return;
}
void freeTree(Node* np){
    if(np == NULL) return;
    freeTree(np->left);
    freeTree(np->right);
    free(np);
    return;
}
void show_val(){
    for(int i = 1; i <= n; i++){
        printf("%d:%d ", val[i]->token, val[i]->value);
    }
    printf("\n");
}
void printInfix(Node* np){
    if(np == NULL) return;
    printInfix(np->left);
    printf("%c\n", np->token);
    printInfix(np->right);
}
```
