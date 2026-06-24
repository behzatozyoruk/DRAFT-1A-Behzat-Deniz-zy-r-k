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
