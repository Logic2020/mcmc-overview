# MCMC Overview

## Definition & Background:
Markov Chain Monte Carlo (MCMC) is a class of algorithms for sampling from an unknown, desired probability distribution (also called target posterior distribution) by constructing a Markov Chain. The intent of the algorithm is to determine a probability density function (PDF) that allows us to draw samples from the unknown, desired distribution, by using a known PDF that is proportional to the desired distribution.

Finding the exact PDF of the unknown, desired distribution through mathematics and integration can be difficult and relies on having a normalization factor. Since this approach can be extremely difficult in practice, MCMC allows us to derive a PDF by taking many many samples where the distribution of the sample values more closely approximates the unknown, desired distribution.

The MCMC algorithm iteratively adjusts the estimated distribution parameters to yield the best match between the observed data and proposed distribution/equation. The output provides a set of parameter values for the target posterior distribution, and a distribution of likely values for each parameter.

Markov Chains are uniquely defined by the Markov Property - where the probability of transitioning to a new state (the transition probability) is dependent only on the current state and not the past sequence of states. As the Markov process iterates many times, it reaches a stationary state, meaning the probability of the next state will eventually converge to equals that of a prior state. The stationary state allows you to define the probability of every state at any point in time.

## Intuition Behind Algorithm:
MCMCs "wander around" a lumpy surface (e.g. a probability density function), spending time in an area proportional to its height, and thus infers the target PDF without needing to know the exact height.

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

### _Initialization_ ###
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

### _Iterations_ ###
**4. Generate next parameter values (next state) by randomly sampling a value for each parameter from the Proposal Distribution you defined in Step 1**
A very simple way to generate a new position x* from a current position xi for a Normal(0,1) proposal distribution is to add a N(0,1) random number to xi -->
x* = xi + (random number from N(0,1))

**5. Determine if the next state shoud be accepted or rejected by first calculating the Acceptance Probability (also called Hastings Ratio or Acceptance Probability)**
The Hastings Ratio is defined as 
![This is an image of the MCMC Hastings Ratio formula](hastings Ratio_v2.PNG)
and similarly in more layperson's terminology
![This is an image of the MCMC Hastings Ratio formula](hastings Ratio.PNG)
where p(⋅) and g(⋅) are probability density values. p(⋅) stands for the unknown target distribution, while g(⋅) stands for the proposal distribution.
When the proposal distibution is symmetric (e.g. Normal distribution) the g(⋅) ratio solves to 1. 



a component of the targetposterior, then the Metropolis-Hastings ratio is the likelihood ratio.

- the ratio of the target posteriors ensures that the chain will gradually move to high probability regions
- the ratio of the proposal probabilities ensures that the chain is not influenced by “favored” locations in the proposal distribution function
- 

**6. Then generate a randomly generated number *u* from Uniform(0,1). If u < min(1, A.P.) then proceed with proposed parameters (also called allowing a "jump" or "advancing the chain"). If not, then stay on the current state parameters**
If _u_ <= Ratio then the jump will be accepted and the chain advances to the next state.
If _u_ > Ratio then the jump will be rejected and the chain stays at the current state.

In some cases, the density values may be very large or very small and cause a numeric overflow or an underflow. An alternative to the traditional Accept/Reject formulation is to apply logs-->
log(_u_) <= log( p(new) ) - log( p(old) ) + log( h(old) ) - log( h(new) )

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
- https://umbertopicchini.wordpress.com/2017/12/18/tips-for-coding-a-metropolis-hastings-sampler/ (R)
