# Ex19 B+ Tree
## DATE:
## AIM:
To write a **Java program** to implement a basic **B+ Tree** structure and demonstrate the function to perform **sequential traversal** of the elements, which involves navigating the linked list of leaf nodes to print the keys in sorted order.

## Algorithm:
1.  Start.
2.  Define two classes: **`InternalNode`** (for index nodes) and **`LeafNode`** (for leaf nodes). The `LeafNode` must include an array for data keys and a **`next`** pointer to the subsequent leaf node.
3.  Implement the utility function **`findLeftmostLeaf(InternalNode root)`**: Start at the root and recursively follow the leftmost child pointer (`child_ptr[0]`) through the index nodes until a `LeafNode` is reached. 
4.  Define the **`sequentialTraverse()`** method:
    a. Call `findLeftmostLeaf()` to get the starting leaf node.
    b. Start a loop that continues while the current leaf node is **not null**.
    c. Inside the loop, print all data keys stored in the current leaf node.
    d. Move to the next leaf node using the **`next`** pointer (`currentLeaf = currentLeaf.next`).
5.  In the `main` method, construct a sample, balanced B+ Tree structure and call `sequentialTraverse()` on the root to display the sorted data keys.
6.  End.

## Program:
/*
Program to traverse the elements in a B+ Tree sequentially.
Developed by: Dharani dharan K
RegisterNumber: 212223040036
*/

import java.util.Arrays;

// --- Simplified B+ Tree Node Definitions (Order 3) ---

class LeafNode {
    int n; // Number of keys
    int[] data = new int[2]; // Max 2 keys
    LeafNode next; // Pointer to the next leaf node (for sequential traversal)

    public LeafNode() {
        n = 0;
        next = null;
    }
}

class InternalNode {
    int n; // Number of keys
    int[] data = new int[2]; // Max 2 keys
    Object[] child_ptr = new Object[3]; // Max 3 children (Node or LeafNode)

    public InternalNode() {
        n = 0;
    }
}

public class BPlusTreeTraversal {
    private InternalNode root;

    public BPlusTreeTraversal() {
        // Build a minimal B+ Tree structure for demonstration
        
        // 1. Create Leaf Nodes
        LeafNode L1 = new LeafNode();
        L1.data[0] = 10; L1.data[1] = 20; L1.n = 2;

        LeafNode L2 = new LeafNode();
        L2.data[0] = 30; L2.data[1] = 40; L2.n = 2;
        
        LeafNode L3 = new LeafNode();
        L3.data[0] = 50; L3.data[1] = 60; L3.n = 2;

        // 2. Link the leaf nodes (Sequential access)
        L1.next = L2;
        L2.next = L3;

        // 3. Create Root Index Node
        root = new InternalNode();
        root.data[0] = 30; // Index key separating L1 and L2
        root.data[1] = 50; // Index key separating L2 and L3
        root.n = 2;
        root.child_ptr[0] = L1;
        root.child_ptr[1] = L2;
        root.child_ptr[2] = L3;
    }

    // 1. Find the leftmost leaf
    private LeafNode findLeftmostLeaf(InternalNode current) {
        Object child = current.child_ptr[0];
        if (child instanceof LeafNode) {
            return (LeafNode) child;
        } else {
            // Recursively traverse to the leftmost child of the internal node
            return findLeftmostLeaf((InternalNode) child);
        }
    }

    // 2. Perform sequential traversal
    public void sequentialTraverse() {
        if (root == null) {
            System.out.println("Tree is empty.");
            return;
        }
        
        // Start from the first leaf node
        LeafNode currentLeaf = findLeftmostLeaf(root);

        System.out.println("--- Sequential Traversal (Sorted Keys) ---");
        while (currentLeaf != null) {
            // Print keys in the current leaf node
            for (int i = 0; i < currentLeaf.n; i++) {
                System.out.print(currentLeaf.data[i] + " ");
            }
            // Move to the next leaf node
            currentLeaf = currentLeaf.next;
        }
        System.out.println();
    }

    public static void main(String[] args) {
        BPlusTreeTraversal bpt = new BPlusTreeTraversal();
        bpt.sequentialTraverse();
    }
}

## Output:

![image](https://github.com/user-attachments/assets/23cf1270-fdba-4c49-ae95-3c2c5f339d3a)
## Result:
Thus, the **Java program** to implement a B+ Tree and successfully traverse the elements sequentially by following the leaf-level linked list is implemented successfully.
