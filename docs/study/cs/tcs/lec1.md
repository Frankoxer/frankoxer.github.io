---
comments: true
---

# Lecture 1: Problem and DFA

## 1 Introduction to this course

这门课叫做理论计算机科学导引（Introduction to Theoretical Computer Science），研究的是计算问题的上界和下界。

**upper bound**: 取决于问题的解决**算法**。

**lower bound**: 取决于问题的解决**难度**。举个例子，基于比较的排序算法，已经被证明其时间复杂度的下界是 $O(n \log n)$，那么当我们已经达到此下界时，就不需要再去寻找更快的排序算法了。

当然，除此之外我们也可以研究空间复杂度、近似算法、随机算法等等。

为了研究这些问题，我们需要对以下一些概念下清晰的定义：

**problem, computing model, computation, computability, tractability, etc**.

学习时，最重要的是关注定义、定义与证明时候的 idea。

## 2 Problem

???note "举个例子"

    Problem 1: Given a graph $G$, what is its MST?

    Problem 2: Given a graph $G$, what's the total weight of its MST?

    Problem 3: Givan a graph $G$ and an integer $k$, does $G$ have a MST with weight at most $k$?

    第三个问题是一个 decision problem，它等价于：

    Given a string $w$, is $w \in L = \{\text{encode} <G, k>: G \ \text{is a graph that has a weight at most } k\}$?

    推广一下，对于一个 decision problem $L$，我们可以定义其 language $L$ 为：

    $L=\{\text{encodings of yes-instance of }\ P\}$.

    反之，对于一个 language $L$，我们可以定义其 decision problem $P$ 为：

    Given a string $w$, is $w \in L$?

现在对问题（Problem）进行一些比较 formal 的定义。

### 2.1 Alphabet

An **alphabet** is a finite set of symbols.

E.g., $\Sigma = \{0, 1\}, \Sigma=\{\ \}$.

### 2.2 String

A **string** over $\Sigma$ is a finite sequence of symbols from $\Sigma$.

E.g., $w = 1010, w = 01$.

The length of string $w$ is noted as $|w|$.

**Empty string**（空串）: $e$, $|e|=0$.

Some operations over strings:

* Concatenation: $w_1w_2$.
* Exponentiation: $w^k$. Especially, $w^0=e$.
* Reversal: $w^R$.

### 2.3 Language

A **language** over $\Sigma$ is a set of strings over $\Sigma$.

E.g., $L = \{0, 1, 01, 10\}$.

Some operations over languages:

* Concatenation: $L_1L_2 = \{w_1w_2: w_1 \in L_1, w_2 \in L_2\}$.

* Exponentiation: $L^k = \{w^k: w \in L\}$. 
Especially, $L^0 = \{e\}$, $L^{i+1} = LL^i$, $L^* = \bigcup_{i\ge 0}L^i$, $L^+ = \bigcup_{i\ge 1}L^i$.

* Reversal: $L^R = \{w^R: w \in L\}$.

## 3 Deterministic Finite Automaton (DFA)

