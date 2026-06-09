# Longest Subarray With Equal Even and Odd Numbers

## Problem Statement

Given an array of integers.

Find the length of the longest contiguous subarray containing an equal number of even and odd elements.

---

## Input Format

* First line contains an integer `N`.
* Second line contains `N` integers:

```text
A1 A2 ... AN
```

representing the array.

---

## Output Format

Print the length of the longest valid subarray.

---

## Constraints

* `1 ≤ N ≤ 2 × 10^5`
* `1 ≤ Ai ≤ 10^9`

---

## Examples

### Example 1

#### Input

```text
8
1 2 4 5 6 7 8 10
```

#### Output

```text
6
```

#### Explanation

Transform the array as:

```text
odd  -> -1
even -> +1
```

Original array:

```text
1 2 4 5 6 7 8 10
```

Transformed array:

```text
-1 +1 +1 -1 +1 -1 +1 +1
```

The longest subarray having sum `0` has length:

```text
6
```

which corresponds to a subarray containing an equal number of even and odd elements.

---

## Approach

A subarray contains an equal number of even and odd elements if:

```text
countEven = countOdd
```

This condition can be rewritten using a transformation.

Replace:

```text
even -> +1
odd  -> -1
```

Then for any subarray:

```text
sum = (+1 × countEven) + (-1 × countOdd)
```

which becomes:

```text
sum = countEven - countOdd
```

Therefore:

```text
countEven = countOdd
```

if and only if

```text
sum = 0
```

So the problem reduces to:

> Find the length of the longest subarray whose sum is zero.

---

## Prefix Sum Observation

Let:

```text
prefix[i]
```

be the sum of transformed values from index `1` to `i`.

If the same prefix sum appears at two positions:

```text
j < i
```

then:

```text
prefix[i] - prefix[j] = 0
```

which means the subarray:

```text
(j + 1) → i
```

has sum zero.

Therefore, for each prefix sum, we only need to remember its first occurrence.

Whenever the same prefix appears again, we can compute a candidate answer:

```text
length = currentPosition - firstOccurrence
```

and update the maximum length.

---

## Dry Run

Consider:

```text
1 2 4 5 6 7 8 10
```

Transform:

```text
-1 +1 +1 -1 +1 -1 +1 +1
```

Initialize:

```text
firstPos[0] = 0
prefix = 0
answer = 0
```

| Position | Value | Prefix |
| -------- | ----- | ------ |
| 1        | -1    | -1     |
| 2        | +1    | 0      |
| 3        | +1    | 1      |
| 4        | -1    | 0      |
| 5        | +1    | 1      |
| 6        | -1    | 0      |
| 7        | +1    | 1      |
| 8        | +1    | 2      |

Processing repeated prefix sums:

### Prefix = 0 at Position 2

```text
length = 2 - 0 = 2
```

### Prefix = 0 at Position 4

```text
length = 4 - 0 = 4
```

### Prefix = 1 at Position 5

First appeared at position 3.

```text
length = 5 - 3 = 2
```

### Prefix = 0 at Position 6

```text
length = 6 - 0 = 6
```

Maximum becomes:

```text
6
```

---

## Why the Transformation Works

Suppose a subarray contains:

```text
4 even numbers
4 odd numbers
```

After transformation:

```text
(+1 × 4) + (-1 × 4)
=
0
```

Similarly, every subarray with equal counts of even and odd elements produces a zero sum.

Thus, finding the longest balanced even-odd subarray is equivalent to finding the longest zero-sum subarray.

---

## Complexity Analysis

| Operation       | Complexity |
| --------------- | ---------- |
| Array Traversal | O(N)       |
| Map Operations  | O(log N)   |
| Total           | O(N log N) |

### Space Complexity

```text
O(N)
```

for storing the first occurrence of prefix sums.

---

## Reference Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    ll N;
    cin >> N;

    map<ll, ll> firstPos;

    firstPos[0] = 0;

    ll prefix = 0;
    ll ans = 0;

    for (ll i = 1; i <= N; i++) {
        ll x;
        cin >> x;

        if (x % 2 == 0) {
            prefix += 1;
        } else {
            prefix -= 1;
        }

        if (firstPos.count(prefix)) {
            ans = max(ans, i - firstPos[prefix]);
        } else {
            firstPos[prefix] = i;
        }
    }

    cout << ans << endl;
}
```

---

## Key Insight

Convert the problem into a longest zero-sum subarray problem by replacing:

```text
even -> +1
odd  -> -1
```

Then equal counts of even and odd numbers correspond exactly to a subarray whose transformed sum is zero.

By storing the first occurrence of every prefix sum, the longest valid subarray can be found efficiently in:

```text
O(N log N)
```

time.
