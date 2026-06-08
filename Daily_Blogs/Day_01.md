
# 📘 Prefix Sum Practice Set — Day 1

Welcome to **Day 1** of my Competitive Programming practice series.

Today’s focus is:

> 🔥 Mastering Prefix Sum techniques in 1D and 2D problems

These problems are fundamental for solving range query problems efficiently in **O(1)** after preprocessing.

---

# 🧠 Topics Covered

- 1D Prefix Sum
- Prefix Count Arrays
- Condition-based Prefix Queries
- Range Equality Checks
- 2D Prefix Sum (Grid Queries)

---

# 📌 Problems Solved

## 1️⃣ Static Range Sum

### Idea:
Precompute prefix sums of the array.

```text
prefix[i] = A[1] + A[2] + ... + A[i]
```

### Query:
```text
Sum(L, R) = prefix[R] - prefix[L - 1]
```

### Complexity:
- Preprocessing: O(N)
- Query: O(1)

---

## 2️⃣ Number of Zeros in Range

### Idea:
Build prefix count of zeros.

```text
prefix[i] = number of zeros in A[1...i]
```

### Query:
```text
zeros = prefix[R] - prefix[L - 1]
```

### Insight:
Instead of summing values, we count occurrences.

---

## 3️⃣ Even Numbers Query

### Idea:
Count how many elements are even.

```text
if A[i] % 2 == 0 → prefix[i]++
```

### Query:
```text
even_count = prefix[R] - prefix[L - 1]
```

### Insight:
Prefix arrays can track any condition, not just sums.

---

## 4️⃣ Balanced Segment (0s = 1s)

### Idea:
Use prefix count of zeros.

```text
zeros = prefix[R] - prefix[L - 1]
length = R - L + 1
```

### Condition:
```text
zeros == length / 2
```

### Insight:
A binary array is balanced when half are zeros.

---

## 5️⃣ Forest Queries (2D Prefix Sum)

### Idea:
Use 2D prefix sum for grid queries.

```text
prefix[i][j] = total trees from (1,1) to (i,j)
```

### Query:
```text
answer =
prefix[r2][c2]
- prefix[r1-1][c2]
- prefix[r2][c1-1]
+ prefix[r1-1][c1-1]
```

### Insight:
2D prefix sum extends 1D logic into grids.

---

# 🚀 Key Learnings from Day 1

### ✔ Prefix Sum is not just summation
It can track:
- counts
- conditions
- patterns

### ✔ Range queries become O(1)
After O(N) or O(N²) preprocessing.

### ✔ Inclusion-Exclusion is powerful
Used in both 1D and 2D problems.

---

# ⚡ Complexity Summary

| Problem | Preprocessing | Query |
|--------|--------------|-------|
| Range Sum | O(N) | O(1) |
| Zero Count | O(N) | O(1) |
| Even Count | O(N) | O(1) |
| Balanced Segment | O(N) | O(1) |
| Forest Queries | O(N²) | O(1) |

---

# 📈 Final Thoughts

Today’s set builds the foundation for:

- Difference Arrays
- Sliding Window Optimization
- Segment Trees
- Fenwick Trees (BIT)

Prefix sum is the **first real step into competitive programming optimization techniques**.

---

# 🏁 Day 1 Complete

💡 Next step: Move to **Day 2 → Sliding Window + Two Pointers**

---
