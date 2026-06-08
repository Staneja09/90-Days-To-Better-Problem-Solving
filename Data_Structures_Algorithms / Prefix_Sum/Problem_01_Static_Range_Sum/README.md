# Static Range Sum

## Problem Statement

You are given an array **A** of **N** integers.

You need to answer **Q** queries. Each query contains two integers **L** and **R** (`1 ≤ L ≤ R ≤ N`).

For each query, output the sum of elements from index **L** to **R** (inclusive).

---

## Input Format

- First line contains two integers `N` and `Q`.
- Second line contains `N` integers: `A1 A2 ... AN`.
- Next `Q` lines each contain two integers `L` and `R`.

---

## Output Format

For each query, print the sum of elements in the range `[L, R]` on a new line.

---

## Constraints

- `1 ≤ N, Q ≤ 2 × 10^5`
- `-10^9 ≤ Ai ≤ 10^9`
- `1 ≤ L ≤ R ≤ N`

---

## Examples

### Example 1

#### Input
```text
5 3
1 2 3 4 5
1 3
2 5
4 4
```

#### Output
```text
6
14
4
```

### Explanation

- Query `(1,3)` → `1 + 2 + 3 = 6`
- Query `(2,5)` → `2 + 3 + 4 + 5 = 14`
- Query `(4,4)` → `4`

---

### Example 2

#### Input
```text
6 4
5 -2 7 1 3 -4
1 6
2 4
3 3
5 6
```

#### Output
```text
10
6
7
-1
```

---

### Example 3

#### Input
```text
4 2
100 100 100 100
1 4
2 3
```

#### Output
```text
400
200
```

---

## Approach

Since the array is static (no updates), we can preprocess the array using a **Prefix Sum** array.

Let:

```text
prefix[i] = A[1] + A[2] + ... + A[i]
```

Then the sum of any range `[L, R]` can be computed in **O(1)**:

```text
Range Sum = prefix[R] - prefix[L - 1]
```

### Steps

1. Build the prefix sum array.
2. For each query:
   - Compute `prefix[R] - prefix[L - 1]`.
   - Output the result.

---

## Complexity Analysis

| Operation | Complexity |
|------------|------------|
| Build Prefix Sum | O(N) |
| Each Query | O(1) |
| Total | O(N + Q) |

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
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int N, Q;
    cin >> N >> Q;

    vector<long long> prefix(N + 1, 0);

    for (int i = 1; i <= N; i++) {
        long long x;
        cin >> x;
        prefix[i] = prefix[i - 1] + x;
    }

    while (Q--) {
        int L, R;
        cin >> L >> R;

        cout << prefix[R] - prefix[L - 1] << '\n';
    }

    return 0;
}
```

---

## Key Insight

Using a prefix sum array allows each range sum query to be answered in **constant time**, making it efficient for large values of `N` and `Q` (up to `2 × 10^5`).
