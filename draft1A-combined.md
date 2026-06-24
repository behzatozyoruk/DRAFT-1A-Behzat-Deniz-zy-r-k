(Based on *Bisimulation for Labelled Markov processes* by J. Desharnais, A. Edalat, and P. Panangaden.)

To do:
- [ ] probably remove the summary in the introduction and add some questions instead
- [ ] check if figures render properly

After reviewing the classical notions of transition systems and bisimulation equivalence, we make a case for a continuous state space model (Labelled Markov Processes) and generalise the previous notion of bisimulation. Along the way, we will need to introduce measure theoretic tools. In the end, we discuss a rather unexpected logical characterisation of bisimulation for LMPs.

#### Why bisimulation?
Given an abstract system, we often want to reduce it to an equivalent smaller system, check whether the model is equivalent to a different one, or verify that a property holds -- in order to do this, we need to ask: `when do two states have exactly the same observed behaviour?'.

The notion of bisimulation proves to be a natural choice for defining equivalence. It also happens to have a very convenient categorical characterisation.

#### Why continuous models?
Many real-world models have an intrinsic continuous nature (for instance, the movement of an aircraft or the pressure of a gas in a tank). Often, the state space can be discretized, but how can we tell if the discretized system faithfully represents the continuous system? What if we need a finer approximation? We will make a case for a discrete time, continuous state space model [^1], introduce tools from measure theory, and discuss a sensible notion of bisimulation in this case.

[^1]: Eventually, we would also want a fully continuous model; however, this will require a very different set of technical tools, namely stochastic differential equations.

## Labelled transition systems

A convenient representation of discrete systems is given by labelled transition systems (LTSs).

**Example 1.** We can model a vending machine as a labelled transition system, with the states being given by the instructions provided by the machine, such as `Place cup` and `Choose`:

<p align="center">
  <img src="figures/fig1.svg" alt="Example 1" width="300">
</p>

In general, we define this class of systems as follows:

**Definition 2.** A labelled transition system is a triple $(S, \mathcal{A}, \rightarrow_a)$ consisting of a set of states $S$, a set of actions $\mathcal{A}$, and, for each $a \in \mathcal{A}$, a transition relation $\rightarrow_a \subseteq S \times S$.

Note that the definition allows nondeterminism: the transition relation need not be a single pair of states, but a set of pairs. So we can also model the following vending machine:

**Example 3.**
<p align="center">
  <img src="figures/fig2.svg" alt="Example 3" width="400">
</p>

Note that the two vending machines behave differently: in the first machine, the user (the environment) is given the choice between coffee and tea; meanwhile, the second machine gives the user no choice, the choice happening internally. How can express this and differentiate between the faulty machine and the functional one? 

In general, we would want define a sensible notion of equivalence for LTSs. One first attempt could be to look at the language the systems accept. In our case, both machines accept the language $[\text{cup}; (\text{coffee} + \text{tea})]^*$, so this notion is clearly not strong enough. A better option is to look at a finer level, at pairs of states; we expect states that are equivalent to remain equivalent as they evolve, and so we check whether this natural condition is indeed obeyed:

**Definition 4.** Define a bisimulation relation $\sim$ on $S$ by $s \sim t$ if the following  holds: $$\forall s' \in S, \forall a \in \mathcal{A}, \text { if } s \xrightarrow{a} s' \text{ then } \exists t'\ \in S \text { with } t \xrightarrow{a} t' \text{ such that } s' \sim t', \text{ and vice versa.}$$

Now, using our definition[^2], the `Choose` state of the functional vending machine is clearly not equivalent to either of the `Choose` states of the faulty one, and so the two systems are indeed not equivalent.

[^2]: At first glance, the definition might seem circular. However, this definition can be shown to be equivalent to an inductive definition. We omit the discussion here, but we refer the reader to *Sangiorgi, Davide (2011). An introduction to Bisimulation and Coinduction. Cambridge, UK: Cambridge University Press.*

## Labelled Markov chains

Often, we need a model of quantitative nature in which the transitions of the system happen with a given probability. We can easily extend the previous definition to account for this:

**Definition 5.** A labelled Markov chain is a triple $(S, \mathcal{A}, T_a)$ consisting of a set of states $S$, a set of actions $\mathcal{A}$, and, for each $a \in \mathcal{A}$, a transition probability matrix $T_a: S \times S \rightarrow [0,1]$.    

Note that all probabilistic data is internal, and the environment is assumed to be deterministic. That is to say, the model is reactive: we are only concerned with how the model reacts, i.e., what an external observer can see. What does it mean for two systems to be equivalent in this case? 

**Example 6.** Consider the two following systems: 

<p align="center">
  <img src="figures/fig3.svg" alt="Example 6" width="400">
</p>

Are the states $s_0$ and $t_0$ equivalent? Intuively, they have the same behaviour: $s_2$ and $s_3$ behave in the same way $t_2$, and the probability landing in $s_2$ or $s_3$ is the same as landing in $t_2$. This motivates the following notion of equivalence:

**Definition 7** [Larsen and Skou] **.** We say $\sim_{LS}$ is a bisimulation relation if whenever $s \sim_{LS} t$ then for any $C \in S/ \sim_{LS}$ we have 
$$\sum_{v \in C} P_a(s, v) = \sum_{v \in C} P_a(t, v).$$

## Continuous state spaces: The need for measure theory

In a standard discrete labelled Markov chain, transitions are straightforward. If our system is in state $s$, the environment can trigger an interaction (a label $l$), and the system will jump to a new state $s^{\prime}$ with a specific probability. We can write this as a transition matrix $P_{l}(s,s^{\prime})$. What if the state space is not discrete, say, it is a "continuous" space such as $\mathbb{R}^{n}$? Can we define transition probabilities in a similar fashion? 

Consider the stochastic system where the state space is $\mathbb{R}^{2}$ and the set of labels is *L={a,b}*. If the system $a$-transitions from a state $(x_{0},y_{0})$ to a new state $(x,y_{0})$, then the probability of $x\in[s,t]$ is given by: 

$$
\sqrt{\frac{\alpha}{\pi}}\int_{s}^{t}\exp(-\alpha(x-x_{0})^{2})dx
$$

Similarly, if $(x_{0},y_{0})$ $b$-transitions to $(x_{0},y)$, then the probability of $y\in[s,t]$ is given by:

$$
\sqrt{\frac{\beta}{\pi}}\int_{s}^{t}\exp(-\beta(x-x_{0})^{2})dx
$$

In this system, an $a$-transition from a state $(x_{0},y_{0})$ to $(x_{1},y_{0})$ occurs with zero probability. Therefore it is not possible to define transition probabilities between states; instead, we should consider transition probabilities from a state to a set of states. Indeed, singletons are no longer the "atomic building blocks" as in the discrete case, in which the probability of transitioning from a state to a set of states is nothing but the sum of the transition probabilities of each state in the target set. Thus, given an initial state and a label, we should consider assigning a transition probability to subsets of the state space.

Unfortunately, this will fail if we try to make it work for all subsets of a continuous state space because not all subsets are going to be well-behaved enough to be assigned a probability. This is a more general problem; if our goal is to assign a proper notion of "size", say volume, area, length, or probability, to each subset of a given set, then we need to work with measure theory. First, let us define the well-behaved sets we mentioned earlier.

**Definition:** A $\sigma$-algebra on a set $X$ is a collection of subsets of $X$ that contains $X$, is closed under taking complements, and is closed under countable union.

Such a pair $(X, \Sigma)$ is called a measurable space. Almost every object in mathematics comes with morphisms, so here are the morphisms of measurable spaces:

**Definition:** Given measurable spaces $(X, \Sigma)$ and $(Y, \Lambda)$, a function $f:X\rightarrow Y$ is called a measurable function if $\forall B\in\Lambda$, $f^{-1}(B)\in\Sigma.$

Here is how we formally assign "sizes" to these well-behaved sets.

**Definition:** Given a measurable space $(X, \Sigma)$, a *measure* is a function $\mu : \Sigma \rightarrow [0, \infty]$ such that $\mu(\emptyset) = 0$ and given a pairwise disjoint family of sets $\lbrace A_{n} : n \in \mathbb{N} \rbrace$ in $\Sigma$, we have $\mu(\bigcup_{n \in \mathbb{N}} A_{n}) = \sum_{n \in \mathbb{N}} \mu(A_{n})$.

An immediate property that can be concluded from this definition is that a measure is monotone: $A\subset B\Rightarrow\mu(A)\le\mu(B)$.

The Lebesgue measure on $\mathbb{R}$ is a well-known measure which is defined as follows. First of all, given an interval $I=[a,b]$, its length is defined as $l(I)=b-a$.

**Definition:** Given $E \subset \mathbb{R}$, the Lebesgue outer measure $m^\ast(E)$ is defined as:

$$
\inf \left\lbrace \sum_{k=1}^{\infty}l(I_{k}) : E \subset \bigcup_{k=1}^{\infty}I_{k} \text{ where } I_{k} \text{ is an open interval for all } k \in \mathbb{N} \right\rbrace
$$

Now, we restrict $m^\ast$ to subsets $E$ of $\mathbb{R}$ which satisfy $m^\ast(A)=m^\ast(A\cap E)+m^\ast(A\cap E^{c})$ for all $A\subset\mathbb{R}$. These sets are called Lebesgue measurable sets and such sets constitute a $\sigma$-algebra. In addition, the restricted $m^\ast$ becomes a measure, called the Lebesgue measure. It is translation invariant, that is, $m(E+x)=m(E)$ for any $x\in\mathbb{R}$ and Lebesgue measurable set $E$.

Now, we are ready to give an example of a set in $\mathbb{R}$ for which we can't assign a "size", or now that we know what size means, a measure. In other words, we will now construct a set $V$ which is not Lebesgue measurable, assuming the axiom of choice. It is called the Vitali set.

Define the equivalence relation on $[0, 1]$ by $x \sim y$ iff $x - y \in \mathbb{Q}$. This partitions $\mathbb{R}$ into uncountably many equivalence classes $[x] = (\mathbb{Q} + x) \cap [0, 1]$ and invoking the axiom of choice we can choose one representative from each class and collect them to form a set $V$. For the sake of contradiction, suppose $V$ is Lebesgue measurable. So, we can safely assume $m(V)$ is a non-negative real number. Now, enumerate $\mathbb{Q} = (q\_k)\_{k \geq 1}$ and consider translations $V\_k = V + q\_k$. Notice that these sets are pairwise disjoint and also $m(V\_k) = m(V)$ for each $k$. Letting $U = \bigcup\_{k \geq 1} V\_k$, we arrive at the following conclusion:

$$[0, 1] \subset U \subset [-1, 2]$$

Recall that any measure is monotone and any measure distributes over a disjoint union of countably many measurable sets, therefore:

$$1 \leq \sum_{k \geq 1} m(V) \leq 3$$

If $m(V) = 0$, this implies $1 \leq 0$ and if $m(V) > 0$ this implies $\sum_{k \geq 1} m(V) = \infty \leq 3$. In either case, we arrive at a contradiction. Thus, $V$ is not Lebesgue measurable.

---

## Labelled Markov processes in this setting

In this measure-theoretic setting, we can now safely assign transition probabilities. Note that a measure $p$ on a measurable space $(X, \Sigma)$ is called a *probability measure* if $p(X)=1$. Now, let us define Markov kernels.

**Definition:** Given measurable spaces $(X, \Sigma)$ and $(Y, \Lambda)$, a *Markov kernel* $T$ from $(X, \Sigma)$ to $(Y, \Lambda)$ is a function:
$$T:X\times\Lambda\rightarrow[0,1]$$
such that for all fixed $x\in X$, $T(x,-)$ is a probability measure and for all fixed $B\in\Lambda$, $T(-,B)$ is a measurable function.

There is a category **SRel** where objects are measurable spaces and morphisms are Markov kernels. This is the category of stochastic relations. Given a kernel $T$ in **SRel**, by currying, we obtain a map of the form $\overline{T}:X\rightarrow[0,1]^{\Sigma}$ where the codomain is the set of probability measures on $\Sigma$ which is equipped with a special $\sigma$-algebra structure $\Xi$. Indeed, the currying takes place in the category **Meas** of measurable spaces where the assignment $(X,\Sigma)\rightarrow(\Sigma^{[0,1]},\Xi)$ is given by the Giry monad. Note that the morphisms $\overline{T}:X\rightarrow[0,1]^{\Sigma}$ are in fact the morphisms in the Kleisli category of the Giry monad.

This is analogous to classic relations; a relation *R* between sets *X* and *Y* is a function *X* × *Y* → {0,1}, and by currying we obtain a Kleisli morphism of the power set monad. Hence the name stochastic relations.

From now on, we focus on sub-probability measures. A measure $p$ on a measurable space $(X, \Sigma)$ is called a *sub-probability measure* if $p(X)\le1$. We will explain the reason for doing this later. Let's adjust the definition of Markov kernels accordingly for our purposes.

**Definition:** Given a measurable space $(X, \Sigma)$, a *transition probability function* on $X$ is a function:
$$T:X\times\Sigma\rightarrow[0,1]$$
such that for fixed $x\in X$, $T(x,-)$ is a sub-probability measure and for fixed $A\in\Sigma$, $T(-,A)$ is a measurable function.

Let's move on to our main discussion.

**Definition:** A *labelled Markov process* with label set $L$ a tuple $(S, \Sigma, (\tau_l)_{l\in L})$ where $S$ is an analytic space, $\Sigma$ is the Borel $\sigma$-algebra on $S$, and for all $l \in L$

$$\tau_l : S \times \Sigma \rightarrow [0, 1]$$

is a transition probability function.


This definition also captures previously defined labelled Markov chains. The analytic space condition may seem too restrictive, but the situation is the exact opposite. Many researchers in measure-theoretic probability prefer to work with Polish or analytic spaces because they provide a sufficiently general framework in which many fundamental theorems, such as the Bayesian inversion, hold.

From now on, let us fix the label set $L$. We will define a category of labelled Markov processes. Here are the morphisms of this category:

**Definition:** A simulation morphism $f:(S,\Sigma,k_{l})\rightarrow(S^{\prime},\Sigma^{\prime},k_{l}^{\prime})$ between labelled Markov processes is a measurable function with respect to the underlying measurable spaces such that $k_{l}(s,f^{-1}(A^{\prime}))\le k_{l}^{\prime}(f(s),A^{\prime})$ for all $l\in L$, $s\in S$, $A^{\prime}\in\Sigma^{\prime}$.

This is in fact the usual simulation on the underlying labelled transition systems. Labelled Markov processes and simulations form a category, denoted by **LMP**.

---

## Bisimulation for Markov processes

A simulation morphism simply means the target system can at least match the probabilistic moves of the source. Tightening the inequality given by the simulation to a strict equality $k_{l}(s,f^{-1}(A^{\prime}))=k_{l}^{\prime}(f(s),A^{\prime})$ ensures that no probabilities are lost or gained along the mapping. Furthermore, by demanding the morphism be surjective, we guarantee that no state in the target is left unaccounted for.

Such a morphism is called a *zigzag*.

**Definition:** A *zigzag* morphism $f:(S,\Sigma,k_{l})\rightarrow(S^{\prime},\Sigma^{\prime},k_{l}^{\prime})$ is a surjective simulation morphism such that $k_{l}(s,f^{-1}(A^{\prime}))=k_{l}^{\prime}(f(s),A^{\prime})$ for all $l\in L$, $s\in S$, $A^{\prime}\in\Sigma^{\prime}$.

An existence of a zigzag between two systems is enough to ensure bisimilarity. But bisimulation is something more general than just the existence of some zigzag.

We refrain from giving the exact definition of a bisimulation. The full definition differs only slightly from the one presented here, but involves some technicalities. Later work has shown that some of these technicalities can in fact be avoided. Here is the incomplete definition.

**Definition:** Given labelled Markov processes $T$ and $T^{\prime}$, we say $T$ is *probabilistically bisimilar* to $T^{\prime}$ if there exists $S \in \text{LMP}$ and zigzag morphisms $f$ and $g$ such that there is a span:

$$T \stackrel{f}{\longleftarrow} S \stackrel{g}{\longrightarrow} T^{\prime}$$

Proving that bisimulation is an equivalence relation, in particular a transitive relation, is not straightforward. The natural approach would be to compose two bisimulations by taking the pullback of the corresponding spans, but unfortunately **LMP** is not closed under pullbacks. In the paper, authors instead construct a semi-pullback, the construction is quite involved and depends on the analytic structure.

The analytic structure also provides another useful description of bisimulation: it can be defined equivalently as a cospan of zigzags. This definition is sometimes more convenient to work with.

Lastly, recall that we decided to work with sub-probability measures. This is because if we insisted on full probability measures, then every system would be bisimilar to a one-state system!

Here are some examples of bisimulation.

**Example:** Recall the example we began with; it is actually bisimilar to a one-state system which can make $a$ and $b$ transitions with probability 1. This example demonstrates that a continuous state system can be reduced to a finite system.

**Example:** Consider the labelled Markov process with a singleton label set defined by $(\mathbb{R},\mathcal{B},k)$ where $\mathcal{B}$ is the Borel algebra and the transition function is defined on intervals (which can be extended to all Borel sets) by:

$$
k(x,[r,s])=\begin{cases}\frac{\lambda}{2}\int_{r}^{s}e^{-\lambda|x-y|}dy & \text{if } x\ge0, \\ 0 & \text{otherwise.}\end{cases}
$$

where the $\lambda/2$ factor normalizes $k$ on the whole space.

Consider another system $(\mathbb{R}^{2}, \mathcal{B}_{2}, h)$ defined by:

$$
h((x,y),[r,s]\times[p,q])=k(x,[r,s])P([p,q])
$$

where $P$ is an arbitrary probability measure on $\mathbb{R}$. The first coordinate of the latter system behaves similar to the former and the second coordinate's behavior is similar to that of a trivial system—a one-state, one-transition system. Thus, the zigzag projection from the second onto the first system gives us a bisimulation between the two systems.

## Logical characterisations of bisimulation

We have previously defined bisimulation, a notion of equivalence for transition systems. Is there an alternative way to think of equivalance? Can we empirically `test' whether two systems are equivalent? We can do so by thinking of the logical formulas the systems satisfy: if we can find one formula which is satisfied by one system but not the other, than the systems are not equivalent.

