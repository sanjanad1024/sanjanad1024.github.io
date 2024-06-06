---
layout: post
title:  "An Interactive Proof for Count-SAT"
date:   2024-06-04
categories: complexity, TCS
description: An explanation of the interactive proof protocol for counting the number of satisfying assignments to a Boolean formula. 
tags: 
giscus_comments: false
related_posts: false
---

# Introduction

In the <a href="https://sanjanad1024.github.io/blog/2024/interactive-proofs/" target="_blank">previous post</a> on interactive proofs, we defined $\textsf{IP}$ as the class of decision problems which can be solved using an interactive proof with a computationally unbounded prover and computationally bounded, <em>randomized</em> verifier &mdash; meaning that there's an efficient verifier such that for each $\textsf{YES}$ instance there <em>exists</em> a 'good prover' that makes the verifier accept with high probability, while for each $\textsf{NO}$ instance the verifier rejects <em>every</em> 'cheating prover' with high probability. (We refer to these two statements as <span class = 'vocab'>completeness</span> and <span class = 'vocab'>soundness</span>, respectively.)

<div class = 'question'>
    How powerful are interactive proofs &mdash; what sorts of problems are in $\textsf{IP}$?
</div>

In the previous post, we saw that $\textsf{NP} \subseteq \textsf{IP}$. We don't even need interaction (or randomness) for this &mdash; we can just have the prover send over the $\textsf{NP}$-certificate, and the verifier check that it works. For example, there's a very simple protocol for $\textsf{SAT}$ (where the prover wants to convince the verifier that a Boolean formula is satisfiable) &mdash; the prover just sends over a satisfying assignment, and the verifier plugs it in. 

But it turns out that $\textsf{IP}$ is much more powerful than this &mdash; there's tons of problems that we don't expect to be in $\textsf{NP}$, but which nevertheless have interactive proofs. 

<div class = 'theorem' text = 'Shamir 1990'>
    We have $\textsf{IP} = \textsf{PSPACE}$. 
</div>

