---
layout: post
title:  "Off Policy Bandit Evaluation (1)"
categories: data science
---

Recently, I have been working on a project about evaluation of the _bandit_ algorithms. At Walmart, we uses the _bandit_ model for many interactive systems, for example the campaign page use the bandit algorithm to selects which category of items should be recommended to the customers. It's a powerful tool when your model needs to balance between exploration and exploitation. 

Mathematically speaking, the _bandit_ problem is sequential process between the __agent__ and the __environment__. The agent interacts with the enviroment $$n$$ rounds, and at each round the agent needs to take an __action__ $$A_t$$ from the actions space $$\mathcal{A}$$ then it will receive the __reward__ $$R_t \in \mathbb{R} $$. At teach round, the agent is able to make decisions based the observations in the __history__, which is dfined as $$H_{t-1} = (A_1, R_1, \cdots, A_{t-1}, R_{t-1})$$. The agent's decision making is summarized as the __policy__. We denote the policy as $$\pi \in \prod$$. Since realistically, the agent could only see the events happened before, so does his policy. We thus treat the __policy__ as a function that map the $$H_t$$ to $$A_t$$. While the enviroment responds in terms of how the reward is assigned based on the agent's last actions as well as its history. The environment is generally unknown to the agent. It might assume the environment is from some set $$\mathcal{E}$$. In literatures, people usually call $$\mathcal{E}$$ __environment class__. The more we know about the environment, the smaller its space could be. 

Usually, we want the agent to get highest possible cumulative reward in the process. The cumulative reward up to round $$n$$ is defined as $$E[\sum^{n}_{t=1}R_t]$$. 



