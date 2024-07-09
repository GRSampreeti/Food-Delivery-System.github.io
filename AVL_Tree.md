# AVL Tree Implementation

## Description
An AVL tree, named after its inventors Adelson-Velsky and Landis, is a self-balancing binary search tree. It maintains a balanced structure where the heights of the left and right subtrees of any node differ by at most one. If this balance condition is violated during operations like insertion or deletion, the tree undergoes rebalancing through rotations. 

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/fd/AVL_Tree_Example.gif" alt="AVL Tree Example" width="400"/>
</p>

<p align="center"><em>Source:https://en.wikipedia.org/wiki/AVL_tree</em></p>

<p  align="center">
<img src="https://github.com/GRSampreeti/Food-Delivery-System.github.io/raw/main/WhatsApp Image 2024-07-09 at 07.20.58_737d800d.jpg" alt="AVL Workflow" width="400" height="400"/>
</p>
<p  align="center">
Figure 2: AVL Workflow

</p>



## Space and Time Complexity

### Space Complexity

| Space        | Complexity |
|--------------|------------|
| Space        | O(n)       |

### Time Complexity

| Operation    | Amortized   | Worst Case  |
|--------------|-------------|-------------|
| Search       | O(log n)    | O(log n)    |
| Insert       | O(log n)    | O(log n)    |
| Delete       | O(log n)    | O(log n)    |

## Implementation

```cpp
#include <iostream>
#include <string>
using namespace std;

// Define a struct for restaurant details
struct Restaurant {
    string name;
    string cuisineType;
    int rating;
    // Add any other relevant fields here

    // Constructor to initialize the struct
    Restaurant(string n, string c, int r) : name(n), cuisineType(c), rating(r) {}
};

// AVL tree node for restaurant management
class Node {
public:
    Restaurant data;
    Node* left;
    Node* right;
    int height;

    // Constructor to initialize the node
    Node(Restaurant d) : data(d), left(nullptr), right(nullptr), height(1) {}
};

// Function to get the height of a node
int height(Node* N) {
    if (N == nullptr) return 0;
    return N->height;
}

// Function to get maximum of two integers
int max(int a, int b) {
    return (a > b) ? a : b;
}

// Function to perform right rotation
Node* rightRotate(Node* y) {
    Node* x = y->left;
    Node* T2 = x->right;

    x->right = y;
    y->left = T2;

    y->height = max(height(y->left), height(y->right)) + 1;
    x->height = max(height(x->left), height(x->right)) + 1;

    return x;
}

// Function to perform left rotation
Node* leftRotate(Node* x) {
    Node* y = x->right;
    Node* T2 = y->left;

    y->left = x;
    x->right = T2;

    x->height = max(height(x->left), height(x->right)) + 1;
    y->height = max(height(y->left), height(y->right)) + 1;

    return y;
}

// Function to get the balance factor of a node
int getBalance(Node* N) {
    if (N == nullptr) return 0;
    return height(N->left) - height(N->right);
}

// Recursive function to insert a node into the AVL tree
Node* insert(Node* node, Restaurant data) {
    if (node == nullptr) return new Node(data);

    if (data.name < node->data.name)
        node->left = insert(node->left, data);
    else if (data.name > node->data.name)
        node->right = insert(node->right, data);
    else // Duplicate names are not allowed
        return node;

    node->height = 1 + max(height(node->left), height(node->right));

    int balance = getBalance(node);

    // Left Left Case
    if (balance > 1 && data.name < node->left->data.name)
        return rightRotate(node);

    // Right Right Case
    if (balance < -1 && data.name > node->right->data.name)
        return leftRotate(node);

    // Left Right Case
    if (balance > 1 && data.name > node->left->data.name) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }

    // Right Left Case
    if (balance < -1 && data.name < node->right->data.name) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }

    return node;
}

// Function to traverse the AVL tree in inorder and print restaurant details
void inorder(Node* root) {
    if (root != nullptr) {
        inorder(root->left);
        cout << "Name: " << root->data.name << ", Cuisine Type: " << root->data.cuisineType << ", Rating: " << root->data.rating << endl;
        inorder(root->right);
    }
}

// Main function for testing the AVL tree implementation
int main() {
    Node* root = nullptr;

    // Inserting restaurants into the AVL tree
    root = insert(root, Restaurant("Restaurant A", "Italian", 4));
    root = insert(root, Restaurant("Restaurant B", "Mexican", 5));
    root = insert(root, Restaurant("Restaurant C", "Chinese", 3));
    root = insert(root, Restaurant("Restaurant D", "Indian", 4));
    root = insert(root, Restaurant("Restaurant E", "Japanese", 5));

    // Displaying restaurants in sorted order
    cout << "Restaurants in sorted order:" << endl;
    inorder(root);

    return 0;
}
```
