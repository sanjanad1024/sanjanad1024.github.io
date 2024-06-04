---
layout: post
title:  "Anticoncentration of Random Subset Sums"
date:   2024-06-03
categories: combinatorics
description: A proof of Erd&#337;s's theorem that random sums of the form $\varepsilon_1a_1 + \cdots + \varepsilon_na_n$ (for random $\varepsilon_i \in \{0, 1\}$ and fixed $a_i$) cannot be too concentrated.   
tags: 
giscus_comments: false
related_posts: false
---

I took 18.204 (Seminar in Discrete Math) this semester; this is a class where we each choose a topic and give a few presentations on it. My topic was anticoncentration. In this post, I'll talk about a theorem that's in some sense the starting point for all the things I presented on. (I may post about those things later.)

# Introduction 

In this post, we'll consider the following question. 

<div class = 'question'>
    Suppose we have $n$ nonzero real numbers $a_1$, $\ldots$, $a_n$, and we choose a random subset sum of these numbers &mdash; i.e., we consider the random variable $\varepsilon_1a_1 + \cdots + \varepsilon_na_n$ for $\varepsilon_i \in \{0, 1\}$ chosen uniformly and independently at random. How concentrated can this random variable be at a single point?
</div>

In particular, we're interested in proving <em>upper</em> bounds on how concentrated  $\varepsilon_1a_1 + \cdots + \varepsilon_na_n$ can be at a single point $a$ &mdash; this means we want to upper-bound $\max_a \mathbb{P}[\varepsilon_1a_1 + \cdots + \varepsilon_na_n = a]$. This is why the topic is referred to as <span class = 'vocab'>anticoncentration</span> &mdash; we're trying to show that a random variable is <em>not</em> super concentrated at any point.

This question was first considered by Littlewood and Offord (who were studying real roots of random polynomials) in a <a href="https://www.mathnet.ru/links/31fb3a11135873de2d81e19d30ad43aa/sm6161.pdf" target="_blank">paper</a> from 1943; they proved the following upper bound. 

<div class = 'theorem' text = 'Littlewood&ndash;Offord 1943'>
    For all nonzero $a_1, \ldots, a_n \in \mathbb{R}$ and all $a \in \mathbb{R}$, we have \[\mathbb{P}[\varepsilon_1a_1 + \cdots + \varepsilon_na_n = a] \leq \frac{c\log n}{\sqrt{n}}\] (for some absolute constant $c > 0$). 
</div>

This was later improved by Erd&#337;s to the following bound. 

<div class = 'theorem' text = 'Erd&#337;s 1945'>
    For all nonzero $a_1, \ldots, a_n \in \mathbb{R}$ and all $a \in \mathbb{R}$, we have \[\mathbb{P}[\varepsilon_1a_1 + \cdots + \varepsilon_na_n = a] \leq \frac{\binom{n}{\lfloor n/2\rfloor}}{2^n}.\] 
</div>

As some intuition regarding how big the right-hand side is, by Stirling's approximation $\binom{n}{\lfloor n/2\rfloor}$ is roughly $\frac{2^n}{\sqrt{n}}$ (ignoring constant factors), so this essentially removes the $\log n$ from the bound of Littlewood&ndash;Offord. 

Also, a nice thing is that this theorem is tight &ndash; the following construction achieves equality. 

