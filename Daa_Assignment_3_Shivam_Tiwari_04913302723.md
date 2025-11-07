# GGSIPU – UNIT III ASSIGNMENT
---
Name: Shivam Tiwari
---
Enrollment Number: 04913302723
---

## SECTION A – Short Theory [15 Marks]

### Q1) DP essentials — List the three ingredients of DP and one-line purpose of each. [2][1]

**Answer:**

| Ingredient                     | One-line purpose                                                                              |
| ------------------------------ | --------------------------------------------------------------------------------------------- |
| **Optimal substructure**       | Global optimum can be composed from subproblem optima → enables a recurrence.                 |
| **Overlapping subproblems**    | The same subproblems occur repeatedly → motivates caching.                                    |
| **Memo/table (state storage)** | Store subproblem results to avoid recomputation → reduce time from exponential to polynomial. |

---

### Q2) DP vs Divide & Conquer — Two differences (overlap & reuse) + one example each. [2][1]

**Answer:**

| Aspect               | Dynamic Programming                  | Divide & Conquer                |
| -------------------- | ------------------------------------ | ------------------------------- |
| **Subproblems**      | Overlapping                          | Mostly disjoint                 |
| **Reuse**            | Reuses cached solutions (memo/table) | No reuse; solve independently   |
| **Typical examples** | Fibonacci, LCS, Bellman–Ford         | MergeSort, QuickSort, Karatsuba |

---

### Q3) Principle of Optimality — Define + name a problem. [3][4]

**Answer (one sentence):** An optimal solution contains optimal solutions to the subproblems induced by the chosen decision.

**Examples:** Matrix Chain Multiplication (MCM), All-Pairs Shortest Paths (Floyd–Warshall), Bellman–Ford.

---

### Q4) Memoization vs Tabulation — Define each in one line. [1]

**Answer:**

| Technique                  | One-line definition                                                    |
| -------------------------- | ---------------------------------------------------------------------- |
| **Memoization (Top–Down)** | Recursive, compute-on-demand, cache results of calls; uses call stack. |
| **Tabulation (Bottom–Up)** | Iterative filling of DP table in dependency order; no recursion.       |

---

### Q5) Branch & Bound idea — Define BnB + role of bounding/pruning (two lines). [5][6]

**Answer:** Branch & Bound explores a state tree, branching on decisions while tracking the best feasible value. A **bound** gives an optimistic upper bound; if **bound ≤ current best**, the node is **pruned** (cannot beat incumbent).

---

## SECTION B – Algorithms & Recurrences [15 Marks]

### Q6) Matrix Chain Multiplication (A₁: 5×4, A₂: 4×6, A₃: 6×2, A₄: 2×7)

**(a) Write (m[i,j]) recurrence and base (no derivation). [4][7][8]**

Let dimensions be (d=[5,4,6,2,7]) so that (A_1=d_0\times d_1=5\times4), …, (A_4=d_3\times d_4=2\times7).

**Base:** (m[i,i]=0).

**Recurrence:**
[
\boxed{; m[i,j] = \min_{i\le k<j} \big( m[i,k] + m[k+1,j] + d_{i-1},d_k,d_j \big);,\quad 1\le i<j\le n;}
]

**(b) Minimum scalar multiplications (number only). [8][4]**
**Answer:** **158**

**Worked DP table (costs):**

| Interval |               Split k |        Cost |
| -------- | --------------------: | ----------: |
| (m[1,2]) |                     – | (5·4·6=120) |
| (m[2,3]) |                     – |  (4·6·2=48) |
| (m[3,4]) |                     – |  (6·2·7=84) |
| (m[1,3]) |   (k=1): (0+48+5·4·2) |      **88** |
|          |  (k=2): (120+0+5·6·2) |         180 |
| (m[2,4]) |   (k=2): (0+84+4·6·7) |         252 |
|          |   (k=3): (48+0+4·2·7) |     **104** |
| (m[1,4]) |  (k=1): (0+104+5·4·7) |         244 |
|          | (k=2): (120+84+5·6·7) |         414 |
|          |   (k=3): (88+0+5·2·7) |     **158** |

**Optimal order (via split table):** ((A_1,(A_2A_3)),A_4)).

---

### Q7) Longest Common Subsequence (X = "ABCDGH", Y = "AEDFHR")

**(a) Write the LCS(i,j) recurrence and base. [1]**

**Base:** (L(0,j)=L(i,0)=0).

**Recurrence:**
[
\boxed{;L(i,j)=\begin{cases}
1+L(i-1,j-1), & X_i=Y_j,\
\max{L(i-1,j),,L(i,j-1)}, & X_i\ne Y_j~.
\end{cases};}
]

**(b) LCS length (number only). [1]**
**Answer:** **3**

**Short justification table (matches):**

| Match positions | Character |
| --------------- | --------- |
| ((1,1))         | A         |
| ((4,4))         | D         |
| ((6,6))         | H         |

