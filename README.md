# MCMC Overview

## Definition:
Markov Chain Monte Carlo (MCMC) is a class of algorithms for sampling from a probability distribution by constructing a Markov Chain

## Intuition:
MCMCs "wander around" a lumpy surface (e.g. a probability density function), spending time in an area proportional to its height, and thus infers the target PDF without needing to know the exact height

![This is an image of a person "wandering around" a lumpy surface (e.g. a probability density function)](mcmc_graphic.jpg)

## Use Cases:
- Estimate the parameters of a probability distribution or equation (e.g. estimating mu and sigma of a Normal distribution, or slope and intercept of a line)
- 

## High Level Steps:
1. Graph your data and formulate the general equation that fits the shape of the data (! finding the right equation is important to using it later to make accurate predictions). You will not know the specific parameters of the equation - you'll use MCMC to discover these.

![This is an image of a scatter plot with a linear trend](linear_ex.PNG)
![This is an image of a distribution plot with a bimodal bell curve shape](multimodal_ex.PNG)

2. 




## Metropolis Hastings Sampling Algorithm Steps:
This specific sampling algorithm is appropriate for symmetric and non-symmetric distributions, is simple to implement, and can generally be applied to a variety of high dimensional complex problems. Other possible sampling algorithms include: Gibbs Sampling, ensemble sampling, parallel tempering, adaptive MCMC, Hamiltonian Monte-Carlo, and Reversible Jump MCMC. 

1. Choose Proposal Distribution (in order to move around parameter space & calculate the transition probabilities of moving from one point in parameter space to another)
2. Choose random state to begin with
3. Choose the next candidate state
4. Calculate the Acceptance Probability (called Hastings Ratio for the metropolis hastings mcmc) to determine if you should accept or reject the next candidate state. If A.P. > 1 then go to next state. If not, then stay in current state
5. Repeat many times
6. Find target posterior distribution, which is proportional to the MCMC samples


## Resources:
- https://jellis18.github.io/post/2018-01-02-mcmc-part1/ (python)
- https://youtu.be/XRfmdP5Gavs
- https://youtu.be/yCv2N7wGDCw
- https://bayesball.github.io/BOOK/simulation-by-markov-chain-monte-carlo.html (R)
- https://towardsdatascience.com/mcmc-intuition-for-everyone-5ae79fff22b1 (python)
- https://prappleizer.github.io/Tutorials/MCMC/MCMC_Tutorial.html (python)
- https://twiecki.io/blog/2015/11/10/mcmc-sampling/ (python)
