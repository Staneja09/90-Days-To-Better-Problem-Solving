# Range Update and Point Query

## Problem Statement

You are given an array of size `N`, initially containing all zeros.

You need to process `Q` operations of two types.

### Type 1

```text
1 L R X
```

Add `X` to every element in the range:

```text
[L, R]
```

### Type 2

```text
2 P
```

Output the value at position `P`.

---

## Input Format

* First line contains two integers `N` and `Q`.
* Next `Q` lines contain operations.

For an update operation:

```text
1 L R X
```

For a query operation:

```text
2 P
```

---

## Output Format

For every Type 2 query, print the value at position `P`.

---

## Constraints

* `1 ≤ N, Q ≤ 2 × 10^5`
* `1 ≤ L ≤ R ≤ N`
* `1 ≤ P ≤ N`
* `1 ≤ X ≤ 10^9`

---

## Examples

### Example 1

#### Input

```text
5 5
1 1 3 2
1 2 5 1
2 1
2 4
2 5
```

#### Output

```text
2
1
1
```

---

## Explanation

Initially:

```text
0 0 0 0 0
```

After:

```text
1 1 3 2
```

Array becomes:

```text
2 2 2 0 0
```

After:

```text
1 2 5 1
```

Array becomes:

```text
2 3 3 1 1
```

Queries:

```text
Position 1 = 2
Position 4 = 1
Position 5 = 1
```

---

## Approach

A naive solution would update every element in the range `[L, R]`.

For one update:

```text
O(R - L + 1)
```

With up to:

```text
Q = 2 × 10^5
```

operations, this can become:

```text
O(N × Q)
```

which is too slow.

Instead, use a **Difference Array**.

Let:

```text
diff[i]
```

represent the change that starts at position `i`.

For an update:

```text
[L, R] += X
```

perform:

```text
diff[L] += X
diff[R + 1] -= X
```

This marks where the addition begins and where it ends.

---

## Key Observation

The actual value at any position `P` is simply the prefix sum of the difference array up to `P`.

```text
value(P)
=
diff[1] + diff[2] + ... + diff[P]
```

Therefore, whenever a query asks for position `P`, we compute:

```text
prefixSum(1...P)
```

and output it.

---

## Dry Run

Consider:

```text
N = 5
```

Operations:

```text
1 1 3 2
1 2 5 1
```

Initially:

```text
diff = [0 0 0 0 0 0]
```

After:

```text
1 1 3 2
```

```text
diff[1] += 2
diff[4] -= 2
```

```text
diff = [2 0 0 -2 0 0]
```

After:

```text
1 2 5 1
```

```text
diff[2] += 1
diff[6] -= 1
```

```text
diff = [2 1 0 -2 0 -1]
```

### Query Position 1

Prefix sum:

```text
2
```

Answer:

```text
2
```

### Query Position 4

Prefix sum:

```text
2 + 1 + 0 - 2
=
1
```

Answer:

```text
1
```

### Query Position 5

Prefix sum:

```text
2 + 1 + 0 - 2 + 0
=
1
```

Answer:

```text
1
```

---

## Why This Works

A Difference Array stores only the boundaries of updates.

For every range update:

```text
diff[L] += X
diff[R+1] -= X
```

The prefix sum reconstruction automatically applies `X` to every position between `L` and `R`.

Thus:

```text
prefix(diff)[i]
```

always equals the actual value at index `i`.

---

## Complexity Analysis

### Given Implementation

For each update:

```text
O(1)
```

For each query:

```text
O(P)
```

because the prefix sum is recomputed from the beginning every time.

Worst case:

```text
O(N)
```

per query.

Total complexity:

```text
O(Q × N)
```

in the worst case.

### Space Complexity

```text
O(N)
```

for the difference array.

---

## Note

The current implementation correctly demonstrates the Difference Array concept, but it is not optimal for the given constraints.

To support:

```text
N, Q ≤ 2 × 10^5
```

efficiently, the Difference Array should be combined with a data structure such as a **Fenwick Tree (Binary Indexed Tree)** or **Segment Tree**, allowing:

```text
Range Update  -> O(log N)
Point Query   -> O(log N)
```

---

## Reference Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

void update(long long L, long long R, long long X,
            vector<long long>& diff) {
    diff[L] += X;
    diff[R + 1] -= X;
}

int main() {
    long long N, Q;
    cin >> N >> Q;

    vector<long long> diff(N + 2, 0);

    while (Q--) {
        int type;
        cin >> type;

        if (type == 1) {
            long long L, R, X;
            cin >> L >> R >> X;

            update(L, R, X, diff);
        }
        else {
            long long P;
            cin >> P;

            long long ans = 0;

            for (long long i = 1; i <= P; i++) {
                ans += diff[i];
            }

            cout << ans << '\n';
        }
    }

    return 0;
}
```

---

## Key Insight

A Difference Array allows a range addition to be represented using only two updates:

```text
diff[L] += X
diff[R+1] -= X
```

The value at any position is obtained by taking the prefix sum up to that position.

This transforms expensive range updates into constant-time operations while preserving the final array values.
