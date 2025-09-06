# Ex20 AVL Tree - Deletion
## DATE:
## AIM:
To write a C function to delete an element from an AVL Tree.
## Algorithm
1. If node is NULL, return NULL.
2. If key x is greater than node’s data, recursively delete in right subtree and rebalance if needed.
3. If key x is less than node’s data, recursively delete in left subtree and rebalance if needed.
4. If key matches node’s data, replace node’s data with inorder successor’s data (smallest in right subtree), then delete 
   successor and rebalance.
5. Update height and return the node.

## Program:
```

Program to delete an element from an AVL Tree.
Developed by:Dharani dharan K
RegisterNumber: 212223040036
/*typedef struct node
{
int data;
struct node *left,*right;
int ht;
}node;
 
node *insert(node *,int);
node *Delete(node *,int);
void preorder(node *);
void inorder(node *);
int height( node *);
node *rotateright(node *);
node *rotateleft(node *);
node *RR(node *);
node *LL(node *);
node *LR(node *);
node *RL(node *);
int BF(node *);*/
node * Delete(node *T,int x)
{
    if(T==NULL){
        return NULL;
    }
    else if(x>T->data){
        T->right=Delete(T->right,x);
        if(BF(T)==2){
            if(BF(T->left)>=0){
                T=LL(T);
            }
            else{
                T=LR(T);
                
            }
        }
    }
    else if(x<T->data){
        T->left=Delete(T->left,x);
        if(BF(T)==-22){
            if(BF(T->right)>=0){
                T=RR(T);
            }
            else{
                T=RL(T);
                
            }
        }
    }
    else{
        if(T->right!=NULL){
            node *p;
            p=T->right;
            while(p->left!=NULL){
                p=p->left;
            }
            T->data=p->data;
        T->right=Delete(T->right,p->data);
        if(BF(T)==2){
            if(BF(T->left)>=0){
                T=LL(T);
            }
            else{
                T=LR(T);
            }
        }
        }
        else{
            return T->left;
        }
    }
    T->ht=height(T);
    return T;
}



```

## Output:
![avldel](https://github.com/user-attachments/assets/bfdbfb34-3f32-4a8e-b5fb-e734dab8bc64)


## Result:
Thus, the C program to delete an element from an AVL Tree is implemented successfully.
