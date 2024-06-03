---
layout: post
title:  "Setup of interactive proofs"
date:   2024-06-03
categories: complexity, TCS
description: Setup for interactive proofs &mdash; an informal description of how they work and a few examples. 
tags: 
giscus_comments: false
related_posts: false
---

In this post, I'll give an informal description of what interactive proofs are, and a few simple examples. The content of this post is based on lectures from the classes 18.404 (the lecture from December 7) and 18.405 (the lecture from April 11) at MIT. (This post is primarily setup for a few future posts on some really cool things we can do with interactive proofs.)

We'll first set up <em>deterministic</em> interactive proofs (to illustrate the model in a simpler setting, and to motivate what follows &mdash; in particular, why we need randomness); and then we'll extend the model to <em>randomized</em> interactive proofs (which are what we're really interested in). 

# Deterministic interactive proofs

Imagine we've got a <span class = 'vocab'>prover</span>, who we think of as all-powerful, and a <span class = 'vocab'>verifier</span>, who we think of as computationally bounded. (This means we want the verifier to run in polynomial time; we don't care how long the prover takes to run.)

Imagine we've got a statement $x$, and the prover wants to convince the verifier that $x$ is true. To do so, the prover looks at $x$ and sends the verifier some message $m_1$. And then the verifier looks at $x$ and $m_1$, and sends the prover some message $m_2$. And then the prover looks at $x$, $m_1$, and $m_2$, and sends the prover some message $m_3$. And so on &mdash; the prover and verifier keep on talking back and forth, and eventually the verifier decides to <em>accept</em> (meaning they believe the prover and agree $x$ is true) or <em>reject</em> (meaning they don't believe the prover and think $x$ is false). 

<center><img src='/assets/img/ipsat1.png' width='400' height='auto'></center>

Note that since the verifier is supposed to run in $\textsf{poly}(\lvert x\rvert)$ time, the number of messages and the length of each message should be $\textsf{poly}(\lvert x\rvert)$. 

<div class = 'question'>
    What kinds of statements can the prover convince the verifier of? Or in other words, what kinds of problems can we 'solve' using such a proof system? 
</div>

To formalize this, we need to define what it means to solve a problem using such a proof system. Imagine we've got a decision problem, which we can think of as a function $$f \colon \{0, 1\}^\ast \to \{0, 1\}$$ (where we encode the input $x$ as a bit-string, and $f(x)$ is $1$ if $x$ is a $\textsf{YES}$ instance to the decision problem and $0$ if $x$ is a $\textsf{NO}$ instance) &mdash; so here the statement the prover is trying to convince the verifier of is that $f(x) = 1$. 

What should it mean for an interactive proof to 'solve' $f$? When we talk about an interactive proof solving $f$, we're going to say essentially that there is a <em>verifier</em> who can be convinced of true statements of the form $f(x) = 1$, but not false ones. So on one hand, whenever $f(x) = 1$, there should be a <span class = 'vocab'>good prover</span> that successfully convinces the verifier that $f(x) = 1$ (because this is true). On the other hand, when $f(x) = 0$, no <span class = 'vocab'>cheating prover</span> should be able to convince the verifier that $f(x) = 1$ (because this is false) &mdash; so no matter what the prover tries to do, the verifier should reject. 

<div class = 'definition'>
    We say $f$ <span class = 'vocab'>has a deterministic interactive proof</span> if there is a verifier such that:
    <ul>
        <li> 
            <span class = 'vocab'>(Correctness)</span> For each $x$ such that $f(x) = 1$, there exists a prover that makes the verifier accept. 
        </li>
        <li>
            <span class = 'vocab'>(Soundness)</span> For each $x$ such that $f(x) = 0$, there does <em>not</em> exist a prover that makes the verifier accept (i.e., <em>every</em> possible prover causes the verifier to reject). 
        </li>
    </ul>
</div>

In other words, true statements of the form $f(x) = 1$ should have good proofs (that the verifier accepts), and false statements should not. 

