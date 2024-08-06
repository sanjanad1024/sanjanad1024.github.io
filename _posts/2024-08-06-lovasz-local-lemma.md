---
layout: post
title:  "The Lov&aacute;sz Local Lemma"
date:   2024-08-06
categories: combinatorics probabilistic-method
description: A proof of the Lov&aacute;sz local lemma, which states that if we have a collection of bad events with only a few 'local' dependencies, then we avoid all of them with positive probability. 
tags: 
giscus_comments: false
related_posts: false
---

In this post, I'll explain the statement and proof of the Lov&aacute;sz local lemma. 

# Motivation

There are many situations where we have a random process and a collection of 'bad' events $A_1$, $\ldots$, $A_n$, and we want to show that there's a positive probability that none of these bad events happen &mdash; i.e., that
<div>
    \[\mathbb{P}[\overline{A_1} \wedge \cdots \wedge \overline{A_n}] > 0.\]
</div> 
This especially happens when we're using the probabilistic method &mdash; often we're trying to prove the existence of an object with certain properties by taking a *random* construction and showing that it works with positive probability, and in many cases, the construction working corresponds to avoiding a collection of bad events. 

There's two extreme cases where it's easy to show that we avoid all the bad events with positive probability. One is when there's not too many events and their probabilities are all small (relative to the number of events). In this case, we can simply use a union bound &mdash; we have 
<div>
    \[\mathbb{P}[A_1 \vee \cdots \vee A_n] \leq \mathbb{P}[A_1] + \cdots + \mathbb{P}[A_n],\]
</div>
so if $\mathbb{P}[A_1] + \cdots + \mathbb{P}[A_n] < 1$, then we get that 
<div>
    \[\mathbb{P}[\overline{A_1} \wedge \cdots \wedge \overline{A_n}] = 1 - \mathbb{P}[A_1 \vee \cdots \vee A_n] > 0.\]
</div>
In particular, if we know that $\mathbb{P}[A_i] \leq p$ for all $i$ and $pn < 1$, then this means we avoid all the bad events with positive probability. 

The other extreme case is when the events are all *independent*. In this case, it doesn't matter how many events there are or how large their probabilities are (as long as they aren't $1$, of course) &mdash; we simply have
<div>
    \[\mathbb{P}[\overline{A_1} \wedge \cdots \wedge \overline{A_n}] = \prod_{i = 1}^n (1 - \mathbb{P}[A_i]) > 0.\]
</div>

The motivation for the Lov&aacute;sz local lemma is the question of whether we can 'interpolate' between these two extreme cases.

<div class = 'question'>
    What if we have lots of events, but each is only dependent on a few others (where the event probabilities are small relative to the number of <em>dependencies</em>, rather than <em>events</em>)? Can we still avoid all the events with positive probability?
</div>

It turns out the answer is yes! And that's exactly what the Lov&aacute;sz local lemma states. 

# The Lov&aacute;sz local lemma

To state the Lov&aacute;sz local lemma, we first need to formalize what it means for each event to only be dependent on a few others. 

<div class = 'definition'>
    We say an event $A$ is <span class = 'vocab'>independent</span> from a collection of events $\{A_1, \ldots, A_n\}$ if conditioning on any logical combination of the events $A_1$, $\ldots$, $A_n$ doesn't affect the probability of $A$. 
</div>

For example, this means $\mathbb{P}[A \mid A_1 \wedge \overline{A_3} \wedge A_5] = \mathbb{P}[A]$. 

<!-- <div class = 'remark'>
    This definition is stronger than requiring that $A$ be pairwise independent from each of $A_1$, $\ldots$, $A_n$. For example, imagine we're choosing $x_1, x_2, x_3 \in \{0, 1\}$ uniformly at random, and let $A$ be the event that $x_1 + x_2$ is even, $A_1$ the event that $x_1 + x_3$ is even, and $A_2$ the event that $x_2 + x_3$ is even. Then $A$ is pairwise independent from each of $A_1$ and $A_2$ individually, but it's not independent from the collection $\{A_1, A_2\}$ &mdash; for example, we have $\mathbb{P}[A \mid A_1 \wedge A_2] = 1$, which is not the same as $\mathbb{P}[A]$. 
</div> -->

We'll first state a 'symmetric' form of the Lov&aacute;sz local lemma (which is often what's used in applications when our bad events have comparable probabilities). 

<div class = 'theorem' text = 'Symmetric Lov&aacute;sz local lemma'>
    Let $A_1$, $\ldots$, $A_n$ be events such that $\mathbb{P}[A_i] \leq p$ for all $i$, and such that each event $A_i$ is independent from a collection of all but $d$ others. If $ep(d + 1) \leq 1$, then \[\mathbb{P}[\overline{A_1} \wedge \cdots \wedge \overline{A_n}] > 0.\]
