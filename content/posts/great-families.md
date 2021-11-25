---
title: "Draft: 4,362 kids to have a great family"
date: 2021-11-19
draft: false
katex: true
markup: "mmark"
---

Please email me at dwarkesh.sanjay.patel@gmail.com if you figure out that I've fucked up the math :). It wouldn't surprise me since genetics or statistics aren't my thing, and I would really appreciate the correction.

## Introduction

Scott Alexander recently wrote a really interesting blog [post](https://astralcodexten.substack.com/p/secrets-of-the-great-families) about why some families manage to produce super-impressive people generation after generation. He asks how genetic advantages like IQ can persevere through generations given regression to the mean:

> Suppose Erasmus Darwin had a genius-level IQ of 150. And suppose that he tried very hard to marry a bright woman, and his screening mechanism was as successful as the Ivies or Oxbridge when they screen for bright people - in that case his wife would have the same IQ as an average Ivy Leaguer, maybe 130ish. We would predict their average child to have an IQ of 124. Why is the average lower than either parent? Regression to the mean - IQ is probably a combination of genes and random factors, and if your IQ is very high it means you probably have a combination of good genes and good random dice rolls, and even though you can pass on the good genes your kids will probably only get average dice rolls.
> 
> But 124 isn’t even as high as the average Ivy Leaguer. If that IQ 124 kid marries another IQ 130 spouse, things deteriorate less quickly - I think the regression process has selected for his 24 IQ point advantage over average being entirely genetic, so it’s not going to regress further. But his spouse’s IQ can still regress further, so they’ll probably end up with a kid who has an IQ somewhere in the high 110s or low 120s - for comparison, not much higher than the average Ashkenazi Jew. Maintaining a super-high-IQ family over several generations is really hard! ...
> 
> The answer to [this problem] ... is really impressive assortative mating and having vast litters of children.

## Math

I wanted to crunch the numbers to see how difficult it is to have an incredibly brilliant child. Assuming you are yourself a super-genius, is it really as easy as marrying a decently smart partner and siring half a dozen potential intellectual heirs?

### Setup

By reverse engineering Scott's calculation, we can see that he's using 0.6 for the heritability of IQ ($$h$$):

$$124 = \dfrac{150 + 130}{2} \cdot h + 100 (1 - h)$$

$$h = 0.6$$

We want to figure out how many kids a 150 IQ super-genius like Erasmus Darwin would need to have with his 130 IQ wife in order to have a 50% chance of producing another 150 IQ super-genius. We do that by solving for $$kids$$ in the following equation.

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

Using the formula we derived near the start, we can finally figure out **how many kids a super-genius needs to have in order to have at least a 50% chance of producing another super-genius**:

$$
kids = \log_{0.99984} 0.5 \approx 4362
$$

### Results

#### Kids needed to produce a super-genius

We can see how many kids Erasmus Darwin would need to have with his wife in order to produce different probabilities of having a super-genius kid in this graph:

![kids_prob](/kids_needed_for_prob.png)

If even Erasmus Darwin needs to have 4362 kids in order to have coin-flip odds of producing another super-genius, then this really does a number to the genetic explanation for the success of certain families.

But hang on a minute, we made certain assumptions about the heritability of intelligence, the limits of assortative mating, and the average contribution to intelligence from random factors that may not be true.

#### Heritability of IQ

SBF wrote a comment on Scott's blog post:

> You're not taking all successful people at random, you're selecting for people who have successful families -- so you're probably selecting for people who don't just have high IQ, but for whom it's highly genetic/inheritable rather than random factors.

Scott responds:

> This is true! All my regression-to-the-mean calculations were wrong because of selection bias - since we’re looking specifically at geniuses who we know had talented families, we should assume their intelligence was more genetic than average.

We can see how changing the heritability of IQ[^3] while keeping the average parental IQ at 140 changes the amount of kids you have to have.

![heritability](/kids_needed_heritability.png)

As you would expect, as heritability increases past about 0.4, you need fewer kids to produce super-geniuses. 

But wait, why does it become easier to produce super-geniuses if the heritability is below 0.4 too? My guess is that the chance of tail scores increases when population variance $$v_p$$ is mostly constituted by just variance of genetic factors $$v_g$$ or of random factors $$v_e$$, but I'm not sure. That or I did the calculations for $$\sigma_g$$ and $$\sigma_e$$ wrong.

[^3]: I know that heritability is a population level statistic, so just consider the population to be very successful families.

#### Assortative mating

Scott mentions in the article:

> I said before that if an IQ 150 person marries an IQ 130 person, on average their kids will have IQ 124. But I think most of these people are doing better than IQ 150. I don’t know if Charles Darwin can find someone exactly as intelligent as he is, but let’s say IQ 145.

Modifying the equations from the setup, we can see that if we increase average IQ of parents to 147.5, then they would still need to have 591 kids in order to sire a super-genius. So it doesn't seem that assortative mating alone would make the concentration of extreme talent in a family very likely.

Here's a graph which shows how increasing average parental IQ changes the amount of kids you would need to have:

![parental_IQ](/kids_needed_parent_IQ.png)

#### Random factors

Scott writes:

> you can pass on the good genes your kids will probably only get average dice rolls.

And I have assumed the same by setting the average IQ contribution from random factors to 100. But this seems wrong. If you're the child of Erasmus Darwin, even the random factors in your life aren't truly random. Think about how much more stuff you're exposed to as a result of being the son or daughter of a great scientist - books, people, opportunities, etc. 

If we assume that Erasmus Darwin's kids gets 3 standard deviations above average of random factors, then the number of kids he needs to have in order to father a super-genius goes down to an entirely reasonable 7!

Here's the graph which shows how the number of kids you would need to have changes as you change the average IQ contribution from random factors, keeping average parental IQ at 140 and heritability of IQ at 0.6:

![random_factors](/kids_needed_random.png)

## Concluding thoughts

### I.

Regression to the mean is a cruel and leveling force. If you're a world famous scientist and you want at least one of your kids to be as smart as you are, you better not settle for some lowly average Harvard graduate with an IQ of 130. You probably need to marry someone who is herself a world famous scientist, or at least smart enough to become one. 

And even that isn't close to enough! You need to make sure that your kids have an environment which is 3 standard deviations above average to have reasonable odds of having a super-genius kid.

To be fair, even Bryan Caplan (a strong proponent of the twin adoption research out of which we've learned just how heritable traits like IQ are) has said that it is possible to [give children an experience that is literally off the charts](https://www.econlib.org/archives/2015/09/why_im_homescho.html) with regards to this research:

> I suspect – though I’m far from sure – that the Caplan Family School is such an exceptional experience that ordinary twin and adoption evidence isn’t relevant.  For example, my sons are plausibly the only 12-year-olds in the nation taking a college class in labor economics.  Perhaps it really will forever rock their worlds.  More obviously, their peer group now includes Robin Hanson, Alex Tabarrok, Tyler Cowen, Garett Jones, and Nathaniel Bechhofer.  That’s plausibly four standard deviations above whatever peer group they’d have in a conventional middle school.

I'm guessing that George Dyson or Cristian Bohr could provide such an exceptional childhood and education to their kids that it would be unmatched by even the best parents of the adopted twins researched in this literature. 

Of course this isn't even to mention that IQ is not the only trait which you need to become a great achiever, and if your kids regress even just a bit in each important trait, they're regressing a ton in their cumulative ability. And since great achievement follows a power law, loosing even a tiny amount of cumulative ability would mean your kids would have drastically lower odds of great achievement.

### II. 

What does this experiment imply socially? 

We should be less worried about Charles Murray's arguments in *Coming Apart* about a growing cognitive divide between the social classes in America. Unless assortative mating or  environmental effects are really strong, regression to mean is an overwhelming force. 

Our calculations do suggest that it may be possible to preserve a concentrated aristocracy of great achievement which only mates with itself, gives its children great advantages, and leaves its merely good offspring to either celibacy or exile. But I think Murray is more worried about cognitive ability splitting entire classes or geographic areas rather than a few great families.

To be fair to Murray, it is possible to have a broad cognitive elite which simply looks down upon rather than towers over the rest of society. Just because you can't have a social class filled with Charles Darwins doesn't mean you can't have a social class filled Ivg League (or equivalent)graduates.

### III.

What does this imply for individuals?

If you're a Nobel prize winning theoretical physicist and you're a woman, you should probably find anoterh theoretical physicist if you want one of your kids to be as smart as you, and you should invite your kids to Nobel laurate 

And if you're a male theoretical physicist? Well, the odds aren't looking too good for you, my friend.

As for  the rest of us, 