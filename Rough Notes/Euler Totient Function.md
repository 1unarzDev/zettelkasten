---
aliases:
tags:
  - Seed
references:
  - "[Stanford Cryptography Number Theory](https://crypto.stanford.edu/pbc/notes/numbertheory/units.html)"
---
[[Math]], [[Number Theory]]
2026-07-15 01:14

Euler's totient function (which finds the number of coprime numbers less than a given number) is multiplicative for coprime numbers (as in $\phi(mn)=\phi(m)\phi(n)$ where $m$ and $n$ share no factors other than 1) due to the Chinese remainder theorem. This can be seen algebraic fairly easily; however, a more complete proof is more difficult.

if the prime factorization of a number $n$ is $n=p_1^{e_1}p_2^{e_2}\dots p_k^{e_k}$, then the formula is: $$\phi(n)=n(1-\frac{1}{p_1})(1-\frac{1}{p_2})\dots (1-\frac{1}{p_k})$$
When solving for $\phi(mn)$, we can take advantage of the definition of coprime numbers. According to the definition of coprime numbers, $m$ and $n$ share no factors other than 1, so the prime factorization of $mn$ will equal the prime factorization of $m$ and $n$ multiplied. We can intuitively see that all components of the solutions for both $\phi(m)$ and $\phi(n)$ are retained, so our original statement holds true.

