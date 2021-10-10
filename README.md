# top-view-question
the code for top view hackerrank question is given just below...
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

struct node {
    
    int data;
    struct node *left;
    struct node *right;
  
};

struct node* insert( struct node* root, int data ) {
		
	if(root == NULL) {
	
        struct node* node = (struct node*)malloc(sizeof(struct node));

        node->data = data;

        node->left = NULL;
        node->right = NULL;
        return node;
	  
	} else {
      
		struct node* cur;
		
		if(data <= root->data) {
            cur = insert(root->left, data);
            root->left = cur;
		} else {
            cur = insert(root->right, data);
            root->right = cur;
		}
	
		return root;
	}
}
int * leftView(struct node *root, int * p, int * m, int * lp,int * b){
    if(root->left!=NULL){
        lp=leftView(root->left,p,m-1,lp,b);        
        if(*b==0||*m==0){
            *m=root->data;
        }
        if(root->right!=NULL){
            *b=-1;
            lp=leftView(root->right,p,m+1,lp,b); 
            *b=0;
        }
    }
    else if(root->left==NULL){
        if(lp>m){
            lp=m;
            *m=root->data;
            if(root->right!=NULL){
                *b=-1;
                lp=leftView(root->right,p,m+1,lp,b);
                *b=0;
            }
        }
    }
    return lp;
}
int * rightView(struct node* root, int * p, int * m, int * rp, int * b){
    if(root->right!=NULL){
        rp=rightView(root->right,p,m+1,rp,b);
        if(*b==0||*m==0){
            *m=root->data;
        }
        if(root->left!=NULL){
            *b=-1;
            rp=rightView(root->left,p,m-1,rp,b);
            *b=0;
        }
    }
    else if(root->right==NULL){
        if(rp<m){
            rp=m;
            *m=root->data;
            if(root->left!=NULL){
                *b=-1;
                rp=rightView(root->left,p,m-1,rp,b);
                *b=0;
            }
        }
    }
    return rp;
}
void topView( struct node *root) {
    int i,b=0;
    int stk[1000];
    int *lp=&stk[499];
    int *p=&stk[499];
    int *m=&stk[499];
    int *rp=&stk[499];
    for(i=0;i<1000;i++)
        stk[i]=0;
    lp=leftView(root,p,m,lp,&b);
    m=p;
    rp=rightView(root,p,m,rp,&b);
    while(lp<=rp){
        printf("%d ",*lp);
        lp++;
    }
}

int main() {
  
    struct node* root = NULL;
    
    int t;
    int data;

    scanf("%d", &t);

    while(t-- > 0) {
        scanf("%d", &data);
        root = insert(root, data);
    }
  
	topView(root);
    return 0;
}

