### Formal Definitions

**DFA** $M = (Q, \Sigma, \delta, s, F):$

- $Q$  is a finite set of States
- $\Sigma ​$ is an alphabet
- $\delta : Q \times \Sigma \rightarrow Q$ is the **transition function**
- $s$ is the start state
- $F \subseteq Q$ is the set of final states



**NFA** $M = (Q, \Sigma, \Delta, s, F):$

- $Q$  is a finite set of States

- $\Sigma$ is an alphabet

- > $\Delta : Q \times \Sigma \rightarrow 2^Q$ (*subsets of Q*) is the **transition function**
  >
  > - We will write $q \xrightarrow{\sigma}  q'$ for $q' \in \Delta(q, \sigma) $ 

- $s$ is the start state

- $F \subseteq Q$ is the set of final states



**Subset Construction** = NFA $\rightarrow$ DFA

Write out a table with the alphabet along the top and the possible states ($2^\Sigma​$) down the side



**$\epsilon$NFA** $M = (Q, \Sigma, \theta, s, F):$

- $Q$  is a finite set of States

- $\Sigma$ is an alphabet

- > $\theta : Q \times (\Sigma + \{\epsilon\}) \rightarrow 2^Q$ is the **new** transition function
  >
  > - We will write $q \xrightarrow{\sigma}  q'$ for $q' \in \theta(q, \sigma) $ 

- $s$ is the start state

- $F \subseteq Q​$ is the set of final states



**Kleene's Theory** states that any regular language is accepted by an FA and conversely that any language accepted by an FA is regular



### Concatenation

![Screenshot 2018-10-10 at 17.28.52](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/Concatenation.png)



### $L(α^∗)$ is regular (Kleene's Star)

![Screenshot 2018-10-10 at 17.33.08](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/Kleene Star.png)



### Constructing Regular Expressions

> $\alpha_{u,v}^{X}$ is a regular expression that describes all possible paths from $u$ to $v$  that start with $u$, end with $v$ and have all the **intermediate states** in $X​$

![Screenshot 2018-10-11 at 10.33.27](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/Inductive Step.png)

- $\alpha_{u, v}^X$ - paths that **DON'T** go through q
- $\alpha_{u, q}^X \alpha_{q, u}^X$ - paths that go through q **ONCE**
- $\alpha_{u, q}^X \alpha_{q, q}^X \alpha_{q, u}^X$ - paths that go through q **TWICE**

$$
\alpha_{u,v}^{X+\{q\}} \overset{def}{=} \alpha_{u,v}^{X} + \alpha_{u,q}^{X}(\alpha_{q,q}^{X})^*\alpha_{q,v}^{X}
$$

![Screenshot 2018-10-11 at 10.36.25](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/NFA to Regex.png)



### Pumping Lemma - Regular Languages

1. **Demon** picks some $k > 0 $
2. **We** pick $x, y, z$ such that $xyz \in A$ and $\#y \geq k$
3. **Demon** picks $u, v, w$ such that $y = uvw$ and $v \neq \epsilon$
4. **We** pick $i \geq 0$     *Make sure we define what i is...* $uv^iw$

> **We** win if $xuv^iwz \notin A$ 
>
> **Demon** wins if $xuv^iwz \in A$ 	*ie we pick a word that is in the language* 



#### Example

$$
A = \{a^nb^n \space | \space n \in N\}
$$

1. **Demon** picks some $k > 0 $
2. **We** pick $x = a^k$, $y = b^k$, $z = \epsilon$ 
3. **Demon** picks $u, v, w$ such that $y = uvw$ and $v \neq \epsilon$ 
   - Because of our choice of $y, v = b^l$ for some $0 < l \leq k$ 
   - Then $u = b^r$ and $w = b^s$ for some $r, s \geq 0$ with $k = r + l + s$ (since $y = b^k = b^r b^l b^s$). In particular $r + s = k - l$ 
4. **We** pick $i = 0$ 
   - Then $xuv^0wz = a^kb^rb^s \epsilon = a^kb^{r+s} = a^kb^{k-l}$ 
   - We **win** because $a^kb^{k-l} \notin A$ 

> This is a winning strategy, because we have made no assumption on what the demon does. Whatever the demon does, we end up winning. Thus **$A$ is not regular**. 



### PDA

A **PDA** is a 7-tuple $M = (Q, \Sigma, \Gamma, \delta, s, \bot, F)$

- $Q$ is a finite set of states

- > $\Sigma$ is the **input** alphabet

- > $\Gamma$ is the **stack** alphabet

- > $\delta$ is the transition relation

- $s \in Q$ is the start state

- > $\bot \in \Gamma$ is the **initial** stack symbol (bottom of the stack)

- $F \subseteq Q$  is the set of final states



#### Transitions

