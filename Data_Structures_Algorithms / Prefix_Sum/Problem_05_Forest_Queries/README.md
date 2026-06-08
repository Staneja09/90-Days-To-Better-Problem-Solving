# Forest Queries

## Problem Statement

You are given an `N × N` grid.

Each cell contains either:

* `'*'` → Tree
* `'.'` → Empty

You need to answer `Q` queries.

Each query provides:

```text
r1 c1 r2 c2
```

Determine the number of trees inside the rectangle whose corners are:

```text
Top-left     = (r1, c1)
Bottom-right = (r2, c2)
```

---

## Input Format

* First line contains two integers `N` and `Q`.
* Next `N` lines contain the grid.
* Next `Q` lines contain four integers:

```text
r1 c1 r2 c2
```

representing a query rectangle.

---

## Output Format

For each query, print the number of trees inside the specified rectangle.

---

## Constraints

* `1 ≤ N ≤ 1000`
* `1 ≤ Q ≤ 2 × 10^5`
* Grid cell is either `'*'` or `'.'`
* `1 ≤ r1 ≤ r2 ≤ N`
* `1 ≤ c1 ≤ c2 ≤ N`

---

## Examples

### Example 1

#### Input

```text
4 3
*..*
....
.**.
*...
1 1 4 4
2 2 3 3
1 4 4 4
```

#### Output

```text
5
2
1
```

---

### Example 2

#### Input

```text
3 2
***
***
***
1 1 3 3
2 2 3 3
```

#### Output

```text
9
4
```

---

### Example 3

#### Input

```text
5 3
.....
.....
.....
.....
.....
1 1 5 5
2 2 4 4
3 3 3 3
```

#### Output

```text
0
0
0
```

---

### Example 4

#### Input

```text
4 2
*.*.
.*.*
*.*.
.*.*
1 1 4 4
1 1 2 2
```

#### Output

```text
8
2
```

---

## Approach

Since there can be up to `2 × 10^5` queries, checking every cell inside a rectangle for each query would be too slow.

Instead, build a **2D Prefix Sum Array**.

Let:

```text
prefix[i][j]
=
Number of trees in rectangle
(1,1) → (i,j)
```

### Building the Prefix Sum

For every cell:

```text
prefix[i][j]
=
tree(i,j)
+ prefix[i-1][j]
+ prefix[i][j-1]
- prefix[i-1][j-1]
```

where:

```text
tree(i,j) = 1 if grid[i][j] == '*'
            0 otherwise
```

---

## Answering Queries

For a rectangle:

```text
(r1,c1) → (r2,c2)
```

the number of trees is:

```text
prefix[r2][c2]
- prefix[r1-1][c2]
- prefix[r2][c1-1]
+ prefix[r1-1][c1-1]
```

This is the standard **2D Inclusion-Exclusion Principle**.

---

## Complexity Analysis

| Operation        | Complexity |
| ---------------- | ---------- |
| Build Prefix Sum | O(N²)      |
| Each Query       | O(1)       |
| Total            | O(N² + Q)  |

### Space Complexity

```text
O(N²)
```

---

## Reference Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    long long N, Q;
    cin >> N >> Q;

    vector<vector<long long>> prefix(
        N + 1,
        vector<long long>(N + 1, 0)
    );

    for (long long i = 1; i <= N; i++) {
        for (long long j = 1; j <= N; j++) {
            char X;
            cin >> X;

            if (X == '*') {
                prefix[i][j] =
                    1
                    + prefix[i - 1][j]
                    + prefix[i][j - 1]
                    - prefix[i - 1][j - 1];
            } else {
                prefix[i][j] =
                    prefix[i - 1][j]
                    + prefix[i][j - 1]
                    - prefix[i - 1][j - 1];
            }
        }
    }

    while (Q--) {
        long long r1, c1, r2, c2;
        cin >> r1 >> c1 >> r2 >> c2;

        cout
            << prefix[r2][c2]
             - prefix[r1 - 1][c2]
             - prefix[r2][c1 - 1]
             + prefix[r1 - 1][c1 - 1]
            << '\n';
    }

    return 0;
}
```

---

## Key Insight

A 2D prefix sum stores the number of trees from `(1,1)` to `(i,j)`.

Using inclusion-exclusion, any rectangular query can be answered in **O(1)** time, making the solution efficient even for **200,000 queries**.
