---
author:
- Caitlin Lienkaemper
title: When is a single layer ReLU network injective?
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

#  Single layer ReLU networks

Now, we consider the map from $\mathbb R^d$ to $\mathbb R^n$ ($n > d$)
given by a single layer of rectified linear (ReLU) units. Let
$\sigma : \mathbb R\to \mathbb R$ denote the threshold nonlinearity
$\sigma(x) = \max\{x, 0\}$. Then the function computed by a single layer
ReLU network is given by
$$f(\mathbf x) = \sigma(W \mathbf x + \mathbf b),$$ where $W$ is a
$n \times d$ matrix computing a linear map from $\mathbb R^d$ to
$\mathbb R^n$, $\mathbf b \in \mathbb R^n$, and $\sigma$ is applied
entry-wise. If $W$ and $\mathrm b$ are chosen at random, what is the
probability that $f$ is injective? We'll show that if $n >> d$, $f$ is
injective with high probability.

Note that if we forgot about $\sigma$, this would be a linear algebra
question: the function $g(\mathbf x) = W \mathbf x  + \mathbf b$ is
injective as long as $W$ has rank $d$. This is guaranteed if there is at
least one $d\times d$ submatrix of $W$ which has full rank. This happens
with probability 1 if the entries of $W$ are chosen at random according
to any reasonable distribution. From now on, let's assume that $W$ is
generic in the sense that each $d \times d$ submatrix of $W$ has rank
$d$. Now, we'll apply this to the case where we *do* have the ReLU here
to mess things up.

By definition, $f$ is injective if $f(\mathbf x) = f(\mathbf y)$ implies
$\mathbf x = \mathbf y$. So, let's find some conditions for $f$ *not* to
be injective. Suppose $f(\mathbf x ) = f(\mathbf y)$, but
$\mathbf x \neq \mathbf y$. Let the *support* of $f$ at $\mathbf x$ be
given by $s(\mathbf x ) \subseteq [n]$ give the set of indices $i$ such
that $f( \mathbf x)_i > 0$. By restricting $f$ to the coordinates
indexed by $s(\mathbf x)$, we get a map from
$\mathbb R^d \to \mathbb R^{|s(\mathbf x)|}$ which is linear at both
$\mathbf x$ and $\mathbf y$, since both $\mathbf x$ and $\mathbf y$ must
be on the positive side of all the ReLU's here. Thus if
$|s(\mathbf x)| \geq d$, this restricted map is injective. **Thus, the
only way $f$ can fail to be injective is if there is some $\mathbf x$
such that $|s(\mathbf x)| < d$.**

Thus, if $f$ is not injective, there must be some point
$\mathbf x \in \mathbb R^d$ such that $|s(\mathbf x)| < d$. Let's look
for conditions on $W, \mathbf b$ that guarantee this. To do this, we
associate an affine arrangement of $n$ hyperplanes in $\mathbb R^d$ to
$W$. The hyperplanes are given by
$H_i = \{ \mathbf x \mid \mathbf w_{i} \cdot \mathbf x + b_i = 0\}$,
where $\mathbf w_{i}$ denotes the $i^{th}$ row of $W$. Note that on the
positive side of $H_i$, $f(\mathbf x)_i =(W \mathbf x + b)_i > 0$, and
on the negative side of $H_i$, $f(\mathbf x)_i = 0$. Thus,
$i\in s(\mathbf x)$ if and only if $\mathbf x \in H_i ^+$. The
hyperplane arrangement $H_1,  \ldots, H_n$ divides up $\mathbb R^d$ into
a set of chambers. Each chamber corresponds to a different value of
$s(\mathbf x)$.

Thus, here's another way to think about our injectivity question: if we
pick an affine hyperplane arrangement of $n$ hyperplanes in
$\mathbb R^d$ at random, what is the probability that each chamber is on
the positive side of at least $d$ of our hyperplanes? What's interesting
here is that it doesn't really matter how we pick our random
hyperplanes, as long as we do it in a way that is generic and such that
both orientations of a hyperplane are equally likely. Let $P(n, d)$ give
the probability that the bad thing happens, i.e. there is at least one
chamber of the hyperplane arrangement that is *not* on the positive side
of at least $d$ of the hyperplanes. Fix a point $\mathbf x$ in space.
Since both orientations of each hyperplane are equally likely, for each
hyperplane $H_i$, it is equally likely that $\mathbf x$ is on the
positive side of $\mathbf x$ as it is on the negative side of $H_i$.
Thus, the probability that $\mathbf x$ is on the positive side of fewer
than $d$ hyperplanes is given by the binomial distribution with
$p = 1/2$, so it is $\frac{\sum_{i = 1}^{d-1}\binom n i}{2 ^n}$.

Now, we turn and consider the possibility that *any* point $\mathbf x$
is on the positive side of too few hyperplanes. Our generic arrangement
of $n$ hyperplanes in $\mathbb R^d$ has $\sum_{i = 0}^d \binom n i$
chambers with fixed sign pattern. Let $E_C$ be the event that chamber
$C$ is on the positive side of fewer than $d$ hyperplanes. By our
previous argument, for any fixed chamber $C$, the probability of $E_C$
is given by $P(E_C)\frac{\sum_{i = 1}^d \binom n i}{2 ^n}$. The event
that some chamber is on the positive side of too few hyperplanes is the
union of $E_C$ over all chambers $C$. In the worst case, all of these
events are disjoint, so the probability of the event $E$ that some
chamber is on the positive side of too few hyperplanes is bounded above
by
$$P(E) = \left(\sum_{i = 0}^d \binom n i\right) \left(\sum_{i = 0}^d \binom n i\right) 2^{-n}.$$
For fixed $d$, we show that $P(E) \to 0$ as $n \to \infty$. We use the
approximations $$\binom n i \leq \left( \frac{en}{i} \right)^{-i}$$and
$$\sum_{i = 0}^d \binom n i \leq 2^{d} \binom n d  \leq  2^d\left( \frac{en}{d} \right)^{d} =\left( \frac{2 en}{d}\right)^d$$
to obtain the result that
$$P(E) \leq \left( \frac{2 en}{d}\right)^d 2^{-n}.$$ Note that for fixed
$d$, $\left( \frac{2 en}{d}\right)^d$ is polynomial in $n$, while
$2^{-n}$ is exponential in $n$, thus as $n\to \infty$, $P(E) \to 0$.

[^1]: Math people: I mean an affine hyperplane arrangement. Meaning that
    we do not force all the planes to go through the origin.

[^2]: I didn't see it.
