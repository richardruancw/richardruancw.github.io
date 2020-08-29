---
layout: post
title:  "Common sampling techniques and the interview questions"
categories: tech interview
published: true
---

__Random sampling__ is an important statitical technique. It enbales the Monte-Carlo methods which could greatly simplify many computations, provides time complexity guarantees to algorithm such as quicksort, and even been used to protect user's privacy nowdayss. Therefore it becomes a popular topics in coding-interview, especially for the data science/machine learning positions. Its popularity can also be explained by the nature of sampling algorithm: a combination of probability and algorithmic thinking, which are the pillars for most data science domains.

There are rich literatures on various types of sampling which are beyond the scope of this article. We will focus mostly on those problems that could possibly be asked during the interview. Let's start coding. (I will use Python as the pseudo code)

#### Questions 1:

Given a function `getUniform10()`, which returns floating numbers from 1 to 10 uniformly, write a function to return number from 1 to 7 uniformly.

Solution:
```python
def getUniform7() -> float:
    while True:
        u = getuniform10()
        if u < 7:
            return u
```

The property of uniform distribution: conditioning on uniform distribution still gives uniform distribution. 

Assume $$U$$ is uniform from 0 to 10 and $$x \in [1, 7) $$:
\$$
p(U < x|U < 7) = \frac{p(U < x < 7)}{p(U < 7)} = \frac{\frac{x}{10}}{\frac{7}{10}} = \frac{x}{7}
$$

#### Question 2:

Given a function `getUniformInt7()`, which returns interger from 1 to 7 uniformly. Write the function `getUniformInt10()` to return integer from 1 to 10 uniformly.

Solution:
```python
def getUniformInt10() -> int:
    while True:
        u1, u2 = getUniformInt7(), getUniformInt7()
        count = u1 * 7 + u2 # use 7 encoding 
        if count <= 40: # rejection with optimized threshold [(7^2 - 1) // 10] * 10
            return (count % 10) + 1

```

The solution to this question also uses the property mentioned abvoes. Beside, to resolve the issue that 10 is outside the output range of `getUniformInt7()` we call this function twice and consider the product space of its outspace, $$U1 \times U2$$, which has $$7^2 - 1$$ possible events. We establish a bijection using the function $$f(u1, u2) = u1 * 7 + u2$$. Note that a bijection between two discrete spaces means the two space can be considered as the same. Now we reduce this problem to sampling [1,..,10] from [1,..,48]. Here we also use a trick to reduce the rejection probability (use modular arithmetic to remove duplicates). 

To be continued...





