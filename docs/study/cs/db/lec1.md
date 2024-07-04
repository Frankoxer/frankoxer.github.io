---
comments: true
---

# Lecture 1: Introduction

> **A Database Management System (DBMS) is a software system designed to store, manage, and facilitate access to databases.** 

## 1 Purpose of Database Systems

**DBMS 的几个特征：**

* Efficiency and scalability in **data access**.
* Reduced **application development time**.
* Data **independence** (including **physical data independence** and **logical data independence**).
* Data **integrity** and **security**.
* **Concurrent** access and **robustness** (i.e., recovery)

!!! warning "重要内容"

    将其与文件处理系统（File-Processing System）做对比，后者有以下缺点：
    
    * Data redundancy and inconsistency. 多种文件格式和冗余信息
    * Difficulty in accessing data. 每一个新任务都需要编写新程序
    * Data isolation: multiple files and multiple formats. 很难共享
    * Intergrity problems. 很难添加新的条件限制或是改变已有的条件限制
    * No atomicity of updates. 无法保证操作原子性
    * DIfficult to **concurrent access** by multiple users. 并发执行很难
    * Security problems. 安全性问题

## 2 View of Data

数据库的基本架构如图：

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240605230432.png" alt="20240605230432" style="zoom:50%;" />

Schema 和 Instance 的意义以及区别：

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240605230538.png" alt="20240605230538" style="zoom:50%;" />

!!! warning "重要内容"

    **Physical Independence vs. Logical Independence**:
    
    * Independence: Ability to modify a schema definition at one level without affecting a schema definition at a higher level. 
    * **Physical Independence**: The ability to modify the physical schema without changing the logical schema. 这是使用 DBMS 最重要的好处之一
    * **Logical Independence**: Protect application programs from changes in logical structure of data. (*Logical data independence is hard to achieve as the application programs are heavily dependent on the logical structure of data.*)

## 3 Database Language

DDL: Data Definition Language
DML: Data Manipulation Language
DCL: Data Control Language

**SQL** is the most widely used **non-procedural** query language.

SQL = DDL + DML + DCL

## 4 Database Design

上图！

<img src="https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240605231319.png" alt="20240605231319" style="zoom:50%;" />

还有 E-R(Entity-Relationship) Model, first proposed by **Peter Chen**, 和 Relation Model, 以表格形式出现。

## 5 Database Users and Administrators

管理员称为 DBA: Database Administrator，

* DBA has **the highest privilege** for the database.
* DBA **coordinates** all the activities of the database system.
* DBA **controls all users authority** to the database.
* DBA has a good understanding of the enterprise's information resources and requirements.

## 6 Transaction Management

> **A transaction is a collection of operations that performs a single logical function in a database application.**

!!! warning "重要内容"

    Transaction **requirements**:
    
    * Atomicity
    * Consistence
    * Isolation
    * Durability

## 7 Database Architecture

Storage Manager, Query Processor 等。