---
comments: true
---

# Chapter 1 Welcome Aboard

**There is no magic to computing!**

## 1.1 Two Recurring Themes

### 1.1.1 The Notion of Abstracion

- One should try to keep the level of abstraction as long as possible, consistent with getting everything to work efficiently.
- There is an underlying assumption to this: **when everything about the detail is just fine**.

## 1.1.2 Hardware vs. Software

- Not separating the notion of hardware and software in your mind is very important.
- You'll be much more capable if you master both.

## 1.2 A Computer System

- CPU: Processor
- MEM: Storage and Memory
- I/O: Input & Output Devices

## 1.3 Two Very Important Ideas

1. **All computers are capable of computing exactly the same things if they're given enough time and enough memory.**

2. **It is necessary to transform our problem from the human language to the voltages that influence the flow of electrons.**

   | Problems                      |
   | ----------------------------- |
   | **Algorithms**                |
   | **Language**                  |
   | **Machine(ISA) Architecture** |
   | **Microarchitecture**         |
   | **Circuits**                  |
   | **Devices**                   |
   
   It's called the level of transformation.
   
   - **Problem:** The statement of the problem shouldn't have ambiguity.
   - **Algorithm:** A step-by-step procedure that is guaranteed to terminate, such that each step is precisely stated and can be carried out by the computer.(*<u>Definiteness, Effective, Finiteness</u>*)
   - **Program: **Two kinds of programming languages: High-level(at a distance from the underlying computer)/Low-level(tied to the computer).
   - **ISA:** The instruction set architecture is the complete specification of the interface between programs and the underlying computing hardware.(Compiler: Translate from a high-level language to the ISA.)
   - **Microarchitecture:** Each ISA has many ways to realize for different microarchitectures.
