---
layout: post
title:  "Adjoint of a Compact Operator"
date:   2024-06-02
categories: analysis functional-analysis
description: A proof of the theorem from functional analysis that the adjoint of a compact linear operator between Banach spaces is also compact. 
tags: 
giscus_comments: false
related_posts: false
---

While studying for the 18.102 (Introduction to Functional Analysis) final this semester, I came across the following theorem, which I think is quite cool (see below for the relevant definitions). 

<div class = 'theorem'>
	Let $X$ and $Y$ be Banach spaces, and suppose that $T \colon X \to Y$ is a compact bounded linear operator. Then its adjoint $T^* \colon Y^* \to X^*$ is also compact. 
</div>

In this post, I'll explain a proof, based on the first answer to this <a href="https://math.stackexchange.com/questions/41432/easy-proof-adjointcompact-compact" target="_blank">Math StackExchange post</a>. 

# Definitions and setup

First, here are some preliminary definitions (stated here for reference). Throughout this post, we work over $\mathbb{C}$. 
<ul>
<li>
<div class = 'sd-hidden' desc = 'Normed linear spaces and Banach spaces'>

<div class = 'definition'>
	We say $X$ is a <span class = 'vocab'>normed linear space</span> if $X$ is a vector space together with a norm $\lVert \bullet \rVert$ satisfying the following three properties:
	<ul>
		<li> $\lVert x \rVert \geq 0$ for all $x \in X$, with equality if and only if $x = 0$.</li>
		<li> Homogeneity &mdash; $\lVert \alpha x\rVert = \lvert \alpha \rvert \lVert x\rVert$ for all $x \in X$ and $\alpha \in \mathbb{C}$. </li>
		<li> The triangle inequality &mdash; $\lVert x + y \rVert \leq \lVert x \rVert + \lVert y \rVert$ for all $x, y \in X$. </li>
	</ul>
</div> 

<div class = 'definition'>
	A <span class = 'vocab'>Banach space</span> is a complete normed linear space. 
</div>

If $X$ is a normed linear space, then the norm induces a metric $d(x, y) = \lVert x - y\rVert$. When we say that $X$ is complete, we're referring to this norm.
</div>
</li>
<li>
<div class = 'sd-hidden' desc = 'Bounded linear operators'>
Next, we'll state a few definitions regarding linear operators between normed linear spaces. 

<div class = 'definition'>
	If $X$ and $Y$ are normed linear spaces and $T \colon X \to Y$ is a linear operator, we say $T$ is <span class = 'vocab'>bounded</span> if there exists a constant $c$ such that $\lVert Tx\rVert \leq c\lVert x\rVert$ for all $x \in X$.
</div> 
<div class = 'definition'>
	If $T$ is a bounded linear operator, we define its <span class = 'vocab'>operator norm</span> as \[\lVert T\rVert = \sup_{x \neq 0} \frac{\lVert Tx\rVert}{\lVert x\rVert}.\] 
</div>

By homogeneity, we could equivalently define $\lVert T\rVert$ as $\sup_{\lVert x\rVert = 1} \lVert Tx\rVert$. 

<div class = 'fact'>
	If $X$ and $Y$ are normed linear spaces, then the set of bounded linear operators $T \colon X \to Y$ form a normed linear space as well (under the operator norm), which we denote by $\mathcal{B}(X, Y)$. Furthermore, if $Y$ is Banach, then so is $\mathcal{B}(X, Y)$. 
</div>
</div>
</li>
<li>
<div class = 'sd-hidden' desc = 'Dual spaces and adjoints'>
<div class = 'definition'>
	For a normed linear space $X$, its <span class = 'vocab'>dual space</span> $X^\ast$ is defined as $\mathcal{B}(X, \mathbb{C})$. We refer to elements of $X^\ast$ (which are bounded linear functions $f \colon X \to \mathbb{C}$) as <span class = 'vocab'>functionals</span>.
</div>

Note that since $\mathbb{C}$ is Banach (i.e., complete), so is $X^*$ (for any normed linear space $X$). 

<div class = 'definition'>
	For any bounded linear operator $T \colon X \to Y$, we define its <span class = 'vocab'>adjoint operator</span> $T^* \colon Y^* \to X^*$ as the map such that $(T^\ast g)(x) = g(Tx)$ for all $g \in Y^\ast$ and $x \in X$. 
