# 📘 GGSIPU – UNIT III ASSIGNMENT (Dynamic Programming & Branch and Bound)

---
Name - Shivam Tiwari
---
Enrollment Number - 04913302723
---

**Marks:** 30 | **Duration:** 30 Minutes

---

## 🧠 SECTION A – Short Theory [15 Marks]

### Q1. DP essentials

**Question:** *List the three ingredients of DP and one-line purpose of each. [2][1]*

**Detailed Solution:**

| Ingredient                     | Purpose                                                                                             |
| ------------------------------ | --------------------------------------------------------------------------------------------------- |
| **Optimal Substructure**       | Global optimum can be composed from subproblem optima → enables a recurrence relation.              |
| **Overlapping Subproblems**    | Same subproblems recur → motivates caching.                                                         |
| **State Storage (Memo/Table)** | Store subproblem results (top-down or bottom-up) → reduces exponential recomputation to polynomial. |

---

### Q2. DP vs Divide & Conquer

**Question:** *State two differences focusing on subproblem overlap and reuse; give one example for each. [2][1]*

**Detailed Solution:**

| Aspect          | Dynamic Programming                | Divide & Conquer                  |
| --------------- | ---------------------------------- | --------------------------------- |
| **Subproblems** | Overlapping                        | Mostly disjoint                   |
| **Reuse**       | Reuses cached results (memo/table) | No reuse; solve independently     |
| **Example**     | LCS, Fibonacci, Bellman–Ford       | Merge Sort, Quick Sort, Karatsuba |

**Why:** DP benefits when many calls repeat; D&C splits problems that do not require reuse.

---

### Q3. Principle of optimality

**Question:** *Define it in one sentence and name any one problem satisfying it. [3][4]*

**Detailed Solution:**
**Definition:** An optimal solution contains optimal solutions to subproblems induced by the chosen decision.
**Examples:** Matrix Chain Multiplication, Floyd–Warshall, Bellman–Ford, 0/1 Knapsack (DP state view).

---

### Q4. Memoization

**Question:** *Define memoization and contrast with tabulation in one line each. [1]*

**Detailed Solution:**

| Technique                  | One-line definition                                         |
| -------------------------- | ----------------------------------------------------------- |
| **Memoization (Top–Down)** | Recursive; compute on demand; cache results keyed by state. |
| **Tabulation (Bottom–Up)** | Iterative; fill DP table in dependency order; no recursion. |

---

### Q5. Branch and Bound idea

**Question:** *Define BnB and the role of bounding in pruning in two lines. [5][6]*

**Detailed Solution:**
**BnB:** Systematically explores a decision/state tree while tracking the best feasible value (incumbent).
**Bounding:** Computes an optimistic **upper bound** at each node; if `ub ≤ best`, prune the node (cannot beat incumbent).

---

## 🧮 SECTION B – Algorithms & Recurrences [15 Marks]

### Q6. Matrix Chain Multiplication (A₁: 5×4, A₂: 4×6, A₃: 6×2, A₄: 2×7)

**Question (a):** *Write m[i,j] recurrence and base case (no derivation). [4][7][8]*
**Question (b):** *State the minimum scalar multiplications (number only). [8][4]*

**Detailed Solution:**

Let matrix dimensions be
`d = [5, 4, 6, 2, 7]` so `A1=5×4, A2=4×6, A3=6×2, A4=2×7`.

**Base:**
`m[i,i] = 0`.

**Recurrence:**

$$
\boxed{\ m[i,j] = \min_{i \le k < j} \big( m[i,k] + m[k+1,j] + d_{i-1},d_k,d_j \big)\ }\quad (1 \le i < j \le 4)
$$

**DP Table (costs):**

| Interval | Best k |                     Cost |
| -------- | -----: | -----------------------: |
| m[1,2]   |      – |              5·4·6 = 120 |
| m[2,3]   |      – |               4·6·2 = 48 |
| m[3,4]   |      – |               6·2·7 = 84 |
| m[1,3]   |      1 |  0 + 48 + 5·4·2 = **88** |
| m[2,4]   |      3 | 48 + 0 + 4·2·7 = **104** |
| m[1,4]   |      3 | 88 + 0 + 5·2·7 = **158** |

**Answer (b):** **158**
**Optimal Parenthesization:** `(A1 (A2 A3)) A4`.

---

### Q7. Longest Common Subsequence (X = "ABCDGH", Y = "AEDFHR")

**Question (a):** *Write the LCS(i,j) recurrence and base. [1]*
**Question (b):** *Give the LCS length (number only). [1]*

**Detailed Solution:**

**Base:** `L(i,0) = L(0,j) = 0`.

**Recurrence:**

$$
\boxed{\ L(i,j) = \begin{cases}
1 + L(i-1, j-1), & X_i = Y_j, \
\max{ L(i-1, j),\ L(i, j-1) }, & X_i \ne Y_j.
\end{cases}}
$$

**Full DP Table (rows: X incl. 0; cols: Y incl. 0):**