<div class = 'example'>
    Suppose that $a_1 = \cdots = a_n = 1$. Then for every $k$, we have \[\mathbb{P}[\varepsilon_1a_1 + \cdots + \varepsilon_n = k] = \frac{\binom{n}{k}}{2^n}\] (since getting a subset sum of $k$ corresponds to choosing exactly $k$ of the $\varepsilon_i$'s to be $1$).  
</div>

So the bound in <span class = 'crossref'>Theorem 2</span> is the best bound we could possibly hope to get, and it turns out to be true &mdash; this essentially means that $\varepsilon_1a_1 + \cdots + \varepsilon_na_n$ is the most concentrated in the case where all the $a_i$'s are equal. 

Erd&#337;s proved this theorem using a very neat combinatorial argument, which I'll explain in the rest of this post.

# Sperner's theorem

The proof is going to involve Sperner's theorem regarding antichains of subsets of $[n]$, so we'll first state and prove this theorem. 

<div class = 'definition'>
    We say a collection $\mathcal{S}$ of subsets of $[n]$ is an <span class = 'vocab'>antichain</span> if there do not exist any two distinct sets $A, B \in \mathcal{S}$ such that $A \subseteq B$. 
</div>

<div class = 'example'>
    Letting $n = 4$, the collection of subsets $\{1, 2\}$, $\{1, 3, 4\}$, $\{2, 3, 4\}$ form an antichain (as no one of these subsets contains another). 
    <br><br/>
    On the other hand, $\{2, 4\}$, $\{1, 3, 4\}$, $\{2, 3, 4\}$ doesn't form an antichain, as the third contains the first. 
</div>



Sperner's theorem considers the following question. 

<div class = 'question'>
    Given $n$, what's the largest possible antichain of subsets of $[n]$?
</div>

(By the size of an antichain $\mathcal{S}$, we mean the number of subsets in $\mathcal{S}$.)

One potential construction of an antichain is to take all subsets of a fixed size $k$. 

<div class = 'example'>
    For any $0 \leq k \leq n$, the collection $\mathcal{S} = \{A \subseteq [n] \mid \lvert A \rvert = k\}$ is an antichain of size $\binom{n}{k}$. 
</div>

To maximize the size of this antichain, we should take $k = \lfloor n/2\rfloor$. And Sperner's theorem says essentially that this is the best we can do. 

<div class = 'theorem' text = 'Sperner'>
    If $\mathcal{S}$ is an antichain of subsets of $[n]$, then $\lvert \mathcal{S} \rvert \leq \binom{n}{\lfloor n/2\rfloor}$.  
</div>

There are several proofs of this theorem; here's a very cute one that I saw in 18.226 (involving a clever application of probabilistic methods). 

<div class = 'proof'>
    Imagine we start with the set $\emptyset$ and add elements one at a time until we reach $[n]$ &mdash; so we start with the set $I_0 = \emptyset$, then add a random element $i_1 \in [n]$ to it to get the set $I_1 = \{i_1\}$, then add another random element $i_2 \in [n] \setminus \{i_1\}$ to get the set $I_2 = \{i_1, i_2\}$, and so on, until we've added all elements to finally end up with $I_n = \{i_1, \ldots, i_n\} = [n]$. This gives us a random sequence of sets $I_0 \subseteq I_1 \subseteq \cdots \subseteq I_n$. 
    <br><br/>
    On one hand, no matter what order we choose elements in, at most one set in $\mathcal{S}$ can appear in this sequence &mdash; if two sets $A$ and $B$ appeared in the sequence, then we'd have $A \subseteq B$ or $B \subseteq A$, contradicting the fact that $\mathcal{S}$ is supposed to be an antichain.
    <br><br/> 
    On the other hand, what's the <em>expected</em> number of sets $A \in \mathcal{S}$ to appear in this sequence? Each $I_k$ is a uniform random subset of $[n]$ with size $k$ (by symmetry), so if $\lvert A\rvert = k$, then the probability that $A$ appears in our sequence is \[\mathbb{P}[\text{$A$ appears in the sequence}] = \mathbb{P}[I_k = A] = \frac{1}{\binom{n}{k}}\] (since there's $\binom{n}{k}$ equally likely possibilities for $I_k$, one of which is $A$). So by linearity of expectation, the expected number of sets $A \in \mathcal{S}$ to appear in the sequence is \[\mathbb{E} \#\{A \in \mathcal{S} \mid \text{$A$ appears in the sequence}\} = \sum_{A \in \mathcal{S}} \frac{1}{\binom{n}{\lvert A\rvert}} \leq \frac{\lvert \mathcal{S}\rvert}{\binom{n}{\lfloor n/2\rfloor}}.\] And since the left-hand side is at most $1$ (the quantity we're taking the expectation of is always at most $1$), this means $\lvert \mathcal{S}\rvert \leq \binom{n}{\lfloor n/2\rfloor}$, as desired. 
</div>

# Proof of the main theorem

Now we're ready to prove <span class = 'crossref'>Theorem 2</span>. 

First, we can assume that the $a_i$'s are all <em>positive</em>, as flipping the sign of some $a_i$ doesn't affect the value of $\max_a \mathbb{P}[\varepsilon_1a_1 + \cdots + \varepsilon_na_n = a]$. One way to see this is that we could imagine choosing all the $\varepsilon_i$'s from $$\{-1, 1\}$$ instead of $$\{0, 1\}$$ &mdash; this wouldn't affect $\max_a \mathbb{P}[\varepsilon_1a_1 + \cdots + \varepsilon_na_n = a]$, since it'd correspond to performing the transformation $a \mapsto 2a - \frac{1}{2}(a_1 + \cdots + a_n)$ to our random variable $\varepsilon_1a_1 + \cdots + \varepsilon_na_n$. And if we're choosing $$\varepsilon_i \in \{-1, 1\}$$, then clearly flipping the sign of $a_i$ has no effect on the distribution of this random variable. 

Now fix $a$, and define a collection $\mathcal{S}$ consisting of all the subsets of $[n]$ that correspond to subset sums of exactly $a$ &mdash; i.e., for each $(\varepsilon_1, \ldots, \varepsilon_n)$ satisfying $\varepsilon_1a_1 + \cdots + \varepsilon_na_n = a$, we place the corresponding set $$A = \{i \in [n] \mid \varepsilon_i = 1\}$$ into $\mathcal{S}$. 

The key observation (and where Sperner's theorem comes in) is the following. 

<div class = 'claim'> 
    The collection $\mathcal{S}$ is an antichain. 
</div>

<div class = 'proof'>
    Assume for contradiction that there's two sets $A$ and $B$ in $\mathcal{S}$ with $A \subsetneq B$. This means they correspond to $(\varepsilon_1, \ldots, \varepsilon_n)$ and $(\varepsilon_1', \ldots, \varepsilon_n')$ such that $\varepsilon_i \leq \varepsilon_i'$ for all $i$, which means \[\varepsilon_1a_1 + \cdots + \varepsilon_na_n < \varepsilon_1'a_1 + \cdots + \varepsilon_n'a_n.\] (If we're thinking about these quantities as subset sums of $\{a_1, \ldots, a_n\}$, then the point is that the subset sum corresponding to $B$ includes all the terms in the one corresponding to $A$, and at least one extra term; and since all the $a_i$'s are positive, this means its value must be strictly greater.)
    <br><br/>
    But these sums are both supposed to be $a$ (by the definition of $\mathcal{S}$), so this is a contradiction. 
</div>

So then Sperner's theorem implies that $\lvert \mathcal{S}\rvert \leq \binom{n}{\lfloor n/2\rfloor}$, which proves <span class = 'crossref'>Theorem 2</span> (as there's $2^n$ possible outcomes for $(\varepsilon_1, \ldots, \varepsilon_n)$, and the 'good' ones precisely correspond to elements of $\mathcal{S}$). 