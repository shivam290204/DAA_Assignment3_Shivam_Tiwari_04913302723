
# GGSIPU – UNIT III ASSIGNMENT
---
Name: Shivam Tiwari
---
Enrollment number: 04913302723
---

## SECTION A – Short Theory [15 Marks]

### Q1) DP essentials — List the three ingredients of DP and one-line purpose of each. [2][1]

**Answer:**

1. **Optimal substructure:** Global optimum can be formed from subproblem optima → enables a recurrence.
2. **Overlapping subproblems:** Same subproblems recur → motivates caching.
3. **Memo/table:** Store subproblem answers → eliminates recomputation → time ↓ from exponential to polynomial.

---

### Q2) DP vs D&C — Two differences (overlap & reuse) + one example each. [2][1]

**Answer:**

* **Overlap:** DP has overlapping subproblems; D&C has largely disjoint subproblems.
* **Reuse:** DP **reuses** cached results (memo/table); D&C **does not reuse** (solves new subproblems).
* **Examples:** DP → LCS, Fibonacci; D&C → MergeSort, QuickSort.

---

### Q3) Principle of optimality — Define + name a problem. [3][4]

**Answer:**
**Definition:** Any optimal solution contains optimal solutions to all subproblems induced by the chosen decision.
**Examples:** Matrix Chain Multiplication, All-Pairs Shortest Paths (Floyd–Warshall), Bellman–Ford, 0/1 Knapsack (DP state view).

---

### Q4) Memoization — Define & contrast with tabulation in one line each. [1]

**Answer:**

* **Memoization (top-down):** Recursive calls compute **on demand**; cache results.
* **Tabulation (bottom-up):** Iteratively fill table in dependency order; no recursion.

---

### Q5) Branch & Bound idea — Define BnB + role of bounding/pruning (two lines). [5][6]

**Answer:**
BnB explores a **state tree**, branching on choices and tracking the **best feasible** value so far.
A **bound** (optimistic upper bound) is computed per node; if **bound ≤ best**, prune that node (no better solution possible).

---

## SECTION B – Algorithms & Recurrences [15 Marks]

### Q6) Matrix Chain Multiplication (A₁:5×4, A₂:4×6, A₃:6×2, A₄:2×7)

**(a) Write m[i,j] recurrence and base (no derivation). [4][7][8]**
Let dimensions (d=[5,4,6,2,7]).
**Base:** (m[i,i]=0).
**Recurrence:**
[
m[i,j]=\min_{i\le k<j}\left{, m[i,k]+m[k+1,j]+d_{i-1},d_k,d_j \right}.
]

**(b) Minimum scalar multiplications (number only). [8][4]**
**Answer: 158**

**Detailed work (DP table):**

* (d=[5,4,6,2,7]) (so (A_1=5!\times!4,\ A_2=4!\times!6,\ A_3=6!\times!2,\ A_4=2!\times!7))

**Cost table (m[i,j]):**

* Length 2:

  * (m[1,2]=5\cdot4\cdot6=120)
  * (m[2,3]=4\cdot6\cdot2=48)
  * (m[3,4]=6\cdot2\cdot7=84)
* Length 3:

  * (m[1,3]=\min{(1|2,3):120+48+5\cdot4\cdot2=208,\ (1,2|3):120+0+5\cdot6\cdot2=180}=88) ← (recompute carefully)
    **Correct calc:**

    * (k=1:\  m[1,1]+m[2,3]+5\cdot4\cdot2=0+48+40=88)
    * (k=2:\  m[1,2]+m[3,3]+5\cdot6\cdot2=120+0+60=180) → pick **88**
  * (m[2,4]=\min{k=2:\ 0+84+4\cdot6\cdot7=252,\ k=3:\ 48+0+4\cdot2\cdot7=104}=\mathbf{104})
* Length 4:

  * (m[1,4]=\min{k=1:0+104+5\cdot4\cdot7=244,\ k=2:120+84+5\cdot6\cdot7=414,\ k=3:88+0+5\cdot2\cdot7=\mathbf{158}})

**Split table (s[i,j]) (argmin k):** (s[1,3]=1,\ s[2,4]=3,\ s[1,4]=3).
**Optimal order:** split at (k=3): ((A_1..A_3),A_4); then split (A_1..A_3) at (k=1): (A_1,(A_2A_3)).
So **((A_1,(A_2A_3)),A_4)** with cost **158**. *(Note: not (((A_1A_2)(A_3A_4)))).*

