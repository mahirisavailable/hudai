# Binary Smile — Editorial

## Key Observations

**1. Impossible case**
If the total count of `1`s in `A` ≠ total count of `1`s in `B`, output `-1`.

**2. Each operation moves exactly one `1`**
Reversing a substring with at most one `1` repositions that single `1` without crossing any other `1`. So the relative order of `1`s is preserved — the *i*-th `1` in `A` must map to the *i*-th `1` in `B`.

**3. Counting misplaced `1`s**
A `1` at position `i` in `A` needs to move if and only if the prefix count of `1`s in `A` up to `i` exceeds the prefix count of `1`s in `B` up to `i` — meaning this `1` in `A` has no matching `1` in `B` at or before position `i`.

This is captured by the condition:
```
A[i] == '1'  &&  (B[i] == '0'  ||  prefixOnes(A, i) != prefixOnes(B, i))
```

Each such `1` needs exactly one operation to reach its target, and these operations are independent (each touches only one `1`).

**Answer = number of positions satisfying the condition above.**

---

## Complexity

- **Time:** O(N) per test case  
- **Space:** O(N)

---

## Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;
    while (t--) {
        int n, c1 = 0, c2 = 0, ans = 0;
        string ss1, ss2;
        cin >> n >> ss1 >> ss2;

        for (int i = 0; i < n; i++) {
            if (ss1[i] == '1') c1++;
            if (ss2[i] == '1') c2++;
            if (ss1[i] == '1' && (ss2[i] == '0' || c1 != c2))
                ans++;
        }

        cout << (c1 == c2 ? ans : -1) << '\n';
    }
    return 0;
}
```

---

## Example Trace

`A = "110"`, `B = "011"`

| i | A[i] | B[i] | c1 | c2 | Condition | ans |
|---|------|------|----|----|-----------|-----|
| 0 | 1    | 0    | 1  | 0  | `'1' && ('0' \|\| 1≠0)` → ✓ | 1 |
| 1 | 1    | 1    | 2  | 1  | `'1' && (false \|\| 2≠1)` → ✓ | 2 |
| 2 | 0    | 1    | 2  | 2  | `'0' && ...` → ✗ | 2 |

Output: **2** ✓