</div>

(Here $e$ denotes the constant $e \approx 2.718$.)

The important feature of this theorem is that the condition $ep(d + 1) \leq 1$ doesn't reference the number of *events* at all &mdash; it only considers the number of *dependencies*.

We'll actually deduce the symmetric form of the Lov&aacute;sz local lemma from a more general 'asymmetric' form (which is useful even when the bad events *don't* have comparable probabilities). 

<div class = 'theorem' text = 'Asymmetric Lov&aacute;sz local lemma'>
    Let $A_1$, $\ldots$, $A_n$ be a collection of events, and for each $i \in [n]$, let $\mathcal{D}_i \subseteq [n]$ be a set such that $A_i$ is independent from the collection of events $\{A_j \mid j \not\in \mathcal{D}_i\}$. 
    <br><br/>
    Suppose there exist $x_1, \ldots, x_n \in (0, 1)$ such that $\mathbb{P}[A_i] \leq x_i\prod_{j \in \mathcal{D}_i} (1 - x_j)$ for all $i \in [n]$. Then \[\mathbb{P}[\overline{A_1} \wedge \cdots \wedge \overline{A_n}] \geq \prod_{i = 1}^n (1 - x_i) > 0.\]
</div>

First, here's why the asymmetric version of the Lov&aacute;sz local lemma implies the symmetric version &mdash; given $A_1$, $\ldots$, $A_n$ as in the symmetric version, we can set $x_i = 1/(d + 1)$ for each $i$. This satisfies the hypothesis of the asymmetric version &mdash; for each $i$, we have $\lvert \mathcal{D}_i \rvert \leq d$, so
<div>
\[x_i\prod_{j \in \mathcal{D}_i}(1 - x_j) \geq \frac{1}{d + 1}\left(1 - \frac{1}{d + 1}\right)^d \geq \frac{1}{(d + 1)e},\]
</div>
and the assumption that $ep(d + 1) \leq 1$ means that this is at least $p$ (which was an upper bound on $\mathbb{P}[A_i]$). So the asymmetric version tells us we avoid all the bad events with positive probability, as desired.

In the rest of this post, we'll prove this asymmetric version of the Lov&aacute;sz local lemma.  

# Proof of the asymmetric version

We're going to actually prove the following claim. 

<div class = 'claim'>
    For all $\mathcal{S} \subseteq [n]$ and $i \not\in \mathcal{S}$, we have $\mathbb{P}[A_i \mid \bigwedge_{j \in \mathcal{S}} \overline{A_j}] \leq x_i$. 
</div>

Once we have this claim, we can deduce <span class = 'crossref'>Theorem 4</span> by writing
<div class = 'eqn'>
\[\mathbb{P}[\overline{A_1} \wedge \cdots \wedge \overline{A_n}] = \prod_{i = 1}^n \mathbb{P}[\overline{A_i} \mid \overline{A_1} \wedge \cdots \wedge \overline{A_{i - 1}}] \leq \prod_{i = 1}^n (1 - x_i).\]
</div>

We'll prove this claim by induction on $\lvert\mathcal{S}\rvert$. When $\mathcal{S}$ is empty, we have $\mathbb{P}[A_i] \leq x_i\prod_{j \in \mathcal{D}_i} (1 - x_j) \leq x_i$, so the claim is true. Now suppose that $\mathcal{S}$ is not empty, and that we've proven the claim for all smaller sets $\mathcal{S}'$. 

We're trying to consider the probability of $A_i$ conditioned on a bunch of events, so we'll start by splitting these events into two &mdash; we'll separately consider the ones that are independent from $A_i$ and the ones that aren't. So we split up the event that we're trying to condition on as $B_{\text{ind}} \wedge B_{\text{dep}}$, where 
<div>
    \[B_{\text{ind}} = \bigwedge_{j \in \mathcal{S} \setminus \mathcal{D}_i} \overline{A_j} \quad \text{and} \quad B_{\text{dep}} = \bigwedge_{j \in \mathcal{S} \cap \mathcal{D}_i} \overline{A_j}.\]
</div>
First, since $A_i$ is independent of $$\{A_j \mid j \not\in \mathcal{D}_i\}$$ and $B_{\text{ind}}$ is a logical combination of these events, we know that conditioning on $B_{\text{ind}}$ doesn't affect the probability of $A_i$ &mdash; i.e., we have $\mathbb{P}[A_i \mid B_{\text{ind}}] = \mathbb{P}[A_i]$. But we *don't* know anything about how conditioning on $B_{\text{dep}}$ affects $A_i$, so the best thing we can do is try to get rid of it &mdash; so we drop the conditioning on $B_{\text{dep}}$ using the fact that 
<div class = 'eqn'>
    \[\mathbb{P}[A_i \mid B_{\text{ind}} \wedge B_{\text{dep}}] = \frac{\mathbb{P}[A_i \wedge B_{\text{dep}} \mid B_{\text{ind}}]}{\mathbb{P}[B_{\text{dep}}]} \leq \frac{\mathbb{P}[A_i \mid B_{\text{ind}}]}{\mathbb{P}[B_{\text{dep}}]}.\]
