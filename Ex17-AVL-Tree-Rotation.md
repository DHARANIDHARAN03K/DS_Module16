# Ex17 AVL Tree – Rotation
## DATE:
## AIM:
To write a **Java program** to implement an **AVL Tree** and demonstrate the **Right Rotation (LL Case)** function used for balancing a left-heavy subtree.

## Algorithm:
1.  Start.
2.  Define a class **`Node`** with integer `data`, pointers to `left` and `right` children, and an integer `height`.
3.  Define the utility function **`height(Node N)`** to determine the current height of a node.
4.  Define the **`rightRotate(Node x)`** method, where $x$ is the unbalanced node:
    a. Let **$y$** be the left child of $x$ ($y = x.left$). ($y$ will become the new root.)
    b. Let **$T_2$** be the right child of $y$ ($T_2 = y.right$).
    c. Perform the rotation: set $x$'s left child to $T_2$ ($x.left = T_2$).
    d. Set $y$'s right child to $x$ ($y.right = x$). 
    e. **Update the heights** of the moved nodes $x$ and $y$.
    f. Return $y$, the new root of the rotated subtree.
5.  In the `main` method, construct a sample tree that requires a Left-Left (LL) imbalance, call the `rightRotate` function to fix it, and verify the structure.
6.  End.

## Program:
/*
Program to perform right rotation in AVL Tree
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

public class AVLTreeRotation {
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
    
    // Function to perform Right Rotation (LL Case)
    Node rightRotate(Node x) {
        // x is the unbalanced node
        Node y = x.left;
        Node T2 = y.right; // T2 is the subtree that needs re-parenting

        // 1. Perform rotation
        y.right = x;
        x.left = T2;

        // 2. Update heights (must be done bottom-up)
        x.height = max(height(x.left), height(x.right)) + 1;
        y.height = max(height(y.left), height(y.right)) + 1;

        // 3. Return new root
        return y;
    }

    // Utility function to insert (simplified, focusing on rotation demo)
    public Node insert(Node node, int data) {
        if (node == null)
            return new Node(data);
        if (data < node.data)
            node.left = insert(node.left, data);
        else if (data > node.data)
            node.right = insert(node.right, data);
        return node;
    }

    // Utility function for Inorder traversal
    void inOrder(Node node) {
        if (node != null) {
            inOrder(node.left);
            System.out.print(node.data + " ");
            inOrder(node.right);
        }
    }

    public static void main(String[] args) {
        AVLTreeRotation tree = new AVLTreeRotation();

        // 1. Build an imbalanced tree (LL Case)
        tree.root = tree.insert(tree.root, 30);
        tree.root.left = tree.insert(tree.root.left, 20);
        tree.root.left.left = tree.insert(tree.root.left.left, 10);
        
        // Manually set heights to simulate an AVL structure before rotation
        tree.root.left.height = 2;
        tree.root.height = 3; 

        System.out.println("--- AVL Right Rotation Demonstration ---");
        System.out.print("Inorder before rotation (Root=30): ");
        tree.inOrder(tree.root); 
        System.out.println();
        System.out.println("Imbalance detected at 30 (Balance Factor = 2)");

        // 2. Perform Right Rotation
        tree.root = tree.rightRotate(tree.root);

        // 3. Verify the balanced tree
        System.out.print("Inorder after rotation (New Root=20): ");
        tree.inOrder(tree.root); 
        System.out.println();
    }
}

## Output:


![image](https://github.com/user-attachments/assets/23cf1270-fdba-4c49-ae95-3c2c5f339d3a)
## Result:
Thus, the **Java program** to implement and successfully perform the Right Rotation (LL Case) in an AVL Tree is implemented successfully.
