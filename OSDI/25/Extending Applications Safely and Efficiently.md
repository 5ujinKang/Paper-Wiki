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
 But the current tools are neither safe nor efficient, because they suffer from a security-efficiency tradeoff. 
Since every extension has different requirements for security & efficiency, it's hard to optimize the environment for each.
<br>

**Their Solution** 
 Suggest a new model EIM, optimize for each extension, fulfilling their requirements for security & efficiency level. 
EIM abstracts the features of extension by seeing extension as a "resource", and provides a framework to deal with each. 
By fulfilling each extension's requirements for security/efficiency level, it solves the security-efficiency trade-off problem. 
<br>

**Performance**

<br>

**Limitation**

---
<br> <br>

# 🔎 Details

## 0. Abstract
- **High-level Problem statement**
  
  *( what kind of the problem the paper is trying to solve, why is this a significant and/or interesting problem, what is the limitation of the prior work)*
  : the current state-of-the-art is neither safe nor efficient enough.

<br>

- **High-level design principle / key novelty** 
: presents new model, EIM and extension framework that enforces an EIM specification, bpftime.
  - Extension Interface Model (EIM) : model that treats each required feature of an extension as a resource


<br> <br>

## 1. Introduction
- What they bring : 
- Problem statement  
- Design principle  
- Overall storyline of the work
  
<br>

**Storyline Summary:**  
1. Why the problem is important  
2. What prior works exist and their limitations  
3. What this paper proposes  
4. What challenges the design faces  
5. How the design overcomes them  
6. What results show
   
<br>

**Notes (if unclear parts):**  

<br> <br>

## 2. Low-Level Details

<br>

### Background
- **What is the motivation behind the problem?**  
- **What background knowledge does the paper provide?**  
- **Did I fully understand why the problem is significant?**

<br>

### Motivation
- **How did prior work solve the problem?**  
- **What were their limitations?**

<br>

### Design
- **Design principle(s):**  
- **Design goals or challenges:**  
- **How the design addresses the challenges:**  

<br>

### Implementation
- **My guess of implementation before reading:**  
- **Key implementation details learned:**  
- **Does the implementation strengthen the storyline understanding?**  

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