**Example 8.** Consider the following two discrete systems: 

<p align="center">
  <img src="figures/fig4.svg" alt="Example 8" width="300">
</p>

Clearly, the formula $\left<a\right> \lnot \left<b\right> \top$ distinguishes between $S$ and $T$, and hints to the fact that we cannot get by without negation. Indeed, there there is no negation-free logic that can characterise bisimulation in this case. One logic that does characterise it is the Hennessey-Milner logic:
$$\mathcal{L}_{HM} := \top \mid \lnot \phi \mid \phi_1 \land \phi_2 \mid \left<a\right> \phi$$ 
where $s \vDash \left<a\right> \phi$ means that there is an $a$-action from the state $s$ to a state $s'$ which satisfies $\phi$.

#### Labelled Markov Processes
We would expect the situation to become more complicated when we move to continuous state space systems, and a more powerful logic to be needed. Rather surprisingly, this is not the case. In fact, we don't even need negation or infinite conjunction. 

**Example 9.** Let's reconsider the example above, this time adding probabilities to transitions: for $S$, suppose that the two $a$-actions have probabilities $p$ and $q$ respectively, and the $b$-action has probability $1$; for $R$, the $a$-action has probability $r$ and the $b$-action probability $1$.

<p align="center">
  <img src="figures/fig5.svg" alt="Example 9" width="300">
