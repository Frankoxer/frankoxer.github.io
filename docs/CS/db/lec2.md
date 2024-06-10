---
comments: true
---

# Lecture 2: Relational Model

关系模型。关系数据库就是由一个或者更多的关系所构成的。

## 1 Structure of Relational Databases

### 1.1 Relation

Given sets $D_1$, $D_2$, ..., $D_n$, a relation r is a subset of $D_1 \times D_2 \times ... \times D_n$, which is a **Cartesian product** of a list of domain $D_i$.

Attribute values are (normally) required to be **atomic**, i.e., **indivisible**(--1st NF, 关系理论第一范式).

两个概念：

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610164629.png" alt="20240610164629" style="zoom: 67%;" />

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610164643.png" alt="20240610164643" style="zoom: 50%;" />

**The Properties of Relation**:

* The order of tuples is **irrelevant**. 顺序不影响
* **No duplicated** tuples in a relation. 没有重复的行
* Attribute values are **atmoic**.

### 1.2 Key

$K \subseteq R$.

K is a:

* Superkey（超码）, if values od K are **sufficient** to **identify** a unique tuple of each possible relation r(R).
* Candidate key（候选码）, if K is **minimal superkey**.
* Primary key（主码）, if K is a candidate key and is **defined by user explicitly**.

**Foreign Key**:

Assume r(<u>A</u>, B, C), s(<u>B</u>, D), we can say that attribute B in relation r is **foreign key** referencing s, and r is a **referencing relation**（参照关系）, and s is a **referenced relation**（被参照关系）.

**参照关系中外码的值必须在被参照关系中实际存在，或者为 null**。

Primary key and foreign key are **integrated constraints**.

## 2 Fundamental Relational-Algebra Operations

六种基本运算：

* Select 选择
* Project 投影
* Union 并
* Set Difference 差（集合差）
* Cartesian Product 笛卡儿积
* Rename 改名（重命名）

The operators take **one or two** relations as inputs, and return a new relation as a result.

### 2.1 Select

**Notation**: $\sigma_p(r)$, where p is called the **selection predicate**.

**Definition**: $\sigma_p(r)=\{t|t\in r\ \mathrm{and}\ p(t)\}$

E.g., $\sigma_{\mathrm{branch\_name = 'Perryridge'}}(\mathrm{account})$

### 2.2 Project

**Notation**: $\Pi_{\mathrm{A1, A2, ..., Ak}}(r)$

*Duplicate rows are removed from result, since relations are sets*.

E.g., $\Pi_{\mathrm{account\_number, balance}}(\mathrm{account})$

### 2.3 Union

**Notation**: $r \cup s$

**Definition**: $r \cup s=\{t|t\in r\ \mathrm{or}\ t\in s\}$

For $r \cup s$ to be valid：
* r and s must have the same arity (i.e., the same number of attributes) 
* The attribute domains must be **compatible**.


E.g., $\Pi_{\mathrm{customer\_name}}(\mathrm{depositor}) \cup \Pi_{\mathrm{customer\_name}}(\mathrm{borrower})$

### 2.4 Set Difference

**Notation**: $r-s$

**Definition**: $r-s=\{t|t\in r\ \mathrm{and}\ t\notin s\}$

Set differences must be taken between **compatible relations**.

* r and s must have the same arity. 
* Attribute domains of r and s must be compatible.

### 2.5 Cartesian-Product

??? "举个例子"

    ![20240610210835](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610210835.png)

**Notation**: $r\times s$

**Definition**: $r\times s=\{\{tq\}|t\in r\ \mathrm{and}\ q\in s\}$

这里假设 R 和 S 的 attributes 没有相同的交集。如果有交集，需要重命名重复的 attributes。例如：

??? "需要重命名属性的笛卡尔积"

    ![20240610211145](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610211145.png)

可以用多重操作得到新的关系。

### 2.6 Rename

* Allows us to name, and therefore to refer to, the results of relational-algebra expressions. (procedural) 
* Allows us to refer to a relation by more than one name. 

E.g,

$$\rho_X(E)$$

returns the expression E under the name X.

If a relational-algebra expression E has arity n, then

$$\rho_{(A1, A2, ..., An)}(E)$$

returns the result of expression E.

### 2.7 Examples

??? "Find all loans of over $2000"

    $\sigma_{\mathrm{amount}>1200}(loan)$

??? "Find the loan number for each loan of an amount reater than $1200"

    $\Pi_{\mathrm{loan\_number}}(\sigma_{\mathrm{amount}>1200}(loan))$

