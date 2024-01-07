---
layout: post
title:  "Perfect Power Polynomials"
date:   2022-06-04
categories: number-theory olympiad
description: when is an intger-coefficient polynomial always a perfect power
tags: 
giscus_comments: false
related_posts: false
---

This post is about when an integer-coefficient polynomial is always a perfect power. Some notational conventions: we'll use $R[x]$ to denote the set of polynomials with coefficients in $R$, and $\nu_p(n)$ to mean the greatest power of $p$ dividing $n$. 

<div class='definition'>
	An integer $n$ is a perfect power if $n = m^k$ for integers $m$ and $k$, with $k > 1$. 
</div>

<div class='definition'>
	A polynomial $P \in \mathbb{Z}[x]$ is a perfect power if $P(x) = Q(x)^k$ for some polynomial $Q(x) \in \mathbb{Z}[x]$, and some integer $k > 1$. 
</div>

If a polynomial $P$ is a perfect power, then $P(n) = Q(n)^k$ is a perfect power for all integers $n$. We can ask whether the converse is true. It turns out the answer is yes:

<div class='theorem'>
	If $P \in \mathbb{Z}[x]$ such that $P(n)$ is a perfect power for all positive integers $n$, then $P$ itself must be a perfect power. 
</div>

In this post, we'll explain some theory around the behavior of integer-coefficient polynomials, and see how to use this theory to prove this. 

First, factor the polynomial as \\[P(x) = c\cdot Q_1(x)^{e_1}\cdot Q_2(x)^{e_2}\cdots Q_k(x)^{e_k},\\] for distinct irreducible polynomials $Q_i(x) \in \mathbb{Z}[x]$, and some integer $c$. (Unique factorization *does* hold in $\mathbb{Z}[x]$, but if you are uncomfortable with this, then you can perform the factorization in $\mathbb{Q}[x]$ and clear denominators, allowing $c$ to be rational &mdash; this doesn't affect the proof at all.)

The main idea of the proof is: we'd like to construct distinct primes $p_1$, $p_2$, $\ldots$, $p_k$ and some positive integer $n$, such that for each $i$, we have $\nu_{p_i}(Q_i(n)) = 1$ and $\nu_{p_i}(Q_j(n)) = 0$ for $j \neq i$ (so we're constructing one prime per factor, and trying to use these primes to "pull out" the exponents $e_i$). Then we'd have $\nu_{p_i}(P(n)) = e_i$ (as long as the primes don't divide $c$), so since $P(n)$ is a perfect power, then we must have $\gcd(e_1, \ldots, e_k) = d > 1$, and $P$ is a $d$th power. 

Now we'll see some facts about integer-coefficient polynomials that allow us to find such primes. 

## Bounded GCDs

<div class='lemma'>
	Let $P, Q \in \mathbb{Z}[x]$ be two relatively prime polynomials. Then there is some constant $c$ such that $\gcd(P(n), Q(n)) \leq c$ for all integers $n$. 
</div>

In order to prove this, we can use the Euclidean Algorithm. 

### Euclidean Algorithm for Polynomials

If we're trying to find the gcd of two *integers* $a$ and $b$, we know that $\gcd(a, b) = \gcd(a - kb, b)$ for any integer $k$. So then we can repeatedly replace the larger number with its remainder mod the smaller number until we end up with $\gcd(0, d) = d$ for some nonzero integer $d$. For example, \\[\gcd(20, 14) = \gcd(6, 14) = \gcd(6, 2) = \gcd(0, 2) = 2.\\]

