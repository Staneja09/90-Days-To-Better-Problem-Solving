# Number of Zeros in Range

## Problem Statement

You are given a binary array consisting only of **0s** and **1s**.

For each query `[L, R]`, determine how many **zeros** appear in that range (inclusive).

---

## Input Format

* First line contains two integers `N` and `Q`.
* Second line contains `N` integers: `A1 A2 ... AN`.
* Next `Q` lines each contain two integers `L` and `R`.

---

## Output Format

For each query, print the number of zeros in the range `[L, R]` on a new line.

---

## Constraints

* `1 ≤ N, Q ≤ 2 × 10^5`
* `Ai ∈ {0,1}`
* `1 ≤ L ≤ R ≤ N`

---

## Examples

### Example 1

#### Input

```text
8 3
1 0 0 1 1 0 1 0
1 4
3 8
5 7
```

#### Output

```text
2
3
1
```

### Explanation

* Query `(1,4)` → `1 0 0 1` → 2 zeros
* Query `(3,8)` → `0 1 1 0 1 0` → 3 zeros
* Query `(5,7)` → `1 0 1` → 1 zero

---

### Example 2

#### Input

```text
5 2
0 0 0 0 0
1 5
2 4
```

#### Output

```text
5
3
```

---

### Example 3

#### Input

```text
6 3
1 1 1 1 1 1
1 6
2 5
4 4
```

#### Output

```text
0
0
0
```

---

## Approach

Use a **Prefix Count Array** to store the cumulative number of zeros.

Let:

```text
prefix[i] = Number of zeros in A[1...i]
```

Then the number of zeros in any range `[L, R]` can be calculated as:

```text
Zeros in Range = prefix[R] - prefix[L - 1]
```

This allows answering each query in constant time.

### Steps

1. Initialize a prefix array of size `N + 1`.
2. Traverse the array:

   * If the current element is `0`, increase the zero count.
   * Otherwise, carry forward the previous count.
3. For each query:

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

        if (X == 0) {
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

Instead of counting zeros separately for every query, precompute the cumulative count of zeros using a prefix array. This reduces query processing time from **O(N)** to **O(1)**, making the solution efficient for up to `2 × 10^5` queries.
