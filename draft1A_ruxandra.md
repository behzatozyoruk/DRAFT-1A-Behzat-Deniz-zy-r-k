(Based on *Bisimulation for Labelled Markov processes* by J. Desharnais, A. Edalat, and P. Panangaden.)


After reviewing the classical notions of transition systems and bisimulation equivalence, we make a case for a continuous state space model (Labelled Markov Processes) and generalise the previous notion of bisimulation. Along the way, we will need to introduce measure theoretic tools. In the end, we discuss a rather unexpected logical characterisation of bisimulation for LMPs.

#### Why bisimulation?
Given an abstract system, we often want to reduce it to an equivalent smaller system, check whether the model is equivalent to a different one, or verify that a property holds -- in order to do this, we need to ask: `when do two states have exactly the same observed behaviour?'.

The notion of bisimulation proves to be a natural choice for defining equivalence. It also happens to have a very convenient categorical characterisation.

#### Why continuous models?
Many real-world models have an intrinsic continuous nature (for instance, the movement of an aircraft or the pressure of a gas in a tank). Often, the state space can be discretized, but how can we tell if the discretized system faithfully represents the continuous system? What if we need a finer approximation? We will make a case for a discrete time, continuous state space model\footnote{ Eventually, we would also want a fully continuous model; however, this will require a very different set of technical tools, namely stochastic differential equations.}, introduce tools from measure theory, and discuss a sensible notion of bisimulation in this case.

## Labelled transition systems

A convenient representation of discrete systems is given by labelled transition systems (LTSs).

**Example 1.** We can model a vending machine as a labelled transition system, with the states being given by the instructions provided by the machine, such as \`Place cup' and \`Choose':
(add picture 1)

In general, we define this class of systems as follows:

**Definition 2.** A labelled transition system is a triple $(S, \mathcal{A}, \rightarrow_a)$ consisting of a set of states $S$, a set of actions $\mathcal{A}$, and, for each $a \in \mathcal{A}$, a transition relation $\rightarrow_a \subseteq S \times S$.

Note that the definition allows nondeterminism: the transition relation need not be a single pair of states, but a set of pairs. So we can also model the following vending machine:

**Example 3.**
(add picture 2)

Note that the two vending machines behave differently: in the first machine, the user (the environment) is given the choice between coffee and tea; meanwhile, the second machine gives the user no choice, the choice happening internally. How can express this and differentiate between the faulty machine and the functional one? 

In general, we would want define a sensible notion of equivalence for LTSs. One first attempt could be to look at the language the systems accept. In our case, both machines accept the language $[\text{cup}; (\text{coffee} + \text{tea})]^*$, so this notion is clearly not strong enough. A better option is to look at a finer level, at pairs of states; we expect states that are equivalent to remain equivalent as they evolve, and so we check whether this natural condition is indeed obeyed:

**Definition 4.** Define a bisimulation relation $\sim$ on $S$ by $s \sim t$ if the following  holds:
$$\forall s' \in S, \forall a \in \mathcal{A}, \text { if } s \xrightarrow{a} s' \text{ then } \exists t'\ \in S \text { with } t \xrightarrow{a} t' \text{ such that } s' \sim t'$$
and vice versa.

\footnote{At first glance, the definition might seem circular. However, we can show that this definition is equivalent to an inductive definition. We omit the discussion here, but we refer the reader to ..}

Now, using our definition, the \`Choose' state of the functional vending machine is clearly not equivalent to either of the `Choose' states of the faulty one, and so the two systems are indeed not equivalent.

## Labelled Markov chains

Often, we need a model of quantitative nature in which the transitions of the system happen with a given probability. We can easily extend the previous definition to account for this:

**Definition 5.** A labelled Markov chain is a triple $(S, \mathcal{A}, T_a)$ consisting of a set of states $S$, a set of actions $\mathcal{A}$, and, for each $a \in \mathcal{A}$, a transition probability matrix $T_a: S \times S \rightarrow [0,1]$.    

Note that all probabilistic data is internal, and the environment is assumed to be deterministic. That is to say, the model is reactive: we are only concerned with how the model reacts, i.e., what an external observer can see. What does it mean for two systems to be equivalent in this case? 

**Example 6.** Consider the two following systems: 
(add picture 3) Are the states $s_0$ and $t_0$ equivalent? Intuively, they have the same behaviour: $s_2$ and $s_3$ behave in the same way $t_2$, and the probability landing in $s_2$ or $s_3$ is the same as landing in $t_2$. This motivates the following notion of equivalence:

**Definition 7** [Larsen and Skou] **.** We say $\sim_{LS}$ is a bisimulation relation if whenever $s \sim_{LS} t$ then for any $C \in S/ \sim_{LS}$ we have 
$$\sum_{v \in C} P_a(s, v) = \sum_{v \in C} P_a(t, v).$$

## Logical characterisations of bisimulation

We have previously defined bisimulation, a notion of equivalence for transition systems. Is there an alternative way to think of equivalance? Can we empirically `test' whether two systems are equivalent? We can do so by thinking of the logical formulas the systems satisfy: if we can find one formula which is satisfied by one system but not the other, than the systems are not equivalent.

**Example 8.** Consider the following two discrete systems: (add picture 4) Clearly, the formula $\left<a\right> \lnot \left<b\right> \top$ distinguishes between $S$ and $T$, and hints to the fact that we cannot get by without negation. Indeed, there there is no negation-free logic that can characterise bisimulation in this case. One logic that does characterise it is the Hennessey-Milner logic:
$$\mathcal{L}_{HM} := \top \mid \lnot \phi \mid \phi_1 \land \phi_2 \mid \left<a\right> \phi$$ 
where $s \vDash \left<a\right> \phi$ means that there is an $a$-action from the state $s$ to a state $s'$ which satisfies $\phi$.

#### Labelled Markov Processes
We would expect the situation to become more complicated when we move to continuous state space systems, and a more powerful logic to be needed. Rather surprisingly, this is not the case. In fact, we don't even need negation or infinite conjunction. 

**Example 9.** Let's reconsider the example above, this time adding probabilities to transitions: for $S$, suppose that the two $a$-actions have probabilities $p$ and $q$ respectively, and the $b$-action has probability $1$; for $R$, the $a$-action has probability $r$ and the $b$-action probability $1$.

(add picture 5)

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
