## 889. Construct Binary Tree from Preorder and Postorder Traversal

Given two integer arrays, preorder and postorder where preorder is the preorder traversal of a binary tree of distinct values and postorder is the postorder traversal of the same tree, reconstruct and return the binary tree.

If there exist multiple answers, you can return any of them.

 

Example 1:  

Input: preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]  
Output: [1,2,3,4,5,6,7]  
Example 2:  

Input: preorder = [1], postorder = [1]  
Output: [1]  
 

Constraints:  

1 <= preorder.length <= 30  
1 <= preorder[i] <= preorder.length  
All the values of preorder are unique.  
postorder.length == preorder.length  
1 <= postorder[i] <= postorder.length  
All the values of postorder are unique.  
It is guaranteed that preorder and postorder are the preorder traversal and postorder traversal of the same binary tree.  

## Approach - 
first element in the preorder is always the root of tree and it is also equal to the last element in postorder traversal.
1.  keep the track of first and last index of both traversal array.
2.  make the root from the first element in the preorder.
3.  find successor of root i.e. the element just after the root element in the preorder array.
4.  find the index of successor in the postorder(say x) and on the basis of that index, we will divide the postorder into two part ( for left and right subtree) -
     (i).  from postStart to x,
     (ii).  from x+1  to postEnd-1
    (postEnd-1) beacause last element in the postorder array is always the root element which has been made.
6.  The number of element between postStart and x will lie in the left subtree and remaining element will lie in the right subtree. So, find the number of element in the left subtree. Using the number of element in the left subtree , we will divide the preorder array into two part -
     (i)  from preStart+1 to preStart+leftSubtreeSize
     (ii)  from preStart+leftSize+1 to preEnd.

```
class Solution {
    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        return rec(0, preorder.length - 1, preorder, 0, postorder.length - 1, postorder);
    }

    private TreeNode rec(int preStart, int preEnd, int[] preorder, int postStart, int postEnd, int[] postorder) {
        if (preStart > preEnd) return null;
        
        TreeNode root = new TreeNode(preorder[preStart]);
        if (preStart == preEnd) return root;

        int successor = preorder[preStart+1];
        int x = findIndex(postorder,successor,postStart,postEnd);

        int leftSize = x-postStart+1;

        root.left = rec(preStart+1,preStart+leftSize,preorder,postStart,x,postorder);
        root.right = rec(preStart+leftSize+1,preEnd,preorder,x+1,postEnd-1,postorder);
        return root;
    }

    private int findIndex(int[] arr, int target, int start, int end) {
        for (int i = start; i <= end; i++) {
            if (arr[i] == target) return i;
        }
        return -1;
    }
}
