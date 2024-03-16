---
comments: true
---

# Lecture 3: Inverted File Index

The real **key technique** used by search engines.

!!!note "引入：举个栗子"
    假如我们想搜索带有“Computer Science”关键词的网站，有哪些方法？

    * **Solution 1**: 对每个页面进行 "Computer Science" 字符串的扫描？ ***Wait till your next life!!!***
    * **Solution 2**: 使用 Term-Document Incidence Matrix
    ![20240316143541](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240316143541.png)
    会出现的问题：矩阵可能会变得非常稀疏（有很多 0）。

如何解决？使用倒排索引。

!!!note "定义"

    **Index** is a mechanism for locating a given term in a text. 简单来说就是一个存储位置的指针。

    **Inverted file** contains a list of pointers (e.g. the number of a page) to all occurrences of that term in the text.

![20240316153126](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240316153126.png)

!!!info "为什么要记录出现次数 Frequency"

    在做多关键词求交集结果的时候，我们常常选择从频率低的关键词入手，这样能够加快求解的速度。

问题变为如何生成这些 index。

**Index Generator**:
```pseudocode
while ( read a document D )
{
    while ( read a term T in D ){
        if ( Find ( Dictionary, T ) == false )
            Insert( Dictionary, T );
        Get T's posting list;
        Insert a node to T's posting list;
    }
}
Write the inverted index to disk;
```

以上涉及很多部分。