---
title: "Find fascists in Secret Hitler using a Bayesian network"
date: 2021-11-17
draft: false
katex: true
markup: "mmark"
---

Here is a link to the [rules](https://www.secrethitler.com/assets/Secret_Hitler_Rules.pdf) of Secret Hitler.

## Our Approach

Based on whether the policy played at the end of a turn is liberal or fascist, we want to update the probability that the president during that turn is fascist and the probability that the chancellor during that turn is fascist.

The difficulty is that the probability of the president being fascist is in part determined by the probability that the chancellor is fascist and visa versa. For example, if a fascist policy is played, but we have a high prior on the chancellor being fascist, then this should be somewhat exculpatory for the president.

Therefore, a straightforward application of Bayes rule will not do. We will need to create a Bayesian network which updates based on the players involved. Let us define the variables.

### Variables

$$P(P_f)$$: probability that the president is fascist

$$P(C_f)$$: probability that the chancellor is fascist

$$P(L)$$: probability that a liberal policy is played

$$P(F)$$: probability that a fascist policy is played

$$P(x, y)$$ where $$x + y = 3$$: probability that the president draws $$x$$ liberal cards from the pile and $$y$$ fascist cards

$$P(x, y)$$ where $$x + y = 2$$: probability that the chancellor recieves $$x$$ liberal cards from the president and $$y$$ fascist cards

Since we observe $$L$$ or $$F$$ at the end of every turn, our program must use this information to update the probabilities that the chancellor and president are fascist.

Specifically, **our program must be able to figure out $$P(P_f | L)$$, $$P(P_f | F)$$, $$P(C_f | L)$$, and $$P(C_f | F)$$**.

### The Network

![network](/bayesian-network.png)

As you can see, the president has some probability of drawing 1 of 4 different combinations of 3 cards: 3 liberals 0 fascists, 2 liberals 1 fascists, and so on. Which 2 cards he hands down to the chancellor depend on what he drew and whether he is a fascist. That is unless he draws 3 of the same kind, in which case it doesn't matter if he is a fascist or not.

The chancellor recieves 2 cards, which could be both liberal, both fascist, or 1 of each. He only gets to make a decision when he receives 1 of each, and again that decision will depend on whether he is a fascist.

### Applying Bayes Rule

Bayes rule tells us that:

$$
P(A | B)  = \frac{P(B | A) \cdot P (A)}{P(B)}
$$

Suppose we just witnessed a fascist policy get played. How would we update the probabilities that the president and chancellor are fascist? Let's start with the president and use Bayes rule:

#### President

$$
P(P_f | F) = \frac{P(F | P_f) \cdot P (P_f)}{P(F)}
$$

Let's attempt to resolve the terms on hte right:

$$P(P_f)$$ is simply our stored value for the prior that the president is fascist. This value starts off equal for all players and changes as more turns get recorded.

$$P(F)$$ is the probability that a fascist card would get played this turn, and this is basically the proportion of remaining cards in the pile that are fascists.

So the only thing that remains to be figured out is $$P(F | P_f)$$. What is the probability a fascist is played if the president is fascist? Well, this depends on what the president draws from the pile. If he draws 3 fascists and 0 liberals, then the final card played is guaranteed to be a fascist. Likewise, if a fascist president draws 2 fascists and 1 liberal, he will discard the 1 liberal and the final outcome will remain fascist. 

The only other case where a fascist is played is when the president draws 1 fascist and 2 liberals, discards the liberal, and hands the heterogenous pair to a chancellor who is also fascist, who then discards the remaining liberal to play a fascist. In other words, for a fascist to be played when 1 fascist and 2 liberals are drawn, the chancellor also has to be fascist. 

With $$P(C_f)$$ being our prior that the chancellor is fascist, we finally have the formula, 

$$ 
P(F | P_f) = P(0, 3) + P(1,2) + P(C_f) \cdot P(2,1) 
$$

We update the president using the expanded Bayes rule:

$$
P(P_f | F) = \frac{(P(0, 3) + P(1,2) + P(C_f) \cdot P(2,1)) \cdot P (P_f)}{P(F)}
$$

#### Chancellor

We will discuss how to calculate the draw probabilities in a second, but first let us consider how to apply Bayes rule to the chancellor when a fascist card is played. Let's start with the raw formula:

$$
P(C_f | F) = \frac{P(F | C_f) \cdot P (C_f)}{P(F)}
$$

Like last time, $$P (C_f)$$ remains our prior that the chancellor is fascist and $$P(F)$$ the proporition of remaing cards in the pile that are fascist.

$$P(F | C_f) $$, the probability a fascist is played given that the chancellor is fascist, is the probability that the chancellor draws 2 fascists and 0 liberals from the president plus the probability that the chancellor draws 1 fascist and 1 liberal from the president (a fascist chancellor will just discard the 1 liberal). In other words,

$$ 
P(F | C_f) = P(0,2) + P (1,1)
$$

Let's see if we can put this formula in terms of initial draw probabilities using the dependencies in the network picture above.

$$
P(0,2) = P(0,3) + P(P_f) \cdot P(1,2)
$$

$$
P(1,1) = P(P_f) \cdot P(2,1) + (1 - P(P_f) ) \cdot P(1,2)
$$

$$ 
P(F | C_f) = P(0,3) + P (1,2) + P(P_f) \cdot P(2,1)
$$

Hey, what do you know, this is identical to the formula for a fascist being played given the president is fascist (except with the term for the other player switched). I was pretty excited when I had finished doing the arithmetic and found this neat isomorphism.

Anyways, now we can put together the extended Bayes rule for the chancellor when a fascist card is played:

$$
P(C_f | F) = \frac{(P(0,3) + P (1,2) + P(P_f) \cdot P(2,1)) \cdot P (C_f)}{P(F)}
$$

#### When a liberal is played [ fix]

The formulas stay pretty similar when a liberal is played. 

Remember that $$P(L | P_f) $$ is contingent on the president being a fascist, so we exclude the draws where the player could have avoided a liberal outcome. In particular, if the draw was 1 liberal and 2 fascists, then both the president and chancellor would get a chance to ensure that a fascist card is played.

$$
P(C_f | L) = \frac{(P(3,0) + P (2,1) + P(P_f) \cdot P(2,1)) \cdot P (C_f)}{P(F)}
$$

#### Draw probabilities



#### See Jupyter notebook

