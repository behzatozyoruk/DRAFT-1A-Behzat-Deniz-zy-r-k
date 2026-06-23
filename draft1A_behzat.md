## CONTINUOUS STATE SPACES: THE NEED FOR MEASURE THEORY

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

## LABELLED MARKOV PROCESSES IN THIS SETTING

In this measure-theoretic setting, we can now safely assign transition probabilities. A measure $p$ on a measurable space $(X, \Sigma)$ is called a *probability measure* if $p(X)=1$. Now, let us define Markov kernels.

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

## BISIMULATION FOR MARKOV PROCESSES

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
