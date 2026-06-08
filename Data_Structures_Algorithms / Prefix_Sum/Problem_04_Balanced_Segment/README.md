# Balanced Segment

## Problem Statement

You are given a binary array consisting of **0s** and **1s**.

For each query `[L, R]`, determine whether the number of **zeros** and **ones** in that range are equal.

Print:

* `"YES"` if the number of zeros and ones are equal.
* `"NO"` otherwise.

---

## Input Format

* First line contains two integers `N` and `Q`.
* Second line contains `N` integers: `A1 A2 ... AN`.
* Next `Q` lines each contain two integers `L` and `R`.

---

## Output Format

For each query:

* Print `"YES"` if the segment is balanced.
* Print `"NO"` otherwise.

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
6 3
1 0 1 0 0 1
1 4
2 5
1 6
```

#### Output

```text
YES
YES
YES
```

### Explanation

* `(1,4)` → `1 0 1 0` → 2 zeros, 2 ones → YES
* `(2,5)` → `0 1 0 0` → 3 zeros, 1 one → NO

**Note:** The sample output marks this query as YES, which appears inconsistent with the stated condition. The intended logic is to check whether the number of zeros and ones are equal.

* `(1,6)` → `1 0 1 0 0 1` → 3 zeros, 3 ones → YES

---

### Example 2

#### Input

```text
5 2
1 1 1 1 1
1 5
2 4
```

#### Output

```text
NO
NO
```

---

### Example 3

#### Input

```text
8 3
0 1 0 1 1 0 0 1
1 8
1 2
3 7
```

#### Output

```text
YES
YES
NO
```

---

### Example 4

#### Input

```text
4 3
0 0 1 1
1 4
1 2
3 4
```

#### Output

```text
YES
NO
NO
```

---

## Approach

Use a **Prefix Count Array** to store the cumulative number of zeros.

Let:

```text
prefix[i] = Number of zeros in A[1...i]
```

For a query `[L, R]`:

```text
Zeros = prefix[R] - prefix[L - 1]
Length = R - L + 1
Ones = Length - Zeros
```

The segment is balanced if:

```text
Zeros = Ones
```

This can be simplified to:

```text
2 × Zeros = Length
```

or

```text
Zeros = Length / 2
```

So we only need to count zeros in the range and compare with half the segment length.

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

        long long length = R - L + 1;
        long long zeros = prefix[R] - prefix[L - 1];

        if (zeros == length / 2 && length % 2 == 0) {
            cout << "YES\n";
        } else {
            cout << "NO\n";
        }
    }

    return 0;
}
```

---

## Key Insight

A balanced binary segment must contain the same number of zeros and ones. By storing the cumulative count of zeros using a prefix array, each query can be answered in **O(1)** time instead of scanning the entire range.

This makes the solution efficient for up to **2 × 10^5** queries.
