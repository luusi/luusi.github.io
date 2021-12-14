---
layout: post_page
title: Digital Twins Composition via Markov Decision Processes
description: Master of Science in Engineering in Computer Science 
date: 2021-04-16 22:00:00 UTC
image: _pages/Multi_UAV/scenario.png
authors:
  - name: Luciana Silo
    url: https://luusi.github.io
  - name: Supervised by Prof. Giuseppe De Giacomo
    url: https://www.diag.uniroma1.it/~degiacom/
permalink: /DT_Composition_via_MDP
includelink: true
identifier: /DT_Composition_via_MDP
order: 4
bibliography: _pages/Multi_UAV/Multi-UAV.bib

---

The use of Digital Twins is key in Industry 4.0, in the Industrial Internet of Things, engineering, and manufacturing business space. For this reason, they are becoming of particular interest for different fields in Artificial Intelligence (AI) and Computer Science (CS). In this work, we focus on the orchestration of Digital Twins. We manage this orchestration using Markov Decision Processes (MDP), given a specification of the behaviour of the target service, to build a controller, known as an orchestrator, that uses existing stochastic services to satisfy the requirements of the target service. The solution to this MDP induces an orchestrator that coincides with the exact solution if a composition exists. Otherwise, it provides an approximate solution that maximizes the expected discounted sum of values of user requests that can be serviced. We formalize stochastic service composition and we present a proof-of-concept implementation, and we discuss a case study in an Industry 4.0 scenario.

## Introduction 
The continuous evolution of technologies in the fields of communication, networking, storage and computing, applied to the more traditional world of industrial automation, in order to increase productivity and quality, to ease workers’ lives, and to define new business opportunities, has created the so-called smart
manufacturing, or Industry 4.0. Digital Twins (DTs)1 are up-to-date digital descriptions of physical objects and their operating status. Modern information
systems and industrial machines may natively come out with their digital twin; in other cases especially when the approach is applied to already established
factories and production processes, digital twins are obtained by wrapping actors that are already in place. The main goal is to establish a tight integration
between the physical world and the virtual world, in order to make production 
more efficient, reliable, flexible and faster. The Digital Twin is an ideal tool to
accomplish the purpose of Industry 4.0, since it enables massive exchange of data
that can be interpreted by analytical tools, in order to improve decision making.
Inspired by the research about automatic orchestration and composition of
software artifacts, such as Web services, in [4] it has been argued that an important step towards the development of new automation techniques in smart
manufacturing is the modeling of DT services and data as software artifacts,
and that the principles and techniques for composition of artifacts in the digital
world can be leveraged to improve automation in the physical one. In particular, inspired by the Roman model for service composition [2, 1], they consider
smart manufacturing scenarios where DTs of physical systems —or, simply, twins
—provide stateful services wrapping the functionalities of machines and tasks
of human operators. Nevertheless, there is an inherent limitation of approach
based on the classical Roman model, which is the assumption that the available services, i.e. the services that can be used to realize the target service,
behave deterministically. This assumption is often unrealistic, because in practice the underlying physical system modeled as a set of services might show
non-deterministic behaviour due to the complexity of the domain, or due to an
inherent uncertainty on the dynamics of such system. In these cases, the deterministic service model is not expressive enough to capture crucial facets of the
system being modelled. Moreover, the above-mentioned techniques work only
when the target is fully realizable, i.e. the specification can either be satisfied
or not, with no middle ground. In the context of Industry 4.0 this might be
seldom the case, and instead it would be preferred a technique that, rather than
returning no answer, returns the “best-possible” solution under the actual circumstances. The work [3] contributes in this direction by providing a solution
technique that coincides with the exact solution if a composition exists; otherwise it provides an approximate solution that maximizes the expected sum of
values of the target service’s requests. Unfortunately, such model is not expressive enough to capture the non-determinsitic behaviour of the available services
which, as argued above, is a must-have in our setting.
In this paper, we marry the vision of employing service composition techniques to orchestrate digital twins. We propose a generalization to the service
composition in stochastic setting proposed in [3], in which not only the target
but also the services are allowed to behave stochastically. Moreover, we allow
the services to be taken into account in the optimization problem by associating
a reward to each service’s transition, besides the target’s rewards.

## Objectives of the abstract problem
The objectives of the abstract problem are:
1. Find an optimal policy for each agent, reducing interference between agent policies
2. Agents must respect the mission priority associated with each goal and reach the goals in the correct order



## Assumptions 
- We assume a global clock, which measures the time during the evolution of the scenario for each agent $\alpha_i$, from 0 to T.
- Let’s consider a set of asynchronous goals, in which each agent can decide to execute its task at any time independently from the other agents.
- Each agent does not know where the other agents are at any given instant of time.

## Problem Definitions 
The scenario of our case study is a tuple defined as follows:

A Scenario $\mathcal{S}  = \langle \mathcal{A, M, RB} \rangle $. 

| ![](_pages\Multi_UAV\problem.gif)| 
|:--:| 
| Scenario definition   |

Where $\mathcal{A}$ is the set of agent $ \alpha_i $ modelled by the MDP, in our case each agent is a UAV.
$\mathcal{RB}$ is the set of Restraining Bolts, which define the behaviour of the UAV to reach the goal.
The agent interacts with the $ W $ world through actions $ A_i $.



## Restraining Bolt augmented
$\mathcal{RB_i^\mathcal{+}}=\mathcal\langle\ \mathcal{L}, (\varphi_{i}, r_{i}, P_{i}) \rangle$

