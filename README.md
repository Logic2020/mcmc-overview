# MCMC Overview

## Definition & Background:
Markov Chain Monte Carlo (MCMC) is a class of algorithms for sampling from an unknown, desired probability distribution (also called target posterior distribution) by constructing a Markov Chain. The intent of the algorithm is to determine a probability density function (PDF) that allows us to draw samples from the unknown, desired distribution, by using a known PDF that is proportional to the desired distribution.

Finding the exact PDF of the unknown, desired distribution through mathematics and integration can be difficult and relies on having a normalization factor. Since this approach can be extremely difficult in practice, MCMC allows us to derive a PDF by taking many many samples where the distribution of the sample values more closely approximates the unknown, desired distribution.

The MCMC algorithm iteratively adjusts the estimated distribution parameters to yield the best match between the observed data and proposed distribution/equation. The output provides a set of parameter values for the target posterior distribution, and a distribution of likely values for each parameter.

## Intuition Behind Algorithm:
MCMCs "wander around" a lumpy surface (e.g. a probability density function), spending time in an area proportional to its height, and thus infers the target PDF without needing to know the exact height. As more and more sample values are produced, the distribution of values more closely approximates the desired distribution.

![This is an image of a person "wandering around" a lumpy surface (e.g. a probability density function)](mcmc_graphic.jpg)

## Use Cases of MCMC:
- **Parameter Estimation** of a probability distribution or equation (e.g. estimating mu and sigma of a Normal distribution, or slope and intercept of a line)

## High Level Steps, in Practice:
1. Graph your data and formulate the general equation that fits the shape of the data (! finding the right equation is important to using it later to make accurate predictions). You will not know the specific parameters of the equation - you'll use MCMC to discover these.

![This is an image of a scatter plot with a linear trend](linear_ex.PNG)
![This is an image of a distribution plot with a bimodal bell curve shape](multimodal_ex.PNG)

2. 



## Metropolis Hastings Sampling Algorithm Steps:
This specific sampling algorithm is appropriate for symmetric and non-symmetric distributions, is simple to implement, and can generally be applied to a variety of high dimensional complex problems. Other possible sampling algorithms include: Gibbs Sampling, ensemble sampling, parallel tempering, adaptive MCMC, Hamiltonian Monte-Carlo, and Reversible Jump MCMC. 

**1. Choose Proposal Density Function (also called Proposal Distribution of Jumping Proposal)**
The Proposal Distribution is needed in order to move around parameter space & calculate the transition probabilities of moving from one point in parameter space to another. 

This density function is used to suggests candidate parameters for the next state, given the current state parameters:
x* ~ q(x* | xi),
where xi is the current state of distro parameters and x* is the next state of distro parameters

For each iteration of the algorithm, we draw a proposed parameters x*.

The Proposal Distribution allows us to move around the unknown parameter space and specify the probability of moving to point xi+1 in the parameter space given that we are currently at xi.

This is ultimately helping us determine the target posterior distribution parameters (e.g. the histogram of the MCMC samples produces the target posterior distribution)

***Ideal Proposal Distribution:***
The chain will converge to the target distribution if the transition probability is:
- irreducible: From any point in parameter space, we must be able to reach any other point in the space in a finite number of steps.
- positive recurrent: For any point in parameter space, the expected number of steps for the chain to return to that point is finite. This means that the chain must be able to re-visit previously explored areas of parameter space.
- aperiodic: The number of steps to return to a given point must not be a multiple of some value k. This means that the chain cannot get caught in cycles.

The proposal probability density function needs to be proportional to the target posterior PDF, but not necessarily connected to a choice of prior or likelihood in a Bayesian model. So using a prior distro for the proposal distro may not be the most efficient choice. Gaussian with fixed standard deviation is a very efficient proposal distribution to use.


**2. (Optional) Specify prior ranges for the parameters**
Why allow the chain to spend time exploring areas that you know are not possible?


**3. Choose arbitrary intial parameter values to begin with (inital state) -> you can randomly generate a value for each parameter using a uniform continuous distribution**

**4. Generate next parameter values (next state) by randomly sampling a value for each parameter from the Proposal Distribution you defined in Step 1**

**5. Determine if the next state shoud be accepted or rejected by first calculating the  Acceptance Probability (also called Hastings Ratio)**
The Hastings Ratio is defined as 
![This is an image of the MCMC Hastings Ratio formula](hastings Ratio.PNG)
where p(⋅) and h(⋅) are probability densities. p(⋅) stands for the distribution you want to simulate from (target posterior distribution) but cannot, while h(⋅) stands for the distribution you can simulate from (proposal distribution) and thus did simulate your next states from.
When the proposal distibution is a component of the targetposterior, then the Metropolis-Hastings ratio is the likelihood ratio.


The probability of transitioning to the next candidate state, given the current state is a function of the ratio of the value of the posterior at the new point to the old point and the ratio of the transition probabilities at the new point to the old point.

**6. Then generate a randomly generated number *u* from Uniform(0,1). If u < min(1, A.P.) then proceed with proposed parameters (also called allowing a "jump" or "advancing the chain"). If not, then stay on the current state parameters**
- if this ratio is > 1 then the jump will be accepted (i.e. the chain advances to the next state)
- the ratio of the target posteriors ensures that the chain will gradually move to high probability regions
- the ratio of the transition probabilities ensures that the chain is not influenced by “favored” locations in the proposal distribution function


**7. Posterior predictive checking: calculate log likelihood for each state**

**8. Repeat many times**

**9. Find target posterior distribution, which is proportional to the MCMC samples**


## Resources:
- https://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm#cite_ref-9
- https://jellis18.github.io/post/2018-01-02-mcmc-part1/ (python)
- https://youtu.be/XRfmdP5Gavs
- https://youtu.be/yCv2N7wGDCw
- http://modernstatisticalworkflow.blogspot.com/2017/05/model-checking-with-log-posterior.html
- https://towardsdatascience.com/mcmc-intuition-for-everyone-5ae79fff22b1 (python)
- https://prappleizer.github.io/Tutorials/MetropolisHastings/MetropolisHastings_Tutorial.html (python)
- https://prappleizer.github.io/Tutorials/MCMC/MCMC_Tutorial.html (python)
- https://twiecki.io/blog/2015/11/10/mcmc-sampling/ (python)
- https://bayesball.github.io/BOOK/simulation-by-markov-chain-monte-carlo.html (R)
