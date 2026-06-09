# 📘 Prefix Sum Practice Set — Day 2

Welcome to **Day 2** of my Competitive Programming practice series.

Today’s focus is:

> 🔥 Difference Arrays and Advanced Prefix Sum Transformations

These techniques extend the basic prefix sum idea and help solve range update problems and subarray counting problems efficiently.

---

# 🧠 Topics Covered

* Difference Array
* Range Updates
* Point Queries
* Prefix Sum + Hash Map
* Prefix Transformation
* Longest Zero-Sum Subarray Pattern

---

# 📌 Problems Solved

## 1️⃣ Range Additions

### Idea

Instead of updating every element in a range:

```text
[L, R] += X
```

use a Difference Array.

For each update:

```text
diff[L] += X
diff[R + 1] -= X
```

After processing all updates, take a prefix sum to reconstruct the final array.

### Insight

Range updates become:

```text
O(1)
```

instead of:

```text
O(R - L + 1)
```

### Complexity

* Updates: O(Q)
* Reconstruction: O(N)
* Total: O(N + Q)

---

## 2️⃣ Range Update and Point Query

### Idea

Use the same Difference Array concept.

For:

```text
[L, R] += X
```

perform:

```text
diff[L] += X
diff[R + 1] -= X
```

To obtain the value at position:

```text
P
```

compute:

```text
diff[1] + diff[2] + ... + diff[P]
```

### Insight

A Difference Array stores changes, not actual values.

The actual value at any position is obtained through prefix sums.

### Learning

This problem introduces the classic:

```text
Range Update + Point Query
```

pattern, which later evolves into Fenwick Trees and Segment Trees.

---

## 3️⃣ Count Subarrays With Sum K

### Idea

Use Prefix Sum + Hash Map.

Let:

```text
prefix = sum of elements so far
```

For a subarray ending at the current position:

```text
subarraySum = K
```

we need:

```text
previousPrefix = prefix - K
```

If this prefix sum appeared before, every occurrence forms a valid subarray.

### Formula

```text
answer += frequency[prefix - K]
```

### Insight

Instead of checking all subarrays, we convert the problem into counting previous prefix sums.

### Complexity

* Time: O(N)
* Space: O(N)

---

## 4️⃣ Longest Subarray With Equal Even and Odd Numbers

### Idea

Transform the array:

```text
even -> +1
odd  -> -1
```

Now the problem becomes:

```text
Find the longest subarray with sum = 0
```

### Prefix Observation

If the same prefix sum appears twice:

```text
prefix[i] = prefix[j]
```

then:

```text
sum(i+1 ... j) = 0
```

Store the first occurrence of every prefix sum.

Whenever the same prefix appears again:

```text
answer = max(answer, current - firstOccurrence)
```

### Insight

Many counting and balancing problems can be transformed into zero-sum subarray problems.

### Complexity

* Time: O(N log N) using map
* Space: O(N)

---

# 🚀 Key Learnings from Day 2

### ✔ Difference Arrays make range updates cheap

Instead of updating every element:

```text
O(length)
```

we only update boundaries:

```text
O(1)
```

---

### ✔ Prefix Sums can count subarrays

Using:

```text
prefix - K
```

we can count subarrays with a target sum in linear time.

---

### ✔ Transformations simplify problems

Sometimes the original condition is difficult.

Converting:

```text
even -> +1
odd  -> -1
```

turns a parity problem into a zero-sum problem.

---

### ✔ Prefix Sums + Hash Maps are extremely powerful

They appear in:

* Subarray Sum Equals K
* Longest Zero Sum Subarray
* Balanced Binary Array
* Equal Even/Odd Problems
* Frequency Difference Problems

---

# ⚡ Complexity Summary

| Problem                    | Main Technique        | Complexity  |
| -------------------------- | --------------------- | ----------- |
| Range Additions            | Difference Array      | O(N + Q)    |
| Range Update & Point Query | Difference Array      | O(1) Update |
| Count Subarrays With Sum K | Prefix Sum + Hash Map | O(N)        |
| Equal Even and Odd         | Prefix Transformation | O(N log N)  |

---

# 📈 Final Thoughts

Day 1 focused on answering range queries efficiently.

Day 2 introduced a deeper idea:

> Instead of storing answers, store changes and relationships.

Difference Arrays taught how to process massive updates efficiently.

Prefix Sum Transformations showed how seemingly unrelated problems can become standard zero-sum or prefix-frequency problems after a clever conversion.

These patterns appear frequently in competitive programming and form the foundation for many advanced techniques.

---

# 🏁 Day 2 Complete

💡 Next step: Move to **Day 3 → Sliding Window + Two Pointers**

Topics ahead:

* Fixed Size Window
* Variable Size Window
* Longest Valid Segment
* At Most K Distinct Elements
* Two Pointer Optimization
