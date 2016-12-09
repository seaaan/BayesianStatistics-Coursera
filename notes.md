# Bayesian statistics

Part of the specialization "Statistics with R" from Duke

## Week 1: Introduction

### Lecture: Bayes' Rule

Conditional probability: Bayes' Rule = Probability of A given B = P(A | B) = P(A & B) / P(B)
	E.g. probability of using online dating service given age 30-49 is the number of people in that age range who report using the services divided by the total number of people in that age group, so A = using the service and B = in the age range, so A & B means use the service and in the age range

HIV example: 
	Early testing in the US military
	ELISA sensitivity (true positive, P(+ | HIV)) 93
		false positive P(- | HIV) 7
	ELISA specificity (true negative, P(- | no HIV) 99
		false negative P(+ | no HIV) 1 
	Blot  sensitivity 99.9
	Blot  specificity 99.1
	prevalence 1.48/1000 

	prior probability = P(HIV) = 0.00148

	What is the probability of having HIV given a positive ELISA test? 
		P(HIV | ELISA+): P(HIV & ELISA+) / P(ELISA+)
				(0.00148 * 0.93) / (top + bottom)
				12%

		P(no HIV | ELISA+): P(no HIV & ELISA+) / P(ELISA+)
				(0.99852 * 0.01) / (top + bottom)
				88%	
		where (top + bottom) is the numerator for each probability because the probability of a positive test result is the sum of the probabilities of the test result given both possible statuses

Bayesian updating: 
What is the probability of having HIV if the second ELISA also yields a positive result? 
Now the prior probability of the hypothesis that they have HIV is 12% (the posterior probability of the previous test). The rest of the calculation is the same. The posterior probability after the second ELISA is then 93%
- In this example, you only updated the prior, not the sensitivity and specificity (because you did the same diagnostic)
- When you switch to western blot, you also change the sensitivity and specificity, in addition to the prior

Bayesian vs. frequentist definitions of probability
Frequentist: probability of an event is the proportion of the event occurring out of n trials as n goes to infinity
Bayesian: probability of an event is equal to the probability you think the event has

Frequentist confidence intervals are conducted given the probability that 95% of confidence intervals will capture the true population value, but don't say anything about the given sample
Bayesian credible intervals allow describing the true parameter with a probability distribution, not with a fixed value, and then to say it's 95% likely to be in this interval

## Inference for a proportion: Frequentist vs. Bayesian
4/20 become pregnant in treatment group, 16/20 become pregnant in control group, does treatment work?
p = probability that a given pregnancy comes from the treatment group
Null hypothesis: p = 0.5 (equally likely that pregnancy comes from either group)
Alternative hypothesis: p < 0.5 (pregnancy is less likely to come from the treatment group)
4/20 pregnancies are in treatment group, calculate with binomial distribution, p -> 0.006

Inference for a proportion: Bayesian approach
Consider 9 models: p = 0.1, 0.2, ..., 0.9
Specify prior probability (state of belief prior to current experiment)
	For example, use prior of 0.52 for p = 0.5 and then equally distribute prior of 0.06 to each other model
Calculate P(data | model), also known as the likelihood
	P(data | model) = P(k = 4 | n = 20 & prior for model)
	p <- seq(0.1, 0.9, by = 0.1)
	prior <- c(rep(0.06, 4), 0.52, rep(0.06, 4))
	likelihood <- dbinom(4, size = 20, prob = p)
		 == P(data | model)
	
	Use Bayes rule to calculate posterior probability: P(model | data)
	P(model | data) = P(model & data) / P(data)
			= P(data | model) * P(model) / P(data)
			= likelihood      * prior / P(data)

	numerator <- prior * likelihood
	denominator <- sum(numerator) # all possibilities data might be coming from
	posterior <- numerator / denominator

	posterior probabilities sum to 1
	posterior probability for p = 0.2 is highest, despite low prior probability
	posterior probability for p = 0.5 dropped to 7.8% from 52% based on the data
	"data as or more extreme than what was observed" plays no role

Bayesian paradigm allows you to make direct probability statements about your models
	E.g. probability treatment is more effective than control = sum of probabilities of models with p < 0.5 = 92%

Effect of sample size on the posterior
	Previous experiment: n = 20, k = 4
	What if we do n = 40, k = 8? The likelihood decreases slightly for p = 0.2, but dramatically for all the other values because the binomial probability depends on sample size. This drives the posterior probability to be even more dominated by the likelihood
	In other words, likelihood dominates the prior as sample size increases (as long as no prior is 0)

Frequentist vs. Bayesian inference
Are 10 or 20% of your M&Ms yellow? Each M&M costs $200 and you can buy 5, 10, 15, or 20 M&Ms total. Assume you buy 5: 

Frequentist procedure
	Null hypothesis =  10% yellow M&Ms P(k >= 1 | n = 5, p = alpha)
	Alternative     = >10% yellow M&Ms
	alpha = 0.05 (but higher significance rate might be worth it to minimize type 2 error)
	Sample = RGYBO
	k = 1, n = 5
	probability of k >= 1 is 1 - P(k = 0 | n = 5, p = 0.10)
		if frequency of yellow is 0.1, then the above probability is 1 - 0.9^5 because you picked 5 non-yellows
	p = 0.41, i.e. can't reject null hypothesis, so pick 10% yellow

Bayesian
	H1 = 10% yellow
	H2 = 20% yellow
	(can test both hypotheses rather than only one and not the other, like above)
	Assume equal priors of 0.5
	Binomial probability of 1/5 with model of 0.1 is 0.33, with model of 0.2 is 0.41 = likelihood
	
	To calculate posterior, divide likelihood by sum of 0.33 and 0.41, giving 0.55 posterior for model 0.2
With n = 5-20, p-values always are above 0.05, so we'd always pick the 10% model
	For Bayesian, posterior is always higher for the 20% model, and gets stronger with more data
	However, if we'd set up null hypothesis in reverse for frequentist, it would always pick correctly. For Bayesian, there is no reverse way to do it, which is an advantage	

## Quiz 1
Julia is having an outdoor wedding ceremony tomorrow. In recent years, it has rained on average 50 days per year. Unfortunately, the meteorologist has predicted rain for her wedding day. When it rains, the meteorologist will have correctly predicted it 80 percent of the time. When it does not rain, the meteorologist will have incorrectly predicted rain 30 percent of the time. Given this information, what is the probability that it rains on Julia's wedding day?

P(rain) = 50/365
P(+pred | rains) = 0.8
P(+pred | no rain) = 0.3

P(rain | +pred) = P(rain & +pred) / P(+pred)
	= (50/365 * 0.8) / (50/365*0.8 + 315/365*0.3) 
	= 29.7%

Suppose we have two hypotheses, H0 and H1. Assuming our prior places equal weight on H0 and H1, which of the following statements is false?

* If the p-value is less than .05, the probability that we see data at least as extreme as our observed data is less than .05, given that H0 is true.

* FALSE If the posterior probability of H0 is less than .05, the p-value under H0 will also be less than .05.

* If the cost of making a type-I error is the same as the cost of making a type II error and the posterior probability of H1 is greater than the posterior probability of H0, we should reject H0. 

Suppose 20 people are randomly sampled from the population and their sex is recorded. Which of the following best represents the likelihood of the number of males observed k?

* The probability of at least k males in 20 samples, given p, the true population proportion of males.

* THIS ONE The probability of observing exactly k males in 20 samples, given p, the true population proportion of males.

* The probability of observing exactly k males in 20 samples, given p , the true population proportion of males, and the prior beliefs about p .

* The probability of observing exactly k males in 20 samples, given the prior beliefs about the population proportion p.

Which of the following statements is consistent with both Bayesian and frequentist interpretations of probability?

* Probability can be represented by a degree of belief, which changes as more data are collected.

* Probability is the tendency of an experiment to produce a certain outcome, even if it is performed only once.

* THIS ONE Probability is a measure of the likelihood that an event will occur.

* Probability can be represented by the long-run frequency of an event divided by the number of trials.

You are told that either a coin has either a strong tails bias (p = 0.2), a weak tails bias (p = 0.4 ), no bias (p = 0.5), a weak heads bias ( p = 0.6), or a strong heads bias ( p = 0.8). You assign a prior probability of 1/2 that the coin is fair and distribute the remaining 1/2 prior probability equally over the other four possible scenarios. You flip the coin three times and it comes up heads all three times. What is the posterior probability that the coin is biased towards heads?

models <- c(0.2,   0.4,   0.5, 0.6,   0.8)
priors <- c(0.125, 0.125, 0.5, 0.125, 0.125)
likelihoods <- dbinom(3, size = 3, models)

numerator <- priors * likelihoods
denominator <- sum(numerator)

posteriors <- numerator / denominator
sum(posteriors[4:5])

0.56
You draw two balls from one of three possible large urns, labelled A, B, and C. Urn A has 1/2 blue balls, 1/3 green balls, and 1/6 red balls. Urn B has 1/6 blue balls, 1/2 green balls, and 1/3 red balls. Urn C has 1/3 blue balls, 1/6 green balls, and 1/2 red balls. With no prior information about which urn your are drawing from, you draw one red ball and one blue ball. What is the probability that you drew from urn C?

	Blue 	Green	Red
A 	1/2	1/3	1/6
B	1/6	1/2	1/3
C	1/3	1/6	1/2

n = 2, red = 1, blue = 1
A: 1/6 * 1/2 = 1/12
B: 1/6 * 1/3 = 1/18
C: 1/3 * 1/2 = 1/6

P(C | data) = (1/6) / (1/12 + 1/18 + 1/6) = 54.5%

2 > The probability of observing the data, given μ, σ2.

You go to Las Vegas and sit down at a slot machine. You are told by a highly reliable source that, for each spin, the probability of hitting the jackpot is either 1 in 1,000 or 1 in 1,000,000, but you have no prior information to tell you which of the two it is. You play ten times, but do not win the jackpot. What is the posterior probability that the true odds of hitting the jackpot are 1 in 1,000?

dbinom(0, size = 10, 1/1000)
#> 0.9900449
dbinom(0, size = 10, 1/1000000)
#> 0.99999

0.9900449 / (0.9900449 + 0.99999)
#> 0.4975

4 > False

5 > If conditions identical to tomorrow occurred an infinite number of times, we would observe rain on 30 percent of those days.

6 > The probability that μ falls within the interval is 0.95

Hearing about your brilliant success in working with M&Ms, Mars Inc. transfers you over to the Skittles department. They recently have had complaints about tropical Skittles being mixed in with original Skittles. You decide to conduct a frequentist analysis. If the findings suggest that more than 1% of sampled supposedly original skittles are actually tropical, you will recommend action to be taken and the production process to be corrected. You will use a significance level of α=0.1. You randomly sample 300 supposedly original skittles, and you find that five of them are tropical. What should be the conclusion of your hypothesis test? Hint- H0:p=0.01 and H1:p>0.01

	Null hypothesis =  1% tropical skittles P(k > 3 | n = 300, p = alpha)
        Alternative     = >1% tropical
        alpha = 0.1
        k = 5, n = 300
	P(data | model) = 0.101 via dbinom
        probability of k >= 5 is 1 - sum of P(k = 0:4 | n = 300, p = 0.01)
		via dbinom, get 0.049 + 0.1486 + 0.2244 + 0.2251 + 0.186 = 0.816
		so, probability of k >= 5 is 1 - 0.816 = 0.184
		p = 0.184, can't reject hypothesis that p = 0.01

You decide to conduct a statistical analysis of a lottery to determine how many possible lottery combinations there were. If there are N possible lottery combinations, each person has a 1/N chance of winning. Suppose that 413,271,201 people played the lottery and three people won. You are told that the number of lottery combinations is a multiple of 100 million and less than 1 billion, but have no other prior information to go on. What is the posterior probability that there were fewer than 600 million lottery combinations?

combinations <- 1:9 * 1e8

likelihoods <- dbinom(3, 413271201, 1 / combinations)

posteriors <- likelihoods / sum(likelihoods)

sum(posteriors[1:5])
#> 89.38

You are testing dice for a casino to make sure that sixes do not come up more frequently than expected. Because you do not want to manually roll dice all day, you design a machine to roll a die repeatedly and record the number of sixes that come face up. In order to do a Bayesian analysis to test the hypothesis that p = 1/6 versus p = .175 , you set the machine to roll the die 6000 times. When you come back at the end of the day, you discover to your horror that the machine was unable to count higher than 999. The machine says that 999 sixes occurred. Given a prior probability of 0.8 placed on the hypothesis p = 1/6 , what is the posterior probability that the die is fair, given the censored data? Hint - to find the probability that at least x sixes occurred in N trials with proportion p (which is the likelihood in this problem), use the R command : 1-pbinom(x-1,N,p)
rm

models <- c(1/6, 0.175)
priors <- c(0.8, 0.2)
likelihoods <- 1-pbinom(998, 6000, models)

numerators <- likelihoods * priors
denominator <- sum(numerators)

numerators / denominator

#> 0.684 (but doesn't take into account censoring)

10 > TRUE

## Week 2: Bayesian inference
Continuous variables vs discrete values
Binomial variable like number of heads is discrete because it can only take certain values, 1, 2, 3...
    Probability mass function = histogram with probabilities for each possible value, AUC = 1
Continuous variable can take any numerical value within an interval
    0 probability of any given value
    Can talk about the probability of lying in an interval
    Probability density function instead of probability mass functions
    Density curve instead of histogram

Distributions:
    Continuous: normal, uniform, beta, gamma
    Discrete: binomial, poisson

### Lecture: Elicitation
Expectations about where your data come from and personal beliefs about the mean or probability
    Must obey all laws of probability and be consistent with everything you know

Beta distributions are specified by two parameters, alpha and beta, passed into gamma functions
    uniform distribution when alpha = beta = 1
    alpha = beta center on 0.5 with increasing height as alpha and beta increase
    alpha < beta give higher probabilities below 0.5
    alpha > beta give higher probabilities above 0.5

    mean of beta is alpha / (alpha + beta)

If you observe enough data, different priors will converge to the same posterior data regardless of the prior.

### Lecture: Conjugacy
Conjugacy = posterior distribution is in the same familiy as your prior distribution, but with new parameter values

Bayes rule for discrete variables can't be used for continuous variables because the denominator sums over all the possible values of the variable, which you can't do for continuous variables. 
    Instead, you integrate over the interval in the denominator instea dof summing the possibilities
    The numerator is the probability of your data given a particular probability multiplied by the density function for the probability
    
### Lecture: Inference on a binomial proportion
RU-486 trial of 800 women who reported unprotected vaginal intercourse in previous 72 hours, 50% random allocation to standard therapy and 50% to RU-486. 4 pregnancies in standard, 0 pregnancies in RU-486. 

Binomial distribution with known n and unknown p, but belief that the p is a beta function. If you observe x successes in n trials, your posterior for p will be another beta distribution with parameters alpha + x and beta + n - x. (Derives from bayes rule)

Bayesian analysis: say have no prior knowledge of drug, so prior = uniform density = beta(1, 1)
    posterior probability = beta(1+0, 1-0+4) = beta(1, 5)
    p = mean = 1 / (1+5) = 1/6
    posterior probability that p < 1/2 is 0.96875

### Lecture: Gamma poisson 
Data from a poisson distribution (any integer from 0 to infinity)
    discrete
    lambda = mean = variance for poisson distribution
    probability mass function = (lambda^k / k!)exp(-lambda), summed for each k in the interval of interest
Prior/posterior probabilities from gamma distribution
    continuous    
    Gamma distribution = continuous non-negative values
    probability distribution function  
        parameters: k, theta
        mean = k*theta
        sd   = theta*sqrt(k)
    updates to parameters given new data
        new data = x1, ..., xn
            where each x is a number of counts
        k     = k + sum(x, ..., xn) 
        theta = theta / (n * theta + 1) 

### Lecture: Normal
Parameters = mean, standard deviation
Assume prior for mean is mean v with standard deviation tao
    data x1, ..., xn are independent
    come from a normal distribution with known standard deviation sigma
    given the new data, updated v is: 
        v = (v * sigma^2 + n*mean(x1, ..., xn) * tao^2 ) 
           --------------------------------------------
                    (sigma^2 + n * tao^2) 
    updated tao is
        tao =             sigma^2 * tao^2   
                sqrt( ------------------------- )
                        sigma^2 + n * tao^2

### Lecture: Non-conjugate priors
Example: Prior for Ru-486 could be that probability that RU-486 is better than standard therapy is 1, but don't know how much better. This can be expressed as p (probability that a birth is from the RU-486 arm of the trial) having a uniform distribution between 0 and 0.5.

The posterior from such a non-conjugate prior can't be calculated mathematically, must be computed. 
    - JAGS (Just Another Gibbs Sampler) can be used to compute the posterior from the prior

### Lecture: Credible intervals
Point estimate plus or minus the standard error times a critical value
Bayesian: The probability the true mean is contained within the given interval is 0.95
    Any interval such that the posterior probability within the interval is 0.95
    Desirable to find the shortest such interval, which can be done mathematically

### Predictive inference
Predictive inference = goal to find a posterior distribution about a variable that depends on a parameter, rather than about the parameter itself
    You have a probability density function for the variable given the parameter and you have a probability for the parameter

Example: two coins, probability of heads for them is 0.7 and 0.4, draw a coin randomly, get two heads, what is probability that you have the 0.7 coin?
    -> 0.754
    What is the probability that the next toss will come up heads? 
    P(heads | 0.7) * 0.754 + P(heads | 0.4) * (1 - 0.754)
    Don't need integration in this case because the possible values of the parameter are not infinite, just two possibilities
