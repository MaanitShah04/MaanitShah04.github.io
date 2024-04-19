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

_Diagram_

In the gridworld example, the policy was optimal, $\pi' = \pi_{\ast}$. The process of policy iteration always converges to $\pi_{\ast}$.

### Example (Jack's Car Rental):

The problem involves managing a car rental business with two locations, where each location has a maximum capacity of 20 cars. The states in this problem represent the number of cars available at each location. The actions involve moving up to 5 cars between the two locations overnight to rebalance the inventory based on demand. The reward is $\$10$ for each car rented (assuming a car is available at the requested location). The transitions, or the dynamics of the environment, are determined by the random returns and requests for cars at each location. These transitions follow a Poisson distribution, n returns/requests with prob $\frac{{e^{-\lambda } \lambda^n }}{{n!}}$, where:
- At the first location, the average number of car returns and requests is 3.
- At the second location, the average number of car requests is 4, and the average number of car returns is 2.

The goal is to find an optimal policy for moving cars between the two locations to maximize the overall expected reward (revenue from rented cars).

_Diagram_

The plots show the progression of value functions $(v_0, v_1, v_2)$ and intermediate policies $(\pi_0, \pi_1, \pi_2, \pi_3, \pi_4)$ during the iterations of policy evaluation and improvement.

The value functions are depicted as 3D surfaces, representing the expected future rewards for each combination of car counts at the two locations. The policies are visualized as 2D plots, indicating the optimal number of cars to move between locations for each state.

Starting with an initial policy π0, the process alternates between evaluating the current policy to compute the value function and improving the policy greedily based on the value function.

The iterations continue until the policy converges to the optimal policy $\pi^{\ast}$ that maximizes the expected rewards in the Car Rental problem.

### Policy Improvement

Consider a deterministic policy, $a = \pi(s). We can improve the policy by acting greedily,
```math
\pi'(s) = \arg\max_{a\in\mathcal{A}} q_\pi(s, a)
```
This improves the value from any state $s$ over one step,
```math
q_\pi(s, \pi'(s)) = \max_{a\in\mathcal{A}} q_\pi(s, a) \geq q_\pi(s, \pi(s)) = v_\pi(s)
```
It therefore improves the value function, $v_{\pi'}(s) \geq v_\pi(s)$
```math
\begin{align*}
v_\pi(s) &\leq q_\pi(s, \pi'(s)) = \mathbb{E}\pi[R{t+1} + \gamma v_\pi(S_{t+1}) | S_t = s] \
&\leq \mathbb{E}\pi[R{t+1} + \gamma q_\pi(S_{t+1},\pi'(S_{t+1})) | S_t = s] \
&\leq \mathbb{E}\pi[R{t+1} + \gamma R_{t+2} + \gamma^2 q_\pi(S_{t+2},\pi'(S_{t+2})) | S_t = s] \
&\leq \mathbb{E}\pi[R{t+1} + \gamma R_{t+2} + \ldots | S_t = s] = v_{\pi'}(s)
\end{align*}
```
If improvements stop,
```math
q_\pi(s, \pi'(s)) = \max_{a\in\mathcal{A}} q_\pi(s, a) = q_\pi(s, \pi(s)) = v_\pi(s)
```
Then, the Bellman optimality equation has been satisfied
```math
v_\pi(s) = \max_{a\in\mathcal{A}} q_\pi(s, a)
```
Therefore $v_\pi(s) = v_*(s)$ for all $s \in \mathcal{S}$. So $\pi$ is an optimal policy.

## Value Iteration

### Principle of Optimality

Any optimal policy can be subdivided into two components:
- An optimal first action $A$,
- Followed by an optimal policy from successor state $S'$

> Theorem (Principle of Optimality)
> 
> A policy $\pi(a|s)$ achieves the optimal value from state $s$, $v_\pi(s) = v_(s)$, if and only if
> - For any state $s'$ reachable from $s$
> - $\pi$ achieves the optimal value from state $s'$, $v_\pi(s') = v_(s')$

### Deterministic Value Iteration

Value iteration is a technique used to compute the optimal value function v*(s) for a given Markov Decision Process (MDP). The idea is to iteratively update the value function using the Bellman equation until convergence.

If we know the solution to the subproblems $v_{\ast}(s')$, i.e., the optimal values for the next states $s'$, then the optimal value $v_{\ast}(s)$ for the current state s can be found by a one-step lookahead:
```math
v_{\ast}(s) \leftarrow \max_{a \in A} R^a_s + \gamma \sum_{s' \in S} P_{ss'}^a v_{\ast}(s')
```

The value iteration algorithm applies these updates iteratively, starting with an initial approximation and updating the values until convergence.

The intuition behind value iteration is to start with the final rewards and work backwards, computing the optimal values by considering the immediate rewards and the discounted future rewards from the next states.

Although derived for deterministic MDPs, value iteration can also be applied to stochastic (loopy) MDPs.

### Value Iteration

Problem: Find the optimal policy $\pi$

We apply the Bellman Optimality Equation with iterative backups.
```math
v_{k + 1}(s) = \max_{a \in A} \left[R_{s}^a + \gamma \sum_{s' \in S}  P_{ss'}^a v_{k}(s')\right]
```
At each iteration $k + 1$, update $v_{k + 1}(s)$ from $v_{k}(s')$, for all states $s \in \mathcal{S}$, using synchronous backups.

_Diagram_

## Summary (Synchronous Dynamic Programming Algorithms)

_Image_

## References
[1]: Sutton, R. S., & Barto, A. G. (2018). Reinforcement learning: An introduction (2nd ed.). The MIT Press.

[2]: [Markov Decision Processes, Subir Varma](https://subirvarma.github.io/GeneralCognitics/Course2/Lecture2_MDPs.pdf).

[3]: [Markov Decision Processes, David Silver, UCL Course on RL](https://www.davidsilver.uk/wp-content/uploads/2020/03/MDP.pdf).

[4]: J. Norris, “Markov Chains,” Cambridge University Press, Cambridge, 1998.
