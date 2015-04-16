More on ADTs
=============

## Doubly Linked List
Similar to a linked list, but also has a `prev` pointer.
```C++
struct Node{
    string val;
    Node* next;
    Node* prev;
};
```

`insert` and `remove` have changed to accomodate for the `prev` pointer. 

### Sorted List
* Essentially a linked list that is sorted (eg. alphabetically).
* Inserts are done such that the value is put in the right place, so no additional sorting needs to be done. 
* Remove is more efficient usually, as inserts go directly in the right spot.

## Graphs, BSTs
* Graphs are made of edges and vertices
    * They can be directed (arrows) or undirected (no arrows)
    * Trees are a type of graph. 
* Trees have a special node called the root - it has no parent.
    * All other nodes have only 1 parent.
    * Trees must be fully connected - no orphan nodes/clusters. 
    * An internal node is a node with at least one child.
    * A leaf node is a node with no children. 
    * **Height** of a node is the length of the longest path from it to a leaf. 
    * **Depth** of a node is the length of the path from the root to it. 

### Binary Trees
* Each node has at most two children
* Instead of a `next` pointer, each node has a `left` and `right` pointer.
* Can be sorted - heap, BSTs.

```C++
struct Node{
    string val;
    Node* left;
    Node* right;
};
```

### Heap (Binary Heap)
* A heap is a variant of the Binary Tree
* It is a *complete binary tree* 
    * All levels of the tree, except maybe the deepest one, are fully filled. 
    * If the deepest level is not fully filled, the nodes are filled from left to right. 
* *Heap Property*: All nodes are >= or <= than all of its children, depending on which way you set it up. 

### Binary Search Tree (BST)
* *BST Property*: **ALL** nodes in the left subtree are less than the current node. **ALL** nodes in the right subtree are larger than the current node.
* When working with BSTs, it's easiest to use recursive methods.
* Operations are O(log n).

```C++
typedef BSTNode* BST;

bool isEmpty (BST& root){
    return NULL == root;
}

bool lookup (BST& root, string val){
    if (NULL == root){
        return false;
    }
    
    if (val == root->val){
        return true;
    }
    else if (val < root->val){
        return lookup(root->left, val);
    }
    else {
        return lookup (root->right, val);
    }
}

void print (BST& root){
    if (!isEmpty(root)){
        print (root->left);
        cout << root-> val << endl;
        print (root->right);
    }    
}

void insert (BST& root, string val){
    if (isEmpty(root)){
        BST newNode = new BSTNode;
        newNode->val = val;
        newNode->left = NULL;
        newNode->right = NULL;
        root = newNode;
    }
    else if (val < root->val){
        insert(root->left, val);
    }
    else {
        insert(root->right, val);
    }
}
```

#### Deleting from a BST
* If no children, just delete it.
* If it has one child, then connect the node's parent to the node's child.
* If it has two children, then find a replacement node.
    * All the way down the right of the left subtree
    * OR all the way down the left of the right subtree
    * Replacement node will have 0/1 child(ren). Rearrange it if necessary, and put the replacement node where the current node is to minimize disruption to the BST.
* This can lead to unbalanced trees, but we'll deal with that in CS 240.