# Cosmological_Principle.sh

A compact self-decoding Turing-path model of the cosmological principle.

## Formal model

This repository uses a finite 1D cyclic byte lattice as a deliberately minimal, exact model. A state `s` satisfies the discrete cosmological principle iff it is invariant under translation and reflection:

- homogeneity: `s` is unchanged by cyclic translation;
- isotropy: `s` is unchanged by reversal.

For this model the complete solution set is known exactly:

- `F_0 = {ε}`;
- `F_k = { b^k : b in {0,...,255} }` for every `k >= 1`.

Thus the program enumerates solutions directly instead of brute-force filtering all byte strings.

For flat path index `n >= 1`:

```text
k = floor((n-1)/256) + 1
b = (n-1) mod 256
s = b repeated k times
```

Each byte `b` defines an infinite continuation branch:

```text
b -> bb -> bbb -> ...
```

The empty root has 256 admissible branches, so its formal branching entropy is `log2(256)=8` bits. After choosing a branch, continuation preserving `b` is unique, so the branch entropy is `0`.

`H = k+1` and `ETA = 1/H` provide an inverse continuation-scale coordinate, with `ETA -> 0` as branch depth grows.

## Self-decoding

The Python core is a quine-style self representation. Its finite byte code `R` is assigned the exact length-lex index

```text
I(R) = (256^|R| - 1)/255 + int.from_bytes(R, "big")
```

and verified by decoding back to `R`. Run:

```bash
./Cosmological_Principle.sh self
```

Normal enumeration:

```bash
./Cosmological_Principle.sh 8 0
./Cosmological_Principle.sh 3 255
./Cosmological_Principle.sh 0 0   # unbounded enumeration until interrupted
```

## Scope

This is a formal discrete computational model of homogeneity/isotropy, not a claim that a 1D byte lattice is a physical cosmology or that the program uniquely determines the real universe. `OPEN=true` and `FINAL=false` preserve that distinction.