<div class = 'definition'>
    We define $\textsf{DIP}$ as the class of decision problems which have a deterministic interactive proof.  
</div>

## An example --- $\textsf{SAT}$

To illustrate this definition, here's an example of a deterministic interactive proof for $\textsf{SAT}$ &mdash; the problem where we're given a Boolean formula $\varphi$, and we want to figure out whether it's satisfiable or not. 

<div class = 'definition'>
    We define $\textsf{SAT}$ as the following decision problem:
    <ul>
        <li>
            <b>Input:</b> a Boolean formula $\varphi$ (in some collection of variables $x_1$, $\ldots$, $x_n$, and with the operations $\wedge$, $\vee$, and $\neg$, which denote $\textsf{AND}$, $\textsf{OR}$, and $\textsf{NOT}$). 
        </li>
        <li>
            <b>Decide:</b> whether there exists a <span class = 'vocab'>satisfying assignment</span> to $\varphi$ &mdash; i.e., an assignment of values $a_1$, $\ldots$, $a_n$ to the variables (with $a_i \in \{\texttt{T}, \texttt{F}\}$ for each $i$) such that plugging them in makes $\varphi$ evaluate to $\texttt{T}$. 
        </li>
    </ul>
</div>

(In the notation where we think of decision problems as functions $$\{0, 1\}^\ast \to \{0, 1\}$$, we'd say that $\textsf{SAT}(\varphi)$ is $1$ if $\varphi$ is satisfiable and $0$ if not; we think of $\varphi$ as being encoded as a bit-string in some reasonable way.)

<div class = 'example'>
    The problem $\textsf{SAT}$ has a deterministic interactive proof, where the verifier and good prover (for $\textsf{YES}$ instances) are defined as follows. On input $\varphi$: 
    <ul>
        <li> 
            The good prover finds a satisfying assignment to $\varphi$ and sends it to the verifier. 
        </li>
        <li>
            The verifier plugs the assignment it receives into $\varphi$ and checks that it really is a satisfying assignment (and <span class = 'vocab'>accepts</span> if yes and <span class = 'vocab'>rejects</span> if no). 
        </li>
    </ul>
</div>

Note that we don't need to care how long it takes the <em>prover</em> to come up with the satisfying assignment; what's important is that the <em>verifier</em> can check that it's really a satisfying assignment in polynomial time. 

This protocol satisfies correctness &mdash; if $\varphi$ is satisfiable, then the good prover will make the verifier accept. It also satisfies soundness &mdash; if $\varphi$ is <em>not</em> satisfiable, then no matter what assignment the cheating prover sends, when the verifier plugs it in, they'll find that it's not a satisfying assignment (because <em>no</em> assignment is), and so they'll reject. 

## $\textsf{DIP} = \textsf{NP}$

Note that the protocol we gave for $\textsf{SAT}$ doesn't involve any interaction at all &mdash; the prover just sends one message, and the verifier just looks at it and decides to accept or reject. In fact, this type of protocol corresponds exactly to the class $\textsf{NP}$. To see this, one way of defining $\textsf{NP}$ is as the class of problems with <span class = 'vocab'>efficiently verifiable certificates</span>. 

<div class = 'definition'>
    We define $\textsf{NP}$ as the class of decision problems $f$ such that there exists a polynomial-time algorithm $\mathcal{V}$ such that for all $x$, we have \[f(x) = 1 \iff (\exists \, y \text{ of length } \textsf{poly}(\lvert x\rvert))[\mathcal{V}(x, y) \text{ accepts}].\]
</div>

In words, we think of $\mathcal{V}$ as a <span class = 'vocab'>$\textsf{NP}$-verifier</span> for $f$ &mdash; it's an efficient algorithm that takes in both our input $x$ and a polynomial-length <span class = 'vocab'>certificate</span> $y$, and checks that $y$ is a 'good certificate' for $x$. And $\textsf{YES}$ instances $x$ should have good certificates (i.e., when $f(x) = 1$ there should exist a good certificate $y$), while $\textsf{NO}$ instances should <em>not</em> have good certificates. 

<div class = 'example'>
    We have $\textsf{SAT} \in \textsf{NP}$ &mdash; we can define a 'good certificate' for $\varphi$ to be a satisfying assignment to $\varphi$ (we can construct a $\textsf{NP}$-verifier $\mathcal{V}$ which checks whether some assignment is a good certificate for $\varphi$ by simply plugging it in; and by definition $\varphi$ is a $\textsf{YES}$ instance to $\textsf{SAT}$ if and only if there exists a satisfying assignment, i.e., a good certificate for $\varphi$). 
</div>

In this definition, we can think of $\textsf{NP}$ as the class of problems $f$ which have a deterministic interactive proof with no interaction &mdash; the prover just sends a single message, and the verifier decides to accept or reject. (As in <span class = 'crossref'>Example 5</span>, we define the good prover to send over a good certificate for the input $x$, and the verifier to check that the certificate it received really works &mdash; in particular, the verifier for the interactive proof is exactly the $\textsf{NP}$-verifier $\mathcal{V}$.) In particular, this immediately means $\textsf{NP} \subseteq \textsf{DIP}$ (as $\textsf{NP}$ is a special case of $\textsf{DIP}$). 

Our definition of deterministic interactive proofs allows for interaction, so we might hope that it makes the proof system more powerful. Unfortunately, this is false &mdash; it turns out that even <em>with</em> interaction, we can't use such protocols to solve any problems other than the ones already in $\textsf{NP}$ (which we could have solved even without interaction). 

<div class = 'theorem'>
    We have $\textsf{DIP} = \textsf{NP}$. 
</div>

<div class = 'proof'>
    We've already seen that $\textsf{NP} \subseteq \textsf{DIP}$, so it remains to show that $\textsf{DIP} \subseteq \textsf{NP}$. Suppose $f \in \textsf{DIP}$, so we've got some deterministic interactive proof for $f$ &mdash; this means the prover and verifier talk back and forth, and eventually the verifier decides to accept or reject. Our goal is to remove the interaction from this protocol &mdash; i.e., to turn it into one where the prover sends a single message, and the verifier just decides whether to accept or reject. (This is because non-interactive deterministic interactive proofs correspond exactly to $\textsf{NP}$, as seen above.)
    <br><br/>
    The idea is that because the verifier is deterministic, given the input $x$, the prover can know exactly what the entire interaction will look like &mdash; they know they're going to send a message $m_1$; then they can look at $x$ and $m_1$ and figure out exactly what message $m_2$ the verifier is going to send; then they can look at $x$, $m_1$, and $m_2$ and figure out exactly what message $m_3$ they'd respond with; and so on. 
    <br><br/>
    And this means the prover can just send the verifier their <em>entire</em> end of the interaction all in one message &mdash; i.e., the prover sends the verifier $(m_1, m_3, \ldots)$. And the verifier then simulates the interaction by themselves, plugging in these messages for the prover's end of the interaction &mdash; they look at $x$, imagine that the prover sends them $m_1$ and figure out what message $m_2$ they'd respond with, imagine that the prover then sends them $m_3$ and figure out what message $m_4$ they'd respond with, and so on; and in the end, they see whether they'd accept or reject (and they make the same decision here). 
    <br><br/>
    If $f(x) = 1$, then there's some good prover for the original protocol that makes the original verifier accept; so if we take $(m_1, m_3, \ldots)$ to correspond to that good prover, then the new verifier will accept as well (since it's just simulating the original verifier's interaction with that good prover). Meanwhile, if $f(x) = 0$, then every cheating prover for the original protocol makes the original verifier reject, so no matter what list of messages $(m_1, m_3, \ldots)$ the new cheating prover sends, the new verifier will reject as well (since it'll be simulating the original verifier's interaction with some cheating prover). 
<!-- 
    And this means the prover can just send the verifier the <em>entire</em> transcript all at once &mdash; i.e., they send the verifier the entire list of messages $(m_1, m_2, m_3, m_4, \ldots)$. And the verifier can look at this transcript and say, 'Yup, that looks good on my end' &mdash; i.e., they check that on input $x$, if the prover sent them $m_1$ then they'd really respond with $m_2$, then if the prover responded back with $m_3$ they'd really respond with $m_4$, and so on; and that at the end of this interaction, they'd accept. 
    <br><br/>
    To see that this protocol satisfies correctness, if $f(x) = 1$ then the new good prover can just send the correct transcript corresponding to the interaction of the original verifier with the original good prover, and this will make the new verifier accept. 
    <br><br/>
    To see that it satisfies soundness, if $f(x) = 0$ then any transcript the cheating prover sends is going to fall into one of two cases:
    <ul>
        <li>
            <b>Case 1:</b> It's the correct transcript for the interaction of the original verifier with some cheating prover (for the original protocol) &mdash; i.e., all the messages $m_2$, $m_4$, $\ldots$ corresponding to the verifier's end of the interaction are correct. In this case, our new verifier will be satisfied with each step of the transcript, but they'll find that in the end the original verifier would reject (by the soundness of the original protocol), so they'll reject too. 
        </li>
        <li>
            <b>Case 2:</b> It's <em>not</em> the correct transcript for the interaction of the original verifier with any cheating prover &mdash; i.e., one of the messages $m_2$, $m_4$, $\ldots$ on the verifier's end is wrong. In this case, the new verifier is going to catch this (since they're checking that all the messages on the verifier's end of the transcript are really what the old verifier would've said), so they'll reject. 
        </li>
    </ul> -->
</div>

# Randomized interactive proofs

We've seen that with deterministic interactive proofs, we don't get anything out of interaction &mdash; in other words, any deterministic interactive proof that involves interaction can be turned into one that doesn't (which means $\textsf{DIP} = \textsf{NP}$). And the reason for this was essentially that if the verifier is deterministic, then the prover can predict in advance <em>everything</em> that the verifier is going to say, so they can just have all their responses ready and send them all at once. 

We don't like this &mdash; our hope is to define a reasonable model of interactive proofs that <em>increases</em> the class of problems we could solve. And the way we're going to do this is by allowing the verifier to be <em>randomized</em> &mdash; this means the verifier gets to toss some coins when it's choosing its message. So the prover looks at the input $x$ and sends a message $m_1$, as before. And then the verifier looks at $x$ and $m_1$ <em>and tosses some coins</em> to come up with a message $m_2$ to respond with. And then the prover looks at $x$, $m_1$, and $m_2$ to come up with a message $m_3$, and the verifier looks at $x$, $m_1$, $m_2$, and $m_3$ and tosses some more coins to come up with a message $m_4$, and so on; and in the end the verifier either accepts or rejects. 

<center><img src='/assets/img/ipsat2.png' width='400' height='auto'></center>

(In this picture, $r_i$ denotes the randomness the verifier uses at each step &mdash; i.e., the outcomes of the $i$th set of coin tosses.)

<div class = 'remark'>
    We're using a <span class = 'vocab'>private-coin model</span>, where the prover doesn't get to see the outcomes of the verifier's coin tosses. But you could also imagine a <span class = 'vocab'>public-coin model</span>, where the prover <em>does</em> get to see these outcomes. Of course, any public-coin interactive proof can be made into a private-coin one (where we have the verifier simply announce the outcomes of their coin tosses together with the corresponding message). Quite surprisingly, it turns out that the converse is true as well &mdash; any private-coin protocol can also be made into a public-coin one! This is a very nice <a href="https://www.cs.toronto.edu/tss/files/papers/goldwasser-Sipser.pdf" target="_blank">result</a> of Goldwasser and Sipser (1986). 
    <br><br/>
    I'm probably not going to write more about the public-coin model; the protocol for graph non-isomorphism we'll see in this post will rely quite crucially on the fact that the prover doesn't see the verifier's coins (it can be converted to a public-coin protocol by the result of Goldwasser&ndash;Sipser, but this isn't obvious); but the protocols we'll see in future posts won't be (i.e., they can directly be viewed as public-coin protocols as well). 
</div>

We now need to define what it means for a randomized interactive proof to solve a decision problem $f$. This will be very similar to the definition for deterministic interactive proofs, but now that we've got randomness, we need to allow some probability of error (otherwise the randomness wouldn't have any point). 

<div class = 'definition'>
We say $f$ <span class = 'vocab'>has an interactive proof</span> if there is a verifier such that:
<ul>
<li>
    <span class = 'vocab'>(Correctness)</span> For each $x$ such that $f(x) = 1$, there exists a prover that makes the verifier accept with probability at least $\frac{2}{3}$. 
</li>
<li>
    <span class = 'vocab'>(Soundness)</span> For each $x$ such that $f(x) = 0$, for every possible prover, the probability that the verifier accepts is at most $\frac{1}{3}$. 
</li>
</ul>
</div>

There's a few decisions we've made with these definitions that aren't obvious (i.e., it'd have been reasonable to do the opposite) but turn out not to matter:
<ul>
    <li> The values of $\frac{2}{3}$ (which we call the <span class = 'vocab'>correctness parameter</span>) and $\frac{1}{3}$ (the <span class = 'vocab'>soundness parameter</span>) are arbitrary &mdash; if we've got a protocol with these parameters, then we can get one with correctness parameter $1 - \frac{1}{2^k}$ and soundness parameter $\frac{1}{2^k}$ by simply running $\textsf{poly}(k)$ independent trials of the original one (and taking their majority outcome). 
    </li>
    <li> We're allowing error in both the cases $f(x) = 0$ and $f(x) = 1$. But we could imagine instead allowing error in only one of the cases &mdash; we say a protocol has <span class = 'vocab'>perfect correctness</span> if it only has error in the case $f(x) = 0$ (i.e., when $f(x) = 1$, there's a prover that makes the verifier <em>always</em> accept). 
    <br><br/>
    It turns out that any interactive proof protocol can be converted to one with perfect correctness; this is not obvious and is also a very nice result, though I'm not sure who it's due to. All the protocols we'll discuss will have perfect correctness. 
    <br><br/>
    (On the other hand, the error in soundness <em>is</em> necessary &mdash; it's <em>not</em> true that we can convert any protocol to one with perfect soundness.)
    </li>
    <li> We've made the <em>verifier</em> randomized, but we're still keeping the <em>prover</em> deterministic. We could imagine allowing the prover to be randomized as well, but this wouldn't increase the power of the system &mdash; the prover is computationally unbounded, so we can without loss of generality assume that the prover always (deterministically) chooses the response that maximizes the probability of the verifier accepting. 
    </li>
</ul>

## An example &mdash; graph non-isomorphism

We first considered the model of <em>deterministic</em> interactive proofs, and we saw that the only problems we can solve using them are the problems in $\textsf{NP}$ (for which we don't even need the interaction). So the first thing we might wonder is, does this new model of <em>randomized</em> interactive proofs, have the same issue, or does it actually allow us to solve new problems?

So the first thing we'll see is an example of an interactive proof for a problem that we don't know to be in $\textsf{NP}$ &mdash; the problem of determining whether two graphs are <em>non</em>-isomorphic. 

<div class = 'definition'>
    Two graphs $G_1$ and $G_2$ are <span class = 'vocab'>isomorphic</span> if there's a way to permute the vertices of $G_1$ that turns it into $G_2$ &mdash; i.e., a permutation $\pi \colon V(G_1) \to V(G_2)$ such that $\{u, v\}$ is an edge in $G_1$ if and only if $\{\pi(u), \pi(v)\}$ is an edge in $G_2$.
</div>

<div class = 'definition'>
    We define $\textsf{GNI}$ as the following decision problem:
    <ul>
        <li> 
            <b>Input:</b> two graphs $G_1$ and $G_2$ (with the same number of vertices). 
        </li>
        <li>
            <b>Decide:</b> whether $G_1$ and $G_2$ are <em>non</em>-isomorphic. 
        </li>
    </ul>
</div>

Written as a function $$\{0, 1\}^\ast \to \{0, 1\}$$, we'd say that $\textsf{GNI}(G_1, G_2)$ is $1$ if $G_1$ and $G_2$ are non-isomorphic, and $0$ if they are isomorphic. 

The opposite problem $\textsf{GI}$, of determining whether two given graphs $G_1$ and $G_2$ <em>are</em> isomorphic, is in $\textsf{NP}$ &mdash; we can take the certificate to simply be the permutation $\pi \colon V(G_1) \to V(G_2)$ that turns $G_1$ into $G_2$. This in particular means $\textsf{GI}$ has an interactive proof (that doesn't involve any interaction), as with all problems in $\textsf{NP}$. 

But it <em>isn't</em> clear whether $\textsf{GNI}$ is in $\textsf{NP}$ &mdash; there's a simple certificate showing that two graphs <em>are</em> isomorphic, but we don't know of a certificate showing that they're <em>not</em> isomorphic. 

Still, it turns out that $\textsf{GNI}$ <em>does</em> have a (randomized) interactive proof! 

<div class = 'example'>
    The problem $\textsf{GNI}$ has an interactive proof &mdash; on input $(G_1, G_2)$:
    <ul>
        <li> The verifier chooses $i \in \{1, 2\}$ uniformly at random and permutes the vertices of $G_i$ uniformly at random to produce a new graph $H$. </li>
        <li> The verifier sends the prover $H$ and asks them which of $G_1$ and $G_2$ it came from (i.e., what the value of $i$ is). They <span class = 'vocab'>accept</span> if the prover answers correctly and <span class = 'vocab'>reject</span> if not. </li>
    </ul>
</div>

<div class = 'proof'>
    If $G_1$ and $G_2$ are indeed non-isomorphic, then there's a good prover who'll always be able to answer correctly &mdash; $H$ must be isomorphic to the graph $G_i$ that the verifier chose (by construction), and it can't be isomorphic to the other one (because if it were isomorphic to both, then $G_1$ and $G_2$ are isomorphic). So the prover can simply recover $i$ by checking which of $G_1$ and $G_2$ it's isomorphic to. 
    <br><br/>
    Meanwhile, if $G_1$ and $G_2$ are isomorphic, then the graph $H$ that the prover sees has nothing to do with $i$ (i.e., it has the same distribution whether $i$ is $1$ or $2$ &mdash; either way, it's a uniform random element of their isomorphism class). So no matter what the cheating prover tries to do, they're essentially being asked to predict a random coin toss with no useful information, which means they'll only have a $\frac{1}{2}$ chance of being correct. 
</div>

## The class $\textsf{IP}$

The example of an interactive proof for $\textsf{GNI}$ shows that adding randomness to our model has done <em>something</em> nontrivial &mdash; it's allowed us to solve at least one problem that we wouldn't have known how to solve without it. So that's good news; but now we'd like to get a better understanding of <em>how</em> much more we can now solve. 

<div class = 'definition'>
    We define $\textsf{IP}$ as the class of decision problems $f$ which have an interactive proof. 
</div>

<div class = 'question'>
    How powerful is $\textsf{IP}$?
</div>

It turns out, quite surprisingly, that $\textsf{IP}$ is <em>extremely</em> powerful! 

<div class = 'theorem' text = 'Shamir 1990'>
    We have $\textsf{IP} = \textsf{PSPACE}$. 
</div>

(The class $\textsf{PSPACE}$ is defined as the class of problems which can be solved in polynomial <em>space</em>.)

This is quite amazing &mdash; for example, it means that there's an interactive proof that a Boolean formula is <em>not</em> satisfiable (which is already quite non-obvious), as well as lots of problems that look much harder.

One direction of <span class = 'crossref'>Theorem 16</span>, that $\textsf{IP} \subseteq \textsf{PSPACE}$, isn't too difficult &mdash; the idea is that if we start with some interactive proof for some decision problem $f$, then by carefully going through all possible messages the prover and verifier could send, we can calculate the probability that the 'optimal prover' makes the verifier accept; and we can do this only using polynomial space. Here are the details of this argument.

<div class = 'lemma'>
    We have $\textsf{IP} \subseteq \textsf{PSPACE}$. 
</div>

<div class = 'proof'>
    Suppose that $f \in \textsf{IP}$, so it has some verifier as in the definition of $\textsf{IP}$. Then in order to decide $f$ on an input $x$, our goal is to calculate the maximum (over all possible provers) of the probability (over the verifier's internal randomness) that the verifier accepts &mdash; because we know this probability is at least $\frac{2}{3}$ if $f(x) = 1$ (by correctness) and at most $\frac{1}{3}$ if $f(x) = 0$ (by soundness). 
    <br><br/>
    To do so, suppose the protocol runs for $k$ rounds (where $k = \textsf{poly}(\lvert x\rvert)$). Then for each $0 \leq i \leq k$, we'll define $\textsf{Prob}_i(x, m_1, \ldots, m_i)$ as the maximum (over all possible provers) probability that the verifier accepts if we start up the protocol from the end of the $i$th round pretending that these messages have already occurred (i.e., we imagine that the prover has already sent $m_1$, the verifier has already responded with $m_2$, and so on up to $m_i$; and now we're starting the protocol up from the end of the $i$th round, and we want to think about how it continues). Then our goal is to compute $\textsf{Prob}_0(x)$. 
    <br><br/>
    We'll compute these quantities recursively &mdash; suppose we're trying to compute $\textsf{Prob}_{i - 1}(x, m_1, \ldots, m_{i - 1})$. Then we consider what happens on the $i$th round. If $i$ is odd, meaning that the prover speaks, then since we're trying to take a <em>maximum</em> over all possible provers (i.e., we want to consider the <em>best</em> possible message $m_i$ for the prover to send), we have \[\textsf{Prob}_{i - 1}(x, m_1, \ldots, m_{i - 1}) = \max_{m_i}\textsf{Prob}_i(x, m_1, \ldots, m_i).\] Meanwhile, if $i$ is even, meaning that the verifier speaks, then we want to consider the <em>distribution</em> (over the verifier's internal randomness) of the message $m_i$ it sends (given the previous messages), and compute the appropriate weighted average of the acceptance probabilities of the remainder of the protocol, so \[\textsf{Prob}_{i - 1}(x, m_1, \ldots, m_{i - 1}) = \sum_i \mathbb{P}[\text{verifier sends $m_i$}]\cdot \textsf{Prob}_{i - 1}(x, m_1, \ldots, m_i).\]
    This gives a recursion that we can compute in polynomial space &mdash; we start with $i = 0$, then iterate over all messages $m_1$ one at a time, then iterate over all messages $m_2$ one at a time (for each $m_1$), and so on. At every layer of the recursion, we essentially only need to store the current message $m_i$ we're trying, as well as our current computation for the maximum or sum (note that the quantities $\mathbb{P}[\text{verifier sends $m_i$}]$ can be computed in polynomial space as well, simply by iterating over all possible outcomes of the verifier's coin tosses one at a time and running the verifier with each).
    <br><br/>
    So we can use this recursion to compute $\textsf{Prob}_0(x)$ in polynomial space, which lets us solve $f$. 
</div>

So the direction that $\textsf{IP} \subseteq \textsf{PSPACE}$ is not too hard. On the other hand, the direction that $\textsf{PSPACE} \subseteq \textsf{IP}$ &mdash; that every problem which can be solved in polynomial <em>space</em> in fact has an interactive proof where the verifier runs in polynomial <em>time</em> &mdash; is much more surprising, and the proof involves several very cool ideas. I'll explain the proof in future posts. 
