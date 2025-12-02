# Ex4(A) AVL Tree - Insertion
## DATE:
## AIM:
To write a **Java program** to implement an **AVL Tree** and demonstrate the recursive function for **inserting elements** while maintaining the AVL property using balancing rotations (LL, RR, LR, RL).

## Algorithm:
1.  Start.
2.  Define a class `Node` with integer `data`, pointers to `left` and `right` children, and an integer `height`.
3.  Define utility methods: **`height(Node N)`** to get the height of a node and **`getBalance(Node N)`** to compute the balance factor (Height of Left Subtree - Height of Right Subtree).
4.  Define the four rotation methods: **`rightRotate(Node y)` (LL)**, **`leftRotate(Node x)` (RR)**, **`leftRightRotate(Node z)` (LR)**, and **`rightLeftRotate(Node z)` (RL)**, which internally use single rotations. 
5.  Define the recursive **`insert(Node node, int data)`** method:
    a. Perform standard **BST insertion**.
    b. **Update the height** of the current node.
    c. **Calculate the Balance Factor (BF)**.
    d. If $BF > 1$ (Left heavy):
        i. If the new key is less than the left child's key (LL Case), perform **Right Rotation**.
        ii. If the new key is greater than the left child's key (LR Case), perform **Left-Right Rotation**.
    e. If $BF < -1$ (Right heavy):
        i. If the new key is greater than the right child's key (RR Case), perform **Left Rotation**.
        ii. If the new key is less than the right child's key (RL Case), perform **Right-Left Rotation**.
    f. Return the root of the (possibly rotated) subtree.
6.  In the `main` method, create an AVL Tree instance and call `insert()` multiple times to build and balance the tree.
7.  End.

## Program:
/*
Program to insert the elements in an AVL Tree
Developed by: Dharani dharan K
RegisterNumber: 212223040036
*/

class Node {
    int data;
    int height;
    Node left;
    Node right;

    public Node(int data) {
        this.data = data;
        height = 1; // New node is initially at height 1
    }
}

public class AVLTree {
    Node root;

    // Utility function to get the height of the node
    int height(Node N) {
        if (N == null)
            return 0;
        return N.height;
    }

    // Utility function to get maximum of two integers
    int max(int a, int b) {
        return (a > b) ? a : b;
    }

    // Get Balance factor of node N
    int getBalance(Node N) {
        if (N == null)
            return 0;
        return height(N.left) - height(N.right);
    }
    
    // RR Rotation: Right rotation (LL case)
    Node rightRotate(Node y) {
        Node x = y.left;
        Node T2 = x.right;

        // Perform rotation
        x.right = y;
        y.left = T2;

        // Update heights
        y.height = max(height(y.left), height(y.right)) + 1;
        x.height = max(height(x.left), height(x.right)) + 1;

        return x; // New root
    }

    // LL Rotation: Left rotation (RR case)
    Node leftRotate(Node x) {
        Node y = x.right;
        Node T2 = y.left;

        // Perform rotation
        y.left = x;
        x.right = T2;

        // Update heights
        x.height = max(height(x.left), height(x.right)) + 1;
        y.height = max(height(y.left), height(y.right)) + 1;

        return y; // New root
    }

    // Recursive insertion function
    Node insert(Node node, int data) {
        // 1. Perform standard BST insertion
        if (node == null)
            return (new Node(data));

        if (data < node.data)
            node.left = insert(node.left, data);
        else if (data > node.data)
            node.right = insert(node.right, data);
        else // Duplicate keys not allowed
            return node;

        // 2. Update height of current node
        node.height = 1 + max(height(node.left), height(node.right));

        // 3. Get the balance factor
        int balance = getBalance(node);

        // 4. Check for imbalance and perform rotation

        // Left Left Case (LL)
        if (balance > 1 && data < node.left.data)
            return rightRotate(node);

        // Right Right Case (RR)
        if (balance < -1 && data > node.right.data)
            return leftRotate(node);

        // Left Right Case (LR)
        if (balance > 1 && data > node.left.data) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }

        // Right Left Case (RL)
        if (balance < -1 && data < node.right.data) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }

        return node;
    }
    
    // Utility function to print InOrder traversal
    void inOrder(Node node) {
        if (node != null) {
            inOrder(node.left);
            System.out.print(node.data + "(" + getBalance(node) + ") ");
            inOrder(node.right);
        }
    }

    public static void main(String[] args) {
        AVLTree tree = new AVLTree();

        System.out.println("--- Inserting and Balancing AVL Tree ---");
        
        // Insertion sequence that requires rotations
        tree.root = tree.insert(tree.root, 10);
        tree.root = tree.insert(tree.root, 20);
        tree.root = tree.insert(tree.root, 30); // RR rotation
        tree.root = tree.insert(tree.root, 40);
        tree.root = tree.insert(tree.root, 50); // RR rotation
        tree.root = tree.insert(tree.root, 25); // RL rotation at root (30)

        System.out.print("\nInOrder Traversal of the AVL Tree (Data followed by Balance Factor): \n");
        tree.inOrder(tree.root);
        System.out.println();
    }
}

## Output:
![image](https://github.com/user-attachments/assets/23cf1270-fdba-4c49-ae95-3c2c5f339d3a)
## Result:
Thus, the **Java program** to insert elements into an AVL Tree while implementing the necessary self-balancing rotations is implemented successfully.
