---
title: 算法题：二叉树B是否是二叉树A的子结构
tags:
  - 算法题
abbrlink: cc245d1
date: 2020-06-12 21:54:46
---
### 题目描述
输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）
<!-- more -->
#### 解题思路
（1）先在A中找和B的根节点相同的结点
（2）找到之后遍历对应位置的其他结点，直到B中结点遍历完，都相同时，则B是A的子树
（3）对应位置的结点不相同时，退出继续在A中寻找和B的根节点相同的结点，重复步骤，直到有任何一棵二叉树为空退出
![](https://raw.githubusercontent.com/DLineko/myPicGo/master/20200612000640.png)

```javascript
/* function TreeNode(x) {
    this.val = x;
    this.left = null;
    this.right = null;
} */
function HasSubtree(root1,root2) {
  {    
    if(root1 === null || root2 ===null)
        return false;
    return isSubTree(root1, root2) || HasSubtree(root1.left, root2) || HasSubtree(root1.right, root2);
}
}
function isSubTree(root1,root2){
  if(root2 === null) return true
  if(root1 === null) return false
  if (root1.val!=root2.val) return false;
  return isSubTree(root1.left,root2.left) &&
isSubTree(root1.right, root2.right)
}
```
