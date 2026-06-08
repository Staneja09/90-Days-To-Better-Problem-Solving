# Even Numbers Query

## Problem Statement

You are given an array of integers.

For each query `[L, R]`, determine the number of **even elements** in that range (inclusive).

---

## Input Format

* First line contains two integers `N` and `Q`.
* Second line contains `N` integers: `A1 A2 ... AN`.
* Next `Q` lines each contain two integers `L` and `R`.

---

## Output Format

For each query, print the number of even elements in the range `[L, R]` on a new line.

---

## Constraints

* `1 ≤ N, Q ≤ 2 × 10^5`
* `1 ≤ Ai ≤ 10^9`
* `1 ≤ L ≤ R ≤ N`

---

## Examples

### Example 1

#### Input

```text
6 3
2 7 4 9 10 5
1 6
2 5
3 4
```

#### Output

```text
3
2
1
```

### Explanation

* Query `(1,6)` → `2, 7, 4, 9, 10, 5` → 3 even numbers
* Query `(2,5)` → `7, 4, 9, 10` → 2 even numbers
* Query `(3,4)` → `4, 9` → 1 even number

---

### Example 2

#### Input

```text
5 2
1 3 5 7 9
1 5
2 4
```

#### Output

```text
0
0
```

### Explanation

All numbers are odd, so every query returns `0`.

---

### Example 3

#### Input

```text
8 3
2 4 6 8 10 12 14 16
1 8
3 6
5 5
```

#### Output

```text
8
4
1
```

---

## Approach

Use a **Prefix Count Array** to store the cumulative count of even numbers.

Let:

```text
prefix[i] = Number of even elements in A[1...i]
```

Then the number of even elements in any range `[L, R]` is:

```text
Even Count = prefix[R] - prefix[L - 1]
```

This allows each query to be answered in constant time.

### Steps

1. Create a prefix array of size `N + 1`.
2. Traverse the array:

   * If the current element is even, increment the count.
   * Otherwise, copy the previous count.
3. For every query:

   * Compute `prefix[R] - prefix[L - 1]`.
   * Output the result.

---

## Complexity Analysis

| Operation          | Complexity |
| ------------------ | ---------- |
| Build Prefix Array | O(N)       |
| Each Query         | O(1)       |
| Total              | O(N + Q)   |

### Space Complexity

```text
O(N)
```

---

## Reference Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    long long N, Q;
    cin >> N >> Q;

    vector<long long> prefix(N + 1, 0);

    for (long long i = 1; i <= N; i++) {
        int X;
        cin >> X;

        if (X % 2 == 0) {
            prefix[i] = prefix[i - 1] + 1;
        } else {
            prefix[i] = prefix[i - 1];
        }
    }

    while (Q--) {
        long long L, R;
        cin >> L >> R;

        cout << prefix[R] - prefix[L - 1] << '\n';
    }

    return 0;
}
```

---

## Key Insight

Instead of checking every element in the range for each query, store the cumulative count of even numbers using a prefix array. This reduces query time from **O(N)** to **O(1)** and efficiently handles up to **2 × 10^5** queries.
