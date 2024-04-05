# Markov Decision Processes

**Reinforcement Learning** is a sub-domain of machine learning where a learner called an agent interacts with its surroundings called environment. In return, the environment provides rewards and a new state determined by the actions of the agent.

---

## The Agent–Environment Interface

> The learner and decision-maker is called the **agent**.
> The thing it interacts with, comprising everything outside the agent, is called the **environment**.

_Placeholder for Image_

The environment refers to the aspects of the problem that the agent cannot directly control or manipulate. The agent's actions, on the other hand, are the decisions that the agent is tasked with learning to make in order to maximize some reward signal.

The state of the environment encompasses all the information that may be useful for the agent in choosing its actions. Notably, the agent is not assumed to be completely ignorant of the environment. For example, the agent may have some knowledge about how its actions and the resulting states lead to the calculation of rewards. However, even though the agent understands this reward function, it is still considered part of the environment because the agent cannot arbitrarily change it.

This distinction between what the agent knows and what it can control is a critical concept in reinforcement learning. An agent may have a thorough understanding of the problem domain, much like a human who knows the rules of solving a Rubik's cube but still struggles actually to find the optimal solution. The agent-environment relationship, therefore, represents the fundamental limitations on the agent's control rather than just the limitations on its knowledge.

## The Markov Property

> "The future is independent of the past given the future."

The agent makes its decisions as a function of a signal from the environment called the environment’s state. Ideally, a state signal should summarise past actions compactly yet in such a way that all relevant information is retained. This normally requires more than the immediate actions but never more than the complete history of all past sensations. A state signal that succeeds in retaining all relevant information is said to be **Markov**, or to have the **Markov property**.

Mathematically, the **Markov property** can be expressed as, 

```math
P[S_{t+1} | S_{t}] = P[S_{t+1} | S_{1}, ..., S_{t}]
```

### Markov Processes

A **Markov Process** is a memoryless random process, i.e. a sequence of random states $S_{1}, S_{2}, ...$ with the Markov property. A Markov Process can be represented as a tuple <$S $, _P_>, where $S$ is a finite set of states and _P_ is the transition state probability matrix, _P_<sub>$ss'$</sub> $= P[S_{t+1} = s' | S_{t} = s]$.

### State Transition Probability

For Markov state $s$ and successor state $s'$, the **state transition probability** is defined by, 

```math
\textit{P}_{ss'} = P[S_{t+1} = s' | S_{t} = s]
```

## Markov Decision Processes

> A reinforcement learning task that satisfies the Markov property is called a Markov decision process, or MDP. If the state and action spaces are finite, then it is called a finite Markov decision process (finite MDP).
