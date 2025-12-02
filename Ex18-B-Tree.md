# Ex19 B+ Tree
## DATE:
## AIM:
To write a **Java program** to implement a basic **B+ Tree** structure and demonstrate the function to perform **sequential traversal** of the elements, which involves navigating the linked list of leaf nodes to print the keys in sorted order.

## Algorithm:
1.  Start.
2.  Define two classes: **`Node`** (for internal/index nodes) and **`LeafNode`** (for leaf nodes). Leaf nodes must contain the actual data keys and a pointer to the **next leaf node**.
3.  Implement the **`sequentialTraverse(Node root)`** method:
    a. Check if the `root` is null; if so, return.
    b. **Find the leftmost leaf node:** Start at the root and recursively follow the leftmost child pointer (`child_ptr[0]`) until a leaf node is reached. 
    c. Once the leftmost `LeafNode` is found, start a loop.
    d. In the loop, print all data keys within the current leaf node.
    e. Move to the next leaf node using the leaf node's **`next`** pointer.
    f. Continue the loop until the `next` pointer is null (the rightmost leaf is reached).
4.  In the `main` method, construct a small, balanced B+ Tree and call `sequentialTraverse()` on the root to display the sorted sequence of keys.
5.  End.

## Program:
/*
Program to traverse the elements in a B+ Tree sequentially.
Developed by: Dharani dharan K
RegisterNumber: 212223040036
*/

import java.util.Arrays;

// --- Simplified B+ Tree Structure (Order 3) ---

class LeafNode {
    int n; // Number of keys
    int[] data = new int[2]; // Max 2 keys
    LeafNode next;

    public LeafNode() {
        n = 0;
        next = null;
    }
}

class InternalNode {
    int n; // Number of keys
    int[] data = new int[2]; // Max 2 keys
    Object[] child_ptr = new Object[3]; // Max 3 children (Node or LeafNode)
    boolean isLeaf = false;

    public InternalNode() {
        n = 0;
    }
}

public class BPlusTreeTraversal {
    private InternalNode root;

    public BPlusTreeTraversal() {
        // Build a minimal B+ Tree structure for demonstration
        LeafNode L1 = new LeafNode();
        L1.data[0] = 10; L1.data[1] = 20; L1.n = 2;

        LeafNode L2 = new LeafNode();
        L2.data[0] = 30; L2.data[1] = 40; L2.n = 2;
        
        LeafNode L3 = new LeafNode();
        L3.data[0] = 50; L3.data[1] = 60; L3.n = 2;

        // Link the leaf nodes
        L1.next = L2;
        L2.next = L3;

        // Root Index Node
        root = new InternalNode();
        root.data[0] = 30; // Key separating L1 and L2
        root.data[1] = 50; // Key separating L2 and L3
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
            return findLeftmostLeaf((InternalNode) child);
        }
    }

    // 2. Perform sequential traversal
    public void sequentialTraverse() {
        if (root == null) {
            System.out.println("Tree is empty.");
            return;
        }
        
        LeafNode currentLeaf = findLeftmostLeaf(root);

        System.out.println("--- Sequential Traversal (Sorted Keys) ---");
        while (currentLeaf != null) {
            for (int i = 0; i < currentLeaf.n; i++) {
                System.out.print(currentLeaf.data[i] + " ");
            }
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