| i\j |  0 |  A |  E |  D |  F |  H |  R |
| --: | -: | -: | -: | -: | -: | -: | -: |
|   0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |
|   A |  0 |  1 |  1 |  1 |  1 |  1 |  1 |
|   B |  0 |  1 |  1 |  1 |  1 |  1 |  1 |
|   C |  0 |  1 |  1 |  1 |  1 |  1 |  1 |
|   D |  0 |  1 |  1 |  2 |  2 |  2 |  2 |
|   G |  0 |  1 |  1 |  2 |  2 |  2 |  2 |
|   H |  0 |  1 |  1 |  2 |  2 |  3 |  3 |

Thus `L(6,6) = 3`. One LCS is **"ADH"**.
**Answer (b):** **3**.

---

### Q8. Optimal Binary Search Tree (keys: 10,20,30; p: [0.4,0.3,0.3]; assume q = 0)

**Question (a):** *Write w[i,j] and e[i,j] DP formulations with base. [1]*
**Question (b):** *State the minimum expected search cost (number only). [1]*

**Detailed Solution:**

**Base:** `e[i,i-1] = 0`, `w[i,i-1] = 0`.

**Weight sum:** `w[i,j] = w[i,j-1] + p_j` (i.e., `sum_{k=i..j} p_k`).

**Cost DP:**

$$
\boxed{\ e[i,j] = \min_{r=i}^{j} \big( e[i,r-1] + e[r+1,j] + w[i,j] \big)\ }.
$$

**Working tables:**

* Weights `w[i,j]`:

| i→j |   1 |   2 |   3 |
| --: | --: | --: | --: |
|   1 | 0.4 | 0.7 | 1.0 |
|   2 |   – | 0.3 | 0.6 |
|   3 |   – |   – | 0.3 |

* Costs:

  * Singletons: `e[1,1]=0.4`, `e[2,2]=0.3`, `e[3,3]=0.3`
  * Two keys: `e[1,2]=min{0+0.3+0.7, 0.4+0+0.7} = 1.0`; `e[2,3]=min{0+0.3+0.6, 0.3+0+0.6} = 0.9`
  * Three keys:

$$
\begin{aligned}
e[1,3] &= \min{\underbrace{0 + 0.9 + 1.0}*{r=10},\ \underbrace{0.4 + 0.3 + 1.0}*{r=20},\ \underbrace{1.0 + 0 + 1.0}_{r=30}} \
&= \min{1.9,\ \mathbf{1.7},\ 2.0} = \mathbf{1.7}.
\end{aligned}
$$

**Answer (b):** **1.7** (best root = 20).

---

### Q9. 0/1 Knapsack – Branch & Bound (W=5; w={2,3,4,5}, p={3,4,5,6})

**Question (a):** *Write the fractional upper bound formula used for pruning. [6][9][5]*
**Question (b):** *Show level-0 and level-1 nodes (include/exclude first item) with (v,w,ub) only. [5][6]*

**Detailed Solution:**

1. **Sort by density** `p_i / w_i`: 1.5, 1.33…, 1.25, 1.2.

2. **Fractional upper bound** at a node with current profit `v`, weight `w`, remaining capacity `C = W - w`, starting from item index `i`:

$$
\boxed{\ \text{ub} = v + \sum_{\text{fit } k \ge i} p_k ; + ; \frac{C - \sum_{\text{fit}} w_k}{w_t}, p_t\ }\quad (t:\ \text{first item that does not fully fit}).
$$

3. **Nodes (as asked):**

| Level | Decision      | (v,w) | ub reasoning                          |  **ub** |
| ----: | ------------- | ----: | ------------------------------------- | ------: |
|     0 | Root          | (0,0) | Take item1 & item2 fully (3+4)        | **7.0** |
|     1 | Include item1 | (3,2) | Remaining C=3 → take item2 fully (+4) | **7.0** |
|     1 | Exclude item1 | (0,0) | Take item2 (4) + 2/4 of item3 (2.5)   | **6.5** |

*(Further branching would compare bounds vs incumbent to prune, but the question asks only up to level-1.)*

---

### Q10. Traveling Salesman Problem – Dynamic Programming (Held–Karp; 4 cities)

**Question (a):** *Write the C[S,j] recurrence and final answer expression. [1]*
**Question (b):** *Initialize base entries C[{k},k] for k=2..4 (numbers only for the given D). [1]*

**Detailed Solution:**

Assume start city is 1. For `S ⊆ {2,3,4}`, `j ∈ S`:

**Base:**

$$\boxed{\ C[{j}, j] = d(1, j)\ } \quad (j \ne 1).$$

**Recurrence:**

$$\boxed{\ C[S, j] = \min_{k \in S \setminus {j}} \big( C[S \setminus {j}, k] + d(k, j) \big). }$$

**Final tour cost:**

$$\boxed{\ \min_{j \in {2,3,4}} \big( C[{2,3,4}, j] + d(j, 1) \big). }$$

**Base initialization table:**

|  j | C[{j}, j] |
| -: | --------: |
|  2 |    d(1,2) |
|  3 |    d(1,3) |
|  4 |    d(1,4) |

**Illustration (example distance matrix)**

```
D = [
 [0, 10, 15, 20],
 [10, 0, 35, 25],
 [15, 35, 0, 30],
 [20, 25, 30, 0]
]
```

Then `C[{2},2]=10`, `C[{3},3]=15`, `C[{4},4]=20`, and the optimal tour cost is **80** (e.g., `1→2→4→3→1`).

---

