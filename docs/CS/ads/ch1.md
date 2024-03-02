# Lecture 1: AVL Trees, Splay Trees, and Amortized Analysis

AVL 树、伸展树和摊还分析。

## 1 AVL Trees

- **Target**: Speed up searching(with insertion and deletion). 加快二叉树搜索速度。
- **Tool**: Binary Search Trees
- **Problem**: The height of the tree can be as bad as $O(N)$.

一般的二叉搜索树存在问题：树的高度与输入序列有关。例如当我们输入递增/递减序列来构建二叉搜索树的话，树的高度就变成了 $N$。而由于动态问题中不能确定下一个输入的值是多少，因此要重新排列输入序列来获得平衡树是不可能的。

因此解决问题的角度变为**时刻盯紧这棵树**，一有新元素加入就重新拾掇使之平衡。这也是 AVL 树（**A**delson-**V**elskii-**L**andis Trees, 1962）的思想。

!!! info "定义：**Height Balanced**(高度平衡)"

    An empty binary tree is height balanced. If $T$ is a nonempty binary tree with $T_{L}$ and $T_{R}$ as its left and right subtrees, then $T$ is height balanced iff:
    
    1. $T_{L}$ and $T_{R}$ are height balanced, and
    2. $|h_{L}-h_{R}|\leq 1$ where $h_{L}$ and $h_{R}$ are the heights of  $T_{L}$ and $T_{R}$, respectively.

!!! warning "注意"
    空树的高度定义为 -1。

AVL 树的一种实现方法就是**时刻关注 BF 值**，对树进行调整使之平衡。

!!! info "定义：**Balance Factor**(平衡因数)"

    $BF(\mathrm{node})=h_{L}-h_{R}$. In an AVL tree, $BF(\mathrm{node})=-1, 0, \mathrm{or}\  1.$

**在加入新的结点的时候，BF 值自下而上更新**。

接下来介绍 AVL 树进行调整的几种情况：

### 1.1 Single Rotation

![image-20240302131432696](https://5v1a-typora.oss-cn-hangzhou.aliyuncs.com/image-20240302131432696.png)

情况如上。

加入 Nov 结点之后，更新 BF 值，发现 Mar 的 BF 值已经不满足 AVL 树的条件了。此时进行**单旋**（Single Rotation），将 Mar 旋转下来作为 May 的左子树。因为这个问题的 ”Trouble Maker“ 是 ”Trouble Finder“ 的<u>右子树的右子树</u>，所以这种操作又称为 **RR Rotation**。

同理，也有 **LL Rotation**，不再赘述。

一般化此情况，即为：

![image-20240302132044944](https://5v1a-typora.oss-cn-hangzhou.aliyuncs.com/image-20240302132044944.png)

!!! info "提示"

    这里的 $A$ **可以不是树根**。

### 1.2 Double Rotation

![image-20240302133117809](https://5v1a-typora.oss-cn-hangzhou.aliyuncs.com/image-20240302133117809.png)

情况如上。

加入 Jan 结点后，May 的 BF 值不满足条件，此时将 Mar 拎上去作为树根，原来的 May 就作为它的右儿子即可。这种情况下的 ”Trouble Maker“ 是  ”Trouble Finder“ <u>左子树的右子树</u>，所以这种操作称为 **LR Rotation**。同时，这种操作可以看作是两个单旋的组合，称为双旋（Double Rotation）。

同理，也有 **RL Rotation**，不再赘述。

 一般化此情况，即为：

![image-20240302133614463](https://5v1a-typora.oss-cn-hangzhou.aliyuncs.com/image-20240302133614463.png)

![image-20240302133828777](https://5v1a-typora.oss-cn-hangzhou.aliyuncs.com/image-20240302133828777.png)

!!! info "提示"

    这里的 $A$ 同样**可以不是树根**。

### 1.3 Analysis

二叉搜索树的操作时间复杂度为 $O(\mathrm{height})$。这里需要找到这个 height。推导过程如下：

!!! note "推导过程"

    记 $n_{h}$ 为高度为 $h$ 的平衡二叉树包含最少的结点数。则树的形状必定为：

    <img src="https://5v1a-typora.oss-cn-hangzhou.aliyuncs.com/image-20240302134911731.png" alt="image-20240302134911731" style="zoom: 30%;" />

    因此推导出 $n_{h}=n_{h-1}+n_{h-2}+1$。联想到斐波那契数满足 $F_{0}=0, F_{1}=1, F_{i}=F_{i-1}+F_{i-2} ,\ \mathrm{for} \  i>1$，可以证明出 $n_{h}=F_{h+2}-1, \ \mathrm{for}\ h\ge0$.

    由于斐波那契数定理给出了 $F_{i}\approx \frac{1}{\sqrt{5}}(\frac{1+\sqrt{5}}{2})^{i}$，因此可以得到$n_{h}\approx \frac{1}{\sqrt{5}}(\frac{1+\sqrt{5}}{2})^{h+2}-1$，也就是 $h\approx O(\mathrm{ln}\ n)$.

## 2 Splay Trees

- **Target**: 