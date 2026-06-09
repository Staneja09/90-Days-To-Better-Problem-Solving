# Range Additions

## Problem Statement

You are given an array `A` of size `N`, initially containing all zeros.

You need to process `Q` operations.

Each operation contains three integers:

```text
L R X
```

Add `X` to every element in the range:

```text
[L, R]
```

After performing all operations, output the final array.

---

## Input Format

* First line contains two integers `N` and `Q`.
* Next `Q` lines contain three integers:

```text
L R X
```

representing a range update operation.

---

## Output Format

Print the final array after all updates have been applied.

---

## Constraints

* `1 ≤ N ≤ 2 × 10^5`
* `1 ≤ Q ≤ 2 × 10^5`
* `1 ≤ L ≤ R ≤ N`
* `1 ≤ X ≤ 10^9`

---

## Examples

### Example 1

#### Input

```text
5 3
1 3 2
2 5 1
4 5 3
```

#### Output

```text
2 3 3 4 4
```

#### Explanation

After first update:

```text
[2 2 2 0 0]
```

After second update:

```text
[2 3 3 1 1]
```

After third update:

```text
[2 3 3 4 4]
```

---

## Approach

A naive solution would update every element inside the range `[L, R]` for each query.

For one operation:

```text
O(R - L + 1)
```

For `Q` operations, the worst-case complexity becomes:

```text
O(N × Q)
```

which is too slow for:

```text
N, Q ≤ 2 × 10^5
```

Instead, use a **Difference Array**.

Let:

```text
diff[i]
```

store the change that begins at position `i`.

For an update:

```text
[L, R] += X
```

perform:

```text
diff[L] += X
diff[R + 1] -= X
```

This marks where the addition starts and where it stops.

After processing all updates, compute the prefix sum of the difference array to obtain the final values.

---

## Building the Final Array

For every update:

```text
diff[L] += X
diff[R + 1] -= X
```

After all operations:

```text
A[i]
=
A[i-1] + diff[i]
```

or equivalently:

```text
prefix += diff[i]
```

where `prefix` stores the running sum.

---

## Dry Run

Consider:

```text
N = 5
```

Operations:

```text
1 3 2
2 5 1
4 5 3
```

Initially:

```text
diff = [0 0 0 0 0 0]
```

After:

```text
1 3 2
```

```text
diff = [2 0 0 -2 0 0]
```

After:

```text
2 5 1
```

```text
diff = [2 1 0 -2 0 -1]
```

After:

```text
4 5 3
```

```text
diff = [2 1 0 1 0 -4]
```

Now take prefix sums:

```text
2
2+1 = 3
3+0 = 3
3+1 = 4
4+0 = 4
```

Final array:

```text
2 3 3 4 4
```

---

## Complexity Analysis

| Operation         | Complexity |
| ----------------- | ---------- |
| Each Update       | O(1)       |
| All Updates       | O(Q)       |
| Build Final Array | O(N)       |
| Total             | O(N + Q)   |

### Space Complexity

```text
O(N)
```

---

## Reference Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

void update(ll L, ll R, ll Value, vector<ll>& diff) {
    diff[L] += Value;
    diff[R + 1] -= Value;
}

int main() {
    ll N, Q;
    cin >> N >> Q;

    vector<ll> diff(N + 2, 0);

    while (Q--) {
        ll L, R, Value;
        cin >> L >> R >> Value;

        update(L, R, Value, diff);
    }

    ll sum = 0;

    for (ll i = 1; i <= N; i++) {
        sum += diff[i];
        cout << sum << " ";
    }

    return 0;
}
```

---

## Key Insight

A Difference Array allows a range update to be performed in **O(1)** time by marking only the start and end of the affected interval.

After all updates, taking a prefix sum reconstructs the final array, resulting in an efficient overall complexity of:

```text
O(N + Q)
```

which easily handles up to **200,000 updates**.