![Screenshot 2018-10-22 at 19.00.17](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/PDA Transitions.png)
$$
((q, \sigma, \gamma), (q', \gamma_1\gamma_2 ... \gamma_k)) \in \delta
$$
If in state $q​$, reading $\sigma​$ and $\gamma​$ on top of the stack, then pop $\gamma​$ off the stack and go to state $q'​$ pushing $\gamma_1\gamma_2..\gamma_k​$ on the stack ($\gamma_k​$ first, $\gamma_1​$ last) 

![Screenshot 2018-10-22 at 20.24.20](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/PDA Transitions Graph.png)

> **Diagram**: If you read $\sigma$, pop $\gamma$ off the stack and push $\gamma_1\gamma_2..\gamma_k$

![Screenshot 2018-10-23 at 09.52.00](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/PA Example 1.png)

![Screenshot 2018-10-23 at 10.01.49](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/PA Example 1 Simulation.png)

You can accept by **empty stack** OR **final state**



### Context Free Grammars

A CFG is a quadruple:

$G = (N, \Sigma, P, S)$

- $N$ is a finite set of **nonterminal symbols** (*variables*)
- $\Sigma$ is a finite set of **terminal symbols** (*alphabet*)
- $P \sube N \times (N + \Sigma)^*$ are the **productions**
- $S \in N$ is the **start symbol**



#### Normal Forms

A CFG is in **chomsky normal form** (*CNF*) when all productions are of the form
$$
A \rightarrow BC \quad or \quad A \rightarrow a \quad (A, B, C \in N, a \in \Sigma)
$$
`Right side is either terminal or nonterminal NOT both`

A CFG is in **greibach normal form** (*GNF*) when all productions are of the form
$$
A \rightarrow aB_1B_2...B_k \quad (k \geq 0, A, B_1,...,B_k \in N, a \in \Sigma)
$$
`Terminal symbol followed by chain of nonterminals`

> No grammar in *CNF* or *GNF* can generate the empty string $\epsilon$

 

A production $A \rightarrow \epsilon$ is called an **$\epsilon$-production**

A production $A \rightarrow B$ is called **unit-production** (nonterminal to nonterminal)



### CFG to PDA

Let $G = (N, \Sigma, P, S)$ the required PDA is $(\{*\}, \Sigma, N, \delta, *, S, \empty)$

- $*$ is the only state
- $N$ the set of **nonterminals** of $G$, is the stack alphabet
- S the start production, is the initial stack symbol
- We accept by empty stack, so no need for final states
- $\delta$ is consructed as:
  - For each $A \rightarrow \sigma B_1...B_k$ in $P, ((*, \sigma, A), (*, B_1...B_k)) \in \delta$

#### Example

Consider the grammar
$$
S \rightarrow aSB \ | \ aB, \quad B \rightarrow b
$$
The corresponding PDA has one state, initial stack symbol $S$ and the following productions

![Screenshot 2018-11-01 at 19.29.05](/Users/jackcorbett/Documents/University/Theory%20of%20Computing/Part 1/Images/CFG%20to%20PDA%20Example.png)

![Screenshot 2018-11-01 at 19.32.41](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/CFG to PDA Example Derivation.png)



### One state PDA to CFG

Suppose we have a PDA $(\{q\}, \Sigma, \Gamma, \delta, q, \bot, \empty)$ 

Let $G = (\Gamma, \Sigma, P, \bot)$

$P$ is constructed as follows:

- For each $((q, c, A), (q, B_1...B_k)) \in \delta$ 
  - Let $A \rightarrow cB_1...B_k$ be in $P$



### PDA to One State PDA

> Maintain all state info on the stack

![Screenshot 2018-11-01 at 19.43.16](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/PDA to one state PDA.png)

First define a PDA with a single final state $t$ so it can clear it's stack
$$
M = (Q, \Sigma, \Gamma, \delta, s, \bot, \{t\})
$$
Let $\Gamma' \overset{def}{=} Q \times \Gamma \times Q$ so a symbol in $\Gamma'$ is a triple $(p, A, q)$ where $p, q \in Q$ and $A \in \Gamma$

We will write $\langle pAq \rangle$ to denote such an element of $\Gamma'$ 



Let $M' \overset{def}{=} (\{*\}, \Sigma, \Gamma', \delta', *, \langle s \bot t \rangle, \empty)$ where $\delta'$ is defined as follows:

- For each transition $((p, \sigma, A), (q_0, B_1B_2...B_k)) \in \delta$ include:

$$
((*, \sigma, \langle pAq_k \rangle), (*, \langle q_0B_1q_1 \rangle \langle q_1B_2q_2 \rangle ... \langle q_{k-1}B_kq_k \rangle)) \quad \text{in } \delta'  \text{for all possible choices of } q_1, q_2,...,q_k \in Q
$$

#### Example

Consider the PDA: $\{\{0, 1, 2\}, \{a, b\}, \{a, \bot\}, \delta, 0, \bot, \{2\}\}$

![Screenshot 2018-11-01 at 20.28.40](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/PDA to PDA with on state.png)



### CFL Closure

Closed:

- Union
- Concatenation
- Kleene Star (*)
- Intersection **with regular languages**

Not Closed:

- Intersection
- Complement



### Pumping Lemma for CFLs

- **Demon** picks $k \geq 0$
- **We** pick $z \in A$ of length at least $k$ 
- **Demon** picks $u, v, w, x, y$ such that $z = uvwxy, \#(vx) > 0$ and $\#(vwx) \leq k$
- **We** pick $i \geq 0$

**We** win if $uv^iwx^iy \notin A$ if the string is in $A$ then the **demon** wins

### Example

Consider $\{a^nb^nc^n \ | \ n \geq 0\}$

1. **Demon** picks some $k$
2. **We** pick $z = a^kb^kc^k$
3. **Demon** breaks the string up into $uvwxy$ where $\#(vx) > 0$ and $\#(vwx) \leq k$
4. **We** pick $i = 2$, now we need to think about $z' = uv^2wx^2y$

![Screenshot 2018-11-05 at 10.53.40](/Users/jackcorbett/Documents/University/Theory of Computing/Part 1/Images/Pumping Lemma for CFLs Example.png)

- If $vx$ contains only $a$'s, only $b$'s or only $c$'s then $z'$ is not of the form $a^kb^kc^k$ (as it will have too many of either $a$, $b$ or $c​$)
- If either $v$ or $x$ contains two different symbols then $z' = uv^2wx^2y$ is not of the form $a^*b^*c^*$
- If $v$ contains only $a$'s and $x$ contains only $b$'s then $z'$ will not have enough $c$'s
- If $v$ contains only $b$'s and $x$ contains only $c$'s then $z'$ will not have enough $a$'s

> **Remeber** you must cover all possible cases