---
layout: post
title:  "Common sampling techniques and the interview questions"
categories: tech interview
published: true
---

__Random sampling__ is an important statitical technique. It enbales the Monte-Carlo methods which could greatly simplify many computations, provides time complexity guarantees to algorithm such as quicksort, and even been used to protect user's privacy nowdayss. Therefore it becomes a popular topics in coding-interview, especially for the data science/machine learning positions. Its popularity can also be explained by the nature of sampling algorithm: a combination of probability and algorithmic thinking, which are the pillars for most data science domains.

There are rich literatures on various types of sampling which are beyond the scope of this article. We will focus mostly on those problems that could possibly be asked during the interview. Let's start coding. (I will use Python as the pseudo code)

#### Questions 1: Generate uniform distribution from another uniform distribution.

##### 1.1
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

##### 1.2

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


#### Question 2: Random sampling with weight

Finish the implementation of the weighted sampling function, which takes a list of weight which contians the weight for integer from 0 to the length of the list - 1. You are allowed to use `random.random()` to get a uniform random variable from [0,1].

Solution:
```python
def weighted_sample(weights: List[float]) -> int:
    cum_weight = []
    cum_sum = 0
    for w in weights:
        cum_sum += w
        cum_weight.append(cum_sum)
    cum_weight = [x / cum_sum for x in cum_weight]
    
    # Find lowest upper bound
    u = random.random()
    l_ptr, r_ptr = 0, len(cum_weight) - 1
    # Take extra step to deal with boundary condition
    while l_ptr < r_ptr - 1:
        mid = (l_ptr + r_ptr) // 2
        if u < cum_weight[mid]:
            r_ptr = mid
        else:
            l_ptr = mid
    # select the lowest possible upper bound
    if u < cum_weight[l_ptr]:
        return l_ptr
    else:
        return r_ptr
```

Here we transform the sampling problem into a simulation problem: dropping a ball into lines of non-overlapping intervals whos lengths determines their weights. And we use the cumulative sum to represent the new axis. The rest of the algorithm is just to find lower bound which corresponds to the interval that contains the ball.

#### Question 3: Generate random permutation

Write a function to generate a random permutation.

Solution

```python
def random_permutation(n: int) -> List[int]:
    perm = list(range(n))
    for i in range(n - 1):
        j = i + random.randint(n - i - 1)
        perm[i], perm[j] = perm[j], perm[i]
    return perm
```

Generating random permutation is intuitively simple as we can just select the elements from the set one by one without retry. However it takes time to maintain the structure that supports uniform sampling. The swaping operation here helps us maintian the `loop invariant`: all the elements after i are not added considered so far and have equal probability of being chosen.


#### Question 4: Uniform random sampling from infinite long list.

Finish the function that supports uniform sampling from an infinite number stream.


Solution:

```python
def sample_stream(stream: Stream, n: int) -> int:
    val = stream.pull()
    for i in range(1, n):
        u = random.rand()
        if u < 1 / (i + 1):
            val = stream.pull()
    return val
```

Suppose at step _t_, the each elements has 1/_t_ chance to be selected. Then at step _t + 1_, the current number has probability of $$ \frac{1}{t} \frac{t}{t + 1} = \frac{1}{t + 1} $$ to be kept while the new number has $$\frac{1}{t + 1}$$ too. Hence we now find a way to prove the correctness of this algorithm using __mathematical induction__.

#### Question 5: Generate random normal distribution

Given a uniform random variable generator from [0, 1], write a function to generate standard normal distribution.

Solution:
```python
def box_muller():
    u1, u2 = random.random(), random.random()
    return math.sqrt(-2 * math.log(u1)) * math.cos(2 * math.pi * u2)
```

This is based on the famous `Box-Muller` transformation. Specifically, let \$$ U_1, U_2  \sim U[0, 1] $$

Then 

\$$ 
Z_1 = Rcos(\Theta) = \sqrt{-2ln(U_1)} \cdot cos(2 \pi U_2);
Z_2 = Rsin(\Theta) = \sqrt{-2ln(U_1)} \cdot sin(2 \pi U_2) 
$$

The correstness of this transformation can be proved in multiple ways. One direction is to think about polar form of complex number: $$ Z_1 + i Z_2 =  R \cdot e^{2\pi i\Theta} $$. The idea is given the two independent normal random variable, we can prove the angle of their polar form are uniform distribution. While for the radius, we can proceed with $$p(R <= r) = p(R^2 = Z_1^2 + Z_2^2 <= r^2 ) = exp(-\frac{r^2}{2}) $$ (The sum of two squared standard normal random variable follows standard chi-square distribution of degree 2). Then we have $$ p(R <= \sqrt{-2ln(u)}) = u $$ by pluging $$r =  \sqrt{-2ln(u)}$$. It gives the CDF of a standard uniform distribution. 