</div>
(We can't really do anything better with $B_{\text{dep}}$ because we know nothing about how the events $A_j$ that go into it interact with $A_i$.)

Now in the numerator, we're only conditioning on $B_{\text{ind}}$, and we know how to handle this conditioning &mdash; we simply have $\mathbb{P}[A_i \mid B_{\text{ind}}] = \mathbb{P}[A_i]$. So now it remains to get a *lower* bound on the denominator $\mathbb{P}[B_{\text{dep}}]$. 

But $B_{\text{dep}}$ is an event of the form $$\overline{A_{j_1}} \wedge \cdots \wedge \overline{A_{j_m}}$$ (where $j_1$, $\ldots$, $j_m$ are the elements of $\mathcal{S} \cap \mathcal{D}_i$), so we can actually do this in the same way as in <span class = 'crossref'>(1)</span> using the inductive hypothesis! We can write 
<div class = 'eqn'>
\[\mathbb{P}[B_{\text{dep}}] = \prod_{k = 1}^m \mathbb{P}[\overline{A_{j_k}} \mid \overline{A_{j_1}} \wedge \cdots \wedge \overline{A_{j_{k - 1}}}].\]
</div>
And for each term on the right-hand side, the set of indices of the events we're conditioning on is a proper subset of $\mathcal{S} \cap \mathcal{D}_i \subseteq \mathcal{S}$, which means in particular that we've already proven the claim for this set (in our induction). So for each $k$, we have 
<div>
\[\mathbb{P}[A_{j_k} \mid \overline{A_{j_1}} \wedge \cdots \wedge \overline{A_{j_{k - 1}}}] \leq x_{j_k},\]
</div>
which means that the $k$th term on the right-hand side of <span class = 'crossref'>(3)</span> is at least $1 - x_{j_k}$. This gives the lower bound 
<div>
\[\mathbb{P}[B_{\text{dep}}] \geq \prod_{k = 1}^m (1 - x_{j_k}) = \prod_{j \in \mathcal{S} \cap \mathcal{D}_i} (1 - x_j) \geq \prod_{j \in \mathcal{D}_i} (1 - x_j),\]
</div>
Plugging this into <span class = 'crossref'>(2)</span>, we get that 
<div>
\[\mathbb{P}[A_i \mid B_{\text{ind}} \wedge B_{\text{dep}}] \leq \frac{\mathbb{P}[A_i]}{\prod_{j \in \mathcal{D}_i} (1 - x_j)} \leq x_i\]
</div>
(using the bound on $\mathbb{P}[A_i]$ from the given hypothesis), as desired. 

<div class = 'remark'>
    I don't really know how you'd come up with this proof, but I would imagine that you start with the statement of <span class = 'crossref'>Claim 5</span> rather than <span class = 'crossref'>Theorem 4</span> &mdash; you want to find <em>some</em> condition on $\mathbb{P}[A_1]$, $\ldots$, $\mathbb{P}[A_n]$ that ensures $\mathbb{P}[\overline{A_1} \wedge \cdots \wedge \overline{A_n}] > 0$, and one way to ensure this is to try to control the conditional probabilities $\mathbb{P}[A_i \mid \overline{A_1} \wedge \cdots \wedge \overline{A_{i - 1}}]$ (and if we're trying to control these conditional probabilities, we might as well try to control conditional probabilities with more general index sets). 
    <br><br/>
    Once we have the statement of <span class = 'crossref'>Claim 5</span>, the rest of the proof is reasonably intuitive &mdash; we split the event we're conditioning on into a part that we know how to deal with ($B_{\text{ind}}$, which $A_i$ is independent from) and a part we don't know how to deal with ($B_{\text{dep}}$, whose interaction with $A_i$ we can't really say anything about), and get rid of the latter by dropping conditioning (which costs us a factor of $\mathbb{P}[B_{\text{dep}}]$). Then we need a lower bound on $\mathbb{P}[B_{\text{dep}}]$, and if we expand out this probability as a product of conditional probabilities, we realize that we can get a lower bound on each by using induction (each of the terms in the product is of the same form as the one we're trying to bound, but with fewer events being conditioned on). 
    <br><br/>
    And when we plug in that bound, we find that the condition on $\mathbb{P}[A_i]$ that we need to make the proof of the claim go through is precisely the one in <span class = 'crossref'>Theorem 4</span>. 
</div>