# Binary Search Tree

Binary Search Trees (BSTs) are a fundamental data structure commonly used for organizing and managing ordered data efficiently. In the context of a food delivery system, BSTs can be utilized to maintain an ordered database of restaurants or menus based on attributes such as name, cuisine type, or ratings. 

## Time complexity in big O notation

| Operation    | Average    | Worst case |
|--------------|------------|------------|
| Search       | O(log n)   | O(n)       |
| Insert       | O(log n)   | O(n)       |
| Delete       | O(log n)   | O(n)       |

## Space Complexity

| Space        | Average    | Worst case |
|--------------|------------|------------|
| Space        | O(n)       | O(n)       |

## Implementation

Here is a basic implementation of a Binary Search Tree in C:

```c

#include <stdio.h>
#include <stdlib.h>

struct tree {
    int data;
    struct tree *left;
    struct tree *right;
};

typedef struct tree TREE;

TREE *insert(TREE *root, int data);
void inorder(TREE *root);
void preorder(TREE *root);
void postorder(TREE *root);
TREE *delete_node(TREE *root, int data);
int height(TREE *root);

int main() {
    TREE *root = NULL;
    int choice = 0, data = 0;

    while (1) {
        printf("\n******** Menu *************\n");
        printf("1-Insert into BST\n");
        printf("2-Inorder Traversal\n");
        printf("3-Preorder Traversal\n");
        printf("4-Postorder Traversal\n");
        printf("5-Delete from BST\n");
        printf("6-Height of BST\n");
        printf("Any other option to exit\n");
        printf("*****************************\n");
        printf("Enter your choice\n");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter the item to insert\n");
                scanf("%d", &data);
                root = insert(root, data);
                break;
            case 2:
                if (root == NULL)
                    printf("Tree is empty\n");
                else {
                    printf("Inorder Traversal is...\n");
                    inorder(root);
                }
                break;
            case 3:
                if (root == NULL)
                    printf("Tree is empty\n");
                else {
                    printf("Preorder Traversal is...\n");
                    preorder(root);
                }
                break;
            case 4:
                if (root == NULL)
                    printf("Tree is empty\n");
                else {
                    printf("Postorder Traversal is...\n");
                    postorder(root);
                }
                break;
            case 5:
                printf("Enter the item to be deleted\n");
                scanf("%d", &data);
                root = delete_node(root, data);
                break;
            case 6:
                printf("Height of the BST is %d\n", height(root));
                break;
            default:
                exit(0);
        }
    }

    return 0;
}

TREE *insert(TREE *root, int data) {
    TREE *newnode = (TREE *)malloc(sizeof(TREE));
    if (newnode == NULL) {
        printf("Memory allocation failed\n");
        return root;
    }
    newnode->data = data;
    newnode->left = NULL;
    newnode->right = NULL;

    if (root == NULL) {
        root = newnode;
        printf("Root node inserted into tree\n");
        return root;
    }

    TREE *currnode = root;
    TREE *parent = NULL;

    while (currnode != NULL) {
        parent = currnode;
        if (newnode->data < currnode->data)
            currnode = currnode->left;
        else
            currnode = currnode->right;
    }

    if (newnode->data < parent->data)
        parent->left = newnode;
    else
        parent->right = newnode;

    printf("Node inserted successfully into the tree\n");
    return root;
}

void inorder(TREE *root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d\t", root->data);
        inorder(root->right);
    }
}

void preorder(TREE *root) {
    if (root != NULL) {
        printf("%d\t", root->data);
        preorder(root->left);
        preorder(root->right);
    }
}

void postorder(TREE *root) {
    if (root != NULL) {
        postorder(root->left);
        postorder(root->right);
        printf("%d\t", root->data);
    }
}

TREE *delete_node(TREE *root, int data) {
    TREE *currnode = root;
    TREE *parent = NULL;

    while (currnode != NULL && currnode->data != data) {
        parent = currnode;
        if (data < currnode->data)
            currnode = currnode->left;
        else
            currnode = currnode->right;
    }

    if (currnode == NULL) {
        printf("Item not found\n");
        return root;
    }

    TREE *p = NULL;

    if (currnode->left == NULL)
        p = currnode->right;
    else if (currnode->right == NULL)
        p = currnode->left;
    else {
        TREE *successor = currnode->right;
        while (successor->left != NULL)
            successor = successor->left;
        successor->left = currnode->left;
        p = currnode->right;
    }

    if (parent == NULL) {
        free(currnode);
        return p;
    }

    if (currnode == parent->left)
        parent->left = p;
    else
        parent->right = p;

    free(currnode);
    return root;
}

int height(TREE *root) {
    if (root == NULL)
        return 0;
    else {
        int left_height = height(root->left);
        int right_height = height(root->right);

        if (left_height > right_height)
            return (left_height + 1);
        else
            return (right_height + 1);
    }
}

```
