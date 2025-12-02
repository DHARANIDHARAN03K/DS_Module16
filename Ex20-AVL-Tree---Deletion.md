# Ex20 AVL Tree - Deletion
## DATE:
## AIM:
To write a **Java program** to implement an **AVL Tree** and demonstrate the recursive function for **deleting an element** while maintaining the AVL property using balancing rotations.

## Algorithm:
1.  Start.
2.  Define a class `Node` with integer `data`, pointers to `left` and `right` children, and an integer `height`.
3.  Define utility methods: **`height(Node N)`**, **`getBalance(Node N)`**, and the four rotation methods (LL, RR, LR, RL) as previously implemented for AVL insertion.
4.  Define the helper function **`inorderSuccessor(Node node)`** to find the node with the smallest key in the right subtree. 
5.  Define the recursive **`deleteNode(Node root, int key)`** method:
    a. Perform standard **BST deletion** recursively (search for the key).
    b. If the node to be deleted is found:
        i. If it has one or zero children, handle the deletion directly.
        ii. If it has two children, find the **inorder successor** (smallest in the right subtree), copy the successor's data to the current node, and recursively delete the successor.
    c. After deletion from a subtree, **update the height** and **calculate the Balance Factor (BF)** of the current node.
    d. If $|BF| > 1$, perform the necessary rotation (LL, RR, LR, or RL) to rebalance the tree.
    e. Return the root of the (possibly rotated) subtree.
6.  In the `main` method, create a balanced AVL Tree instance, call `deleteNode()` for a key, and verify the balanced structure.
7.  End.

## Program:
/*
Program to delete an element from an AVL Tree.
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
        height = 1;
    }
}

public class AVLTreeDeletion {
    Node root;

    int height(Node N) {
        if (N == null) return 0;
        return N.height;
    }

    int max(int a, int b) {
        return (a > b) ? a : b;
    }

    int getBalance(Node N) {
        if (N == null) return 0;
        return height(N.left) - height(N.right);
    }
    
    // --- Rotation Implementations (Required for Rebalancing) ---
    Node rightRotate(Node y) {
        Node x = y.left;
        Node T2 = x.right;
        x.right = y;
        y.left = T2;
        y.height = max(height(y.left), height(y.right)) + 1;
        x.height = max(height(x.left), height(x.right)) + 1;
        return x;
    }

    Node leftRotate(Node x) {
        Node y = x.right;
        Node T2 = y.left;
        y.left = x;
        x.right = T2;
        x.height = max(height(x.left), height(x.right)) + 1;
        y.height = max(height(y.left), height(y.right)) + 1;
        return y;
    }
    // --- End Rotations ---

    // Finds the node with the minimum value in the tree rooted at node
    Node inorderSuccessor(Node node) {
        Node current = node;
        while (current.left != null)
            current = current.left;
        return current;
    }

    // Recursive function to delete a key from the subtree rooted with node
    Node deleteNode(Node root, int key) {
        // 1. PERFORM STANDARD BST DELETION
        if (root == null)
            return root;

        if (key < root.data)
            root.left = deleteNode(root.left, key);
        else if (key > root.data)
            root.right = deleteNode(root.right, key);
        else {
            // Node with only one child or no child
            if ((root.left == null) || (root.right == null)) {
                Node temp = (root.left != null) ? root.left : root.right;

                if (temp == null) { // No child case
                    temp = root;
                    root = null;
                } else // One child case
                    root = temp; // Copy the contents of the non-empty child
            } else {
                // Node with two children: Get the inorder successor (smallest in the right subtree)
                Node temp = inorderSuccessor(root.right);
                root.data = temp.data; // Copy the successor's data to this node
                root.right = deleteNode(root.right, temp.data); // Delete the successor
            }
        }

        // If the tree had only one node, return
        if (root == null)
            return root;

        // 2. UPDATE HEIGHT OF CURRENT NODE
        root.height = max(height(root.left), height(root.right)) + 1;

        // 3. GET THE BALANCE FACTOR
        int balance = getBalance(root);

        // 4. CHECK FOR IMBALANCE AND PERFORM ROTATION

        // Left Left Case (LL)
        if (balance > 1 && getBalance(root.left) >= 0)
            return rightRotate(root);

        // Left Right Case (LR)
        if (balance > 1 && getBalance(root.left) < 0) {
            root.left = leftRotate(root.left);
            return rightRotate(root);
        }

        // Right Right Case (RR)
        if (balance < -1 && getBalance(root.right) <= 0)
            return leftRotate(root);

        // Right Left Case (RL)
        if (balance < -1 && getBalance(root.right) > 0) {
            root.right = rightRotate(root.right);
            return leftRotate(root);
        }

        return root;
    }
    
    // Utility function to insert
    public void insert(int data) {
        root = insertRec(root, data);
    }

    private Node insertRec(Node node, int data) {
        if (node == null) return new Node(data);
        if (data < node.data) node.left = insertRec(node.left, data);
        else if (data > node.data) node.right = insertRec(node.right, data);
        else return node; // Duplicate keys not allowed

        node.height = 1 + max(height(node.left), height(node.right));
        int balance = getBalance(node);

        // LL Case
        if (balance > 1 && data < node.left.data) return rightRotate(node);
        // RR Case
        if (balance < -1 && data > node.right.data) return leftRotate(node);
        // LR Case
        if (balance > 1 && data > node.left.data) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }
        // RL Case
        if (balance < -1 && data < node.right.data) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }
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
        AVLTreeDeletion tree = new AVLTreeDeletion();

        // Build a sample AVL tree
        int[] elements = {9, 5, 10, 0, 6, 11, -1, 1, 2};
        for (int element : elements) {
            tree.insert(element);
        }
        
        System.out.println("--- AVL Tree Deletion Demonstration ---");
        System.out.print("Inorder Traversal before Deletion: ");
        tree.inOrder(tree.root); 
        System.out.println();

        // Delete key 10 (triggers balancing)
        int keyToDelete = 10;
        tree.root = tree.deleteNode(tree.root, keyToDelete);

        System.out.println("\nDeleted Element: " + keyToDelete);
        System.out.print("Inorder Traversal after Deletion: ");
        tree.inOrder(tree.root); 
        System.out.println();
    }
}

## Output:

![image](https://github.com/user-attachments/assets/23cf1270-fdba-4c49-ae95-3c2c5f339d3a)
## Result:
Thus, the **Java program** to delete an element from an AVL Tree, including the logic for rebalancing using rotations, is implemented successfully.
