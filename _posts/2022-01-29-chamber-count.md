---
layout: post
title: Taking the lazy caterer's sequence into another dimension 
date: 2022-1-29
tag:
  - common_tag
  - other_tag
mathjax: true 
---

# Counting Hyperplane Chambers

The lazy caterer's sequence counts the maximum number of pieces that you
can cut a pancake into with $n$ straight cuts. More mathematically, this
is the maximum number of chambers of an arrangement of $n$ lines in the
plane. Conveniently, there's a great formula for it: setting $C(n)$ as
$n^{th}$ term of the lazy caterer's sequence, we have

$$C(n) = \frac{n^2 + n +2}{2} = \binom n 0 + \binom n 1 + \binom n 2$$

How do we generalize this to an arbitrary dimension $d$? Let's set
$C(n, d)$ as the maximum number of chambers of an arrangemt of $n$
hyperplanes in $\mathbb R^d$.[^1] I mean, there's an "obvious\"[^2]
guess: let's just keep the sum going:

$$C(n, d)  = \binom n 0 + \binom n 1 + \binom n 2 + \cdots + \binom n d  = \sum_{i = 0}^{d} \binom n i$$
Conveniently, this guess is right. As far as I can tell, this is a well
known result, but it's the sort of well known result that is a little
bit annoying to look up online. Supposedly, you can cite \[source\], but
I have no idea where this result appears within \[source\], so I'm gonna
prove the result here.

Finally, I'm sick of saying "maximum\". It turns out that if you pick
any \"generic\" hyperplane arrangement, in the sense that you never let
too many hyperplanes meet at one point, you will achieve the maximum
number of chambers. If you pick your hyperplane arrangement at random
with any reasonable distribution, you will get a generic arrangement and
maximize the number of chambers. Honestly this seems like a bit of a
miracle, but it should fall out as a consequence of our proof.

::: {.thm}
**Theorem 1**. Let $C(n, d)$ be the number of full dimensional chambers
of a generic affine hyperplane arrangement in $\mathbb R^d$. Then
$$C(n, d) = \sum_{i = 0}^{d} \binom n i .$$
:::

::: {.proof}
*Proof.* We first prove that $C(n, d)$ satisfies the recurrence relation
$$C(n, d) = C(n-1, d) + C(n-1, d-1).$$ Consider an arrangement of $n$
hyperplanes in $\mathbb R^d$. Pick out one of the hyperplanes and tell
it that it's special. If we remove out special hyperplane, there are
only $n-1$ hyperplanes left, so the number of chambers is $C(n-1, d)$.
Now, let's think about how many chambers are created when we add our
special hyperplane back in. Each new chamber arises when one of the
chambers gets split in half by our special hyperplane. Now, notice that
the restriction of this hyperplane arrangement to our special hyperplane
is a generic arrangement of $n-1$ hyperplanes in $\mathbb R^{d-1}$. The
number of chambers in this restricted arrangement is counted by
$C(n-1, d-1)$. Further, there is a one-to-one correspondence between
chambers which get split when we add the special hyperplane back in and
chambers of our restricted hyperplane arrangement. Therefore,
$$C(n, d) = C(n-1, d) + C(n-1, d-1).$$

Now, we show that this recurrence relation is satisfied by
$$C(n, d) = \sum_{i = 0}^{d} \binom n i .$$

First, we check the base cases for $n$ and $d$. If $d = 1$, then our
"hyperplanes\" are points on a line, and $n$ points on a line split that
line into $\binom n 0 + \binom n 1 = n+ 1$ chambers, so
$C(n, 1) = \sum_{i = 0}^{1}\binom n i$. If $n \leq d$, then generically
all hyperplanes intersect, creating $2^n$ regions, so
$C(n, d) = \sum_{i = 0}^{d} \binom n i$. Now, suppose for all
$m, k  < n, d$, we have $C(m, k) = \sum_{i = 0}^{k}  \binom m i$. Then
by our recurrence relation, $$\begin{aligned}
C(n, d) &=  C(n-1, d) + C(n-1, d-1) \\
C(n, d) &= \sum_{i = 0}^{d}\binom {n-1} i+  \sum_{i = 0}^{d-1}\binom {n-1} i.\end{aligned}$$
Now, we regroup terms as $$\begin{aligned}
C(n, d) &= \binom {n-1}{0} + \left ( \binom {n-1}{0} + \binom {n-1}{1}\right) + \cdots +\left ( \binom {n-1}{d-1} + \binom {n-1}{d}\right) .\end{aligned}$$
By Pascal's rule (and the fact that $\binom {n-1} 0 = \binom n 0 = 1$,
we can rewrite this as $$\begin{aligned}
C(n, d) &=\binom n 0 + \binom n 1 + \cdots  +\binom n d = \sum_{i = 0}^{d} \binom n i,\end{aligned}$$
as desired. ◻
:::


[^1]: Math people: I mean an affine hyperplane arrangement. Meaning that
    we do not force all the planes to go through the origin.

[^2]: I didn't see it.