The behaviour of each agent is learned through the use of _Reinforcement Learning_ and _Restraining Bolt_.
Restraining Bolt was introduced in 2019 by (De Giacomo et al., 2019)<d-cite key="restraining"> restraining </d-cite> , this thesis work extends the original $\mathcal{RB}$ by adding the concept of priority.

Each $\mathcal{RB}$ is a tuple that contains:
- $\mathcal{L}$ is the set of fluids,  in the case study is the colours assigned to hospitals
- each agent $\alpha_i$ has a goal ($\phi_i, P_i$) where is the _LTLf/LDLf_ formula that the agent must satisfy. 
- $P_i$ is a non-negative integer associated with this formula to indicate its priority.
- $r_i$ is the reward associated with formula $\phi_i$

### Reinforcement Learning + Restraining Bolt augmented
In the _RL_ the agent performs an action and receives a reward and an observation from the environment. With the _Bolt_, the agent receives two independent rewards, one from the _MDP_ for each _state/action_ and another from the $\mathcal{RB}$ based on the state of the automata, which follows the satisfaction state of the $\phi_i$, formula.

| ![](_pages\Multi_UAV\RL-RB+.gif)| 
|:--:| 
| Reinforcement Learning + Restraining Bolt augmented |


Each agent will try to maximize the rewards to find an optimal policy.
The concept of priority permits finding incremental policies. The system is scalable, and each agent learns independently.


## Proposed Solution
Minimizing the risk of collision is a fundamental requirement to carry out _Multi-UAV_ missions safely, this requirement is more important than efficiency and must be guaranteed.
Another fundamental objective of this thesis is the management of missions according to their priority.
Hence, each _UAV_ must reach its goal in the shortest possible time based on the priority of the task assigned to it, ensuring _safety_.

Our solution is given to find  $n$ policies $\rho_{\mathcal{a g}_{i}}$, one for each agent $\alpha_i$, that are individually optimal concerning each agent's goal, and that in the case of conflicts, they are always resolved in favour of the agent who has a goal with higher priority (lower value of $P_i$).

To eliminate interference between agent policies, we define a new _"no-interference" reward function_ $ N\_{\mathcal{ag}\_{i}}$ for each agent.
The $ N\_{\mathcal{ag}\_{i}}$ function is computed by applying, to the original reward function  $R\_{\mathcal{ag}\_{i}}$  a modifier $\mathcal{\tau_i}$ that depends on the trajectories generated by the execution of the optimal policy $\rho_{\mathcal{a g}\_{i}}$ of agents with higher priority goals.
Each agent must satisfy its $\varphi_{i}$ formula and must respect the priority value assigned to this goal.


- If the agent $ \alpha_i $ has $ P_i = 0 $ (highest priority), then $ N_{\mathcal{ag}\_{i}}$ equals the reward function $ R_{\mathcal{ag}\_{i}}$.
- If the agent $ \alpha_i $ has $ P_i > 0 $, then $ N\_{\mathcal{ag}\_{i}} $ reward function is the sum of the reward function $ R_{\mathcal{ag}\_{i}}$ of the agent $\alpha_i$ and of a modifier $\mathcal{\tau_i}$ that depends on the trajectories generated by optimal policies of agents $\alpha_j$ for which $P_j < P_i$.


_More formaly:_
<p style="text-align: center;">
$N_{\mathcal{ag}\_{i}} = R\_{\mathcal{a g}\_{i}} + f ( \{ \tau_j | \alpha_j \ s.t.\ P_j < P_i \})$
</p>

$ N\_{\mathcal{ag}\_{i}} $ is defined on the basis of the policies obtained from the agents $\alpha_j$, who have a higher priority $P_j < P_i$.

Note that, we admit non-deterministic behaviour during optimal policy execution. However, we assume that the variability of the trajectories is limited so that it is possible to define a modifier function $f$ removing interference.




## Case study
The case study is a model of our hospital's problem in which the world is represented in a grid.
$\mathcal{RB_i}$ gives a positive reward if the agent does _hovering/beep_ in the right order on the cells of the hospitals.
In our case, the no-interference reward has been implemented by adding negative weights on the cells crossed by the trajectories of the previous agents.

| ![](_pages\Multi_UAV\LDLf.png)| 
|:--:| 
| The missions of each _UAV_ is specified by _LDLf_ formulas  |


The _LTLf/LDLf_ formulas specify the mission of each _UAV_ and the temporal order of the goals.
The _RL_ algorithm used for the experiments is _SARSA_, _State – action – reward – state – action_, but we should use also other RL algorithms.

| ![](_pages\Multi_UAV\ex3.gif) | ![](_pages\Multi_UAV\ex4.gif) |
|:-------------------------:|:-------------------------:|
| Experiment 3. Without priority | Experiment 4. With priority |

As we can see in experiment 1 without priority, the trajectories of the UAVs intersect with each other. In experiment 2 we resolve these conflicts with priority and with our _no-interference reward functions_.
The purple _UAV_ has the highest priority so its trajectory is the same in both experiments. The orange _UAV_ has priority 1, and the grey _UAV_ has priority 2.
It is possible to perform experiments by increasing the size of the map, the number of goals and agents.

### Conclusion

Our system is scalable, and we can generate hundreds of trajectories at a lower cost than a standard Multi-agent algorithm, in which case the cost would be exponential.
Our method finds an optimal policy for each agent, reducing interference between agents policies.
From the execution of the policies, we generate trajectories with a lower frequency of conflict.
