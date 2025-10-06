# 📄 Title : Extending Applications Safely and Efficiently
- **source:**   https://www.usenix.org/conference/osdi20/presentation/fried
- **Keywords:**  Application extending, safety, efficiency
- **Core Idea (inferred from keywords):**  Maybe it is suggesting a better way to extend the application safer and more efficient

<br>

## Summary :
**Background**
Application extension is very important because it can improve performance, add features, enhance security, etc. 
<br>

**Attack Point**
But the current tools are neither safe nor efficient, because they suffer from a security-efficiency tradeoff. <br>
Since every extension has different requirements for security & efficiency, it's hard to optimize the environment for each.
<br>

**Goal**
Suggest the way to extend Applications Safely and Efficiently 
<br>

**Their Solution** 
Suggest a new model EIM, optimize for each extension, fulfilling their requirements for security & efficiency level. <br>
EIM abstracts the features of extension by seeing extension as a "resource", and provides a framework to deal with each. <br>
By fulfilling each extension's requirements for security/efficiency level, it solves the security-efficiency trade-off problem. 
<br>

**Performance**

<br>

**Limitation**




---
<br> <br>




# 🔎 Details

## 0. Abstract
**High-level Problem statement :**  
  *( what kind of the problem the paper is trying to solve, why is this a significant and/or interesting problem, what is the limitation of the prior work)*
  <br>
  the current state-of-the-art is neither safe nor efficient enough.

<br>

**High-level design principle / key novelty :** 
<br>
presents new model, EIM and extension framework that enforces an EIM specification, bpftime.
  - Extension Interface Model (EIM) : model that treats each required feature of an extension as a resource


<br> <br>

## 1. Introduction
**Problem statement :**
<br>
- software extension is important : add features, improve performance/security etc
- current extension frameworks are inefficient because they employ heavyweight techniques for isolation and safety
  1. extension safety : trade off between extension interconnectedness vs. safety (deployment specific)
  2. extension isolation : prevents a host appication from harming an extension, necessary for security monitoring extensions
  3. extension efficiency : requires the extensions execute at near-native speed
<br>

**Design principle :**
<br>
- **EIM : a new approach for specifying the interface between an extension and a host**
  - Supports fine-grained interconnectedness / safety tradeoffs
  - represent the extension features needed for interconnectedness or restricted for safety through a single abstraction called a resource.

<br>

- **bpftime : a new extension runtime system**
  - support EIM & extension isolation
  1. uses lightweight approaches : provide extension safety & isolation
      - for safety : Uses extended Berkeley Packet Filter (eBPF) style verification => 0 runtime overhead
      - for isolation : uses ERIM-style intraprocess hardware-supported isolation => minimal overhead
  2. introduces concealed extention entries : improve efficiency by eliminating runtime overhead from extension entries that are not in use by a running process
  - fully compatible with with eBPF -> can extend both the kernel & applications

<br> <br>

## 2. Low-Level Details
### Background
**Motivation of the problem :**
*(why this is a significant problem to solve)*
Current state-of-the-art frameworks can't fulfill Key Extension Framework Features

<br>

**Background knowledge to understand this paper :**
- Key Extension Framework Features
  - Fine-grained Safety/Interconnectedness tradeoffs
    : There is no single definition of safety appropriate for all extension use-cases
  - Isolation : Extension must be isolated = ensuring that host applications cannot modify extension states
  - Efficiency

<br>

---
### Motivation
**How the prior work solves the problem :**
- native execution : execute the application & extension in the same execution context, essentially treating the extension as  component of the original program.
- SFI-based tools : many frameworks use SFI to provide isolation.
- Subprocess Isolation : separate extensions from the host application through OS isolation abstractions
- eBPF uprobes : eBPF framework provides userspace extensions through the uprobe interface
- Aspect-oriented progrmming : allows extenions
<br>

**What were their limitations? :**
- native execution : No isolation, Don't support fine-grained safety/interconnectedness tradeoffs.
- SFI-based tools : Some of they can't handle safety/interconnectedness tradeoff correctly. & inefficient (validate extension behavior at runtime)
- Subprocess Isolation :
  - Even though some could provide fine-grained interconnectedness/safety tradeoffs, they do so after code changes to the host application
  - have context-switch-like overheads => inefficient for frequently-executed extension use-cases
- eBPF uprobes :
  - Don't support fine-grained interconnectedness/safety tradeoffs
  - not efficient : eBPF uprobes place a software breakpoint on every extension entry, causing the system to trap into the kernel to execute each extension
- Aspect-oriented Programming : Don't support safety/interconnectedness tradeoffs
- 
<br>

---
### Design
**Design principle(s) :**
<br>

**Design goals or challenges :**  
<br>

**How the design addresses the challenges :**  
<br>

---
### Implementation
**Before reading, think of how to implement the design :**  

<br>

**Make sure you understand the storyline**
<br>
go to the Summary from the front, fill out the blank.
<br> <br>

## 3. Evaluation

<br>

**Before reading:**  
- What questions should the evaluation answer? (e.g., Does it outperform? Improve reliability?)  

<br>

**While reading:**  
- Are those questions addressed?  
- Are explanations of results logical?  
- Any suspicious or unclear claims?  

<br>

**Key Results Summary:**  

<br> <br>

## 4. Limitations / Discussion / Related Work
- What trade-offs or limitations are mentioned?  
- Are they reasonable?  
- Do they inspire new ideas?  

<br>


# 🧐 My Works

## Questions:

<br> <br>

## Learned Knowledge:


<br> <br>

## Insights: 




