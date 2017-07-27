---
layout: "post"
title: "LEARNING A POLICY UNDER A MARKOV DECISION PROCESS WITH METHODS DERIVED FROM BELLMAN EQUATION
"
date: "2017-05-07 13:16"
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [THE MAIN GOAL OF THIS CONSPECT](#the-main-goal-of-this-conspect)
- [IMPORTANT NOTATION AND VARIABLES](#important-notation-and-variables)
	- [MDD PROBLEM MODEL](#mdd-problem-model)
	- [MOST IMPORTANT VARIABLES](#most-important-variables)
- [OFFLINE ALGORITHMS](#offline-algorithms)
	- [PREFACE](#preface)
	- [FINDING $$V^{* }(s)$$](#finding-v-s)
		- [Value Iteration](#value-iteration)
		- [Policy Iteration](#policy-iteration)
	- [POLICY EXTRACTION](#policy-extraction)
		- [Based on Q-values](#based-on-q-values)
		- [Based on $$V(s)$$](#based-on-vs)
	- [POLICY EVALUATION](#policy-evaluation)
		- [Iterative method](#iterative-method)
		- [Linear Equation based](#linear-equation-based)
- [ONLINE ALGORITHMS AKA REINFORCEMENT LEARNING](#online-algorithms-aka-reinforcement-learning)
	- [PASSIVE](#passive)
	- [Model Based](#model-based)
		- [Algorithm Sample a policy to get T and R](#algorithm-sample-a-policy-to-get-t-and-r)
	- [Model Free](#model-free)
		- [Direct Evaluation of states](#direct-evaluation-of-states)
		- [Sampling of transitions to evaluate states - V-value learning](#sampling-of-transitions-to-evaluate-states-v-value-learning)
		- [Difference between last 2 Methods](#difference-between-last-2-methods)
	- [ACTIVE](#active)
		- [Sampling of transitions to evaluate Q-values: Q-value learning](#sampling-of-transitions-to-evaluate-q-values-q-value-learning)
			- [Exploration and Learning parameters](#exploration-and-learning-parameters)
			- [Q-value Approximation & Updated Q-learning- Kinda hard part](#q-value-approximation-updated-q-learning-kinda-hard-part)
	- [Reinforcement vs Linear Regression, Logistic Regression, Gradient Descent](#reinforcement-vs-linear-regression-logistic-regression-gradient-descent)
		- [Similarities](#similarities)
		- [Differences](#differences)
		- [Some Reference](#some-reference)

<!-- /TOC -->




[Formating with Latex](http://bryancshepherd.com/Displaying-LaTeX-in-Atom%27s-Live-Markdown-Preview/)




# THE MAIN GOAL OF THIS CONSPECT

Mainly Recap. I don't think this is a good conspect to get first insight in this topic, because I rely heavily on Notation and the defined model to easily orient in this problem space.

There are a lot of ways to find a policy under a particular problem. What are the  appropriate methods depends on what are the variables that we are looking for, what type of information is available to us and many other things.
Overall, however these algorithms are actually very similiar in nature and its at least interesting to see a  big picture over those different methods.

# IMPORTANT NOTATION AND VARIABLES

## MDD PROBLEM MODEL

* A set of states $$s \in S$$
* A set of action $$a \in A$$
* A transition function $$T(s,a,s')$$ If this is 1 then just a  Search problem.
  * This actually is the probability, if I take action a from state s, Ill end up in s'
* A reward function $$R(s,a,s')$$
* A start state $$s_{0}$$
* A terminal state.

## MOST IMPORTANT VARIABLES

+ $$T(s,a,s')$$.
+ $$V^{* }(s)$$ , expected utility starting in state s and acting optimally
+ $$Q^{* }(s)$$ Expected utility starting in state s, taking action a and acting optimally after that.
+ $$V^{\pi}_{k}(s)$$, where $$\pi$$ is a policy. - This is the expected utility of a state acting according to $$\pi$$
+ $$\pi(s)$$ action taken in state s according to policy $$\pi$$. $$\pi^{* }(s)$$ is the action according to optimal policy.   


# OFFLINE ALGORITHMS



## PREFACE

Everything Im going to cover is based on BELLMAN EQUATION

$$V^{* }(s) = max_{a}\sum{T(s,a,s')[R(s,a,s')+D*V^{* }(s')]}$$  
D here stands for Discount and we assume this is always known previously. User chosen variable.

## FINDING $$V^{* }(s)$$

### Value Iteration

**Input:** $$R(s,a,s'), T(s,a,s'); \forall s,a$$   
**Returns:**  $$V(s),\forall s$$  
**Complexity** This takes $$O(S^{2}A)$$ for each iteration

_Method_

* Set $$V_{0}(s)=0, \forall s$$  
* Recursively perform updates until convergence  $$V_{k}(s) = max_{a}\sum{T(s,a,s')[R(s,a,s')+D*V_{k-1 }(s')]}$$  


### Policy Iteration

**Input:** $$R(s,a,s'), T(s,a,s'); \forall s,a$$   
**Returns:**  $$V(s),\forall s$$  
**Complexity** $$O(S^{2})$$ for step 2

_Method_


1. Initialize a random policy $$\pi_{k=0}$$
2. Perform Policy Evaluation Using the Iterative method until it converges.
3. Update policy $$\pi_{k+1}(s)=max_{a}\sum{T(s,a,s')[R(s,a,s')+D*V^{\pi_{k}}(s')]}$$
4. Repeat from 2. step until Convergence



## POLICY EXTRACTION

### Based on Q-values  

**Input:** $$Q(s,a) \forall s,a$$  
**Output:**$$\pi(s) \forall s$$
**Complexity:** $$O(SA)$$
_Method_

* $$\pi_(s)=max_{Q(s,a)}, \forall s$$

### Based on $$V(s)$$

**Input:** $$V(s), T(s,a,s'), R(s,a,s'); \forall s,$$    
**Output:** $$\pi(s); \forall s$$  
**Complexity:** $$O(SA)$$  

_Method_

* $$\pi(s) = max_{a}\sum{T(s,a,s')[R(s,a,s')+D*V(s')]}$$    
_(1-Step Expectimax)_

## POLICY EVALUATION

### Iterative method
**Input:** $$\pi(s),T(s,a,s'), R(s,a,s');\forall s$$  
**Output:** $$V(s);\forall s$$  
**Complexity:** $$O(S^{2})$$

_Method_

* Set $$V_{0}(s)=0, \forall s$$  
* Recursively perform updates until convergence  $$V_{k}(s) = \sum{T(s,\pi(s),s')[R(s,\pi(s),s')+D*V_{k-1 }(s')]}$$  




### Linear Equation based

**Input:** $$\pi(s),T(s,a,s'), R(s,a,s');\forall s$$  
**Output:** $$V(s);\forall s$$  
**Complexity:** Depends on the method to solve

_Method_

For all s we have an equation:
$$V^{\pi}(s) = \sum{T(s,\pi(s),s')[R(s,\pi(s),s')+D*V^{\pi}(s')]}$$  
We are looking for $$V^{\pi}(s)$$ for all s. Thus have a  a linear equation of count(s) variables and equations

# ONLINE ALGORITHMS AKA REINFORCEMENT LEARNING

In my unverstanding, **the only difference** with online learning is that we dont know R(s,a,s')  and T(s,a,s') anymore and we have to go through the problem space first.



## PASSIVE

**Existence of Some Policy is ASSUMED and this policy is going to be executed.**
_Just execute the policy and learn from experience_

## Model Based

### Algorithm Sample a policy to get T and R

_We try to model R & T first and then get to the evaluation of states_


**Input:** $$\pi$$  
**Output:** $$T(s,a,s'), R(s,a,s'),V(s);\forall s,a$$  
**Complexity:**

_Method_

2 Steps

1. Extraction of T(s,a,s') and R(s,a,s')   
 a. Randomly Initialize policy $$\pi$$  
 b. Execute this policy and for every pair (s,a) <-(you can consider that as a key in hash table) collect samples results T(s,a,s') and R(s,a,s')  
 c. Normalize and give estimates
2. Extraction of State values   
Can be based on Value Iteration or Policy iteration


## Model Free


### Direct Evaluation of states


**Input:** $$\pi$$    
**Output:** $$V(s);\forall s$$  
**Complexity:**


1. Everyt time you visit state s writed down, what the the sum of discounted rewards turned out to be.
2. Average those samples for each state.


Problems  
* Everyt state has to be collected separately
* We can't actually extract the policy and perform policy evaluation from this, because we need T and R for such calculations.
* On slides it claims that state connections are not considered. I am giving a value to every state separately. However some states are closer to each other than others. Neighbouring states might have an effect to each other, but in this scenario we wont consider it.
* Mostly because every state has to be learned separately, this takes a long time to learn.

### Sampling of transitions to evaluate states - V-value learning Temporal Difference learning

**Input:** $$\pi$$    
**Output:** $$V(s);\forall s$$  
**Complexity:**

1. Every time you perform a transformation, collect a sample based on $$(s,\pi(s),s',r)$$. $$sample=R(s,\pi(s),s')+D*V^{\pi}(s')$$
2. Update $$V^{\pi}(s) = (1-\alpha)* V^{\pi}(s) + \alpha*sample$$

* We cant still extract policy.

### Difference between last 2 Methods

In the first method we perform state evaluation separately for every state and only consider the final results. In the second method however, we run the policy and on every step update states involved. We also consider state connections, because Neighbouring states values are also involved in sampling.

## ACTIVE

### Sampling of transitions to evaluate Q-values: Q-value learning

**Input:** $$\emptyset$$    
**Output:** $$Q(s,a);\forall s,a$$  
**Complexity:**


1.  Every time you perform transition $$(s,a)$$ get a sample $$(s,a,t',r)$$.
$$Sample(s,a) = R(s,a,s')+max_{a}Q(s')$$
2.  Instead of updating $$V^{\pi}(s)$$, we now update $$Q(s,a)=(1-\alpha)Q(s,a)+\alpha*sample$$
s
#### Exploration and Learning parameters

+ $$\alpha$$ must decrease eventually
+ Optimal exploration with $$\epsilon$$ Random exploration with some probability $$\epsilon$$, so other states would be collected. This parameter must also decrease, because in the end with knowledge we want to gain information about optimal state.

#### Q-value Approximation & Updated Q-learning- Kinda hard part

_Problem_  
We have to generalize accross states. To keep Q(s,a) for all state and for all actions in very very memory consuming in any kind of realistic environment.

_Solution_  
Thus we generalize states so experience could be used for new unexplored situations. To do this we have to  
**approximate states** - Describe a state using a vector of features.
  * features - functions froms state to real numbers (often binary) that capture IMPORTANT properties of state
    **Example**
    * Distance to closest ghost
    * Distance to closest don
    * Number of ghost
    * Is pacman in a tunnel (binary feature)

If we do this, we can now use learned information for new states based on **feature vector** similiarity.

Now if every state and action  can be represented as a feature vector
$$(s,a) = \begin{bmatrix} f_{1}(s,a) \\ f_{2}(s,a) \\ ... \\ f_{n}(s,a) \end{bmatrix}$$

.We can transfer the $$Q(s,a)=w_{2}f_{1}(s,a) + w_{2}f_{2}(s,a) + ... + w_{n}f_{n}(s,a)$$ then continue on with updating Q(s,a) like this

_Updated Method_

**transition**: $$(s,a,r,s')$$  
**difference**: $$[r(s,a,s')+D*max_{a}Q(s',a')]-Q(s,a)$$

$$Q(s,a)=Q(s,a)+\alpha*difference$$  - Exact Q-value   
$$w_{i}=w_{i}+\alpha * difference * f_{i}(s,a)$$ <- Difference static over all $$f_{i}(s,a)$$

_Intuition_

* If difference is very low (negative) <- Bad Q result and $$f_{i}(s,a)$$ is high, then then $$w_{i}$$ becomes significantly smaller. Tha for the end learning result would mean that high $$f_{i}(s,a)$$ would result in lower $$Q(s,a)$$
* If difference is very large and $$f_{i}(s,a)$$ is very low. then the result would not change much, because $$f_{i}(s,a)$$ as a multiplier is also very low.
* If difference is very large and $$f_{i}(s,a)$$ is very low. then the result would not change much, because $$f_{i}(s,a)$$ as a multiplier is also very low. Hmm, that does not make much sense.  

## Reinforcement vs Linear Regression, Logistic Regression, Gradient Descent

### Similarities

* Both methods perform learning from external data.

### Differences


* Regression, Gradient descent is based on a  Cost function which it tries to minimize on the data. (Macimum Likelihood estimation). Reinforcement learning averages experience and gives scores to actions.
* Regression predicts a certain label


### Some Reference

[Difference Neurla Network Reinforcement](https://cs.stackexchange.com/questions/58131/are-neural-networks-a-type-of-reinforcement-learning-or-are-they-different)
[TSP and reinforcement](https://www.quora.com/Can-machine-learning-Deep-learning-Reinforcement-learning-be-applied-in-dynamic-programming-problems-such-as-TSP-and-Job-Shop-scheduling)
