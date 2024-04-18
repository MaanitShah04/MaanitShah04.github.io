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

_Back Diagram state-value_

Suppose the agent is in state S, and from that state, it can take two actions (a). Instead of using the Bellman Expectation Equation to calculate the value of being in state S by taking the average of the action-values, the agent chooses the action with the greater $q_{\ast}$ value. This gives the agent the value of being in state s.
```math
v_{\ast}(s) = \mathop{\max}_a q_{\ast}(s, a)
```

Similarly,

_Backup diagram action-value_

Suppose the agent has taken an action a in some state s. The environment then transitions the agent to a new state s', which could be any of the possible next states. In this case, rather than using the Bellman Expectation Equation, which would involve taking the average of the values of the possible next states, the agent uses the Bellman Optimality Equation.
```math
q_{\ast}(s, a) = R_s^a + \gamma \sum_{s' \in \mathcal{S}} P_{ss'}^a v_{\ast}(s')
```
If we connect and extend both the back-up diagrams,

_extended state value backup diagram_

An agent in a particular state 's' will take an action a based on a policy that assigns weighted probabilities to the available actions. This action then leads the agent to end up in one of several possible next states s', with the probabilities of ending up in each state s determined by the weighted environment. To find the value of being in the original state s, we simply average the optimal values of all the possible next states s'. This average gives us the overall value of being in state s - it tells us how good it is for the agent to be in that state, taking into account the weighted probabilities of the available actions and the resulting next states.
```math
v_{\ast}(s) = \mathop{\max}_a (R_s^a + \gamma \sum_{s' \in \mathcal{S}} P_{ss'}^a v_{\ast}(s'))
```
There is no dependency on the policy anymore; it is solely a function of the environment's randomness.

_extended action value backup diagram_

Suppose our agent is in a particular state s. The agent takes an action a in that state, which may result in the agent ending up in any of several possible next states s'. From each of these possible next states, the agent wants to identify the action with the highest $q_{\ast}$ value, i.e. the action that will maximize the expected future reward. The agent then backs this information up to the current state s, which allows the agent to determine the overall value of taking the original action a from the current state. In this way, the agent is able to choose the action that will lead to the highest expected future reward by considering the possible future states and the best actions to take in each of them.
```math
q_{\ast}(s, a) = R_s^a + \gamma \sum_{s' \in \mathcal{S}} P_{ss'}^a \mathop{\max}_{a'}q_{\ast}(s', a')
```

_Example_

## References
[1]: Sutton, R. S., & Barto, A. G. (2018). Reinforcement learning: An introduction (2nd ed.). The MIT Press.

[2]: [Markov Decision Processes, Subir Varma](https://subirvarma.github.io/GeneralCognitics/Course2/Lecture2_MDPs.pdf).

[3]: [Markov Decision Processes, David Silver, UCL Course on RL](https://www.davidsilver.uk/wp-content/uploads/2020/03/MDP.pdf).

[4]: J. Norris, “Markov Chains,” Cambridge University Press, Cambridge, 1998.
