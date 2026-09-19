# Logic Minimizer & K-Map

Minimise a Boolean function to its exact smallest sum-of-products or product-of-sums, with a live Karnaugh map, prime implicant list, and C / Verilog / VHDL export. Runs entirely in your browser.

**Live:** <https://logic-minimizer.slippylabs.com/>

## What it does

- Type an expression (`A B' + C(A + D)`, `a & !b | c ^ d`, `(A XOR B) AND NOT C`) or click cells on the Karnaugh map — the two stay in sync.
- 2 to 6 variables, with don't-cares: click a cell to cycle it `0 → 1 → X`.
- Minimal SOP and minimal POS, the prime implicant list with the essential ones marked, and gate/literal counts for both forms.
- Export to C, Verilog, VHDL, Python or plain maths notation.

## How it works

Quine–McCluskey finds every prime implicant, then a **branch-and-bound exact cover** picks a provably minimal one — fewest product terms first, then fewest literals. Most tools stop at a greedy cover, which is often but not always minimal; the difference shows on cyclic maps with no essential prime implicants (there is a preset for one). If the search ever exhausts its node budget the page says so instead of claiming minimality.

The POS form is the SOP minimisation of the complement with De Morgan applied once, so it is minimal by the same argument.

Every result is re-evaluated against all 2^n rows of the truth table before it is displayed.

## Verification

The shipped code was checked against an independent reference: prime implicants recomputed by enumerating all 3^n cubes and keeping the maximal ones, and minimality re-solved as a set-cover MILP with HiGHS (`scipy.optimize.milp`). It agrees on **every one of the 65,536 functions of four variables**, on all 2- and 3-variable functions, and on random 5- and 6-variable functions with don't-cares.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/logic-minimizer.slippylabs.com.git
cd logic-minimizer.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
