# Continuous State Spaces: The need for measure theory

In a standard discrete labelled Markov chain, transitions are
straightforward. If our system is in state $s$, the environment can
trigger an interaction (a label $l$), and the system will jump to a new
state $s'$ with a specific probability. We can write this as a
transition matrix $P_l(s, s')$.

What if the state space is not discrete, say, it is a \"continuous\"
space such as $\mathbb{R}^n$? Can we define transition probabilities in
a similar fashion?

Consider the stochastic system where the state space is $\mathbb{R}^2$
and the set of labels is $L=\{a,b\}.$ If the system $a$-transitions from
a state $(x_0,y_0)$ to a new state $(x,y_0)$, then the probability of
$x\in [s,t]$ is given by
$$\sqrt{\frac{\alpha}{\pi}}\int_s^texp(-\alpha(x-x_0)^2)dx$$ and
similarly if $(x_0,y_0)$ $b$-transitions to $(x_0,y)$, then probability
of $y\in [s,t]$ is given by
$$\sqrt{\frac{\beta}{\pi}}\int_s^texp(-\beta(x-x_0)^2)dx.$$ In this
system, an $a$-transition from a state $(x_0,y_0)$ to $(x_1,y_0)$ occurs
with zero probability.

Therefore it is not convenient to define transition probabilities
between states, instead we should consider transition probabilities from
a state to a set of states. Indeed, singletons are no longer the "atomic
building blocks" as in the discrete case, in which the probability of
transitioning from a state to a set of states is nothing but the sum of
the transition probabilities of each states in the target set. Thus,
given an initial state and a label, we should consider assigning a
transition probability to subsets of the state space. Unfortunately,
this will fail if we try to make it work for all subsets of a continuous
state space because not all subsets are going to be well-behaved enough
to be assigned a probability. This is a much general problem, if our
goal is to assign a proper notion of \"size\", say volume, area, length
or probability, to each subset of a given set, then we need to work with
measure theory. First, let us define the well-behaved sets we mentioned
earlier.

::: definition
**Definition 1**. A $\sigma$-algebra $\Sigma$ on a set $X$ is a
collection of subsets of $X$ that contains $\emptyset$, closed under
taking complements and closed under countable union. Such a pair
$(X,\Sigma)$ is called a *measurable space.*
:::

Almost every object in mathematics comes with morphisms, so here are the
morphisms of measurable spaces:

::: definition
**Definition 2**. Given measurable spaces $(X,\Sigma)$ and $(Y,\Theta)$,
a function $f:X\to Y$ is called a *measurable function* if
$\forall B\in \Theta$, $f^{-1}(B)\in \Sigma.$
:::

::: definition
**Definition 3**. Given a measurable space $(X,\Sigma)$, a *measure* is
a function $\mu:\Sigma \to [0,\infty]$ such that $\mu(\emptyset)=0$ and
given pairwise disjoint family of sets $\{A_n:n\in \mathbb{N}\}$ in
$\Sigma$ then
$\mu(\bigcup_{n\in \mathbb{N}} A_n)=\bigcup_{n\in \mathbb{N}} \mu(A_n)$.
:::

An immediate property that can be concluded from this definition is that
a measure is monotone: $A\subset B \implies \mu(A)\leq \mu(B).$

The Lebesgue measure on $\mathbb{R}$ is a well-known measure which is
defined as follows. First of all, given an interval $I=[a,b]$, its
length is defined as $l(I)=b-a.$

::: definition
**Definition 4**. Given $E\subset \mathbb{R}$ the *Lebesgue outer
measure* $m^*(E)$ is defined as
$$inf\{\sum_{k=1}^{\infty}l(I_k):E\subset \bigcup_{k=1}^{\infty}I_k  \textnormal{ where }I_k \textnormal{ is an open interval for all } k\in \mathbb{N}\}.$$
:::

Now, we restrict $m^*$ to subsets $E$ of $\mathbb{R}$ which satisfy
$m^*(A)=m^*(A\cap E)+m^*(A\cap E^c)$ for all $A\subset \mathbb{R}.$
These sets are called *Lebesgue measurable* sets and such sets
constitute a $\sigma$-algebra. In addition, the restricted $m^*$ becomes
a measure, called the Lebesgue measure. It is translation invariant,
that is, $m(E+x)=m(E)$ for any $x\in \mathbb{R}$ and Lebesgue measurable
set $E$.

Now, we are ready to give an example of a set in $\mathbb{R}$ for which
we can't assign a \"size\", or now that we know what size means, a
measure. In other words, we will now construct a set $V$ which is not
Lebesgue measurable, assuming the axiom of choice. It is called *Vitali
set*.

Define the equivalence relation on \[0,1\] by $x\sim y$ iff
$x-y \in \mathbb{Q}.$ This partitions $\mathbb{R}$ into uncountably many
equivalence classes $[x]=(\mathbb{Q}+x)\cap [0,1]$ and invoking axiom of
choice we can choose one representative from each class and collect them
to from a set $V$. For the sake of contradiction, suppose $V$ is
Lebesgue measurable. So, we can assume $m(V)$ exists. Now, enumerate
$\mathbb{Q}=(q_k)_{k\geq1}$ and consider translations $V_k =V+q_k.$
Notice that these sets are pairwise disjoint and also $m(V_k)=m(V)$ for
each $k$. Letting $U=\bigcup_{k\geq1}V_k$, we arrive at the following
conclusion $$[0,1]\subset U \subset [-1,2].$$ Recall that any measure is
monotone and any measure distributes over a disjoint union of countably
many measurable sets, therefore $$1\leq \sum_{k\geq 1} m(V)\leq 3.$$ If
$m(V)=0,$ this implies $1\leq 0$ and if $m(V)> 0$ this implies
$\sum_{k\geq 1} m(V)=\infty\leq 3.$ In either case we arrive at a
contradiction. Thus, $V$ is not Lebesgue measurable.

# Labelled Markov Processes in this setting

In this measure theoretic setting, we can now safely assign transition
probabilities as follows.

::: definition
**Definition 5**. Given a measurable space $(X,\Sigma),$ *transition
probability function* $$T:X\times \Sigma\to [0,1]$$ is a function such
that for fixed $x\in X$, $T(x,-)$ is a probability measure and for fixed
$A\in \Sigma$, $T(-,A)$ is a measurable function.
:::

There is a category **SRel** where objects are measurable spaces and
morphisms are transition functions. This is the category of stochastic
relations. Given a transition function $T$ in **SRel**, by currying, we
obtain maps of the form $\bar{T}:X\to [0,1]^{\Sigma}$ where the codomain
is the set of probability measures on $\Sigma$ which is equipped with a
special $\sigma$-algebra structure $\Xi$. Indeed, the currying takes
place in the category **Meas** of measurable spaces where the assignment
$(X,\Sigma)\to (\Sigma^{[0,1]},\Xi)$ is given by the *Giry* monad. Note
that the morphisms $\bar{T}:X\to [0,1]^{\Sigma}$ are in fact the
morphisms in the Kleisli category of the Giry monad. This is analogous
to classic relations, a relation $R$ between sets $X$ and $Y$ is a
function $X\times Y \to \{0,1\}$ and by currying we obtain a Kleisli
morphism of the power set monad. Hence the name stochastic relations.

Let's move on to our main discussion.

::: definition
**Definition 6**. A *labelled Markov process* with label set $L$ a tuple
$(S,\Sigma,(\tau_l)_{l\in L})$ where $S$ is an analytic space, $\Sigma$
is the Borel $\sigma$-algebra on $S$, and for all $l\in L$
$$\tau_l:S\times \Sigma \to [0,1]$$ is a transition probability
function.
:::

This in fact generalizes previously defined labelled Markov chains. From
now on, let us fix the label set $L.$

We will define a category of labelled Markov processes. Here is the
morphism of this category.

::: definition
**Definition 7**. A *simulation morphism*
$f:(S,\Sigma,k_l)\to (S',\Sigma',k_l')$ between labelled Markov
processes is a measurable function with respect to the underlying
measurable spaces such that $k_l(s,f^{-1}(A'))\leq k'_l(f(s),A')$ for
all $l\in L$, $s\in S.$, $A'\in \Sigma'$.
:::

This is in fact the usual simulation on the underlying labelled
transition systems.

::: remark
**Remark 8**. There is a category **LMP** whose objects are labelled
Markov processes and morphisms are simulations.
:::

::: definition
**Definition 9**. A *generalized labelled Markov process* is a labelled
Markov process $(S,\Sigma,k_l)$ except we require $k_l(-,A)$ to be
universally measurable for all $A\in \Sigma$, $l\in L.$
:::

::: remark
**Remark 10**. There is a category **LMP\*** whose objects are
generalized labelled Markov processes and morphisms are simulations.
:::

In this case, **LMP** is a full subcategory of **LMP\***

# Bisimulation for Markov Processes

::: definition
**Definition 11**. A *zigzag morphism*
$f:(S,\Sigma,k_l)\to (S',\Sigma',k_l')$ is a surjective simulation
morphism such that $k_l(s,f^{-1}(A'))= k'_l(f(s),A')$ for all $l\in L$,
$s\in S.$, $A'\in \Sigma'$.
:::

::: definition
**Definition 12**. Given labelled Markov proceses $T$ and $T'$, we say
$T$ is *probabilistically bisimilar* to $T'$ if there exists
$S\in \textbf{LMP*}$ and zigzag morphisms $f$ and $g$ such that there is
a span $$\begin{tikzcd}
  & S \arrow[ld, "f"'] \arrow[rd, "g"] &    \\
T &                                    & T'
\end{tikzcd}$$
:::

For some technical reasons, the fact that state spaces are assumed to be
analytic is needed in order to make sure that bisimilarity is an
equivalence relation, in particular, a transitive relation. In fact,
thanks to the analytic structure we can define bisimulation as a cospan
of zigzags as well! This definition is sometimes more convenient to work
with.

Here are some examples of bisimulation.

::: example
**Example 13**. Recall the example we began with, it is actually
bisimilar to a one-state system which can make $a$ and $b$ transitions
with probability 1. This example demonstrates that a continuous state
system can be reduced to a finite system.
:::

::: example
**Example 14**. Consider the labelled Markov process with a singleton
label set defined by $(\mathbb{R},\mathcal{B},k)$ where $\mathcal{B}$ is
the Borel algebra and the transition function is defined on intervals
(which can be extended to all Borel sets) by $$k(x, [r, s]) = 
\begin{cases} 
\lambda/2 \int_r^s e^{-\lambda|x-y|}dy & \text{if } x \ge 0, \\ 
0 & \text{otherwise.} 
\end{cases}$$ where $\lambda/2$ factor normalizes $k$ on the whole
space.

Consider another system $(\mathbb{R}^2,\mathcal{B}^2,h)$ defined by
$$h((x,y),[r,s]\times[p,q])=k(x,[r,s])P([p,q])$$ where $P$ is an
arbitrary probability measure on $\mathbb{R}.$ The first coordinate of
the latter system behaves similar to the former and the second
coordinate's behavior is similar to that of a trivial system, one-state
one-transition system. Thus, the zigzag projection from the second onto
the first system gives us a bisimulation between two systems.
:::
