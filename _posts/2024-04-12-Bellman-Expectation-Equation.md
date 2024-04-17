# Bellman Expectation Equation and Optimality

How do we calculate the value functions $v_\pi (s)$ and $q_\pi (s,a)$, given a policy $\pi$?

## Bellman Expectation Equation

The state-value function can be decomposed into immediate reward plus the discounted value of the successor state,
```math
v_\pi(s) = \mathbb{E}_\pi [R_{t+1} + \gamma V_\pi(S_{t+1}) | S_t = s]
```
The above equation is the Bellman expectation equation for the state-value function.

The action-value function similarly decomposed,
```math
q_\pi(s, a) = \mathbb{E}\pi [R{t+1} + \gamma q_{\pi}(S_{t+1}, A_{t+1}) | S_t = s, A_t = a]
```
The above equation is the Bellman expectation equation for the action-value function.



_Back-up diagram state value_

This backup diagram describes the value of being in a particular state. 

Mathematically, this can be represented as,
```math
v(s) = R_s + \gamma \sum_{s' \in S} P_{ss'} v(s')
```

Similarly,

_backup diagram for action value_

This backup diagram shows us "how good" it is to take an action in a given state under a given policy.

Mathematically, this can be represented as,

```math
q_\pi(s, a) = R_s^a + \gamma \sum_{s' \in S} P_{ss'}^a v_\pi(s')$
```

If we connect and extend both the back-up diagrams to further define $v_\pi (s)$,

_extended state value backup diagram_

Mathematically, this can be represented as,
```math
v_\pi(s) = \sum_{a \in A} \pi(a|s) \left(R_s^a + \gamma \sum_{s' \in S} P_{ss'}^a v_\pi(s')\right)
```

Example:

_Example_

Similarly, if we connect and extend both the back-up diagrams to further define $q_\pi (s, a)$,

_extended action value backup diagram_

Mathematically, this can be represented as,
```math
q_{\pi}(s, a) = R_s^a + \gamma \sum_{s' \in \mathcal{S}} P_{ss'}^a \sum_{a' \in \mathcal{A}} \pi(a'|s') q_{\pi}(s', a')
```

Example:

_Example_

## Optimal Value Function

The Bellman Expectation Equations evaluate how good a state is for a particular policy. However, they do not tell us the optimal policy.

### Optimal State Value Function
The **optimal state-value function** $v_{*} (s)$  is the maximum state-value function for all policies
```math
v_{*}(s) = \mathop{\max}_\pi v_\pi(s)
```
$v_{*} (s)$ tells us the maximum reward that can be extracted from the system when starting in state s. However, it doesn't tell us what policy to follow.

### Optimal Action Value Function

The **optimal action-value function**  $q_{*} (s, a)$ is the maximum action-value function for all policies
```math
q_{*}(s, a) = \mathop{\max}_\pi q_\pi(s, a)
```

$q_{*} (s, a)$ tells us the maximum reward that can be extracted from the system if action a is taken while in state s.

## Optimal Policy

In order to find an optimal policy, we have to define the notion of optimality. What does it mean for one policy to be better than another?

A partial ordering over policies can be defined using the concept of value functions. The partial ordering states that one policy is better than another if the value function of the first policy is greater than or equal to the value function of the second policy for all states. This partial ordering relationship is formally expressed in the equation,
```math
\pi \geq \pi' \text{ if } v_\pi(s) \geq v_{\pi'}(s), \forall s
```

> Theorem:
> 
> For any Markov Decision Process,
> 1. There exists an optimal policy $\pi_{\ast}$ that is better than or equal to all other policies, $\pi_{\ast} \geq \pi, \forall \pi$
> 2. All optimal policies achieve the optimal value function, $v_{\pi_{\ast}}(s) = v_{\ast}(s)$
> 3. All optimal policies achieve the optimal action-value function, $q_{\pi_{\ast}}(s, a) = q_{\ast}(s, a)$

### Finding an Optimal Policy:

An optimal policy can be found by maximising over $q_{\ast}(s, a)$,
```math
\pi_{\ast}(a|s) = \begin{cases}
1 & \text{if } a = \arg\underset{a \in \mathcal{A}}\max q_*(s, a) \\
0 & \text{otherwise}
\end{cases}
```

- There is always a deterministic optimal policy for any MDP.
- If we know $q_{\ast}(s,a )$, we immediately have the optimal policy.

Example:
_EXAMPLE IMAGE_

## Bellman Optimality Equation


