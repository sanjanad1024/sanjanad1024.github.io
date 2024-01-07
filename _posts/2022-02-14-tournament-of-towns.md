---
layout: post
title:  "Tournament of Towns"
date:   2022-02-14
categories: misc olympiad
description: some cool problems from the Tournament of Towns
tags: 
giscus_comments: false
related_posts: false
---

The [Tournament of Towns](https://www.turgor.ru/en/problems/) is an international math olympiad based in Russia that runs twice a year. It has a junior and senior division, and O-level and A-level (according to the website, the A-level test is comparable with All-Russian MO or IMO difficulty; O-level is easier). I didn't know much about it as a contestant, but this winter I was looking at the problems, and it has a lot of cool problems &mdash; especially in combinatorics. 

Here are some problems from it that I like (although there are many other interesting-looking problems I have not yet done). My favorites are **ToT Spring 2021 J-A7** (on a frog jumping on the number line) and **ToT Spring 2019 S-A7** (on worm skeletons). 

<div class='problem' text='ToT Fall 2019 S-A4'>
	Consider an increasing sequence of positive numbers \[\cdots < a_{-2} < a_{-1} < a_0 < a_1 < a_2 < \cdots,\] infinite in both directions. For a positive integer $k$ let $b_k$ be the minimal integer such that the ratio of the sum of any consecutive $k$ elements of the sequence to the largest of those $k$ elements is not greater than $b_k$. Prove that $b_1$, $b_2$, $b_3$, $\ldots$ either is $1$, $2$, $3$, $\ldots$ or is eventually constant.
</div>

<div class='sd-hidden' desc='Solution'>
	First note that \[\frac{a_{i - k + 1} + a_{i - k + 2} + \cdots + a_i}{a_i} \leq \frac{ka_i}{a_i} \leq k\] for all $i$, so then $b_k \leq k$ for all $k$. Also, \[\frac{a_{i - k} + a_{i - k + 1} + \cdots + a_i}{a_i} \geq \frac{a_{i - k + 1} + \cdots + a_i}{a_i},\] so then $b_{k + 1} \geq b_k$ for all $k$. So it suffices to show that if $b_k \leq k - 1$ for some $k$, the $b_i$ are bounded. 
	<br/><br/>
	Let $s_i = a_{i - k + 2} + a_{i - k + 2} + \cdots + a_i$ for each integer $i$. 
	<div class='claim-un'>
		There exists a constant $r < 1$ such that $s_{i - 1} \leq rs_i$ for all $i$. 
	</div>
	<div class='proof'>
    	We have $a_{i - k + 1} + a_{i - k + 2} + \cdots + a_i \leq (k - 1)a_i$, which rearranges to \[\frac{a_{i - k + 1} + a_{i - k + 2} + \cdots + a_i}{a_{i - k + 1} + a_{i - k + 2} + \cdots + a_{i - 1}} \geq \frac{k - 1}{k - 2}.\] But since the $a_i$ are increasing, \[\frac{a_{i - k + 2} + \cdots + a_i}{a_{i - k + 1} + \cdots + a_i} \geq \frac{k - 1}{k}.\] Combining these gives \[\frac{s_i}{s_{i - 1}} \geq \frac{(k - 1)^2}{k(k - 2)} = 1 + \frac{1}{k(k - 2)},\] as desired. (Note that $k \leq 2$ is impossible.) 
	</div>
	Then we have \[\sum_{j \leq i} a_j = s_i + s_{i - (k - 1)} + s_{i - 2(k - 1)} + \cdots \leq s_i(1 + r^{k - 1} + r^{2(k - 1)} + \cdots) \leq cs_i\] for some constant $c$. But $s_i \leq (k - 1)a_i$, so then for any $\ell$ the ratio of the sum of any $\ell$ consecutive terms to the largest is at most $c(k - 1)$, so the $b_i$ are bounded.
</div>

<div class='problem' text='ToT Fall 2009 S-A4'>
	Denote by $[n]!$ the product $1 \cdot 11 \cdot \underbrace{11\cdots 1}_{n}$. Prove that $[n + m]!$ is divisible by $[n]!\times[m]!$.
</div>

<div class='sd-hidden' desc='Solution'>
	We have $[n]! = \frac{10 - 1}{9} \cdot \frac{10^2 - 1}{9} \cdots \frac{10^n - 1}{9}$, so it suffices to show \[\prod_{i = 1}^n (10^i - 1) \cdot \prod_{i = 1}^m (10^i - 1) \mid \prod_{i = 1}^{m + n} (10^i - 1).\] We'll show that for all primes $p$, $\nu_p$ of the RHS is at least $\nu_p$ of the LHS. 
	<br/><br/>
	Suppose $p$ is relatively prime to $10$ (otherwise $p$ cannot divide either side), and let $d = \operatorname{ord}_p 10$. Then only the terms with $i \mid d$ are relevant. If $ad$ and $bd$ are the greatest multiples of $d$ at most $n$ and $m$, respectively, then $(a + b)d \leq n + m$, so it suffices to show \[\sum_{i = 1}^a \nu_p(10^{id} - 1) + \sum_{i = 1}^b \nu_p(10^{id} - 1) \leq \sum_{i = 1}^{a + b} \nu_p(10^{id} - 1).\] But we have \[\nu_p(10^{id} - 1) = \nu_p(10^d - 1) + \nu_p(id) = \nu_p(10^d - 1) + \nu_p(i)\] by LTE. So it suffices to show \[\sum_{i = 1}^a \nu_p(i) + \sum_{i = 1}^b \nu_p(i) \leq \sum_{i = 1}^{a + b} \nu_p(i),\] which is true as $\binom{a + b}{a}$ is an integer.
</div>

<div class='problem' text='ToT Fall 2011 S-A6'>
	Prove that for $n \geq 2$, the integer \[1^1 + 3^3 + 5^5 + \cdots + (2^n - 1)^{2^n - 1}\] is a multiple of $2^n$ but not a multiple of $2^{n + 1}$.
</div>

<div class='sd-hidden' desc='Solution'>
	Induct on $n$. In the base case $n = 2$, $1^1 + 3^3 = 28$ is a multiple of $4$ but not $8$. 
	<br/><br/>
	Now suppose $n \geq 3$ and assume this is true for $n - 1$, so \[1^1 + 3^3 + \cdots + (2^{n - 1} - 1)^{2^{n - 1} - 1} \equiv 2^{n - 1} \pmod{2^n}.\] Now note that for all odd $x$, we have \[\nu_2(x^{2^{n - 1}} - 1) = \nu_2(x^2 - 1) + \nu_2(2^{n - 2}) \geq n + 1,\] so then $x^{2^{n - 1}}$ is always $1$ mod $2^{n + 1}$. So then \[(x + 2^{n - 1})^{x + 2^{n - 1}} \equiv (x + 2^{n - 1})^x \equiv x^x + x^x \cdot 2^{n - 1},\] since $2(n - 1) \geq n + 1$. So this means \[1^1 + 3^3 + \cdots + (2^n - 1)^{2^n - 1} \equiv (2 + 2^{n - 1})(1^1 + 3^3 + \cdots + (2^{n - 1} - 1)^{2^{n - 1} - 1}) \pmod{2^{n + 1}}.\] But $2 + 2^{n - 1}$ is even, and the second sum is $2^{n - 1}$ mod $2^n$ by the induction hypothesis, so then the original sum is $2^n$ mod $2^{n + 1}$, as desired.
</div>

<div class='sd-hidden' desc='Comments'>
	It makes sense to induct because the sum is pretty intractable on its own, but when you lift from $2^n$ to $2^{n + 1}$, a lot of the terms are either the same or closely related. 
</div>

<div class='problem' text='ToT Spring 2017 S-A6'>
	Find all positive integers $n$ such that for every $k \geq n$, there is a number divisible by $n$ with sum of digits $k$.
</div>

<div class='sd-hidden' desc='Solution'>
	The answer is all $n$ not divisible by $3$. First if $3$ divides $n$, then $3$ divides the sum of digits of all multiples of $n$. 
	<br/><br/>
	Now suppose $3 \nmid n$. Let $n = 2^a5^bm$ where $m$ is relatively prime to $10$; then it suffices to show $m$ has this property (as we can append enough $0$'s to multiples of $n$ to get multiples of $n$ with the same sum of digits). 
	<br/><br/>
	Take $x \equiv \frac{10k}{9} \pmod{m}$ with $0 \leq x < m$, and $y = k - x \equiv -\frac{k}{9} \pmod{m}$, which is positive as $k \geq m$. Then take the number with $x$ $1$'s corresponding to digits $10^i$ with $i$ divisible by $\varphi(n)$, and $y$ $1$'s corresponding to digits $10^i$ with $i \equiv 1$ mod $\varphi(n)$, and $0$'s everywhere else. This has sum of digits $k$, and is congruent to \[\frac{10k}{9} + 10 \cdot \left(-\frac{k}{9}\right) \equiv 0 \pmod{m},\] so this is a multiple of $m$ with sum of digits $k$.
</div>

<div class='problem' text='ToT Fall 2016 S-A6'>
	Petya and Vasya play the following game. Petya conceives a polynomial $P(x)$ having integer coefficients. On each move, Vasya pays him a coin, and calls an integer $a$ of his choice, which has not yet been called by him. Petya replies with the number of distinct integer solutions $x$ to $P(x) = a$. The game continues until Petya repeats an answer. What is the minimum amount of rubles Vasya must pay to guarantee a win?
</div>

<div class='sd-hidden' desc='Solution'>
	The answer is $4$. Let $f(x)$ be the answer Petya gives when Vasya queries $x$. 
	<br/><br/>
	First we'll show that Vasya can win in $4$ turns. The main idea is to use $a - b \mid P(a) - P(b)$ and query consecutive values for $P$ &mdash; this means if $P(a) = P(b) + 1$, then $a = b\pm 1$. 
	<br/><br/>
	Vasya first queries $0$. If $f(0) \geq 3$, he queries $1$ and $-1$, and both must be $0$ (for the above reason) so he wins in $3$ moves. 
	<br/><br/>
	If $f(0) = 0$, then he queries $1$, $2$, and $3$. For this to fail, at least one of them has $f(x) \geq 3$, but then $f(x - 1) = f(x + 1) = 0$ and then there would be two zeroes, so Vasya still wins in $4$.
	<br/><br/>
	If $f(0) = 1$, then he queries $f(1)$ and $f(-1)$, which must both be $0$, $1$, or $2$. Assume all are distinct, so WLOG $f(1) = 2$ and $f(-1) = 0$. Then he queries $2$, and he must get $f(2) = 0$ &mdash; this is because if $P(x) = 2$, then $x$ would have to be one away from both solutions to $P(x) =1$, but the unique integer satisfying this is the solution to $P(x) = 0$. So he wins in $4$ again.
	<br/><br/>
	Finally, if $f(0) = 2$, then he queries $f(1)$ and $f(-1)$, and for the same reason as above, both must be $0$ or $1$, so WLOG $f(1) = 1$ and $f(-1) = 0$. Then he queries $2$, and he must get $f(2) \leq 2$ (because $f(1)$ is nonzero), so he wins in $4$. 
	<br/><br/>
	Now we show that he can't win in $3$ moves &mdash; have Petya answer $f(a) = 0$, $f(b) = 1$, and $f(c) = 2$. 
	<br/><br/>
	Then take the polynomial $P(x) = (c - b)\cdot x^k + b$, for some even $k$ we will specify later. Then the only solution to $P(x) = b$ is $x = 0$, and the only solutions to $P(x) = c$ are $\pm 1$. Finally, we want $\frac{a - b}{c - b}$ to not be a $k$th power. Clearly this is not $0$ or $1$, so it isn't a $k$th power for all even $k$ and we can choose some $k$ such that $P(x) = a$ has no solutions. 
	<br/><br/>
	So then no matter what Vasya's first three queries are, Petya can give answers $0$, $1$, and $2$ which are consistent with some polynomial, which means Vasya must use at least $4$ turns.
</div>

<div class='problem' text='ToT Fall 2019 S-A7'>
	Some of the integers $1$, $2$, $\ldots$, $n$ have been colored red so that for each triplet of red numbers $a$, $b$, $c$ (not necessarily distinct), if $a(b - c)$ is a multiple of $n$ then $b = c$. Prove that there are no more than $\varphi(n)$ red numbers.
</div>

<div class='sd-hidden' desc='Solution'>
	Let $p_1 < p_2 < \cdots < p_r$ be the primes dividing $n$ which divide some red number, and $q_1$, $q_2$, $\ldots$, $q_s$ the primes dividing $n$ which don't divide any red number. If $r = 0$ then all red numbers are relatively prime to $n$ so there are at most $\varphi(n)$ red numbers; now assume $r \geq 1$. 
	<div class='claim-un'>
    	There are at most $\frac{n}{p_r} \cdot \prod \frac{q_i - 1}{q_i}$ red numbers. 
	</div>
	<div class='proof'>
    	Let $n = p_r\cdot m$, so $m$ is divisible by each of the $q_i$. Then each residue class mod $m$ contains at most one red number (if $b \equiv c \pmod{m}$, then take $a$ to be a red multiple of $p_r$, so $a(b - c)$ is a multiple of $n$). Additionally, all red numbers are relatively prime to each $q_i$, so all red residues mod $m$ are relatively prime to each $q_i$ as well. So then there are at most $m \prod \frac{q_i - 1}{q_i}$ red numbers. 
	</div>
	But we have \[\varphi(n) = n \cdot \prod_{i = 1}^r \frac{p_i - 1}{p_i} \cdot \prod_{i = 1}^s \frac{q_i - 1}{q_i}.\] We have the bound \[\prod_{i = 1}^r \frac{p_i - 1}{p_i} \geq \prod_{i = 2}^{p_i} \frac{i - 1}{i} = \frac{1}{p_i},\] so then our bound on the count of red numbers is at most $\varphi(n)$.
</div>

<div class='sd-hidden' desc='Comments'>
	You can start by trying small cases &mdash; here the small case is when we only have one prime dividing $n$ with a red multiple since having none gives exactly $\varphi(n)$. If we look at which residues are allowed then we get exactly $\frac{n}{p}\prod \frac{q - 1}{q}$. Then if we try to go to a more general case, we realize that there <em>isn't</em> really a much better bound we can get (at least, not one that I could find), so we probably still have to use this bound &mdash; and then thinking about size a bit finishes. 
</div>

<div class='problem' text='ToT Spring 2009 S-A7'>
	Initially, the number $6$ is written on a blackboard. On the $n$th step (for $n \geq 1$), if the number $k$ is on the blackboard, it is replaced with $k + \gcd(k, n)$. Prove that at each step, the number on the blackboard increases either by $1$ or by a prime number.
</div>

<div class='hidden' desc='Solution'>
	The first step is $6 \to 7$, the second step is $7 \to 8$, and the third step is $8 \to 9$.  
	<div class='claim-un'>
    	Suppose that after step $n$, the number on the blackboard is $3n$. Then if the number next increases by more than $1$ on step $m$, it increases by a prime and becomes $3m$. 
	</div>
	<div class='proof'>
		We have that $m$ is the smallest integer greater than $n$ for which \[\gcd(m, 3n + m - n - 1) = \gcd(m, 2n - 1) > 1.\] But if $p$ is any prime dividing $2n - 1$, then $n \equiv \frac{p + 1}{2} \pmod{p}$, so the smallest $m > n$ with $p \mid m$ is \[m = n + \frac{p - 1}{2}.\] So then the first $m$ for which the gcd is not $1$ is $m = n + \frac{p - 1}{2}$ where $p$ is the smallest prime dividing $2n - 1$, and here the gcd is \[\gcd(2n + p - 1, 2n - 1) = p.\] So step $m$ is \[3n + \frac{p - 1}{2} - 1 \to 3n + \frac{p - 1}{2} - 1 + p = 3m,\] as desired. 
	</div>
	Since after step $3$ the number is $3\cdot 3$, by induction this means all additions are $1$ or prime, and each prime addition results in $3m$ on the board after turn $m$.
</div>

<div class='hidden' desc='Comments'>
	I think this is a pretty rigid problem &mdash; the idea is to try out the process for small values of $n$, and then you notice that there's a pattern: every nontrivial jump ends up in a position $(n, 3n)$, which is surprising but turns out to be not that hard to prove. Maybe it is even expected that the sequence should have some nice property like this, because if there's no good relationship between $n$ and $k$, the problem seems pretty intractable. But even without that heuristic, doing small cases is a good idea. 
</div>

<div class='problem' text='ToT Spring 2011 S-A6'>
	In every cell of a square table is a number. The sum of the largest two numbers in each row is $a$ and the sum of the largest two numbers in each column is $b$. Prove that $a = b$.
</div>

<div class='hidden' desc='Solution'>
	Assume not, so WLOG $a < b$. Label the rows and columns $1$ through $n$. Draw a graph on $n$ vertices and for each column, draw an edge between the row numbers with the two greatest elements of that column (breaking ties arbitrarily) &mdash; multiple edges between two vertices are allowed. 
	<br/><br/>
	This graph has $n$ vertices and $n$ edges, so it must contain a cycle $(r_1r_2\cdots r_k)$, possibly of length $2$. Suppose edge $r_ir_{i + 1}$ corresponds to column $c_i$. Then if $x(r, c)$ denotes the entry in $(r, c)$, we have \[\sum x(r_i, c_i) + x(r_{i + 1}, c_i) = bn\] by looking at columns. But \[\sum x(r_i, c_{i - 1}) + x(r_i, c_i) \leq an\] by looking at rows (since each term is at most the sum of the two largest numbers in each row). But these are the same sum, so $bn \leq an$, contradiction as $a < b$.   
</div>

<div class='hidden' desc='Comments'>
	I think this is an arrows problem &mdash; the idea is that you draw a vertical line for each column connecting the two largest numbers in that column, and then you can get a cycle by adding in horizontal lines, which are at most the largest numbers in the rows. 
	<br/><br/>
	<center><img src='/assets/img/11ttssa6-arrows.png' width='250' height='auto'></center>
</div>

<div class='problem' text='ToT Spring 2016 J-A7'>
	Let $N \geq 3$ be a positive integer. There are $N$ batteries at least half of which are good; the remaining batteries are bad. A lamp uses two batteries and works if and only if both of them are good. What is the least number of attempts needed to guarantee the lamp works?
</div>

<div class='hidden' desc='Solution'>
	Draw a graph, where the batteries are vertices and queries are edges. Then a graph works iff it is impossible to label at most half of the vertices bad such that every edge has a bad vertex. 
	<br/><br/>
	If $N = 2n + 1$, then the answer is $n + 2$. To achieve this, take a triangle ($3$ edges) and pair up the remaining batteries ($\frac{2n + 1 - 3}{2} = n - 1$ edges). Some component must have strictly more good than bad batteries, and this component will have an edge between two good batteries. 
	<br/><br/>
	Now we show $n + 1$ doesn't work (which clearly means less than $n  +1$ doesn't work either). First, the average degree is $\frac{2(n + 1)}{2n + 1} > 1$, so some vertex has degree at least $2$. First label this vertex bad, which takes care of at least $2$ edges. Now we have $n - 1$ edges left and $n - 1$ possible bad batteries, so we can just label any of the two vertices bad for each edge. 
	<br/><br/>
	If $N = 2n$ with $n \geq 3$, then the answer is $n + 3$. To achieve this, take two disjoint triangles ($6$ edges) and pair up the remaining batteries ($\frac{2n - 6}{2} = n - 3$ edges). Again some component must have strictly more good than bad batteries (because triangles can't have equality), and this component will have an edge between two good batteries. 
	<br/><br/>
	Now we show $n + 2$ doesn't work. Similarly to before, the average degree is $\frac{2(n + 2)}{2n} > 1$, so some vertex has degree at least $2$. If this vertex has degree at least $3$, then we can label it bad, and end up with $n - 1$ edges and $n - 1$ possible bad batteries, so we win. If all vertices have degree at most $2$, then when we label one of these vertices bad, we're left with $n$ edges and $n - 1$ batteries, so the average degree is still greater than $1$ and there must be another vertex with degree at least $2$. Then after we label it bad, we're left with $n - 2$ edges and $n - 2$ possible bad batteries, so we also win. 
	<br/><br/>
	Finally, for $N = 4$ the answer is $6$, achievable by querying everything. If there are less than $6$ queries, then some edge was not queried, so if we label its two vertices good and the others bad, then every edge has a bad vertex.
</div>
<div class='problem' text='ToT Spring 2016 S-A4'>
	There are $64$ towns in a country, and some pairs of towns are connected by roads but we don't know these pairs. We may choose any pair of towns and find out whether they are connected by a road. Our aim is to determine whether it is possible to travel between any two towns using roads. Prove that there is no algorithm which would enable us to do this in less than $2016$ questions.
</div>

<div class='hidden' desc='Solution'>
	Call the person answering us the <em>oracle</em>; we'll show that the oracle can ensure that at every step until the end, the current knowledge is compatible with both the graph being connected and not. 
	<br/><br/>
	Color an edge red if the oracle answers no, and blue if yes. Define a <em>blob</em> to be a set of vertices $S$ such that all edges in $S$ are drawn, and the blue edges form exactly a tree. Call a position <em>great</em> if it is a collection of blobs (possibly with size 1) so that there are no blue edges between distinct blobs. 
	<br/><br/>
	Then the oracle can preserve greatness: suppose the position is great, and we query $uv$ with $u$ in blob $S$ and $v$ in blob $T$. The oracle answers no unless every other edge between blobs $S$ and $T$ has been queried (and colored red), in which case he answers yes. 
	<br/><br/>
	This preserves greatness, as an answer of yes merges the blobs into a bigger blob. The graph will end up connected, but in the last turn there were two blobs, and if the oracle had answered no instead then the graph would be disconnected. So this works. 
</div>

<div class='hidden' desc='Comments'>
	This is essentially the strategy of saying yes unless forced to say no &mdash; if you have a connected blue subgraph and the other edges between vertices in that subgraph have not been colored red, then you don't ever have to query those edges. Any strategy that preserves greatness and ends with the graph connected should work; in particular saying no unless that would disconnect the graph works as well. 
</div>

<div class='problem' text='ToT Spring 2021 J-A7'>
	Let $p$ and $q$ be two coprime positive integers. A frog hops along the number line such that on each hop, it moves either $p$ units to the right or $q$ units to the left. Eventually, the frog returns to the initial point. Prove that for every positive integer $d < p + q$, there are two numbers visited by the frog which differ by $d$.
</div>

<div class='hidden' desc='Solution'>
	Write the list of jumps $+p$ and $-q$ made by the frog in a circle, so there are $kq$ points on the circle labelled $+p$ and $kp$ points labelled $-q$, in some order. Then it suffices to show that there is some subset of consecutive points on this circle whose sum of labels is exactly $d$. 
	<br/><br/>
	By Bezout's Theorem there are positive integers $a$ and $b$ with $d = ap - bq$. Look at subsets of exactly $a + b$ consecutive points on the circle. Then it suffices to show there is a subset with at most $a$ points labelled $+p$, and a subset with at least $a$ points labelled $+p$ &mdash; as we walk around the circle (taking the subset starting at each point), the number of points $+p$ in the subset changes by $0$ or $\pm 1$ each step, so by Discrete IVT there then must exist a subset with exactly $a$ points $+p$. 
	<br/><br/>
	First assume for contradiction that all subsets have at least $a + 1$ points labelled $+p$. Then sum over all $k(p + q)$ subsets. Each point is counted in $a + b$ subsets, and there are exactly $kq$ points $+p$, so then \[k(p + q)(a + 1) \leq kq(a + b).\] This implies $ap - bq \leq -p - q$, contradiction. Similarly, if all subsets have at most $a - 1$ points $+p$, then \[k(p + q)(a - 1) \geq kq(a + b),\] which implies $ap - bq \geq p + q$, contradiction. 
	<br/><br/>
	So then there exists a subset with at most $a$ points $+p$, and a subset with at least $a$ points $+p$, so by Discrete IVT there exists one with exactly $a$ points $+p$, and this subset sums to exactly $d$.
</div>

<div class='hidden' desc='Comments'>
	I really like this problem; I think it's a cool combination of local (look at a frame and shift it) and global (sum over all frames) ideas. I think the main realization is that you want to write the jumps in a circle and look at a <em>fixed</em> set of counts (meaning a specific pair of $a$ and $b$) &mdash; you can sort of motivate this by the fact that you want to look at <em>something</em>, and keeping track of multiple possible solutions would be hard as these seem pretty unrelated to each other (and there sometimes is only one).
</div>

<div class='problem' text='ToT Fall 2021 S-A6'>
	There are $20$ buns with jam and $20$ buns with treacle arranged in a row in random order. Alice and Bob take in turn a bun from any end of the row. Alice starts, and wants to finally obtain $10$ buns of each type; Bob tries to prevent this. Is it true for any order of the buns that Alice can win no matter what are the actions of Bob?
</div>

<div class='hidden' desc='Solution'>
	The answer is yes. Color the buns red and blue, in alternating order. 
	<div class='claim-un'>
		If there are $2k$ buns in a row, alternating between red and blue, such that there are $r$ red jam buns and $b$ blue jam buns, with $r \leq b$, then for any $r \leq x \leq b$ Alice can guarantee that she ends up with exactly $x$ jam buns.
	</div>
	<div class='proof'>
		Induct on $k$; for $k = 1$ this is true. 
		<br/><br/>
    	On each of Alice's turns, one bun at the end of the row is red, and the other is blue. If $b = x$, Alice can take the blue bun every turn, so in the end Alice has all the blue buns and Bob has all the red buns. 
    	<br/><br/>
    	Otherwise, if $b < x$, Alice takes the red bun on this turn, which means Bob takes a blue bun. Let $r'$ and $b'$ be the numbers of red and blue jam buns remaining, and $x'$ the number of jam buns Alice needs to take after this turn. Then $r$ and $x$ both decrease by $0$ (if Alice took a treacle bun) or $1$ (if Alice took a jam bun), so $r' \leq x'$. Meanwhile $b$ also decreases by $0$ or $1$, and $x \leq b + 1$, so $x' \leq b'$. So by induction, Alice can guarantee she ends up with exactly $x'$ of the remaining jam buns. 
	</div>
	At the start $r + b = 20$, so $10$ is between $r$ and $b$. So Alice can guarantee she ends up with $10$ jam buns.
</div>

<div class='problem' text='ToT Spring 2019 S-A7'>
	Consider lattice paths of finite length which start from $(0, 0)$ and move only up and right, which we call <em>skeletons</em>. For each skeleton, we define a <em>worm</em> consisting of all unit cells in the plane sharing at least one point with the skeleton. (For example, for the path from $(0, 0)$ to $(1, 0)$, the corresponding worm has six cells). Prove that for every integer $n > 2$, the number of worms which can be tiled by dominoes in exactly $n$ ways is equal to $\varphi(n)$.
</div>

<div class='hidden' desc='Solution'>
	First, we're going to find a way to recurse the number of tilings of a worm. 
	<br/><br/>
	Note that the number of ways to tile the worm of skeleton $(0, 0)$ is $2$, since it is a $2 \times 2$ grid. Also, define the number of ways to tile the worm of the empty skeleton as $1$. 
	<br/><br/>
	Define the <em>twisty part</em> of a skeleton $S$ to be the maximal sequence of points starting with the end for which the skeleton alternates direction, and define the <em>head</em> of a skeleton to be the remainder not in the twisty part (the head may be empty). Here are some examples, with the heads in red and the twisty parts in blue. 
	<br/><br/>
	<center><img src='/assets/img/19ttssa7-twistypart.png' width='400' height='auto'></center>
	<div class='claim-un'>
		To get the number of ways to tile skeleton $S$, we add the number of ways to tile the skeleton with the last point removed, and the number of ways to tile the head of $S$
	</div>
	<div class='proof'>
		WLOG the last move in the skeleton is vertical. Now if we place a horizontal domino at the top-right corner, the remaining worm is the worm of the skeleton where we delete the last point of $S$. 
		<br/><br/>
		<center><img src='/assets/img/19ttssa7-recurse1.png' width='250' height='auto'></center>
		Meanwhile, if we place a vertical domino in the top-right corner, then this forces the positions of a bunch of other dominoes. Placing all these other dominoes has the effect of removing the entire twisty part, leaving us with the worm of the head of $S$. 
		<br/><br/>
		<center><img src='/assets/img/19ttssa7-recurse2.png' width='250' height='auto'></center>
		So then the total number of ways to tile $S$ is the sum of these two new skeletons, the one with the last point deleted and the head. 
	</div>
	Now we can make a big tree, where the top entry is the skeleton $(0, 0)$, and when we go down, left means adding a step right while right means adding a step up. At each node, we write the number of ways to tile that skeleton. We also write the two entries we summed according to the previous claim &mdash; if this was a left child, then we write the skeleton with the last element removed on the left and the head on the right, while if this was a right child then we do the opposite.
	<br/><br/>
	<center><img src='/assets/img/19ttssa7-tree.png' width='600' height='auto'></center>
	We claim this becomes the Euclidean Algorithm tree where $(a, b)$ has descendants $(a + b, a)$ and $(b, a + b)$.
	<br/><br/>	
	To show this, if $(a, b)$ with skeleton $S$ is a left child, then $a$ is the number of ways for $S$ with its last point removed, and $b$ is the number of ways for the head of $S$. 
	<br/><br/>
	Then if we go left one step to get skeleton $T$, then $S$ with $a + b$ ways is $T$ with its last point removed, and $S$ with its last point removed is the head of $T$ (since the twisty part is only the last two points), which gives $(a + b, a)$. 
	<br/><br/>
	Meanwhile, if we go one step right to get skeleton $T$, then $S$ is still $T$ with its last point removed, but the head of $S$ is the head of $T$ (since we went left to get to $S$, and then right to get to $T$, so $T$ swallows the twisty part of $S$), which gives us $(b, a + b)$. 
	<br/><br/>
	Similar things happen when $S$ is a right child, which proves that our tree is the Euclidean Algorithm tree. But it's clear that every pair $(a, b)$ with $\gcd(a, b) = 1$ occurs exactly once in this tree (since we can trace it back to $(1, 1)$ uniquely), and no pairs with $\gcd(a, b) > 1$ are in this tree. So then the number of occurrences of $n$ in this tree are the number of ways to write $n = a + b$ with $\gcd(a, b) = 1$, which is $\varphi(n)$.
</div>

<div class='hidden' desc='Comments'>
	This is one of my favorite problems of all time. It's a rigid problem &mdash; the point is to start by trying to figure out how to count the number of tilings of a given worm. You can try placing the tile at the end and drawing the dominoes that are forced, and you notice that you end up with a smaller skeleton with a nice characterization. Once you get the recursion you can try drawing a tree to perform the recursion for small worms, and eventually you notice that this is very similar to the Euclidean Algorithm tree &mdash; and you can do the tracking in such a way that it actually <em>becomes</em> the Euclidean Algorithm tree. 
</div>