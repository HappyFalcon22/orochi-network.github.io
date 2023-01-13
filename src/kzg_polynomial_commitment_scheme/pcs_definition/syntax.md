# Syntax
A polynomial commitment scheme, for polynomials over a field \\(\mathbb{F}\\), is a tuple of \\(5\\) algorithms
\\[(\mathsf{Setup}, \mathsf{Commit}, \mathsf{VerifyPoly}, \mathsf{CreateWitness}, \mathsf{VerifyEval})\\]
working as follows:
- \\(\mathsf{Setup}(1^\kappa, t):\\) On inputs security parameter \\(1^\kappa\\) and a degree bound \\(t\\), this algorithm returns a commitment key \\(ck\\). The key \\(ck\\) allows to commit any polynomial in \\(\mathbb{F}[X]\\) whose degree is at most \\(t\\).
> Above we use the notation \\(\mathbb{F}[X]\\) to denote the field extension of \\(\mathbb{F}\\). For intuition, this field extension contains every polynomial of the form \\(f(X) = \sum_{j = 0}^{\deg(f)} c_j \cdot X^j\\) where \\(\deg(f)\\) is the degree of \\(f(X)\\) and \\(c_j \in \mathbb{F}\\) for all \\(j \in \\{0, \dots, \deg(f)\\}\\). Hence, with the algorithm \\(\mathsf{Setup}\\) on input \\(t\\), we can assume that it allows to commit to any polynomial \\(f\\) satisfying \\(\deg(f) \leq t\\). 
>
> We may have a question about what will happen if we try to use \\(ck\\) to commit to a polynomial whose degree is larger than \\(t\\). In this case, the execution and correctness of the algorithms below or the security of the scheme are not guaranteed.

- \\(\mathsf{Commit}\left(ck, f(X)\right):\\) On inputs commitment key \\(ck\\) and polynomial \\(f(X) \in \mathbb{F}[X]\\), this algorithm returns a commitment \\(c\\) and an opening (or decommitment) \\(d\\). We note that \\(f(X)\\) here is recommended to have degree at most \\(t\\) with respect to \\(ck\\) output by \\(\mathsf{Setup}(1^\kappa, t)\\).

- \\(\mathsf{VerifyPoly}(ck, f(X), c, d):\\) On inputs commitment key \\(ck\\), polynomial \\(f(X) \in \mathbb{F}[X]\\), commitment \\(c\\) and opening \\(d\\), this algorithm deterministically returns a bit \\(b \in \\{0, 1\\}\\) to specify whether \\(c\\) is a correct commitment to \\(f(X)\\). If \\(c\\) is such a correct commitment, then \\(b = 1\\). Otherwise, \\(b = 0\\).
> At this moment, we may wonder why \\(\mathsf{Commit}\\) does output both \\(c, d\\) and \\(\mathsf{VerifyPoly}\\) does use both \\(c, d\\). In fact, when participating in an interactive protocol, one party may commit to some secret by exposing commitment \\(c\\) to other parties. This commitment \\(c\\) guarantees that the secret behind, namely, polynomial \\(f(X)\\) in this case, is still secret, guaranteed the hiding property to be discussed later. 
>
> On the other hand, since we abuse the word **commit**, it means that the party publishing \\(c\\) has only one opening, namely, \\(d\\), to show that \\(c\\) is a correct commitment to \\(f(X)\\). It is extremely hard for this party to show that \\(c\\) is correct commitment to some other polynomial \\(f'(X) \not= f(X)\\). This is guaranteed by the binding property of the polynomial commitment scheme, to be discussed later. 
- \\(\mathsf{CreateWitness}(ck, f(X), i, d):\\) On inputs commitment key \\(ck\\), polynomial \\(f(X)\\), index \\(i\\) and opening \\(d\\), this algorithm returns a witness \\(w_i\\) to ensure that \\(c\\), related to opening \\(d\\), is commitment to \\(f(X)\\) whose evaluation at index \\(i\\) is equal to \\(f(i)\\).
> Let us explain in detail the use of \\(\mathsf{CreateWitness}\\). Assume that a party published \\(c\\) which is a commitment to \\(f(X)\\). It then publishes a point \\((i, v)\\) and claims that \\(f(i) = v\\) without exposing \\(f(X)\\). By using the algorithm \\(\mathsf{VerifyEval}\\) defined below, this claim is assured if \\(f(i)\\) is actually equal to \\(v\\). Moreover, for all other indices \\(i' \\) satisfying \\(i' \not= i\\), if \\(f(i')\\) has not been opened before, then \\(f(i')\\) is unknown to any party who does not know \\(f(X)\\).
> 
> We also remark that, from basic algebra, if we know evaluations at \\(\deg(f) + 1\\) distinct indices, we can recover the polynomial \\(f(X)\\). Therefore, the above claim assumes that other parties do not know up to \\(\deg(f) + 1\\) distinct evaluations.
- \\(\mathsf{VerifyEval}(ck, c, i, v, w_i):\\) On inputs commitment key \\(ck\\), commitment \\(c\\), index \\(i\\), evaluation \\(v\\) and witness \\(w_i\\), this algorithm returns \\(\\{0, 1\\}\\) deciding whether \\(c\\) is a commitment key \\(f(X)\\) satisfying \\(f(i) = v\\).