</div>

In other words, we're defining $T^\ast$ as the map sending each bounded linear operator $g \colon Y \to \mathbb{C}$ (which is an element of $Y^\ast$) to the bounded linear operator $g \circ T \colon X \to \mathbb{C}$ (an element of $X^\ast$). 

<div class = 'fact'>
	If $T$ is a bounded linear operator, then so is $T^\ast$. 
</div>
</div>
</li>
<li>
<div class = 'sd-hidden' desc = 'Compactness in metric spaces'>
<div class = 'definition'>
	For a metric space $X$, we say a subset $M \subseteq X$ is <span class = 'vocab'>compact</span> if every open cover of $M$ has a finite subcover &mdash; in other words, for any collection $\mathcal{U}$ of open sets whose union contains $M$, we can find a <em>finite</em> subcollection of $\mathcal{U}$ whose union still contains $M$. 
</div>

<div class = 'definition'>
	For a metric space $X$, we say a subset $M \subseteq X$ is <span class = 'vocab'>sequentially compact</span> if every sequence $(x_n) \subseteq M$ has a subsequence converging to some point in $M$. 
</div>

<div class = 'fact'>
	Compactness and sequential compactness are equivalent (for metric spaces). 
</div>

The reason there's two separate terms is because both can be defined in greater generality for general topological spaces, and there they're not necessarily equivalent. In this post, we'll make use of both notions. 
</div>
</li>
</ul>

The main focus of this post is a result about compact operators, so now we'll discuss what it means for an operator to be compact. (We'll only consider the case where we're working with bounded linear operators between two Banach spaces.) 

<div class = 'definition'>
	For Banach spaces $X$ and $Y$, we say a bounded linear operator $T \colon X \to Y$ is <span class = 'vocab'>compact</span> if for every bounded sequence $(x_n) \subseteq X$, its image $(Tx_n) \subseteq Y$ has a convergent subsequence. 
</div>

There's an equivalent characterization of when an operator is compact, which may make it more clear where the name comes from. (In the post, we're going to make use of both characterizations.)

<div class = 'lemma'>
	For Banach spaces $X$ and $Y$, an operator $T \colon X \to Y$ is compact if and only if for every bounded subset $M \subseteq X$, its image $TM \subseteq Y$ has compact closure. 
</div>

We'll use $\operatorname{Cl}(M)$ to refer to the closure of a subset $M$, so the condition here states that $\operatorname{Cl}(TM) \subseteq Y$ is compact (as a metric space) whenever $M \subseteq X$ is bounded. We'll refer to this condition as $(\star)$. 