In the previous post, we proved one direction &mdash; that $\textsf{IP} \subseteq \textsf{PSPACE}$. In this post, we're not yet going to prove the opposite direction &mdash; that $\textsf{PSPACE} \subseteq \textsf{IP}$ &mdash; but we'll give an interactive proof for a specific problem that is already quite surprising, and illustrates most of the main ideas that go into the proof of $\textsf{PSPACE} \subseteq \textsf{IP}$ in a somewhat simpler setting. (In a post which will probably appear at some indefinite point in the future, we'll prove the full theorem, by extending these ideas to give an interactive proof for <em>every</em> problem in $\textsf{PSPACE}$.)

The specific problem we'll consider is the problem of counting the number of solutions to a Boolean formula. 

<div class = 'definition'>
    We define $\textsf{Count-SAT}$ as the following decision problem:
    <ul>
        <li> 
            <b>Input:</b> a Boolean formula $\varphi$ in variables $x_1$, $\ldots$, $x_n$, and an integer $k$. 
        </li>
        <li>
            <b>Decide:</b> whether there are at least $k$ satisfying assignments to $\varphi$.
        </li>
    </ul>
</div>

<div class = 'theorem'>
    There is an interactive proof for $\textsf{Count-SAT}$. 
</div>

Note that this means there's also an interactive proof for the problem $\textsf{UNSAT}$, where the prover wants to convince the verifier that a formula is <em>not</em> unsatisfiable &mdash; a formula $\varphi$ in $x_1$, $\ldots$, $x_n$ is unsatisfiable if and only if $\neg \varphi$ has at least $2^n$ satisfying assignments, and this is a statement that a prover can convince a verifier of using the $\textsf{Count-SAT}$ protocol. This is already quite surprising. 

In the rest of the post, we'll explain this protocol. The content of this post is based on lectures from 18.404 (the lectures from December 7 and 12) and 18.405 (the lecture from April 16).

# Some notation

First, here's some notation we'll use throughout the proof. Throughout this post, we'll assume $\varphi$ is a Boolean formula in $n$ variables $x_1$, $\ldots$, $x_n$. We're going to think of Boolean values as $0$ (which corresponds to $\texttt{False}$) and $1$ (which corresponds to $\texttt{True}$) &mdash; so $\varphi$ defines a function $$\{0, 1\}^n \to \{0, 1\}$$. 

We'll use $\texttt{#}\textsf{SAT}(\varphi)$ to refer to the number of satisfying assignments to $\varphi$, which we can write as 

<div class = 'eqn'>
\[\texttt{#}\textsf{SAT}(\varphi) = \sum_{a_1, \ldots, a_n \in \{0, 1\}} \varphi(a_1, \ldots, a_n).\]
</div>
(This is because we're summing over all possible assignments; and each satisfying assignment we plug in will contribute $1$ to the sum, while each unsatisfying assignment will contribute $0$.)

We'll also set up some notation for plugging in 'partial assignments' &mdash; for a string $b = b_1\ldots b_i$ (representing an assignment of the first $i$ variables $x_1$, $\ldots$, $x_i$), we use $\varphi_b$ to denote the formula in $x_{i + 1}$, $\ldots$, $x_n$ obtained by plugging in $x_1 = b_1$, $\ldots$, $x_i = b_i$. Then $\texttt{#}\textsf{SAT}(\varphi_b)$ denotes the number of satisfying assignments (to the remaining variables $x_{i + 1}$, $\ldots$, $x_n$) of $\varphi_b$, or equivalently the number of satisfying assignments to $\varphi$ that begin with $b_1$, $\ldots$, $b_i$; we can write this as 
<div class = 'eqn'>
\[\texttt{#}\textsf{SAT}(\varphi_b) = \sum_{a_{i + 1}, \ldots, a_n \in \{0, 1\}} \varphi(b_1, \ldots, b_i, a_{i + 1}, \ldots, a_n).\]
</div>

Note that if $b$ has length $n$ (i.e., $b = b_1\ldots b_n$), then $\texttt{\#}\textsf{SAT}(\varphi_b)$ is just $\varphi(b_1, \ldots, b_n)$, while otherwise 
<div class = 'eqn'>
\[\texttt{#}\textsf{SAT}(\varphi_b) = \texttt{#}\textsf{SAT}(\varphi_{b0}) + \texttt{#}\textsf{SAT}(\varphi_{b1}).\]
</div>
In words, this is because on the left-hand side we're trying to count the number of satisfying assignments to $\varphi$ which begin with $x_1 = b_1$, $\ldots$, $x_i = b_i$, and on the right-hand side we're splitting this count into the two cases $x_{i + 1} = 0$ and $x_{i + 1} = 1$. (This also can be seen directly from <span class = 'crossref'>(2).</span>)


Finally, we'll use $\varepsilon$ to denote the empty string (so $\varphi_\varepsilon = \varphi$). 

# A first attempt

First, here's an <em>attempt</em> to get an interactive proof for $\textsf{Count-SAT}$. This won't work &mdash; in particular, the verifier will be extremely inefficient &mdash; but trying to figure out how to fix it motivates the actual protocol. 

<ul>
<li> 
    First, the prover says, <span class = 'mypurple'>'I claim $\texttt{#}\textsf{SAT}(\varphi) = k_\varepsilon$'</span> for some integer $k_\varepsilon$ (i.e., they send the verifier an integer $k_\varepsilon$, which the verifier interprets as making this claim). The verifier checks that $k_\varepsilon \geq k$, and <span class = 'myaqua'>rejects</span> if not. (This is because the entire goal of the procedure is for the prover to convince the verifier that $\texttt{#}\textsf{SAT}(\varphi)$ is at least $k$, so if the prover's already telling them that it's a quantity <em>less</em> than $k$, then they should automatically reject.) 
</li>
<li>
    Now the verifier thinks, <span class = 'myaqua'>'Well, why should I believe you?'</span> And the prover says, <span class = 'mypurple'>'Well, the reason I know $\texttt{#}\textsf{SAT}(\varphi) = k_\varepsilon$ is because $\texttt{#}\textsf{SAT}(\varphi_{0}) = k_{0}$ and $\texttt{#}\textsf{SAT}(\varphi_{1}) = k_{1}$'</span> (by sending over two new integers $k_{0}$ and $k_{1}$). And the verifier checks that this makes sense (i.e., that the new claims justify the old claim) &mdash; they know from <span class = 'crossref'>(3)</span> that $\texttt{#}\textsf{SAT}(\varphi) = \texttt{#}\textsf{SAT}(\varphi_{0}) + \texttt{#}\textsf{SAT}(\varphi_{1})$, so they check that $k_\varepsilon = k_0 + k_1$ and <span class = 'myaqua'>reject</span> if not. 
</li>
<li>
    Now the verifier thinks, <span class = 'myaqua'>'Well, if your new claims about $\texttt{#}\textsf{SAT}(\varphi_{\texttt{T}})$ and $\texttt{#}\textsf{SAT}(\varphi_{\texttt{F}})$ are true, then your original claim is too. But why should I believe these new claims?'</span> And the prover says, <span class = 'mypurple'>'Well, the reason I know $\texttt{#}\textsf{SAT}(\varphi_0) = k_0$ is because $\texttt{#}\textsf{SAT}(\varphi_{00}) = k_{00}$ and $\texttt{#}\textsf{SAT}(\varphi_{01}) = k_{01}$, and the reason I know $\texttt{#}\textsf{SAT}(\varphi_{1}) = k_{1}$ is because $\texttt{#}\textsf{SAT}(\varphi_{10}) = k_{10}$ and $\texttt{#}\textsf{SAT}(\varphi_{11}) = k_{11}$'</span> (so they're sending over four new integers $k_{00}$, $k_{01}$, $k_{10}$, and $k_{11}$). And the verifier checks that these statements make sense (meaning that the new claims justify the old claims) &mdash; they check that $k_0 = k_{00} + k_{01}$ and that $k_1 = k_{10} + k_{11}$, and <span class = 'myaqua'>reject</span> if not. 
</li>
<li>
    This continues &mdash; at the start of the $i$th step, the prover has made claims of the form <span class = 'mypurple'>'$\texttt{#}\textsf{SAT}(\varphi_b) = k_b$'</span> for all $b = b_1\ldots b_{i - 1}$ (i.e., they've considered all ways to set the first $i - 1$ variables, and claimed that each of these corresponds to a certain number of satisfying assignments). The verifier thinks, <span class = 'myaqua'>'Well, why should I believe these claims?'</span> And for each $b$, the prover justifies the claim $\texttt{#}\textsf{SAT}(\varphi_b) = k_b$ by sending over two new claimed counts $k_{b0}$ and $k_{b1}$ &mdash; so for each $b$, they're making a statement <span class = 'mypurple'>'Well, the reason I know $\texttt{#}\textsf{SAT}(\varphi_b) = k_b$ is because $\texttt{#}\textsf{SAT}(\varphi_{b0}) = k_{b0}$ and $\texttt{#}\textsf{SAT}(\varphi_{b1}) = k_{b1}$.'</span> And the verifier checks that these new claims justify the old claims &mdash; they check that $k_b = k_{b0} + k_{b1}$ for all $b$, and <span class = 'myaqua'>reject</span> if not. 
</li>
<li>
    Finally, after $n$ steps, the prover has made claims of the form <span class = 'mypurple'>'$\texttt{#}\textsf{SAT}(\varphi_b) = k_b$'</span> for all $b = b_1\ldots b_n$ (corresponding to plugging in values for <em>all</em> the variables), and the verifier is satisfied that if these claims are true, then so is the original claim. And the verifier can check these claims themselves &mdash; we have $\texttt{#}\textsf{SAT}(\varphi_b) = \varphi(b_1, \ldots, b_n)$, so the verifier can simply plug this assignment into $\varphi$ and check that it evaluates to the claimed value $k_b$. Finally, they <span class = 'myaqua'>reject</span> if one of these checks fail (as now they've caught the prover in a lie) and <span class = 'myaqua'>accept</span> if all the checks pass (they know all the prover's claims in the final step were correct, and since they're satisfied that the prover's claims from the $i$th step justify their claims from the $(i - 1)$th step for all $i$, this means they're satisfied that the original claim was correct too). 
</li>
</ul>
This procedure certainly satisfies correctness and soundness, but it's <em>extremely</em> inefficient &mdash; on the $i$th step, the prover is sending over $2^i$ different numbers (one for each of the ways to set the first $i$ variables). So the messages get exponentially long, and the verifier is going to run in exponential time. This is a problem, as we want the verifier to be <em>polynomial</em>-time. 

<center><img src='/assets/img/ipcount1.png' width='300' height='auto'></center>

The problem with this procedure &mdash; i.e., the reason it's so inefficient &mdash; is that every time the verifier asks the prover to justify a claim $\texttt{\#}\textsf{SAT}(\varphi_b) = k_b$, the prover responds with <em>two</em> new claims &mdash; for $\varphi_{b\texttt{T}}$ and $\varphi_{b\texttt{F}}$ &mdash; and the verifier then has to check <em>both</em> claims. This gives the protocol the structure of a binary tree (as shown above), and means it grows to have exponential size. So if we want to fix this protocol to be efficient, we have to somehow fix this issue &mdash; we have to make it so that for each claim $\texttt{\#}\textsf{SAT}(\varphi_b) = k_b$ that the prover makes, when they provide justification for it by making two new claims $\texttt{\#}\textsf{SAT}(\varphi_{b\texttt{T}}) = k_{b\texttt{T}}$ and $\texttt{\#}\textsf{SAT}(\varphi_{b\texttt{F}}) = k_{b\texttt{F}}$, the verifier only needs to be convinced of <em>one</em> statement to be convinced of both of these claims (rather than asking to be convinced of each claim separately). 

<div class = 'remark'>
    When I first saw this proof, the first thing I thought was, why doesn't the verifier just choose a random one of these two claims and ask the prover to convince them of just that one (i.e., they flip a coin and ask the prover either to convince them that $\texttt{#}\textsf{SAT}(\varphi_{b0}) = k_{b0}$ or that $\texttt{#}\textsf{SAT}(\varphi_{b1}) = k_{b1}$, rather than asking the prover to convince them of both)?
    <br><br/>
    But this doesn't work because it won't satisfy soundness. To see this, imagine that $\texttt{#}\textsf{SAT}(\varphi)$ is $50$, but a cheating prover is trying to convince the verifier that it's actually $100$. Then the cheating prover can't give honest values of <em>both</em> $\texttt{#}\textsf{SAT}(\varphi_{0})$ and $\texttt{#}\textsf{SAT}(\varphi_{2})$, because the verifier would catch them (the correct values sum to $\texttt{#}\textsf{SAT}(\varphi) = 50$, while the verifier is checking that the values the prover sends sum to $k_\varepsilon = 100$). But they can give a honest value for <em>one</em> of them &mdash; for example, if the correct values are $40$ and $10$, then the prover can instead claim the values of $40$ and $60$ (i.e., they send $k_0 = 40$ and $k_1 = 60$). 
    <br><br/>
    Then if the verifier chooses the first claim &mdash; that $\texttt{#}\textsf{SAT}(\varphi_0) = 40$ &mdash; and asks the prover to convince them of just this claim, the prover can successfully do so, because this claim is true. So they'll have gotten away with their original lie, and the verifier will incorrectly have been convinced that $\texttt{#}\textsf{SAT}(\varphi) = 100$. 
    <br><br/>
    Meanwhile, if the verifier chooses the second claim &mdash; that $\texttt{#}\textsf{SAT}(\varphi_{1}) = 60$ &mdash; and asks the prover to convince them of just this claim, then at least the prover hasn't <em>immediately</em> gotten away with things, because the statement they're trying to convince the verifier of is still false.
    <br><br/>
    But now the same thing happens &mdash; for example, imagine that the correct values of $\texttt{#}\textsf{SAT}(\varphi_{10})$ and $\texttt{#}\textsf{SAT}(\varphi_{11})$ are $3$ and $7$. Then the prover can't send the correct values of both (since $3 + 7$ is not $60$), but they <em>can</em> send the correct value for one of them &mdash; for example, they can send $3$ and $57$. And then if the verifier asks them to justify $\texttt{#}\textsf{SAT}(\varphi_{10}) = 3$, they'll be able to do so, because this statement is true &mdash; so they'll have gotten away with their lie again. 
    <br><br/>
    This means the prover has a $\frac{1}{2}$ probability of getting away with a lie at each step (i.e., the claim they were trying to convince the verifier of at the start of the $i$th step was false, but the claim they're trying to convince the verifier of at the end of the $i$th step is actually true &mdash; which means they can successfully convince the verifier). So the probability that they <em>never</em> get away with a lie, and the verifier really catches them in the end and rejects, is just $\frac{1}{2^n}$. This is pretty terrible, as we want the verifier to reject with high probability. 
</div>

So our goal is to find a way of compressing the two claims $\texttt{#}\textsf{SAT}(\varphi_{b0}) = k_{b0}$ and $\texttt{#}\textsf{SAT}(\varphi_{b1}) = k_{b1}$ into a single claim $(\star)$, and finding a <em>single</em> statement $(\star\star)$ that the verifier can ask the prover to convince them of that would suffice to convince them of $(\star)$. This statement is going to be randomized, but we'll have to do something better than just randomly choosing one of the two claims &mdash; we need the probability that the prover 'gets away with a lie' at each step (meaning that $(\star)$ is false but $(\star\star)$ is true) to be much smaller than $\frac{1}{2}$. And the key idea that's going to let us do this is <span class = 'vocab'>arithmetization</span>. 

# Arithmetization

We've got a Boolean formula $\varphi$, which takes inputs in $$\{0, 1\}^n$$ and spits out some output in $$\{0, 1\}$$. But the idea is that instead of thinking of $\varphi$ as a Boolean formula, we're going to think of it as a polynomial over some finite field $\mathbb{F} = \mathbb{F}_p$ (where $p$ is a large prime we'll fix at the start of the protocol). 

To arithmetize $\varphi$ (i.e., to turn it into a polynomial), we can use the following correspondences:
<ul>
    <li> 
        $\neg x$ becomes $1 - x$.
    </li> 
    <li>
        $x \wedge y$ becomes $xy$. 
    </li>
    <li>
        $x \vee y$ becomes $x + y - xy$. 
    </li>
</ul>
The point is that the arithmetic expressions on the right match the Boolean ones on the left when we plug in $$\{0, 1\}$$-valued inputs, so this is a 'faithful' way of representing Boolean operations. 

<div class = 'example'>
    Suppose that we begin with the Boolean formula $\varphi(x_1, x_2, x_3) = x_1 \vee (\neg x_2 \wedge x_3)$. Then to arithmetize it, we first replace $\neg x_2$ with $1 - x_2$, then we replace $(1 - x_2) \wedge x_3$ with $(1 - x_2)x_3$, and finally we replace $x_1 \vee (1 - x_2)x_3$ with $x_1 + (1 - x_2)x_3 - x_1(1 - x_2)x_3$, to get the polynomial \[\varphi(x_1, x_2, x_3) = x_1 + (1 - x_2)x_3 - x_1(1 - x_2)x_3.\]
</div>

If we only care about Boolean (i.e., $$\{0, 1\}$$-valued) inputs, then it doesn't matter whether we think of $\varphi$ as a formula or a polynomial &mdash; we'll get the same outputs either way. But the advantage of thinking of $\varphi$ as a polynomial is that it allows us to plug in quantities that are <em>not</em> Boolean &mdash; for example, now it makes sense to talk about plugging in $x_1 = 5$ (which would make no sense if viewing $\varphi$ just as a Boolean formula). 

<div class = 'remark'>
    The verifier might not be able to write down $\varphi$ as a polynomial in $x_1$, $\ldots$, $x_n$ (this polynomial could potentially have a huge number of terms), but they definitely <em>can</em> evaluate it at any specific input &mdash; they can simply evaluate it one operation at a time, using arithmetic operations instead of Boolean ones. For example, if they wanted to find $\varphi(5, 3, 7)$ for the $\varphi$ from <span class = 'crossref'>Example 6</span>, they'd first think of it as $5 \vee (\neg 3 \wedge 7)$. Then they'd first evaluate $\neg 3$ as $1 - 3 = -2$; then they'd evaluate $-2 \wedge 7$ as $(-2)(7) = -14$; and finally they'd evaluate $5 \vee (-14)$ as $5 + (-14) - 5(-14) = 61$. 
    <br><br/>
    And in the protocol, this is all we'll expect the verifier to do with $\varphi$ &mdash; all other computations with $\varphi$ (as a polynomial) will be done on the prover's end. 
</div>

For our purposes, it'll be important that the polynomial version of $\varphi$ has decently low degree. 

<div class = 'fact'>
    The degree of $\varphi$ as a polynomial is at most the size of $\varphi$ as a formula.
</div>
We'll refer to these quantities as $\textsf{deg}(\varphi)$ and $\textsf{size}(\varphi)$, respectively. We measure the size of a Boolean formula by the number of appearances of variables &mdash; for example, we'd say that $\textsf{size}(x_1 \vee (\neg x_1 \wedge x_3))$ is $3$ (there's three appearances of variables, namely $x_1$, $x_1$, and $x_3$). Then the reason this fact is true is essentially that each time we perform the arithmetic version of an $\wedge$ or $\vee$ on two smaller formulas, we're at worst adding together their degrees.


We've previously defined the notation $\texttt{#}\textsf{SAT}(\varphi_b)$ where $b = b_1\ldots b_i$ is an assignment of <em>Boolean</em> values to $x_1$, $\ldots$, $x_i$ &mdash; we defined this as the number of ways to set Boolean values $$a_{i + 1}, \ldots, a_n \in \{0, 1\}$$ for the remaining variables $x_{i + 1}$, $\ldots$, $x_n$ that give satisfying assignments. We'd now like to define $\texttt{#}\textsf{SAT}(\varphi_r)$ where $r = r_1\ldots r_i$ is an assignment of <em>arbitrary</em> values from $\mathbb{F}$ to $x_1$, $\ldots$, $x_i$ (rather than just $0$'s and $1$'s). This definition no longer makes sense, but the equation <span class = 'crossref'>(2)</span> for $\texttt{#}\textsf{SAT}(\varphi_b)$ <em>does</em> make sense, so we'll simply take this equation to be our definition of $\texttt{#}\textsf{SAT}(\varphi_r)$ &mdash; i.e., we define 
<div class = 'eqn'>
\[\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_i}) = \sum_{a_{i + 1}, \ldots, a_n \in \{0, 1\}} \varphi(r_1, \ldots, r_i, a_{i + 1}, \ldots, a_n).\]
</div>
Then the equation <span class = 'crossref'>(3)</span> still holds &mdash; if $r$ has length less than $n$, then 
<div class = 'eqn'>
\[\texttt{#}\textsf{SAT}(\varphi_r) = \texttt{#}\textsf{SAT}(\varphi_{r0}) + \texttt{#}\textsf{SAT}(\varphi_{r1}),\]
</div>
while if $r = r_1\ldots r_n$ has length $n$, then $\texttt{#}\textsf{SAT}(\varphi_r)$ is just $\varphi(r_1, \ldots, r_n)$. 

# The actual protocol. 

Now we're ready to describe an interactive proof protocol that <em>does</em> work. 
<ul>
    <li>
        We're going to work over some finite field $\mathbf{F} = \mathbb{F}_p$, so we first need to fix a suitable prime $p$. For this, we'll have the prover send over a prime $p \in [2^n \cdot \textsf{size}(\varphi), 2^{n + 1} \cdot \textsf{size}(\varphi)]$. We need a way of making sure that $p$ is actually prime (so that if a cheating prover sends over some $p$ which isn't prime, the verifier catches them). We can do this because primality testing is in $\textsf{NP}$ (this is nontrivial and involves a bit of number theory &mdash; for example, see <a href = 'https://en.wikipedia.org/wiki/Primality_certificate' target = '_blank'>this Wikipedia article</a>), so the prover can send over a certificate showing that $p$ is prime, and the verifier can check it. 
        <div class = 'remark'>
            In fact, primality testing is actually in $\textsf{P}$, so the verifier could check that $p$ is prime on their own; but this is a much more difficult result.
        </div>
        <div class = 'remark'>
            The specific size of $p$ is not particularly important. We need $p > 2^n$, because what the prover will be doing is really convincing the verifier of the value of $\texttt{#}\textsf{SAT}(\varphi)$ in $\mathbb{F}_p$ &mdash; and if $p$ were less than $2^n$ then this wouldn't be enough to recover the actual value of $\texttt{#}\textsf{SAT}(\varphi)$ (because of numbers wrapping around mod $p$). And we need $p \leq 2^{\textsf{poly}(n)}$ so that we can do computations with numbers in $\mathbb{F}_p$. But other than this, the value of $p$ doesn't matter (the bound we've got here will be way more than enough to ensure soundness). 
        </div>
        Now that we've got $p$, we can proceed with the main part of the protocol (where all arithmetic is done over $\mathbb{F} = \mathbb{F}_p$). 
    </li>
    <li>
        First, as in our failed attempt, the prover starts by saying <span class = 'mypurple'>'I claim that $\texttt{#}\textsf{SAT}(\varphi) = k_0$'</span> for some $k_0$ (by sending over $k_0$). (The indices we're going using here are a bit different from the ones in our failed attempt &mdash; they'll be integers rather than bit-strings.) The verifier checks that $k_0 \geq k$, and <span class = 'myaqua'>rejects</span> if not. 
    </li>
    <li>
        Now the verifier again thinks, <span class = 'myaqua'>'Well, why should I believe you?'</span> In our failed attempt, the prover tried to justify their claim by making new claims about the values of $\texttt{#}\textsf{SAT}(\varphi_0)$ and $\texttt{#}\textsf{SAT}(\varphi_1)$. Here, they'll do something different &mdash; they'll make a single claim about the value of $\texttt{#}\textsf{SAT}(\varphi_z)$ as a <em>polynomial</em> in $z$. 
        <br><br/>
        What does this mean? In <span class = 'crossref'>(5)</span>, we defined \[\texttt{#}\textsf{SAT}(\varphi_{r_1}) = \sum_{a_2, \ldots, a_n \in \{0, 1\}} \varphi(r_1, a_2, \ldots, a_n)\] for any $r_1 \in \mathbb{F}$. We can imagine thinking of $r_1$ as a variable $z$ &mdash; then $\texttt{#}\textsf{SAT}(\varphi_z)$ is the single-variable polynomial in $z$ obtained by summing the $2^{n - 1}$ polynomials $\varphi(z, a_2, \ldots, a_n)$. This is some polynomial in $z$, and it'll have degree at most $\textsf{deg}(\varphi) \leq \textsf{size}(\varphi)$.
        <div class = 'example'>
            Suppose that $\varphi(x_1, x_2, x_3) = x_1 \vee (\neg x_1 \wedge x_2 \wedge x_3)$, which becomes the polynomial \[\varphi(x_1, x_2, x_3) = x_1 + (1 - x_1)x_2x_3 - x_1(1 - x_1)x_2x_3.\] Then to compute $\texttt{#}\textsf{SAT}(\varphi_z)$ we want to plug in $z$ for $x_1$, and sum over all ways to plug in $a_2, a_3 \in \{0, 1\}$ for $x_2$ and $x_3$. So we'll have a sum over four terms:
            <ul>
                <li> $(a_2, a_3) = (0, 0)$ gives $z + (1 - z)0\cdot 0 - z(1 - z)0\cdot 0 = z$. 
                </li>
                <li> $(a_2, a_3) = (0, 1)$ and $(a_2, a_3) = (1, 0)$ similarly give $z$.  
                </li>
                <li> $(a_2, a_3) = (1, 1)$ gives $z + (1 - z)1\cdot 1 - z(1 - z)1\cdot 1 = 1 - z + z^2$. 
                </li>
            </ul>
            Summing these up, we get $\texttt{#}\textsf{SAT}(\varphi_z) = z + z + z + (1 - z + z^2) = 1 + 2z + z^2$. 
        </div>
        <div class = 'remark'>
            It might take exponentially long for the prover to <em>compute</em> the polynomial $\texttt{#}\textsf{SAT}(\varphi_z)$, but we don't care &mdash; the prover is computationally unbounded. What's important is that this polynomial has low degree, so the prover can write it down succinctly &mdash; in the form $\sum a_iz^i$ &mdash; and the verifier can efficiently do computations with it. 
        </div>
        So the prover says, <span class = 'mypurple'>'Well, I know $\texttt{#}\textsf{SAT}(\varphi) = k_0$ because $\texttt{#}\textsf{SAT}(\varphi_z) = Q_1(z)$'</span> &mdash; so they're sending the verifier a polynomial $Q_1(z)$ of degree at most $\textsf{size}(\varphi)$. (If the polynomial they send has degree higher than this, the verifier immediately knows it's wrong, so they <span class = 'myaqua'>reject</span>.)
        <br><br/>
        And the first point of this idea is that if the verifier's got $\texttt{#}\textsf{SAT}(\varphi_z)$ as a polynomial in $z$, then they can automatically compute $\texttt{#}\textsf{SAT}(\varphi_0)$ and $\texttt{#}\textsf{SAT}(\varphi_1)$ themselves, by simply plugging in $z = 0$ and $z = 1$. So we've found a way of bundling the two claims from our failed attempt into one single claim. 
        <br><br/>
        So the verifier first checks that this new claim does justify the old claim &mdash; they know from <span class = 'crossref'>(5)</span> that $\texttt{#}\textsf{SAT}(\varphi) = \texttt{#}\textsf{SAT}(\varphi_0) + \textsf{SAT}(\varphi_1)$, so they check that $k_\varepsilon = Q_1(0) + Q_1(1)$ and <span class = 'myaqua'>reject</span> if not. 
    </li>
    <li>
        Now the verifier thinks, <span class = 'myaqua'>'Well, if you've really given me the correct polynomial $\texttt{#}\textsf{SAT}(\varphi_z)$, then your original claim about $\texttt{#}\textsf{SAT}(\varphi)$ is true. But why should I believe you've really given me the correct polynomial?'</span>
        <br><br/>
        And so the verifier is going to choose a uniform random $r_1 \in \mathbb{F}$ and say, <span class = 'myaqua'>'Okay, convince me that your polynomial is correct at $r_1$, and then I'll believe you that it's actually correct.'</span> In other words, instead of asking the prover to convince them that $\texttt{#}\textsf{SAT}(\varphi_z)$ and $Q_1(z)$ are equal as polynomials, the verifier is choosing a random point $r_1$ to evaluate them at, and asking the prover to convince them that they're equal at $r_1$ &mdash; i.e., that $\texttt{#}\textsf{SAT}(\varphi_{r_1}) = Q_1(r_1)$. (And if the prover manages to convince them of this, then they'll believe the prover's claim that the two polynomials are equal.)
        <div class = 'remark'>
            Why do we need to plug in some $r_1$ &mdash; why don't we want the verifier to just ask the prover to convince them of the full claim $\texttt{#}\textsf{SAT}(\varphi_z) = Q_1(z)$? The problem with doing this is that it'd blow up the complexity of the claims. What's nice about what we've done here is that we started with the prover trying to prove a statement that $\texttt{#}\textsf{SAT}(\varphi)$ is equal to a certain integer, where $\texttt{#}\textsf{SAT}(\varphi)$ is a sum of $2^n$ polynomials as in <span class = 'crossref'>(4)</span>; and we've ended up with them trying to prove a statement that $\texttt{#}\textsf{SAT}(\varphi_{r_1})$ is equal to another certain integer, where $\texttt{#}\textsf{SAT}(\varphi_{r_1})$ is a sum over $2^{n - 1}$ polynomials as in <span class = 'crossref'>(4)</span> (we're no longer summing over $a_1$). This is essentially a statement of the same form as the original one (but with one fewer variable), so we can iterate &mdash; we can ask the prover to justify this claim in the same way. 
            <br><br/>
            Meanwhile, if we <em>didn't</em> plug in a value $r_1$ &mdash; and instead left $z$ as a variable &mdash; then if we tried to iterate (leaving everything as a variable), in the end we'd be asking the prover to prove a statement of the form $\texttt{#}\varphi_{z_1\ldots z_n} = Q(z_1, \ldots, z_n)$ (where we've got polynomials in $n$ variables rather than just $1$). And this is bad, because these polynomials could be enormous (unlike the one-variable polynomials we'll get from the actual proof). 
        </div>
        <div class = 'remark'>
            Why is this a reasonable thing to do? We saw in <span class = 'crossref'>Remark 5</span> that randomly choosing to check either the prover's claim about $\texttt{#}\textsf{SAT}(\varphi_0)$ or their claim about $\texttt{#}\textsf{SAT}(\varphi_1)$ doesn't work (since a cheating prover has a $\frac{1}{2}$ probability of getting away with a lie at each step &mdash; if one of these two claims is wrong, there's only a $\frac{1}{2}$ probability we choose to investigate the wrong claim). In contrast, here in some sense the prover is making a claim about $\texttt{#}\textsf{SAT}(\varphi_{r_1})$ for <em>all</em> $r_1 \in \mathbb{F}$, by giving the verifier $\texttt{#}\textsf{SAT}(\varphi_z)$ as a polynomial, and the verifier is choosing a random $r_1 \in \mathbb{F}$ to check. 
            <br><br/>
            And the key difference is that if the prover gives us an <em>incorrect</em> polynomial (i.e., they're lying), then this polynomial is wrong on nearly all inputs $r_1 \in \mathbb{F}$ (because two low-degree polynomials can't agree in too many places). So the probability they get away with the lie &mdash; i.e., the new statement they're trying to prove is true &mdash; is tiny. This shows why arithmetization is nice &mdash; if we could only plug in $0$ and $1$, then we'd only have a $\frac{1}{2}$ probability of plugging in something that preserved falsity (i.e., such that the new claim the prover is trying to prove is still false). But if we can plug in arbitrary values from a much larger field, then this probability is much higher. 
            <br><br/>
            (We'll explain this in more detail when we prove soundness, but this is the main intuition behind why the protocol is sound.)
        </div>
        So the verifier has chosen a random $r_1 \in \mathbb{F}$ and computed $Q_1(r_1)$, which we'll call $k_1$; and now they're challenging the prover to convince them that $\texttt{#}\textsf{SAT}(\varphi_{r_1}) = k_1$. 
    </li>
    <li> 
        For the prover to do so, we know that $\texttt{#}\textsf{SAT}(\varphi_{r_1}) = \texttt{#}\textsf{SAT}(\varphi_{r_10}) + \texttt{#}\textsf{SAT}(\varphi_{r_11})$ from <span class = 'crossref'>(5)</span>, so the prover is going to send over $\texttt{#}\textsf{SAT}(\varphi_{r_1z})$ as a polynomial in $z$ &mdash; this means we plug in the number $r_1$ for $x_1$, the variable $z$ for $x_2$, and all possible $\{0, 1\}$-assignments to the remaining variables, i.e., \[\texttt{#}\textsf{SAT}(\varphi_{r_1z}) = \sum_{a_3, \ldots, a_n \in \{0, 1\}} \varphi(r_1, z, a_3, \ldots, a_n).\]
        <div class = 'example'>
            Let $\varphi$ be the same formula from <span class = 'crossref'>Example 11</span>, which as a polynomial is \[\varphi(x_1, x_2, x_3) = x_1 + (1 - x_1)x_2x_3 - x_1(1 - x_1)x_2x_3,\] and suppose the verifier chose $r_1 = 5$. Then the polynomial the prover is supposed to send is \[\texttt{#}\textsf{SAT}(\varphi_{5z}) = \varphi(5, z, 0) + \varphi(5, z, 1).\] The first term is $\varphi(5, z, 0) = 5 + (-4)z\cdot 0 - 5(-4)z\cdot 0 = 5$, while the second term is $\varphi(5, z, 1) = 5 + (-4)z\cdot 1 - 5(-4)z\cdot 1 = 5 + 16z$, so \[\texttt{#}\textsf{SAT}(\varphi_{5z}) = 5 + (5 + 16z) = 10 + 16z.\]
        </div>
        So the prover says, <span class = 'mypurple'>'The reason I know $\texttt{#}\textsf{SAT}(\varphi_{r_1}) = k_1$ is because $\texttt{#}\textsf{SAT}(\varphi_{r_1}z) = Q_2(z)$'</span> &mdash; this means they're sending the verifier another polynomial $Q_2(z)$ of degree at most $\textsf{size}(\varphi)$. And the verifier again checks that this justifies the previous claim &mdash; i.e., that $k_1 = Q_2(0) + Q_2(1)$ &mdash; and <span class = 'myaqua'>rejects</span> if not. 
    </li>
    <li>
        And the verifier again thinks, <span class = 'myaqua'>'Well, if you've given me the correct polynomial $\texttt{#}\textsf{SAT}(\varphi_{r_1z})$, then your claim about $\texttt{#}\textsf{SAT}(\varphi_{r_1})$ is true. But why should I believe your polynomial?'</span> And so they again choose a random $r_2 \in \mathbb{F}$ to evaluate it at, and they say, <span class = 'myaqua'>'Okay, convince me that your polynomial is correct at $r_2$, and then I'll believe you that it's actually correct.'</span> So they're computing $Q_2(r_2)$, which we call $k_2$, and then challenging the prover to convince them that $\texttt{#}\textsf{SAT}(\varphi_{r_1r_2}) = k_2$. 
    </li>
    <li>
        This continues &mdash; at the start of the $i$th step, the prover is trying to convince the verifier of a claim $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}}) = k_{i - 1}$ (where $r_1, \ldots, r_{i - 1} \in \mathbb{F}$ have been chosen uniformly at random by the verifier). To do so, they send over $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}z})$ as a polynomial in $z$ &mdash; this is defined as \[\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}z}) = \sum_{a_{i + 1}, \ldots, a_n \in \{0, 1\}} \varphi(r_1, \ldots, r_{i - 1}, z, a_{i + 1}, \ldots, a_n).\] So they say, <span class = 'mypurple'>'The reason I know $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}}) = k_{i - 1}$ is because $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}z}) = Q_i(z)$,'</span> by sending over a polynomial $Q_i(z)$ of degree at most $\textsf{size}(\varphi)$. And the verifier first checks that this justifies the previous claim according to <span class = 'crossref'>(5)</span> &mdash; i.e., that $k_{i - 1} = Q_i(0) + Q_i(1)$ &mdash; and <span class = 'myaqua'>rejects</span> if not. 
        <br><br/>
        And then the verifier thinks, <span class = 'myaqua'>'Well, why should I believe your polynomial?'</span> So they choose a random point $r_i \in \mathbb{F}$ to evaluate it at, and they say, <span class = 'myaqua'>'Okay, convince me that your polynomial is correct at $r_i$, and then I'll believe you that it's actually correct.'</span> In other words, they're computing $k_i = Q_i(r_i)$ and then asking the prover to convince them that $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_i}) = k_i$. 
    </li>
    <li>
        Finally, after the $n$th step, the prover is trying to convince the verifier that $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_n}) = k_n$, for some $r_1, \ldots, r_n \in \mathbb{F}$ (which have been randomly chosen by the verifier). And the verifier can actually just check this claim themselves &mdash; by definition we have \[\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_n}) = \varphi(r_1, \ldots, r_n),\] and the verifier can plug all these values into the arithmetized version of $\varphi$ (as discussed in <span class = 'crossref'>Remark 7</span>) to compute the right-hand side and check that it's equal to $k_n$; they <span class = 'myaqua'>accept</span> if it is and <span class = 'myaqua'>reject</span> if it isn't. 
    </li>
</ul>

Here's a more succinct description of what actually occurs in this protocol. 
<div class = 'algorithm'>
On input $\varphi$ (where $\varphi$ is a Boolean formula in $x_1$, $\ldots$, $x_n$):
<ul>
    <li> 
        The prover sends the verifier a prime $p \in [2^n \cdot \textsf{size}(\varphi), 2^{n + 1}\cdot \textsf{size}(\varphi)]$ together with a proof that $p$ is prime, which the verifier checks; all arithmetic will be done over $\mathbb{F} = \mathbb{F}_p$. 
    </li>
    <li>
        The prover sends the verifier an integer $k_0$, and the verifier checks that $k_0 \geq k$ (and <span class = 'myaqua'>rejects</span> if not). 
    </li>
    <li>
        For each $i = 1$, $\ldots$, $n$:
        <ul>
            <li> The prover sends the verifier a single-variable polynomial $Q_i(z)$. The verifier checks that $\textsf{deg}(Q_i) \leq \textsf{size}(\varphi)$ and $k_{i - 1} = Q_i(0) + Q_i(1)$, and <span class = 'myaqua'>rejects</span> if not. 
            </li>
            <li> The verifier chooses $r_i \in \mathbb{F}$ uniformly at random and sends it to the prover. 
            </li>
            <li> We set $k_i = Q_i(r_i)$ (which both parties can compute). 
            </li>
        </ul>
    </li>
    <li> 
        Finally, the verifier checks that $k_n = \varphi(r_1, \ldots, r_n)$ by plugging $r_1$, $\ldots$, $r_n$ into $\varphi$ (using arithmetic operations instead of Boolean ones). They <span class = 'myaqua'>accept</span> if yes and <span class = 'myaqua'>reject</span> if no. 
    </li>
</ul>
</div>

# Correctness

First, we'll show that this protocol has perfect correctness &mdash; i.e., whenever $(\varphi, k)$ is a $\textsf{YES}$ instance of $\textsf{Count-SAT}$, there's a good prover that makes the verifier <em>always</em> accept. 

The good prover simply tells the truth at each step, as in the more verbose description of the protocol &mdash; they first send $k_0 = \texttt{#}\textsf{SAT}(\varphi)$, and at each step they send the polynomial 
<div>
\[Q_i(z) = \texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}z}) = \sum_{a_{i + 1}, \ldots, a_n \in \{0, 1\}} \varphi(r_1, \ldots, r_{i - 1}, z, a_{i + 1}, \ldots, a_n).\]
</div>
<!-- Here's an example of a full protocol for the formula from <span class = 'crossref'>Example 11</span> and <span class = 'crossref'>Example 15</span>. (We'll ignore $p$ for the purposes of the example for simplicity, but in reality the prover and verifier set $p$ at the start and do everything over $\mathbb{F}_p$.) -->

<!-- <div class = 'example'>
    Let $\varphi(x_1, x_2, x_3) = x_1 \vee (\neg x_1 \wedge x_2 \wedge x_3)$ and $k = 4$. We have $\texttt{#}\textsf{SAT}(\varphi) = 5$ (if $x_1 = 1$, then all assignments to $x_2$ and $x_3$ are satisfying, while if $x_1 = 0$, then we need to take $x_2 = x_3 = 1$), so $(\varphi, k)$ is a $\textsf{YES}$ instance to $\textsf{Count-SAT}$. And as seen in <span class = 'crossref'>Example 11</span>, the polynomial version of $\varphi$ is \[\varphi(x_1, x_2, x_3) = x_1 + (1 - x_1)x_2x_3 - x_1(1 - x_1)x_2x_3.\]
    <ul>
        <li> First, the prover says, <span class = 'mypurple'>'I claim $\texttt{#}\textsf{SAT}(\varphi) = 5$,'</span> and the verifier checks that $5 \geq 4$ (which is true). 
        </li>
        <li> Then the verifier thinks, <span class = 'myaqua'>'Why should I believe you?'</span> so the prover says, <span class = 'mypurple'>'$\texttt{#}\textsf{SAT}(\varphi) = 5$ because $\texttt{#}\textsf{SAT}(\varphi_z) = 1 + 2z + z^2$'</span> (we saw how to compute this in <span class = 'crossref'>Example 11</span>). And the verifier checks that this really justifies the previous claim &mdash; if the prover is telling the truth about $\texttt{#}\textsf{SAT}(\varphi_z)$, then plugging in $z = 0$ and $1$ gives $\texttt{#}\textsf{SAT}(\varphi_0) = 1 + 2\cdot 0 + 0^2 = 1$ and $\texttt{#}\textsf{SAT}(\varphi_1) = 1 + 2\cdot 1 + 1^2 = 4$, so $\texttt{#}\textsf{SAT}(\varphi) = 1 + 4 = 5$ (which means their original claim is true too).  
        </li> 
    </ul>
</div> -->

Then the verifier will always accept. Intuitively, we've built the verifier such that these are the answers they're <em>expecting</em>, and they only reject when they catch the prover lying &mdash; so if the prover doesn't ever lie, the verifier won't ever reject.

# Soundness

Now we'll show that this protocol satisfies soundness &mdash; i.e., that whenever $(\varphi, k)$ is a $\textsf{NO}$ instance of $\textsf{Count-SAT}$, no matter what a cheating prover tries to do, the verifier will reject with high probability. 

Suppose we've got some cheating prover; we can assume the cheating prover is trying to <em>not</em> get caught, so they'll never do anything that makes the verifier instantly reject. In particular, this means the value $k_0$ they send over at the start is a lie &mdash; i.e., it's not the actual value of $\texttt{#}\textsf{SAT}(\varphi)$ &mdash; because if they sent over the correct value, the verifier would instantly reject (as it's less than $k$). 

This means the polynomial $Q_1(z)$ they send over has to be a lie as well (i.e., it's not the actual value of $\texttt{#}\textsf{SAT}(\varphi_z)$) &mdash; the verifier checks that $Q_1(0) + Q_1(1) = k_0$, but $\texttt{#}\textsf{SAT}(\varphi_0) + \texttt{#}\textsf{SAT}(\varphi_1)$ is $\texttt{#}\textsf{SAT}(\varphi)$, which is not $k_0$ (because $k_0$ is a lie). So if the cheating prover sent the correct polynomial, they'd immediately get caught. 

But then $Q_1(z)$ and $\texttt{#}\textsf{SAT}(\varphi_z)$ are both polynomials of degree at most $\textsf{size}(\varphi)$, which means they're equal on at most $\textsf{size}(\varphi)$ points $r_1 \in \mathbb{F}$ (as their difference has at most this many roots). This means 
<div>
\[\mathbb{P}_{r_1}[Q_1(r_1) = \texttt{#}\textsf{SAT}(\varphi_{r_1})] \leq \frac{\textsf{size}(\varphi)}{p} \leq \frac{1}{2^n}\] 
</div>
(since we chose $p$ large enough).

If this event occurs, then the cheating prover can successfully trick the verifier &mdash; even though their original claim that $\texttt{#}\textsf{SAT}(\varphi) = k_0$ was false, their new claim that $\texttt{#}\textsf{SAT}(\varphi_{r_1}) = Q_1(r_1)$ &mdash; is actually true, which means they can successfully convince the verifier of it (and therefore of the original false claim). But fortunately, this is very unlikely &mdash; with probability at least $1 - \frac{1}{2^n}$, the new claim is false as well. 

And the same thing happens at each step &mdash; suppose that the claim the cheating prover is trying to convince the verifier of at the start of the $i$th step, that $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}}) = k_{i - 1}$, is a lie. Then the prover has to lie about the polynomial $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}z})$ as well &mdash; i.e., they have to send some polynomial $Q_i(z)$ which doesn't actually equal $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}z})$ &mdash; since if they sent the correct polynomial, the verifier would catch them when checking that $Q_i(0) + Q_i(1) = k_{i - 1}$. And then since $Q_i(z)$ and $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_{i - 1}z})$ are distinct polynomials of degree at most $\textsf{size}(\varphi)$, we have 
<div>
\[\mathbb{P}_{r_i}[Q_i(r_i) = \texttt{#}\textsf{SAT}(\varphi_{r1\ldots r_i})] \leq \frac{\textsf{size}(\varphi)}{p} \leq \frac{1}{2^n}.\]
</div>
So with probability at least $1 - \frac{1}{2^n}$, the claim that the prover will be trying to convince the verifier of at the start of the next step &mdash; that $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_i}) = Q_i(r_i)$ &mdash; is a lie as well. 

Finally, since we're doing this for $n$ steps and each preserves falsity with probability at least $1 - \frac{1}{2^n}$ (meaning that if the prover's claim at the start of one step is a lie, then their claim at the start of the next step will be a lie with probability at least $1 - \frac{1}{2^n}$), this means that the prover's claim after all $n$ steps is false with probability at least $(1 - \frac{1}{2^n})^n \geq 1 - \frac{n}{2^n}$ (meaning that $\texttt{#}\textsf{SAT}(\varphi_{r_1\ldots r_n})$ isn't actually $k_n$). But the verifier checks this claim directly (by plugging in $r_1$, $\ldots$, $r_n$ into $\varphi$), so if it's a lie, they'll catch the prover and reject. 

This means the verifier rejects the cheating prover with probability at least $1 - \frac{n}{2^n}$, which is very close to $1$; so the protocol does satisfy soundness. 