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
- Input