<div class = 'proof'>
	First, the backwards direction is pretty direct &mdash; suppose that $T$ has the property $(\star)$. Consider some bounded sequence $(x_n) \subseteq X$, and let $M = \{x_n \mid n \in \mathbb{N}\}$ be the set it forms (so that $M$ is bounded). Then $(Tx_n)$ is contained in $\operatorname{Cl}(TM)$, which is compact by $(\star)$; so it must have a convergent subsequence. 
	<br/><br/>
	Now we'll prove the forwards direction &mdash; we'll suppose that $T$ is compact (as in the original definition) and show that it satisfies $(\star)$. Fix some bounded set $M \subseteq X$. We want to show $\operatorname{Cl}(M)$ is compact, and we'll do so by showing that it's <em>sequentially</em> compact &mdash; i.e., that any $(y_n) \subseteq \operatorname{Cl}(TM)$ has a convergent subsequence whose limit is also in $\operatorname{Cl}(TM)$.
	<br><br/>
	First, to apply the compactness of $T$, we really want to be working with a sequence in $TM$ rather than its closure, so the first step is to replace $(y_n) \subseteq \operatorname{Cl}(TM)$ with a sequence $(y_n') \subseteq TM$ that's 'close' to it &mdash; since $TM$ is dense in its closure, for each $n \in \mathbb{N}$ we can find some $y_n' \in TM$ with $\lVert y_n - y_n'\rVert < \frac{1}{n}$, and since $y_n' \in TM$, we can find $x_n \in M$ with $y_n' = Tx_n$. 
	<br><br/>
	And now the sequence $(x_n)$ is bounded (as it's contained in $M$, which is bounded), so by the compactness of $T$, its image $(y_n') = (Tx_n) \subseteq Y$ must have a convergent subsequence. And since $\lVert y_n - y_n'\rVert \to 0$ (as $n \to \infty$), this means the corresponding subsequence of $(y_n)$ is convergent as well. 
	<br><br/>
	Finally, we've found a subsequence of $(y_n)$ that converges to <em>some</em> point $y \in Y$. But since $\operatorname{Cl}(TM)$ is closed and each $y_n$ is in $\operatorname{Cl}(TM)$, this means $y \in \operatorname{Cl}(TM)$ as well. This proves that $\operatorname{Cl}(TM)$ is sequentially compact (and therefore compact), so we're done. 
</div>

# The proof

We'll now prove the main theorem. Suppose that $T \colon X \to Y$ is compact. Our goal is to show that $T^\ast \colon Y^\ast \to X^\ast$ is compact as well; using the definition of a compact operator, this means we've got some bounded sequence $(g_n) \subseteq Y^\ast$, and we want to show that $(T^\ast g_n) \subseteq X^\ast$ has a convergent subsequence. 

## Step 1 &mdash; reformulating the conclusion

First, we're going to find a sufficient condition for a sequence $(T^\ast g_n) \subseteq X^\ast$ to be convergent that's more convenient to work with (and we're going to find a subsequence satisfying this property instead). 

<div class = 'claim'>
	Let $B = \{x \in X \mid \lVert x\rVert \leq 1\}$ be the closed unit ball in $X$, and let $K = \operatorname{Cl}(TB)$ be the closure of its image in $Y$. 

	Suppose that $(g_n) \subseteq Y^\ast$ is a sequence of functionals such that for every $\varepsilon > 0$, there exists $N$ such that for all $m, n \geq N$ we have \[\sup_{y \in K} \lvert g_m(y) - g_n(y)\rvert \leq \varepsilon.\] Then the sequence $(T^\ast g_n) \subseteq X^\ast$ is convergent. 
</div>

<div class = 'proof'>
	First, it's enough to show that $(T^\ast g_n)$ is <em>Cauchy</em> &mdash; this automatically implies it's convergent, as $X^\ast$ is complete. So we want to show that for every $\varepsilon > 0$, there exists $N$ such that for all $m, n \geq N$ we have $\lVert T^\ast g_m - T^\ast g_n\rVert \leq \varepsilon$. 

	But for any functional $g$, we have \[\lVert T^\ast g\rVert = \sup_{\lVert x\rVert = 1} \lvert (T^\ast g)(x)\rvert = \sup_{\lVert x\rVert = 1} \lvert g(Tx)\rvert \leq \sup_{y \in K} \lvert g(y)\rvert\] (since if $\lVert x\rVert = 1$, then $Tx \in TB \subseteq K$). Taking $g = g_m - g_n$, we get \[\lVert T^\ast g_m - T^\ast g_n\rVert \leq \sup_{y \in K} \lvert g_m(y) - g_n(y)\rvert\] for all $m$ and $n$; so if we know the right-hand side is at most $\varepsilon$ for all $m, n \geq N$, then so is the left-hand side (with the same value of $N$). 
</div>

Note that $K$ is compact (because $T$ is compact, and $B \subseteq X$ is bounded) &mdash; this is the one place where we're going to use the compactness of $T$. (After this, $T$ will basically disappear from the picture.)

## Step 2 &mdash; finding a sequence of representatives

Now our goal is to find a subsequence $(g_n') \subseteq (g_n)$ satisfying the condition of <span class = 'crossref'>Claim 4</span>. The first step towards doing so is finding a countable sequence of 'representatives' $y_1$, $y_2$, $\ldots$ for $K$ with certain nice properties. The reason for this is that we're then going to construct our subsequence $(g_n') \subseteq (g_n)$ such that it converges pointwise at each of these representatives $y_i$, and argue that this implies the desired conclusion. 

<div class = 'claim'>
	There exists a sequence $(y_i) \subseteq K$ with the property that for every $\varepsilon > 0$, there exists $N$ such that every $y \in K$ is within $\varepsilon$ of one of $y_1$, $\ldots$, $y_N$.
</div>

<div class = 'proof'>
	First, the compactness of $K$ implies that for every $n \in \mathbb{N}$, we can cover $K$ with a finite number of balls of radius $\frac{1}{n}$ &mdash; i.e., there exists a finite set of points $S_n \subseteq K$ such that $\bigcup_{y \in S_n} \mathbb{B}(z, \frac{1}{n}) \supseteq K$. (This is because the balls $\mathbb{B}(y, \frac{1}{n})$ over <em>all</em> points $y \in K$ form an open cover of $K$, and by the compactness of $K$, this open cover must have a finite subcover.)
	<br><br/>
	Then we can take $(y_i)$ to consist of the points in $S_1$, then $S_2$, then $S_3$, and so on (in that order). 
	<br><br/>
	<center><img src='/assets/img/compact3.png' width='600' height='auto'></center>
	<br><br/>
	This will have the desired property &mdash; given any $\varepsilon > 0$, we can fix $n \in \mathbb{N}$ such that $\frac{1}{n} \leq \varepsilon$. Then every $y \in K$ is within $\varepsilon$ of some point in $S_n$, so we can take $N = \lvert S_1\rvert + \cdots + \lvert S_n\rvert$.
</div>

## Step 3 &mdash; defining the subsequence

Now we're going to define our subsequence $(g_n') \subseteq (g_n)$, so that it has the following property. 

<div class = 'lemma'>
	There is a subsequence $(g_n') \subseteq (g_n)$ such that for each $i \in \mathbb{N}$, the sequence $(g_n'(y_i)) \subseteq \mathbb{C}$ is convergent (as $n \to \infty$). 
</div>

<div class = 'proof'>
	We're going to use a diagonalization argument &mdash; the idea is that we'll first restrict to a subsequence along which $(g_n(y_1))$ converges, then further restrict to a subsequence along which $(g_n(y_2))$ converges, and so on; and in the end, we'll take the 'diagonal' of all these subsequences. 
	<br><br/>
	First, we know the sequence $(g_n)$ is bounded (by assumption), so there's some $c$ such that $\lVert g_n\rVert \leq c$ for all $n \in \mathbb{N}$. This means $\lvert g_n(y_1)\rvert \leq c\lVert y_1\rVert$ for all $n$, so the sequence $(g_n(y_1))$ is a bounded sequence in $\mathbb{C}$, which means it has a convergent subsequence (as it's a sequence in a compact subset of $\mathbb{C}$). So we can define a subsequence $(g_{n1}) \subseteq (g_n)$ such that $(g_{n1}(y_1))$ is convergent. 
	<br><br/>
	Next, we're going to further restrict this subsequence to deal with $y_2$ &mdash; we have $\lvert g_{n1}(y_2)\rvert \leq c\lVert y_2\rVert$ for all $n$, so the sequence $(g_{n1}(y_2))$ is also a bounded sequence in $\mathbb{C}$, which means it also has a convergent subsequence. So we can define a subsequence $(g_{n2}) \subseteq (g_{n1})$ such that $(g_{n2}(y_2))$ is convergent. 
	<br><br/>
	And we can continue doing this to get a nested list of subsequences $(g_n) \supseteq (g_{n1}) \supseteq (g_{n2}) \supseteq \cdots$ such that for each fixed $i \in \mathbb{N}$, the sequence $(g_{ni}(y_i)) \subseteq \mathbb{C}$ is convergent. 
	<br><br/>
	<center><img src='/assets/img/compact1.png' width='600' height='auto'></center>
	<br><br/>
	Finally, we define the subsequence $(g_n')$ by $g_n' = g_{nn}$ for each $n \in \mathbb{N}$ &mdash; so we're essentially taking the 'diagonal' of our list of nested subsequences. 
	<br><br/>
	<center><img src='/assets/img/compact2.png' width='600' height='auto'></center>
	<br><br/>
	(For example, in the above picture our sequence $(g_n')$ begins with $g_1$, $g_3$, $g_9$, $\ldots$.)
	<br><br/>
	To see that this works, fix some $i \in \mathbb{N}$. Then if we take the sequence $(g_n'(y_i))$ and remove the first $i - 1$ terms, the resulting sequence is a subsequence of $(g_{ni}(y_i))$, which is convergent by construction; so this means $(g_n'(y_i))$ is convergent as well. 
</div>

## Step 4 --- concluding

Finally, we're ready to conclude &mdash; we'll use the fact that $(y_i)$ forms a nice sequence of representatives for $K$ (from <span class = 'crossref'>Claim 5</span>) together with the fact that $(g_n')$ converges pointwise at each $y_i$ (from <span class = 'crossref'>Lemma 6</span>) to conclude that $(g_n')$ satisfies the condition described in <span class = 'crossref'>Claim 4</span>. 

<div class = 'claim'>
	For every $\varepsilon > 0$, there exists $N$ such that for all $m, n \geq N$ and all $y \in K$, we have \[\lvert g_m'(y) - g_n'(y)\rvert \leq \varepsilon.\]
</div>

<div class = 'proof'>
	First, let $c$ be a constant such that $\lVert g_n\rVert \leq c$ for all $n \in \mathbb{N}$ (such $c$ exists because $(g_n)$ is bounded). 
	<br><br/>
	Now fix $\varepsilon > 0$. Then using <span class = 'crossref'>Claim 5</span>, we can first find a constant $M$ such that for every $y \in K$, there is some $i \in \{1, \ldots, M\}$ with $\lVert y - y_i\rVert \leq \frac{\varepsilon}{4c}$; fix this value of $M$. 
	<br><br/>
	Then for each $i \in \{1, \ldots, M\}$, we know the sequence $(g_n'(y_i))$ is convergent and therefore Cauchy (by <span class = 'crossref'>Lemma 6</span>), so there is some $N_i$ such that for all $m, n \geq N_i$ we have $\lVert g_m'(y_i) - g_n'(y_i)\rVert \leq \frac{\varepsilon}{2}$. 
	<br><br/>
	Finally, let $N = \max\{N_1, \ldots, N_M\}$. To see that this has the desired property, consider any $y \in K$, and fix $y_i$ with $i \in \{1, \ldots, M\}$ such that $\lVert y - y_i\rVert \leq \frac{\varepsilon}{4c}$. Then we can write \[\lVert g_m'(y) - g_n'(y)\rVert \leq \lVert g_m'(y) - g_m'(y_i)\rVert + \lVert g_m'(y_i) - g_n'(y_i)\rVert + \lVert g_n'(y_i) - g_n'(y)\rVert\] by the triangle inequality. (The intuition here is that we know $g_m'$ and $g_n'$ should be 'close' at $y_i$, and we know $y$ should be close to $y_i$, so we should be able to bound each of these terms.)
	<br><br/>
	For the first term, we have \[\lVert g_m'(y) - g_m'(y_i)\rVert \leq \lVert g_m'\rVert \lVert y - y_i\rVert \leq c\cdot \frac{\varepsilon}{4c} = \frac{\varepsilon}{4}\] (the first inequality is by the definition of the operator norm of $g_m'$). We can do the same for the last term to get that it's at most $\frac{\varepsilon}{4}$ as well. Finally, for the middle term, we have \[\lVert g_m'(y_i) - g_n'(y_i)\rVert \leq \frac{\varepsilon}{2},\] since we know $m, n \geq N \geq N_i$ and we defined $N_i$ to guarantee this inequality holds. 
	<br><br/>
	Putting these together gives that \[\lVert g_m'(y) - g_n'(y)\rVert \leq \frac{\varepsilon}{4} + \frac{\varepsilon}{2} + \frac{\varepsilon}{4} = \varepsilon\] for all $m, n \geq N$ and $y \in K$, as desired. 
</div>
This concludes the proof of <span class = 'crossref'>Theorem 1</span> &mdash; we've started with an arbitrary bounded sequence $(g_n) \subseteq Y^*$ and obtained a subsequence $(g_n') \subseteq (g_n)$ satisfying the condition of <span class = 'crossref'>Claim 4</span> (using <span class = 'crossref'>Claim 5</span> to choose a nice set of representatives, <span class = 'crossref'>Lemma 6</span> to find a subsequence converging pointwise at each representative, and finally using the triangle inequality to get the conclusion, as stated in <span class = 'crossref'>Claim 7</span>), which by <span class = 'crossref'>Claim 4</span> means that the sequence $(T^\ast g_n')$ is convergent. 