One LCS is **“ADH”** ⇒ length **3**.

---

### Q8) Optimal Binary Search Tree (keys: 10,20,30; (p=[0.4,0.3,0.3]); assume (q=0))

**(a) Write (w[i,j]) and (e[i,j]) formulations with base. [1]**

**Base:** (e[i,i-1]=0,; w[i,i-1]=0).

**Weights (prefix accumulation):**
(w[i,j]=w[i,j-1]+p_j) ((=\sum_{k=i}^j p_k)).

**Cost DP:**
[
\boxed{; e[i,j]=\min_{,r=i..j}\big( e[i,r-1]+e[r+1,j]+w[i,j] \big);}
]

**(b) Minimum expected search cost (number only). [1]**
**Answer:** **1.7** (best root = 20)

**Worked tables:**

* **Weights (w[i,j]):**

| i→ j↓ |   1 |   2 |   3 |
| ----: | --: | --: | --: |
| **1** | 0.4 | 0.7 | 1.0 |
| **2** |   – | 0.3 | 0.6 |
| **3** |   – |   – | 0.3 |

* **Single-key costs:** (e[1,1]=0.4), (e[2,2]=0.3), (e[3,3]=0.3)

* **Two-key intervals:**

  * (e[1,2]=\min{0+0.3+0.7,\ 0.4+0+0.7}=\min{1.0,1.1}=\mathbf{1.0}) (root 10)
  * (e[2,3]=\min{0+0.3+0.6,\ 0.3+0+0.6}=\mathbf{0.9}) (either root)

* **Three-key interval:**

  * (e[1,3]=\min{\underbrace{0+0.9+1.0}*{r=10}=1.9,\ \underbrace{0.4+0.3+1.0}*{r=20}=\mathbf{1.7},\ \underbrace{1.0+0+1.0}_{r=30}=2.0}\Rightarrow \mathbf{1.7}) (root 20)

---

### Q9) 0/1 Knapsack – Branch & Bound ((W=5); (w={2,3,4,5}), (p={3,4,5,6}))

**(a) Fractional upper bound formula used for pruning. [6][9][5]**

Sort items by density (p_k/w_k) in non-increasing order. For a node at level (i) with current profit (v), weight (w), remaining capacity (C=W-w):
[
\boxed{;\text{ub} = v + \sum_{\text{fit }k\ge i} p_k ; + ; \frac{C-\sum_{\text{fit}} w_k}{w_t}, p_t;}
]
where (t) is the first item that **doesn’t** fully fit (omit the fractional term if all fit).

**Densities:** (\tfrac{3}{2}=1.5,\ \tfrac{4}{3}\approx1.33,\ \tfrac{5}{4}=1.25,\ \tfrac{6}{5}=1.2).

**(b) Level-0 & Level-1 nodes (include/exclude first item) with (v, w, ub). [5][6]**

| Level | Decision                | (v, w) | ub computation                                                    |  **ub** |
| ----: | ----------------------- | -----: | ----------------------------------------------------------------- | ------: |
|     0 | Root                    | (0, 0) | Take items 1 & 2 fully (3+4), capacity filled                     | **7.0** |
|     1 | Include item1 (w=2,p=3) | (3, 2) | Remaining C=3 → take item2 fully (+4)                             | **7.0** |
|     1 | Exclude item1           | (0, 0) | Take item2 (4,3) + fraction of item3: extra C=2 → (2/4\cdot5=2.5) | **6.5** |

These bounds guide pruning: any node with (\text{ub} \le) incumbent best profit can be discarded.

---

### Q10) TSP – Dynamic Programming (Held–Karp; 4 cities, example D given)

**(a) Recurrence (C[S,j]) and final expression. [1]**

Assume start city is 1. Let (S\subseteq{2,3,4}), (j\in S).

**Base:** (\boxed{\ C[{j},j]=d(1,j)\ }) for all (j\neq1).

**Recurrence:**
[
\boxed{; C[S,j] = \min_{k\in S\setminus{j}} \big( C[S\setminus{j},k] + d(k,j) \big) ;}
]

**Final answer:**
[
\boxed{; \min_{j\in{2,3,4}} \big( C[{2,3,4},j] + d(j,1) \big) ;}
]

**(b) Initialize base entries (C[{k},k]) for (k=2..4) (numbers only for a given (D)). [1]**

|  k | (C[{k},k]) |
| -: | ---------: |
|  2 |   (d(1,2)) |
|  3 |   (d(1,3)) |
|  4 |   (d(1,4)) |

**Illustration (if** (D=\begin{bmatrix}0&10&15&20\10&0&35&25\15&35&0&30\20&25&30&0\end{bmatrix})**):**
(C[{2},2]=10), (C[{3},3]=15), (C[{4},4]=20); final optimal tour cost for this (D) is **80** (e.g., (1\to2\to4\to3\to1)).

---