??? "Find the names of all customers who at least have a loan or/and an account at bank"

    $\Pi_{\mathrm{customer\_name}}(borrower)\cup / \cap \Pi_{\mathrm{customer\_name}}(depositor)$

??? "Find the names of all customers who have a loan at the Perryridge branch"

    ![20240610212315](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610212315.png)
    
    先做选择操作更好一些。

??? "Find the names of all customers who have loans at the Perryridge branch but do not have an account at any branch of the bank"

    ![20240610212419](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610212419.png)

??? "Find the largest account balance (i.e., self-comparison)"

    * Step 1: Rename *account* relation as d.
    * Step 2: Find the relation including all balances **except the largest one**.
      $\Pi_{\mathrm{account.balance}}(\sigma_{\mathrm{account.balance < d.balance}}(account\times \rho_d(account)))$
    * Step 3: Find the largest account balance.
      $\Pi_{\mathrm{balance}}(account)-\Pi_{\mathrm{account.balance}}(\sigma_{\mathrm{account.balance < d.balance}}(account\times \rho_d(account)))$

## 3 Additional Relational-Algebra Operations

Four basic operators:

* Set intersection 交
* Natural join 自然连接
* Division 除
* Assignment 赋值

这些操作并没有使得关系代数系统更强大，而是将常用语句进行简化。

### 3.1 Set-Intersection

**Notation**: $r\cap s$

**Definition**: $r\cap s=\{t|t\in r\ \mathrm{and}\ t \in s\}$

Assume: 

* r and s have the same arity. 
* attributes of r and s are compatible.

**NOTE**: $r\cap s = r-(r-s)$

### 3.2 Natural Join

**Notation**: $r\bowtie s$

??? "举个例子"

    ![20240610214725](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610214725.png)
    
    ![20240610214952](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610214952.png)

**拓展：Theta Join**

Notation: $r\bowtie_{\theta} s$, where $\theta$ is the predicate on attributes in the schema.

Definition: $r\bowtie_{\theta}s=\sigma_{\theta}(r\times s)$

### 3.3 Division

适合有所谓的 "for all" 请求。

??? "举个例子"

    ![20240610215737](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610215737.png)

**Notation**: $r\div s$

一张图看懂：

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610215917.png" alt="20240610215917" style="zoom: 50%;" />

### 3.4 Assignment

就是一种简记法。

### 3.5 Example

??? "Find all customers who have an account from at least the “Downtown” and the “Uptown” branches"

    一种方式：
    
    $\Pi_{\mathrm{customer-name}}(\sigma_{\mathrm{branch-name='Downtown'}}(depositor \bowtie account)) \cap \Pi_{\mathrm{customer-name}}(\sigma_{\mathrm{branch-name='Uptown'}}(depositor \bowtie account))$
    
    另一种方式：
    
    $\Pi_{\mathrm{customer-name, branch-name}}(depositor\bowtie account) \div \rho_{\mathrm{temp(branch-name)}}(\{('Downtown'), ('Uptown')\})$

### 3.6 Priority

* Project
* Select
* Cartesian Product
* Join, division
* Intersection
* Union, difference

## 4 Extended Relational-Algebra Operations

两种。

* Generalized Projection
* Aggregate Functions

### 4.1 Generalized Projection

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610221614.png" alt="20240610221614" style="zoom:50%;" />

### 4.2 Aggregate Functions

Takes a collection of values and returns a single value as a result.

* avg: average
* min: minimum
* max: maximum
* sum: sum of values
* count: number of values

用法：$G1, G2, ..., Gn\ \ g_{F1(A1), F2(A2),...,Fn(An)}(E)$，g 前面是分组（可以为空），后面放对应的函数。

E.g., 求平均存款余额：$g_{avg{\mathrm{balance}}}(acocunt)$

更清楚的例子：

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610222124.png" alt="20240610222124" style="zoom:50%;" />

## 5 Modification of the Database

一般三种操作：增、删、改，都用赋值运算符来表示。

例如：删除就是 $r \leftarrow r-E$，这里的 E 是一个关系代数表达式；插入就是 $r \leftarrow r\cup E$，这里的 E 是一个关系代数表达式；删除就是 $r \leftarrow \Pi_{F1, F2, ..., Fn}(r)$，这里的 F 是属性。

??? "来一个 update 的例子"

    ![20240610222538](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240610222538.png)