# Binary Smile — Editorial

## The Core Idea

You can only reverse a substring with **at most one `1`**. This means you can never move a `1` past another `1`. So **the 1s must stay in their original order** — the 1st `1` in A goes to where the 1st `1` is in B, the 2nd goes to the 2nd, and so on.

If A and B have a different number of `1`s → impossible, print `-1`.

---

## How Many Operations?

Each misplaced `1` costs exactly **1 operation** to fix (just reverse a range containing only that `1`).

So the answer is simply: **how many `1`s in A are not already at their correct target position in B?**

---

## How the Code Detects This in One Pass

Instead of extracting positions separately, the code tracks a running count of `1`s seen so far in A (`c1`) and B (`c2`).

At any position `i` where `A[i] = '1'`:
- If `B[i] = '0'` → this `1` clearly isn't where it should be.
- If `B[i] = '1'` but `c1 ≠ c2` → the prefix counts are out of sync, meaning this `1` in A doesn't actually line up with a `1` in B at the same index.

Either way, that `1` needs to move → increment `ans`.

---

## Example

`A = "110"`, `B = "011"`

- 1st `1` of A is at index 0, 1st `1` of B is at index 1 → mismatch → needs 1 op  
- 2nd `1` of A is at index 1, 2nd `1` of B is at index 2 → mismatch → needs 1 op  

**Answer: 2**

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
