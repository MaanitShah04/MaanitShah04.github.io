# Policy Iteration and Value Iteration

## Dynamic Programming

> The term dynamic programming (DP) refers to a collection of algorithms that can be used to compute optimal policies given a perfect model of the environment as a Markov decision process (MDP).

Dynamic programming involves solving complex problems by breaking them down into sub-problems. Dynamic programming is a very general solution method for problems that have two properties,

- **Optimal Substructure**: The optimal solution to a dynamic optimisation problem can be found by combining the optimal solutions to its sub-problems. This is known as the _Principle of Optimality_. Optimal solutions can be decomposed into subproblems.
- **Overlapping subproblems**: Sub-problems can recur many times. The solution of one sub-problem is cached and reused to solve recursive sub-problems.

Markov decision processes satisfy both properties,

- Bellman equation gives recursive decomposition. The equation breaks down finding the value function of a state by dividing it into sub-problems.
  ```math
  v_\pi(s) = \mathbb{E}_\pi [R_{t+1} + \gamma V_\pi(S_{t+1}) | S_t = s]
  ```
- Value function stores and reuses solutions.

### Planning by Dynamic Programming

Dynamic programming assumes full knowledge of the MDP. It is used for _planning_ in an MDP.

For Prediction,
- Input: MDP $\langle S, A, \textit{P}, R, \gamma \rangle$ and policy $\pi$ **OR** MRP $\langle S, \textit{P}^{\pi}, R^{\pi}, \gamma \rangle$
- Output: value function $v_\pi$

For Control:
- Input: MDP $\langle S, A, \textit{P}, R, \gamma \rangle$
- Output: optimal value function $v_{\ast}$ and optimal policy $\pi_{\ast}$.

## Iterative Policy Evaluation

How do we evaluate an arbitrary policy $\pi$?

We apply the Bellman Expectation Equation with iterative backups. We calculate the value of the next state by backing up the value of the current state from the previous iteration.
```math
v_{k+1}(s) = \sum_{a \in A} \pi(a|s) \left( R_{s}^a + \gamma \sum_{s' \in S} P_{ss'}^{a} v_k(s') \right)
```
At each iteration $k + 1$, update $v_{k + 1}(s)$ from $v_{k}(s')$, for all states $s \in \mathcal{S}$. Here, $s'$ is a successor state of $s$.

_Backup diagram_

### Example (Gridworld):

_Image1_

The gridworld consists of a 4x4 grid with 15 states, where each cell represents a state. There is one terminal state (shown as shaded squares), and the agent receives a reward of -1 until it reaches the terminal state. The agent follows a uniform random policy, where it has an equal probability of 0.25 to move in any of the four directions (up, down, left, or right) from each state. 
```math
\pi(n|.) = \pi(e|.) = \pi(s|.) = \pi(w|.) = 0.25
```
Actions that would lead the agent out of the grid leave the state unchanged. The goal is to evaluate the performance of this random policy in terms of the undiscounted episodic MDP, where the discount factor $\gamma$ is set to 1, indicating that future rewards are not discounted.

_Image 2_

## Policy Iteration

Given an initial policy $\pi$, the process of policy improvement involves evaluating the policy and then improving it iteratively. The policy evaluation step estimates the value function $v_{\pi}(s)$ for each state s under the current policy $\pi$.
```math
v_{\pi}(s) = \mathbb{E}[R_{t+1} + \gamma R_{t+2} + ... | S_t = s]
```

The policy improvement step then updates the policy to a new policy $\pi'$ by acting greedily with respect to the value function $v_{\pi}$. 
```math
\pi' = greedy(v_{\pi})
```

In the gridworld example, the policy was optimal, $\pi' = \pi_{\ast}$. The process of policy iteration always converges to $\pi_{\ast}$.

