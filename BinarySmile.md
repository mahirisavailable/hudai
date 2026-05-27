# Editorial — Binary Smile

Let:

* (A) = initial binary string
* (B) = target binary string

Operation:

* Choose any substring containing **at most one `'1'`**
* Reverse it

---

## Key Observation

Reversing a substring with at most one `'1'` only moves a single `'1'` across surrounding zeroes.

So:

* The relative order of all `'1'`s never changes.
* Number of `'1'`s must remain same.

If count of `'1'` differs in (A) and (B), answer is `-1`.

---

## Important Insight

Let:

- positions of `'1'` in `A` be: \(p_1, p_2, \dots\)
- positions of `'1'` in `B` be: \(q_1, q_2, \dots\)

Since one operation can only move a single `'1'` across zeroes:

- the relative order of `'1'`s never changes
- total number of `'1'`s must remain equal

If \(p_i \ne q_i\), then the \(i\)-th `'1'` must be moved once.

Therefore:

\[
\text{minimum operations}
=
\#\{i \mid p_i \ne q_i\}
\]

---

# Simpler Characterization

While scanning left to right:

Maintain:

* `c1` = number of `'1'` seen in (A)
* `c2` = number of `'1'` seen in (B)

Whenever:

* current char in (A) is `'1'`
* and either:

  * current char in (B) is `'0'`, or
  * prefix counts become equal

we count one operation.

This is exactly what the given code does.

---

# Algorithm

For each test case:

1. Count total `'1'`s in both strings.
2. If unequal → print `-1`
3. Otherwise scan the strings:

   * maintain prefix counts
   * count required movements
4. Print answer.

---

# Correctness

Each operation can reposition exactly one `'1'`.

Every misplaced `'1'` must be moved at least once.

Also, each misplaced `'1'` can always be moved independently using a substring containing only that `'1'`.

Hence minimum operations equals number of misplaced `'1'`s.

---

# Complexity

`O(N)` per test case.

---

# Reference Code

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
        string a, b;

        cin >> n >> a >> b;

        for (int i = 0; i < n; i++) {
            if (a[i] == '1') c1++;
            if (b[i] == '1') c2++;

            if (a[i] == '1' && (b[i] == '0' || c1 != c2))
                ans++;
        }

        cout << (c1 == c2 ? ans : -1) << '\n';
    }
}
```