---

### Q7) Longest Common Subsequence (X="ABCDGH", Y="AEDFHR")

**(a) LCS(i,j) recurrence and base. [1]**
**Base:** (LCS(0,,j)=LCS(i,,0)=0).
**Recurrence:**
[
L(i,j)=
\begin{cases}
1+L(i-1,j-1), & X_i=Y_j\
\max{L(i-1,j),,L(i,j-1)}, & X_i\ne Y_j
\end{cases}
]

**(b) LCS length (number only). [1]**
**Answer: 3**

**Brief table idea:** matches at ((A,A), (D,D), (H,H)) ⇒ one LCS = **“ADH”**; table ends with (L(6,6)=3).

---

### Q8) Optimal Binary Search Tree (keys: 10,20,30; p: 0.4,0.3,0.3; assume q=0)

**(a) Write (w[i,j]) and (e[i,j]) DP formulations with base. [1]**
**Base:** (e[i,i-1]=0,\ w[i,i-1]=0).
**Weight:** (w[i,j]=w[i,j-1]+p_j) (sum of (p_i..p_j)).
**Cost:**
[
e[i,j]=\min_{r=i}^j \big( e[i,r-1] + e[r+1,j] + w[i,j] \big).
]

**(b) Minimum expected search cost (number only). [1]**
**Answer: 1.7**

**Brief working:** Try roots:

* Root 20 (depths: 20@1, 10@2, 30@2): (0.3\cdot1+0.4\cdot2+0.3\cdot2=\mathbf{1.7}) (best)
* Root 10 → 1.9; Root 30 → 2.0/2.1.

---

### Q9) 0/1 Knapsack – Branch & Bound (W=5; w={2,3,4,5}, p={3,4,5,6})

**(a) Fractional upper bound formula used for pruning. [6][9][5]**
Items sorted by density (p_k/w_k). For a node at level (i) with current ((v,w)) and capacity (W):
[
\text{ub}=v+\sum_{\text{fit }k\ge i} p_k\ +\ \frac{W-w-\sum_{\text{fit}} w_k}{w_t}\cdot p_t,
]
where (t) is the first item that **doesn’t** fully fit (omit last term if none).

**(b) Level-0 & Level-1 nodes (include/exclude first item) with (v,w,ub) only. [5][6]**
Densities: (1.5,,1.\overline{3},,1.25,,1.2).

* **Level-0 (root):** ((0,0,\ \text{ub}=3+4= \mathbf{7})).
* **Level-1 include item1 (2,3):** ((3,2,\ \text{ub}=3+4=\mathbf{7})) (left capacity 3 fits item2 fully).
* **Level-1 exclude item1:** capacity 5; take item2 (4,3) + **fraction** of item3: (5-3=2\Rightarrow 2/4) of profit 5 → 2.5;
  ((0,0,\ \text{ub}=4+2.5=\mathbf{6.5})).

*(These ubs guide pruning; actual feasibility still checked by weight.)*

---

### Q10) TSP – Dynamic Programming (Held–Karp; 4 cities, example D given)

**(a) (C[S,j]) recurrence and final answer. [1]**
Start at city (1).
**Base:** (C[{j},j]=d(1,j)) for (j\neq1).
**Recurrence:**
[
C[S,j]=\min_{k\in S\setminus{j}}\ \big( C[S\setminus{j},k] + d(k,j) \big),\quad j\in S,\ 1\notin S.
]
**Final:**
[
\min_{j\in{2,3,4}}\ \big( C[{2,3,4},j] + d(j,1) \big).
]

**(b) Initialize base entries (C[{k},k]) for (k=2..4) (numbers only for the given (D)). [1]**

* **Generic:** (C[{2},2]=d(1,2),\ C[{3},3]=d(1,3),\ C[{4},4]=d(1,4)).
* **Illustration (if (D=\begin{bmatrix}0&10&15&20\10&0&35&25\15&35&0&30\20&25&30&0\end{bmatrix})):**
  (C[{2},2]=10,\ C[{3},3]=15,\ C[{4},4]=20).
  *(For this (D), optimal tour cost is 80 via 1→2→4→3→1.)*

---

