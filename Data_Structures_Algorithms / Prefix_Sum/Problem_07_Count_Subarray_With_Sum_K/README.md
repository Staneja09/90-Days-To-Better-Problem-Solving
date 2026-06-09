# Count Subarrays With Sum K

## Problem Statement

Given an array of `N` integers and an integer `K`, count the number of subarrays whose sum is exactly `K`.

A subarray is a contiguous segment of the array.

---

## Input Format

* First line contains two integers `N` and `K`.
* Second line contains `N` integers:

```text
A1 A2 ... AN
```

representing the array elements.

---

## Output Format

Print the number of subarrays whose sum equals `K`.

---

## Constraints

* `1 ≤ N ≤ 2 × 10^5`
* `-10^9 ≤ Ai ≤ 10^9`
* `-10^14 ≤ K ≤ 10^14`

---

## Examples

### Example 1

#### Input

```text
5 3
1 2 1 1 2
```

#### Output

```text
3
```

#### Explanation

The valid subarrays are:

```text
[1, 2]
[2, 1]
[1, 1, 1]
```

All of them have sum equal to:

```text
3
```

---

## Approach

A brute force solution would consider every possible subarray and compute its sum.

There are:

```text
O(N²)
```

possible subarrays, which is too slow for:

```text
N ≤ 2 × 10^5
```

Instead, use a combination of **Prefix Sum** and **Hash Map**.

Let:

```text
prefix[i]
```

represent the sum of elements from index `1` to `i`.

Suppose the current prefix sum is:

```text
prefix
```

and we want a subarray ending at the current position whose sum equals `K`.

Then:

```text
prefix - previousPrefix = K
```

Rearranging:

```text
previousPrefix = prefix - K
```

So, for every position, we need to know how many times the value:

```text
prefix - K
```

has appeared before.

A hash map stores the frequency of every prefix sum encountered so far.

---

## Key Observation

If:

```text
currentPrefix = prefix
```

then every previous occurrence of:

```text
prefix - K
```

forms a valid subarray ending at the current position.

Therefore:

```text
answer += frequency[prefix - K]
```

---

## Dry Run

Consider:

```text
Array = [1, 2, 1, 1, 2]
K = 3
```

Initialize:

```text
freq[0] = 1
prefix = 0
answer = 0
```

### Element = 1

```text
prefix = 1
need = 1 - 3 = -2
```

Not found.

```text
freq[1]++
```

### Element = 2

```text
prefix = 3
need = 3 - 3 = 0
```

```text
answer += freq[0] = 1
```

Valid subarray:

```text
[1, 2]
```

### Element = 1

```text
prefix = 4
need = 4 - 3 = 1
```

```text
answer += freq[1] = 1
```

Valid subarray:

```text
[2, 1]
```

### Element = 1

```text
prefix = 5
need = 5 - 3 = 2
```

No match.

### Element = 2

```text
prefix = 7
need = 7 - 3 = 4
```

```text
answer += freq[4] = 1
```

Valid subarray:

```text
[1, 1, 1]
```

Final answer:

```text
3
```

---

## Why Negative Numbers Matter

When all numbers are positive, techniques like Sliding Window can work.

However, this problem allows:

```text
Ai < 0
```

which means the window sum is not monotonic.

Therefore, Sliding Window fails.

Prefix Sum + Hash Map works correctly for both positive and negative values.

---

## Complexity Analysis

| Operation           | Complexity   |
| ------------------- | ------------ |
| Traverse Array      | O(N)         |
| Hash Map Operations | O(1) Average |
| Total               | O(N)         |

### Space Complexity

```text
O(N)
```

for storing prefix sum frequencies.

---

## Reference Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    ll N, K;
    cin >> N >> K;

    vector<ll> arr(N);

    for (ll i = 0; i < N; i++) {
        cin >> arr[i];
    }

    map<ll, ll> freq;

    freq[0]++;

    ll prefix = 0;
    ll ans = 0;

    for (ll x : arr) {
        prefix += x;

        ll need = prefix - K;

        if (freq.count(need)) {
            ans += freq[need];
        }

        freq[prefix]++;
    }

    cout << ans << endl;
}
```

---

## Key Insight

For every position, a subarray ending there has sum `K` if a previous prefix sum equal to:

```text
currentPrefix - K
```

exists.

By storing the frequency of all previous prefix sums in a hash map, we can count all valid subarrays in a single pass, achieving:

```text
O(N)
```

time complexity even for arrays containing negative numbers.
