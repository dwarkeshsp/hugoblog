---
title: "Draft: 4,362 kids to have a great family"
date: 2021-11-19
draft: false
katex: true
markup: "mmark"
---

Please email me at dwarkesh.sanjay.patel@gmail.com if you figure out that I've fucked up the math :). It wouldn't surprise me since genetics or statistics aren't my thing, and I would really appreciate the correction.

## Introduction

Scott Alexander recently wrote a really interesting blog [post](https://astralcodexten.substack.com/p/secrets-of-the-great-families) about why some families have a long list of super impressive members. He asks how genetic advantages like IQ can persevere through generations given regression to the mean:

> Suppose Erasmus Darwin had a genius-level IQ of 150. And suppose that he tried very hard to marry a bright woman, and his screening mechanism was as successful as the Ivies or Oxbridge when they screen for bright people - in that case his wife would have the same IQ as an average Ivy Leaguer, maybe 130ish. We would predict their average child to have an IQ of 124. Why is the average lower than either parent? Regression to the mean - IQ is probably a combination of genes and random factors, and if your IQ is very high it means you probably have a combination of good genes and good random dice rolls, and even though you can pass on the good genes your kids will probably only get average dice rolls.
> 
> But 124 isn’t even as high as the average Ivy Leaguer. If that IQ 124 kid marries another IQ 130 spouse, things deteriorate less quickly - I think the regression process has selected for his 24 IQ point advantage over average being entirely genetic, so it’s not going to regress further. But his spouse’s IQ can still regress further, so they’ll probably end up with a kid who has an IQ somewhere in the high 110s or low 120s - for comparison, not much higher than the average Ashkenazi Jew. Maintaining a super-high-IQ family over several generations is really hard! ...
> 
> The answer to [this problem] ... is really impressive assortative mating and having vast litters of children.

## Math

I wanted to crunch the numbers to see how difficult it is to have an incredibly brilliant child. Assuming you are yourself a super-genius, is it really as easy as marrying a decently smart partner and siring half a dozen potential super-genius heirs.

By reverse engineering Scott's calculation, we can see that he's using 0.6 for the heritability of IQ ($$h$$). I'm assuming that he's using an equation like, IQ = genetic component * heritability + random factors * (1 - heritability). Solving for heritability, we get:

$$124 = \dfrac{150 + 130}{2} \cdot h + 100 (1 - h)$$

$$h = 0.6$$

We want to figure out how many kids a 150 IQ super-genius like Erasmus Darwin would need to have with his 130 IQ super-smart wife to have a 50% chance of producing another 150 IQ super-genius. We do that by solving for $$kids$$ in the following equation.

$$0.5 = Pr[1 \text{ or more kids out of } k \text{ have IQ} > 150] \\= 1 - Pr[0 \text{ kids have IQ} \leq 150] \\= 1 - Pr[\text{Average kid has IQ} \leq 150] ^ {kids}$$

Rearranging this a bit, we get:

$$kids = \log_{Pr[\text{Average kid has IQ} \leq 150]} 0.5$$

To figure out $$Pr[\text{Average kid has IQ} \leq 150] $$[^1], we first need to figure out the standard deviation of the genetic component of IQ ($$\sigma_g$$) and the environmental component of IQ ($$\sigma_e$$). We can start with the equation for heritability, which is the proportion of the total variance of a trait explained by genetics:

[^1]: I know technically speaking it should be $$Pr[\text{Average kid has IQ} < 150] $$ instead of $$Pr[\text{Average kid has IQ} \leq 150] $$, but the later makes it easier to use the normal distribution CDF.

$$h = \frac{v_g}{v_p}$$

$$0.6 = \frac{v_g}{15 ^ 2}\\ v_g = 135 \\ \sigma_g = \sqrt{v_g} = \sqrt{135}$$


$$v_e = v_p - v_g = 15^2 - 135 = 90 \\ \sigma_e = \sqrt{v_e} = \sqrt{90}$$

To figure out the probability that their average kid has an IQ less than 150, we first need to figure out how many standard deviations above average on genetic ($$z_g$$) and environmental factors ($$z_e$$) the kid would need to be in order to have an IQ above 150. Basically, how lucky is the genius kid?

$$150 = (\dfrac{150 + 130}{2} + \sqrt{135} \cdot z_g) \cdot 0.6 + (100 + \sqrt{90} \cdot z_e) \cdot (1 - 0.6)$$

This evaluates to:

$$z_e \approx 6.8516 - 1.83712 z_g$$

The probability that their average kid has an IQ over 150 is the sum of $$P(Z > z_g) \cdot P(Z > z_e)$$[^2] over all values of $$z_g$$ and the corresponding value of $$z_e$$. Which means:

[^2]: $$P(Z < x)$$, aka $$\phi(x)$$, is the cumulative density function of the normal distribution, explained [here](https://www.probabilitycourse.com/chapter4/4_2_3_normal.php).

$$
Pr[\text{Average kid has IQ} >150] =\\ \int_{-\infty}^{\infty} (1 - P(Z \leq z_g)) \cdot (1 - P(Z \leq 6.8516 - 1.83712 z_g)) dz_g \\ \approx 0.00016
$$

$$
Pr[\text{Average kid has IQ} \leq 150] \approx 1- 0.00016 = 0.99984
$$

Using the formula we derived near the start, we can finally figure out how many kids you need to have in order to produce a super-genius:

$$
kids = \log_{0.99984} 0.5 \approx 4362
$$

We can see how many kids Erasmus Darwin would need to have with his wife in order to produce different probabilities of having a super-genius kid in this graph:

![num_kids_to_prob](/num_kids_to_prob_supergenius.png)

## Conclusions

4362 is a lot of kids - enough to put Genghis Khan to shame. Here are the implications:

1. Scott's claim that you can solve the problem of regression to the mean by "really impressive assortative mating and having vast litters of children" seems implausible. Here we assume that a 150 IQ man was able to find a 130 IQ wife (the average of an Ivy League college), and even still the couple would need to have as many children as a fecund frog in order to reproduce the father's intelligence.
2. Scott's claim might be correct if there are extremely strong environmental effects. I assumed above that the IQ contribution from the environment is the same as the population's average IQ (100). But this is likely a significant underestimate for the British aristocracy a century or two ago. They were probably providing their kids with private tutoring (see [2 sigma problem](https://www.wikiwand.com/en/Bloom%27s_2_sigma_problem)), good nutrition, and of course the "hero mindset" which Scott explains later in the post.
3. We should be less worried about Charles Murray's arguments in *Coming Apart* about a growing cognitive divide between the social classes in America. Unless environmental effects are really strong, regression to mean is an overwhelming force. 


