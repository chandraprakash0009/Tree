## You're given a binary tree. Your task is to find the size of the largest subtree within this binary tree that also satisfies the properties of a Binary Search Tree (BST). The size of a subtree is defined as the number of nodes it contains.

Note: A subtree of the binary tree is considered a BST if for every node in that subtree, the left child is less than the node, and the right child is greater than the node, without any duplicate values in the subtree.

Examples :

Input: root = [5, 2, 4, 1, 3]
Root-to-leaf-path-sum-equal-to-a-given-number-copy
Output: 3
Explanation:The following sub-tree is a BST of size 3
Balance-a-Binary-Search-Tree-3-copy  

Input: root = [6, 7, 3, N, 2, 2, 4]
Output: 3
Explanation: The following sub-tree is a BST of size 3:

Constraints:
1 ≤ number of nodes ≤ 105
1 ≤ node->data ≤ 105

## Java code
```

class Solution {
    static int max;

    // Return the size of the largest sub-tree which is also a BST
    static int largestBst(Node root) {
        max = 0;  // Reset max for each call
        if (root == null) {
            return 0;
        }
        rec(root);
        return max;
    }

    static int[] rec(Node root) {
        if (root == null) {
            return new int[]{0, Integer.MAX_VALUE, Integer.MIN_VALUE}; // {size, min, max}
        }

        if (root.left == null && root.right == null) {
            max = Math.max(max, 1);
            return new int[]{1, root.data, root.data};  // A single node is always a BST of size 1
        }

        int leftSize[] = rec(root.left);
        int rightSize[] = rec(root.right);

        if (leftSize[0] == -1 || rightSize[0] == -1) {
            return new int[]{-1, -1, -1};  // If any subtree is not a BST, return -1
        }

        // Check if the current node forms a BST with its left and right children
        if ((root.left == null || leftSize[2] < root.data) && 
            (root.right == null || rightSize[1] > root.data)) {
            
            int bstSize = 1 + leftSize[0] + rightSize[0];
            max = Math.max(max, bstSize);
            int minValue = Math.min(root.data, leftSize[1]);
            int maxValue = Math.max(root.data, rightSize[2]);

            return new int[]{bstSize, minValue, maxValue};
        }

        return new int[]{-1, -1, -1};  // If it's not a BST
    }
}
