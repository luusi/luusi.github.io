---
layout: post_page
title: Digital Twins Composition via Markov Decision Processes
description: Master of Science in Engineering in Computer Science 
date: 2021-04-16 22:00:00 UTC

authors:
  - name: Luciana Silo
    url: https://luusi.github.io
  - name: Supervised by Prof. Giuseppe De Giacomo
    url: https://sites.google.com/a/dis.uniroma1.it/iocchi/home
permalink: /DT_Composition_via_MDP
includelink: true
identifier: /DT_Composition_via_MDP
order: 4
bibliography: _pages/Multi_UAV/Multi-UAV.bib

---

The use of Digital Twins is key in Industry 4.0, in the Industrial Internet of Things, engineering, and manufacturing business space.
	For this reason, they are becoming of particular interest for different fields in Artificial Intelligence (AI) and Computer Science (CS).
	In this work, we focus on the orchestration of Digital Twins. We manage this orchestration using Markov Decision Processes (MDP), given a specification 
	of the behaviour of the target service, to build a controller, known as an orchestrator, that uses existing stochastic services to satisfy the requirements 
	of the target service. The solution to this MDP induces an orchestrator that coincides with the exact solution if a composition exists. Otherwise, it provides
	an approximate solution that maximizes the expected discounted sum of values of user requests that can be serviced. We formalize stochastic service composition
	and we present a proof-of-concept implementation, and we discuss a case study in an Industry 4.0 scenario.
  
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

## Preliminaries
MDPs.</b>
A Markov Decision Process (MDP) $\M =
\langle S,A,T,R\rangle$ contains a set $S$ of states, a set $A$ of actions, a
transition function $T : S\times A \to Prob(S)$ that returns for
every state $s$ and action $a$ a distribution over the next state,
and a reward function $R : S\times A\to \mathbb{R}$
that specifies the reward (a real value) received by the agent when transitioning
from state $s$ to state $s'$ by applying action $a$. A
solution to an MDP is a function, called a \emph{policy}, assigning
an action to each state, possibly with a dependency on past
states and actions. The \emph{value} of a policy $\rho$ at state $s$, denoted
$v_\rho(s)$, is the expected sum of (possibly discounted by a factor $\lambda$, with $0\leq \lambda < 1$) rewards when starting at state $s$ and
selecting actions based on $\rho$. Typically, the MDP is assumed
to start in an initial state $s_0$, so policy optimality is evaluated
w.r.t. $v_\rho(s_0)$. Every MDP has an optimal policy $\rho^*$. In discounted cumulative settings, there exists an optimal policy
that is Markovian $\rho : S \to A$, i.e., $\rho$ depends only on the
current state, and deterministic \cite{puterman1994markov}.
Among techniques for finding 
an optimal policy of an MDP,
there are <i>value iteration</i> and <i>policy iteration</i> \cite{sutton2018reinforcement}.

<b>The Roman Model in stochastic settings.</b>
The problem of service composition,
i.e. the ability to generate
new, more useful services from existing ones, has been considered in the literature for over a decade \cite{hull2008artifact,medjahed2011service,degiacomo2014automated}.
The goal is, given a specification of the behavior of the target
service, to build a controller, known as an <i>orchestrator</i>, that uses existing services
to satisfy the requirements of the target service.
Here we concentrate on the approach known in literature as the “Roman model" \cite{berardi2003automatic,berardi2005automatic}: each available
service is modeled as a finite-state machines (FSM), in which at each state, the service offers
a certain set of actions, where each action changes the state of the service in some way. The
designer is interested in generating a new service (referred to as target) from the set of existing
services. The required service (the requirement) is specified using a FSM, too.

Unfortunately, it is not always possible to synthesize a service that fully conforms with the
requirement specification. This zero-one situation, where we can either synthesize a perfect
solution or fail, often is not very applicable. Rather than returning no answer, we may want
notion of the ``best-possible" solution. A model with this notion has been developed in \cite{brafman2017service}, where the authors discuss and elaborate upon a probabilistic model for the service
composition problem, first presented in \cite{yadav2011decision}. In this model, an optimal
solution can be found by solving an appropriate probabilistic planning problem (e.g. an MDP)
derived from the services and requirement specifications.
Due to lack of space, we do not report the details of such technique.

## Assumptions 
- We assume a global clock, which measures the time during the evolution of the scenario for each agent $\alpha_i$, from 0 to T.
- Let’s consider a set of asynchronous goals, in which each agent can decide to execute its task at any time independently from the other agents.
- Each agent does not know where the other agents are at any given instant of time.

## Problem  
Before stating the problem, we give preliminary definitions.
A <i>stochastic service</i> is a tuple
$\tilde{\S} = \langle \Sigma_s, \sigma_{s0}, F_s, A, P_s, R_s\rangle$, 
where $\Sigma_s$ is the finite set of service states,  $\sigma_{s0}\in \Sigma$ is the initial state, $F_s\subseteq\Sigma_s$ is the set of the service's final state, $A$ is the finite set of services' actions,
$P_s: \Sigma_s\times A \to Prob(\Sigma_s)$ is the transition function,
and $R_s : \Sigma_s\times A\to \mathbb{R}$ is the reward function.
In short words, the stochastic service is the stochastic variant
of the service defined in the classical Roman model, and it can be seen as an MDP itself.

A <i>target service</i>, as defined in \cite{brafman2017service}, is $\T= \langle \Sigma_t, \sigma_{t0}, F_t, A, \delta_t,P_t,R_t\rangle$, 
where 
$\Sigma_t$ is the finite set of service states,
$\sigma_{t0}\in \Sigma$ is the initial state,
$F_t\subseteq\Sigma$ is the set of the service's final state,
$A$ is the finite set of services' actions,
$\delta_{t}: \Sigma\times A \to \Sigma$
is the service's deterministic and partial transition function,
$P_t: \Sigma_t \to \pi(A) \cup \emptyset$ is the action distribution function, 
$R_t : \Sigma_t \times A \to \mathbb{R}$ is the reward function.

