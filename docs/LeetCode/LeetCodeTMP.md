## 950

Todo

题目是特殊的排序手段

```
Input: [17,13,11,2,3,5,7]
Output: [2,13,3,11,5,17,7]
Explanation: 
We get the deck in the order [17,13,11,2,3,5,7] (this order doesn't matter), and reorder it.
After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
We reveal 2, and move 13 to the bottom.  The deck is now [3,11,5,17,7,13].
We reveal 3, and move 11 to the bottom.  The deck is now [5,17,7,13,11].
We reveal 5, and move 17 to the bottom.  The deck is now [7,13,11,17].
We reveal 7, and move 13 to the bottom.  The deck is now [11,17,13].
We reveal 11, and move 17 to the bottom.  The deck is now [13,17].
We reveal 13, and move 17 to the bottom.  The deck is now [17].
We reveal 17.
Since all the cards revealed are in increasing order, the answer is correct.
```

**想法**

首先先自己排个序？
需要使用队列

## 814

给定一个树，树的节点只有两个值，0或1
实现一个剪枝算法，将该树的所有不包含1的子树剪枝

```
Example 1:
Input: [1,null,0,0,1]
Output: [1,null,0,null,1]

         1           
               0     
            0     1
        |
        |
         1           
               0     
                 1   
```

**想法**

递归咯

### 法一

```
         1           
   0           1     
0     0     0     1  

增加一次清洗，把左子树的0给去掉

         1           
   0           1     
                  1  

```

怎么优化呢

```py
class Solution:
    def pruneTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return root
        if (not root.left) and (not root.right) and root.val == 0:
            return None
        else:
            root.left = self.pruneTree(root.left)
            root.left = self.pruneTree(root.left)
            root.right = self.pruneTree(root.right)
            return root
```

**优化**

还是逻辑有问题，先清理子树，在清理自己

```py
class Solution:
    def pruneTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return root
        root.left = self.pruneTree(root.left)
        root.right = self.pruneTree(root.right)
        if (not root.left) and (not root.right) and root.val == 0:
            return None
        return root
```

## 039

Todo

给定一个数组nums和一个数target，从数组中凑数字使得和为target，其中，nums的数字可以重复

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**想法**

使用hashmap来记录可能的差值？然后用递归？重复计算的问题用map？

### 其他

使用深度搜索