We can do the same thing with polynomials with *rational* coefficients &mdash; given any two polynomials with rational coefficients, in order to find their <span class='sd-hidden' desc='gcd'>The gcd of two rational-coefficient polynomials is defined as the highest-degree monic polynomial (also with rational coefficients) which divides both of them in $\mathbb{Q}[x]$.</span> we can repeatedly replace the higher-degree one with its remainder mod the lower-degree one, until we end up with $\gcd(0, R) = R$ for some polynomial $R \in \mathbb{Q}[x]$. (The reason we're using *rational* coefficients rather than integer coefficients here is because we can't necessarily do polynomial division in $\mathbb{Z}[x]$ &mdash; if we're dividing by a non-monic polynomial, we may introduce fractions.)

<div class='example'> 
	Find $\gcd(x^3 + 3x + 1, x^2 - 2)$. 
</div>

<div class='proof'>
	First, we can use polynomial division to get \[x^3 + 3x + 1 = (x^2 - 2)x + 5x + 1,\] which means \[\gcd(x^3 + 3x + 1, x^2 - 2) = \gcd(5x + 1, x^2 - 2).\] Now we can perform polynomial division again to get \[x^2 - 2 = (5x + 1)(\tfrac{1}{5}x - \tfrac{1}{25}) - \tfrac{49}{25}.\] So we have \[\gcd(5x + 1, x^2 - 2) = \gcd(5x + 1, \tfrac{49}{25}) = \gcd(0, \tfrac{49}{25}) = 1.\]
</div>

This process successfully terminates (meaning that we can make one term $0$) for the same reason that it does in the integers &mdash; every step decreases the degree of our polynomials. 

Similarly to the case of integers, every polynomial we write down in this process is a linear combination of $P$ and $Q$ (where the coefficients are rational-coefficient polynomials). In the integer case, this gives us Bezout's Theorem; similarly, here this means \\[\gcd(P, Q) = A\cdot P + B\cdot Q\\] for some rational-coefficient polynomials $P$ and $Q$. 

### Proving the Lemma

Now this gives us the tools to prove that relatively prime polynomials have bounded gcds:

<div class='proof'>
	Since $P$ and $Q$ are relatively prime, we have $\gcd(P, Q) = 1$, which means \[1 = A\cdot P + B\cdot Q\] for rational-coefficient polynomials $A$ and $B$. This means \[d = A^*\cdot P + B^* \cdot Q\] for <em>integer</em>-coefficient polynomials $A^*$ and $B^*$, by clearing denominators. So then $\gcd(P(n), Q(n))$ must divide $d$, for all integers $n$. 
</div>

## Schur's Theorem

Earlier, we wrote our polynomial as \\[P(x) = c\cdot Q_1(x)^{e_1}\cdot Q_2(x)^{e_2}\cdots Q_k(x)^{e_k},\\] and we hoped to find primes $p_1$, $p_2$, $\ldots$, $p_k$ such that each $p_i$ functions as a sort of indicator for $Q_i(n)$ &mdash; we want $p_i$ to divide $Q_i(n)$, but not $Q_j(n)$ for any $j \neq i$. 

Since all the $Q_i$ are relatively prime as polynomials, we know that their pairwise gcds are all bounded. So if we choose our primes to be large enough, then if $p_i$ divides $Q_i(n)$ it *definitely* can't divide $Q_j(n)$ for any other $j$. 

But what if we *can't* choose a large enough prime &mdash; what if all primes which divided $Q_i(n)$ (over all $n$) were less than $100$, for instance? This is where Schur's Theorem comes in. 

<div class='theorem' text='Schur&apos;s Theorem'>
	If $P \in \mathbb{Z}[x]$ is a nonconstant polynomial, then there are infinitely many primes which divide $P(n)$ for positive integers $n$. 
</div>

In fact, the following more general statement is true:

<div class='problem' text='ISL 2009 N3'>
	Let $f : \mathbb{N} \to \mathbb{Z}$ be a nonconstant function, such that $a - b \mid f(a) - f(b)$ for all positive integers $a$ and $b$. Prove that there are infinitely many primes $p$ such that $p$ divides $f(c)$ for some positive integer $c$. 
</div>

<div class='sd-hidden' desc='Proof'>
	First, the given condition means that $f$ is mod-preserving &mdash; if $a \equiv b \pmod{n}$, then $f(a) \equiv f(b) \pmod{n}$ as well. 
	<br/><br/>
	Assume for contradiction that the only primes dividing $f(c)$, over all $c$, are $p_1$, $p_2$, $\ldots$, $p_r$. Now the idea is to construct a bunch of values of $n$ which have the same value of $f(n)$, by imposing conditions mod powers of these primes. 
	<br/><br/>
	Fix some $a$ for which $f(a) \neq 0$, and for each prime $p_i$, let $\nu_{p_i}(f(a)) = e_i$. Then construct \[n \equiv a \pmod{p_i^{e_i + 1}}\] for all $i$ (this is possible by the Chinese Remainder Theorem). Then we have \[f(n) \equiv f(a) \pmod{p_i^{e_i + 1}}\] for all $i$ as well, which means \[\nu_{p_i}(f(n)) = \nu_{p_i}(f(a))\] for all $i$. But since these are the only primes which can divide either $f(n)$ or $f(a)$, this means $|f(n)| = |f(a)|$. 
	<br/><br/>
	So then there is some value $k$ for which there are infinitely many $c$ with $f(c) = k$. Now for each $n \in \mathbb{N}$, we have \[n - c \mid f(n) - f(c) = f(n) - k\] for all such $c$. But as we vary $c$, the LHS can be made arbitrarily large, while the RHS is constant; so we must have $f(n) - k = 0$. So $f$ is constant, contradiction.
</div>

So by Schur's Theorem, we know that we can find arbitrarily large (and distinct) primes $p_1$, $p_2$, $\ldots$, $p_k$, and positive integers $n_1$, $n_2$, $\ldots$, $n_k$, such that $p_i \mid Q_i(n_i)$ for each $i$. We can then combine these $n_i$ into one $n$, by choosing \\[n \equiv n_i \pmod{p_i}\\] for all $i$ (which is possible by the Chinese Remainder Theorem). Then by the fact that gcds are bounded, we know that $p_i$ doesn't divide $Q_j(n)$ for any $j \neq i$. 

## Hensel's Lemma

We've *almost* constructed everything we wanted to &mdash; we now have our indicator primes, and we've shown that they aren't affected by any factor except the one they're assigned to. But there's one thing missing. It's not enough to know that $p_i \mid Q_i(n)$ &mdash; we need to know that *exactly one* power of $p_i$ divides $Q_i(n)$, or in other words, $\nu_{p_i}(Q_i(n)) = 1$. (This is so that we can get that the power of $p_i$ in the prime factorization of $P(n)$ is exactly $e_i$.)

So we want to try actually choosing the $n_i$ mod $p_i^2$, such that $Q_i(n_i)$ is divisible by $p_i$, but not $p_i^2$. In order to do this, we'll use Hensel's Lemma:

<div class='theorem' desc='Hensel&apos;s Lemma'>
	Let $P \in \mathbb{Z}[x]$ be a polynomial, and $p$ a prime and $n_1$ an integer. If $p \nmid P'(n_1)$, then for any positive integer $e$ and any $c \equiv P(n_1) \pmod{p}$, we can find some $n_e \equiv n_1 \pmod{p}$ such that \[P(n_e) \equiv c \pmod{p^e}.\]
</div>

So essentially, Hensel's Lemma states that if we can solve a polynomial equation mod $p$, and at that point the *derivative* isn't divisible by $p$, then we can solve it mod any power of $p$. 

Before we see the proof, we'll first look at a specific example:

<div class='example'>
	Show that if $p > 2$ and there exists $n_1$ such that $n_1^2 + 2 \equiv 0 \pmod{p}$, then for every integer $a$, there exists $n \equiv n_1 \pmod{p}$ such that $n^2 + 2 \equiv ap \pmod{p^2}$. 
</div>

<div class='proof'>
	Let $n = n_1 + pt$, for some integer $t$. Then we have \[n^2 + 2 = n_1^2 + 2pn_1t + p^2t^2 + 2 \equiv n_1^2 + 2 + 2pn_1t \pmod{p^2}.\] We know $n_1^2 + 2$ is some multiple of $p$, and since $p \nmid 2n_1$, then $2pn_1t$ must run over all possible multiples of $p$. So as $t$ varies, $n^2 + 2$ must cover all multiples of $p$ mod $p^2$. 
</div>

The proof in the general case is essentially the same:

<div class='proof' text='Proof of Hensel&apos;s Lemma'>
	Induct on $e$ &mdash; in the base case $e = 1$, there is nothing to prove. Now choose $n_{e - 1} \equiv n_1 \pmod{p}$ such that $P(n_{e - 1}) \equiv c \pmod{p^{e - 1}}$, using the inductive hypothesis. Now let $n_e = n_{e - 1} + p^{e - 1}t$ for any integer $t$. 
	<br/><br/>
	Let $P(x) = a_nx^n + a_{n - 1}x^{n - 1} + \cdots + a_1x + a_0$. Then we have \[(n_{e - 1} + p^{e - 1}t)^i \equiv n_{e - 1}^i + ip^{e - 1}tn_{e - 1}^{i - 1} \pmod{p^e}\] by the Binomial Theorem (every other term is divisible by $p^e$, since $2(e - 1) \geq e$). So this means \[P(n_e) \equiv P(n_{e - 1}) + p^{e - 1}\cdot P'(n_{e - 1})t \pmod{p^e}.\] Since $P'(n_{e - 1})$ is not divisible by $p$, then the second term must cover all multiples of $p^{e - 1}$ mod $p^e$, which means over all values of $t$, we must be able to get $P(n_e) \equiv c \pmod{p^e}$. 
</div>

Now in our case, we know $Q_i$ and $Q_i'$ are relatively prime, as polynomials (since the $Q_i$ are irreducible), so their gcds are again bounded. So as long as we chose our primes $p_i$ to be large enough, we know that if $p_i \mid Q_i(n_i)$, then $p_i \nmid Q_i'(n_i)$. So we can actually choose $n_i$ mod $p_i^2$ such that $Q_i(n_i) \equiv p_i \pmod{p_i^2}$. 

Then, by the Chinese Remainder Theorem we can again choose $n$ such that $n \equiv n_i \pmod{p_i^2}$ for all $i$. So for this choice of $n$, we have $\nu_{p_i}(Q_i(n)) = 1$ for all $i$. 

## Conclusion

We've written \\[P(x) = cQ_1(x)^{e_1}Q_2(x)^{e_2}\cdots Q_k(x)^{e_k}\\] for irreducible polynomials $Q_i$, and chosen huge distinct primes $p_1$, $p_2$, $\ldots$, $p_k$ and a positive integer $n$ such that $\nu_{p_i}(Q_i(n)) = 1$, and $\nu_{p_i}(Q_j(n)) = 0$ for all $j \neq i$. 

This means for each $i$, we have \\[\nu_{p_i}(P(n)) = e_i.\\] But we know $P(n)$ is a $d$th power of an integer, for some $d > 1$. So $d$ must divide $e_i$ for all $i$. Finally, then $Q_1(n)^{e_1}\cdots Q_k(n)^{e_k}$ is a $d$th power, so $c$ must be a $d$th power as well. 

So $P$ is a $d$th power of a polynomial, and is therefore a perfect power. 

