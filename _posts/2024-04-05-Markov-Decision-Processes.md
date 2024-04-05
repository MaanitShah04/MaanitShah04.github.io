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

A **Markov Process** is a memoryless random process, i.e. a sequence of random states $S_{1}, S_{2}, ...$ with the Markov property. A Markov Process can be represented as a tuple $\langle S, \textit{P} \rangle$, where $S$ is a finite set of states and _P_ is the transition state probability matrix, _P_<sub>$ss'$</sub> $= P[S_{t+1} = s' | S_{t} = s]$.

### State Transition Probability

For Markov state $s$ and successor state $s'$, the **state transition probability** is defined by, 

```math
\textit{P}_{ss'} = P[S_{t+1} = s' | S_{t} = s]
```

The state transition matrix _P_ defines transition probabilities from all states to all successor states.

```math
\textit{P} = \begin{bmatrix} 
    \textit{P}_{11} & \dots & \textit{P}_{1n} \\
    \vdots & \ddots & \\
    \textit{P}_{n1} &        & \textit{P}_{nn} 
    \end{bmatrix}
```
### Example of Markov chain

_Image Placeholder_

## Reward
> All of what we mean by goals and purposes can be well thought of as the maximization of the expected value of the cumulative sum of a received scalar signal (called reward).

In simpler terms, it is a numerical value given to the agent based on some action at some state in the environment.

## Return

If the sequence of rewards received after time step $t$ is denoted $R_{t+1}, R_{t+2}, R_{t+3}, . . .$, we seek to maximize the expected return, where the return $G_t$ is defined as some specific function of the reward sequence.

```math
G_t = R_{t+1} + R_{t+2} + R_{t+3} + · · · + R_T,\text{ where T is a final time step}
```

## Episodic and Continuing Tasks

> The agent–environment interaction breaks naturally into subsequences, which we call episodes.  Each episode ends in a special state called the terminal state, followed by a reset to a standard starting state or to a sample from a standard distribution of starting states. Tasks with episodes of this kind are called **episodic tasks**.

However, this is not always the case.

>  In many cases the agent–environment interaction does not break naturally into identifiable episodes, but goes on continually without limit. We call these **continuing tasks**.

## Discount Rate

> The discount rate determines the present value of future rewards.

The agent tries to select actions so that the sum of the discounted rewards it receives over the future is maximized. In particular, it chooses $A_t$ to maximize the expected discounted return:

```math
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}
```
$\gamma$ is a parameter, $0 \leq \gamma \leq 1$, called the **discount rate**. It determines the importance given to future rewards.

## Markov Decision Processes

> A reinforcement learning task that satisfies the Markov property is called a Markov decision process, or MDP. If the state and action spaces are finite, then it is called a finite Markov decision process (finite MDP).

A Markov Decision Process can be represented as a tuple $\langle S, A, \textit{P}, R, \gamma \rangle$ where,
- $S$ is a finite set of states
* $A$ is a finite set of actions
* _P_ is a state transition matrix, $P^a_{ss'} = \mathbb{P}\left[S_{t+1} = s' | S_t = s, A_t = a\right]$
* $R$ is a reward function, $R^a_s = \mathbb{E}\left[R_{t+1} | S_t = s, A_t = a\right]$
* $\gamma$ is a discount rate, $\gamma \in [0,1]$

## Policies

> A policy $\pi$ is a probability distribution over actions given states.

```math
\pi (a|s) = \mathbb{P}\left[A_t = a|S_t = s\right]
```
> A policy defines the learning agent’s way of behaving at a given time. Roughly speaking, a policy is a mapping from perceived states of the environment to actions to be taken when in those states.

## Value Function

> Value Functions estimate how good it is for the agent to be in a given state (or how good it is to perform a given action in a given state). The notion of “how good” here is defined in terms of expected return.

### State-Value Function

The **state-value function** $V_{\pi}(s)$ of an MDP is the expected return starting from a state $s$ under a policy $\pi$.
```math
V_{\pi}(s) = \mathbb{E}\left[G_t|S_t = s\right] = E_\pi \left[ \sum_{k=0}^\infty \gamma^k R_{t+k+1} \, \middle| \, S_t = s \right]
```
### Action-Value Function

The **action-value function** $q_{\pi}(s, a)$ is the expected return starting from $s$, taking the action $a$, and thereafter following policy $\pi$.
```math
q_\pi(s, a) = \mathbb{E}_\pi \left[ G_t \, \middle| \, S_t = s, A_t = a \right] = \mathbb{E}_\pi \left[ \sum_{k=0}^\infty \gamma^k R_{t+k+1} \, \middle| \, S_t = s, A_t = a \right]
```

## References
[1]: Sutton, R. S., & Barto, A. G. (2018). Reinforcement learning: An introduction (2nd ed.). The MIT Press.

[2]: [Markov Decision Processes, Subir Varma](https://subirvarma.github.io/GeneralCognitics/Course2/Lecture2_MDPs.pdf).

[3]: J. Norris, “Markov Chains,” Cambridge University Press, Cambridge, 1998.
