2015/02/03
###题目

> Given a binary tree, find its minimum depth.

> The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

###分析

####方法一

代码如下：

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int minDepth(TreeNode root){
        if(root == null){
            return 0;
        }
		List<Integer> depthList = new ArrayList<Integer>();
		root.val = 1;
		int min = Integer.MAX_VALUE;
		depthList = searchAllDepthes(root,depthList);
		for(int i : depthList){
			min = min < i ? min:i;
		}
		return min;
	}
	public List<Integer> searchAllDepthes(TreeNode node,List<Integer> depthList){
		if(node.left != null){
			node.left.val = node.val+1;
			depthList = searchAllDepthes(node.left,depthList);
		}
		if(node.right != null){
			node.right.val = node.val+1;
			depthList = searchAllDepthes(node.right,depthList);
		}
		if(node.left == null && node.right == null){
			depthList.add(node.val);
			return depthList;
		}
		return depthList;
	}
}
```


###参考

####参考一

代码如下：
```java
public class Solution {
	public int minDepth(TreeNode root) {
	    if (root == null) return 0;
	    List<TreeNode> list = new ArrayList<TreeNode>();
	    list.add(root);
	    return levelOrder(list, 0);
	}
	private int levelOrder(List<TreeNode> list, int count) {
	    count++;
	    List<TreeNode> next = new ArrayList<TreeNode>();
	    for (TreeNode node : list) {
	        if (node.left == null && node.right == null) return count;
	        if (node.left != null) next.add(node.left);
	        if (node.right != null) next.add(node.right);
	    }
	    return levelOrder(next, count);
	} 
}
```