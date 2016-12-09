## Question 1
Which of the following statements is true?
- The likelihood is a mixture between the prior and posterior.
- The posterior is a mixture between the prior and likelihood.
    Above is true. 
- The prior is a mixture between the posterior and likelihood.

## Question 2 
Which of the following distributions would be a good choice of prior to use if you wanted to determine if a coin is fair when you have a strong belief that the coin is fair? (Assume a model where we call heads a success and tails a failure).
- Beta(9, 1)
- Beta(10, 90)
- Beta(50, 50)
    Above. Equal alpha and beta give probability centered on 0.5 and higher values of alpha and beta give steeper dropoff away from 0.5
- Beta (10, 10)
- Beta (1, 1)

## Question 3 
If Amy is trying to make inferences about the average number of customers that visit Macy’s between noon and 1 p.m., which of the following distributions represents a conjugate prior?
- Poisson
- Normal
- Gamma
    Gamma makes sense. Poisson represents the distribution for the data (counts) and gamma is the probability distribution that goes with poisson data.
- Beta

## Question 4 
Suppose that you sample 24 M&Ms from a bag and find that 3 of them are yellow. Assuming that you place a uniform Beta(1,1) prior on the proportion of yellow M&Ms p, what is the posterior probability that p < 0.2 ?

Given x successes in n trials, posterior distribution has alpha =  alpha + x and beta = beta + n - x
Posterior:  alpha = 1 + 3 = 4
            beta  = 1 + 24 - 3 = 22
Probability less than 0.2: 
    pbeta(0.2, 4, 22)
    0.766 
0.60
0.69
0.77
    Above.
0.92

## Question 5 
Suppose you are given a coin and told that the coin is either biased towards heads (p = 0.6) or biased towards tails (p = 0.4). Since you have no prior knowledge about the bias of the coin, you place a prior probability of 0.5 on the outcome that the coin is biased towards heads. You flip the coin twice and it comes up tails both times. What is the posterior probability that your next two flips will be heads?
P(headbias | two tails) = P(headbias) * P(two tails | head bias) / P(two tails) 
    Numerator = 0.5 * (0.4 * 0.4) = 0.08
    Denominator = 0.5 * (0.4 * 0.4) + 0.5 * (0.6 * 0.6) = 0.26
    Result = 4/13

    (P(heads | headbias) * 4/13 + P(heads | tailsbias) * (9/13)) ^ 2
    = 0.222

0.2
0.212
0.222
    Above
0.25