</p>

If $p = 0$, then $S$ and $T$ are in fact equivalent. So assume $p \neq 0$.

We claim that $S$ are $T$ are bisimilar for any $p, q, r \in [0, 1]$ with $p \neq 0$.

If $r > p + q$, then then $T$ satisfies formula  $\left< a \right>_r T$, while $S$ doesn't. Similarly for $r < p + q$. 

If $r = p + q$, then the formula $\left< a \right>_r \left< b \right>_1 T$ is satisfied by T, but not by S.

In some sense, probability makes up for the lack of negation, and shows that probabilistic systems are in fact very close to deterministic systems.


Thus, unlike the discrete case above, bisimulation for labelled Markov processes can be shown to be characterised by a very weak logic:

$$\mathcal{L} := T \mid \phi_1 \land \phi_2 \mid \left< a \right>_q \phi$$

where $s \vDash \left<a\right>_q \phi$ means that if the system is in the state $s$ then after action $a$, with probability at least $q$, the new state will satisfy formula $\phi$. Two systems are bisimilar iff they satisfy the same formulas of $\mathcal{L}$.

(We can exhibit several other logics of different expressive power that also characterise bisimulation, for example $\mathcal{L}' := \mathcal{L} \mid \lnot \phi$. However, the logic $\mathcal{L}$ is the weakest.)

We note that this also applies to deterministic, purely discrete systems.