A <i>stochastic system service</i> $\tilde{\Z}$ 
of a community of stochastic services  $\tilde{\C}=\{\tilde{\S_1},\dots,\tilde{\S_n}\}$ is a stochastic service
where $\tilde{\Z} = \langle \Sigma_z, \sigma_{z0}, F_z, A, P_z, R_z\rangle$ are defined as follows:
$\Sigma_z = \Sigma_1 \times \cdots \times \Sigma_n$,
$\sigma_{z_0} = (\sigma_{10}, \dots, \sigma_{n0})$,
$F_z = \{ (\sigma_1, \dots, \sigma_n) \mid \sigma_i\in F_i, 1\le i \le n\}$,
$A_z = A \times \{1,\dots n\}$ is the set of pairs $(a, i)$ formed 
by a shared action $a$ and the index $i$ of the service that executes it,
$P_z(\bm{\sigma'} \mid \bm{\sigma}, (a, i)) = P(\sigma_i' \mid \sigma_i, a)$, for $\bm{\sigma} = (\sigma_1\dots\sigma_n)$,
$\bm{\sigma'} = (\sigma_1'\dots\sigma_n')$
and $a\in A_i(\sigma_i)$,
with $\sigma_i\in\Sigma_i$ and  $\sigma_j = \sigma'_j$ for $j\neq i$,
$R_z(\bm{\sigma}, (a, i)) = R_i(\sigma_i, a)$ for $\bm{\sigma}\in \Sigma_z$, $a\in A_i(\sigma_i)$.

We define the set of joint histories of the target and the system service
as $H_{t,z} = \Sigma_t\times\Sigma_z \times (A\times \Sigma_t\times\Sigma_z)^*$. A joint history
$h_{t,z}=\sigma_{t,0}\sigma_{z,0}a_1\sigma_{t,1}\sigma_{z,1}a_2\dots$ is an element of $H_{t,z}$. 
The projection of $h_{t,z}$ over the target (system)
actions is $\pi_t(h_{t,z}) = h_t$ ($\pi_z(h_{t,z}) = h_z$).

An orchestrator 
$\gamma: \Sigma_t\times\Sigma_z\times A \to \{1,\dots,n\}$,
is a mapping from a state of the target-system service and user action
$(\sigma_t, \sigma_z, a)\in \Sigma_t\times\Sigma_z\times A$
to the index $j\in \{1,\dots,n\}$ of the service that must handle it.
Crucially, since the stochasticity comes also from the services,
the orchestrator \emph{does} affect the probability of an history $h_{t,z}$.
Moreover, in general, there are \emph{several} system histories associated to a given 
target history.

Let 
$P_\gamma(h) = \prod_{i=0}^{|h|} P_t \big(\sigma_{t,i}, a_{i+1}\big)
P_z\big(\sigma_{z,i+1}\mid \sigma_{z,i},\langle a_{i+1},\gamma(\sigma_{t,i}, \sigma_{z,i}, a_{i+1})\rangle)\big)$ be the probability of a (joint) history $h =  \sigma_{t0}\sigma_{z0}\langle a_1,j_1 \rangle \sigma_{t1}\sigma_{z1}\langle a_2,j_2 \rangle\dots$ under orchestrator $\gamma$.
% 
Intuitively, at every step, we take into account the probability,
determined by $P_t$, that the
user does action $a_{i+1}$ in the target state $\sigma_{t,i}$,
in conjunction with the probability, determined by $P_z$, that 
the system service does the transition 
$\sigma_{z,i}\xrightarrow{(a_{i+1}, j)} \sigma'_{z,{i+1}}$, 
where $j$ is the choice of the orchestrator at step $i$
under orchestrator $\gamma$, i.e. $j = \gamma(\sigma_{t,i}, \sigma_{z,i}, a_{i+1})$.

The value of a joint history under orchestrator $\gamma$ 
is the sum of discounted rewards, both from the target and the system services:
$v_\gamma(h) = \sum\limits_{i=0}^{|h|} \lambda^i 
         \bigg(
            R_t\big(\sigma_{t,i}, a_{i+1}\big) +
            R_z\big(
                    \sigma_{z,i}, 
                    \langle a_{i+1}, \gamma(\sigma_{t,i}, \sigma_{z,i}, a_{i+1})\rangle)
            \big)
        \bigg)$

Intuitively, we take into account both the reward that comes
from the execution of action $a_{i+1}$ in the target service,
but also the reward associated to the execution of that action
in service $j$ chosen by orchestrator $\gamma$.
% 
Now we can define the expected value of an orchestrator
to be:
$v(\gamma) = \mathbb{E}_{h_{t,z}\sim P_\gamma} \big[v_\gamma(h_{t,z})\cdot realizable(\gamma, \pi_t(h_{t,z})) \big]$
where $realizable(\gamma,\pi_t(h_{t,z}))$ is 1 if $h_t = \pi_t(h_{t,z})$ 
is realizable in $\gamma$ (i.e. all the possible target histories are processed correctly), and $0$ otherwise. That 
is, $v(\gamma)$ is the expected value of histories realizable in $\gamma$. 
Finally, we define an optimal orchestrator to be
$\gamma = \arg \max_{\gamma'} v(\gamma')$.

It can be shown that, under certain assumptions (i.e. target is realizable, every history has strictly positive value, and the target's rewards are always greater than services' rewards),
	optimality of the orchestrator implies that the target is realized by the orchestrator.



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
