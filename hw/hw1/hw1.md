# Homework 1: Slot machines

Two slot machines. Probability of winning = 0.3 or 0.5, equal probability of each machine being the good one. 

What is the probability of machine 1 being the good one given that you win once on machine one? 

P(M1 good | win M1) = P(win M1 | M1 good) * P(M1 good)
                      --------------------------------
                                P(win M1)
    where
    P(win M1) = P(win M1 | M1 good) * P(M1 good) + P(win M1 | M1 bad) * P(M1 bad)
              =      1/2            *   1/2      +      1/3           * 1/2
              = 5/12

    so
P(M1 good | win M1) = 1/2 * 1/2 / 5 / 12 = 12/20 = 0.6

In other words, the probability that machine 1 is good increases slightly given a single win. 

To calculate the probability that machine 1 is bad given that you win on machine 1 you can just subtract 0.6 from 1. Or:

P(M1 bad | win M1) = P(win M1 | M1 bad) * P(M1 bad)
                   --------------------------------
                            P(win M1)
    where
    P(win M1) = P(win M1 | M1 good) * P(M1 good) + P(win M1 | M1 bad) * P(M1 bad)
              = 5/12
    so
    P(M1 bad | win M1) = 1/3 * 1/2 / 5/12
                       = 12/30
                       = 2/5 = 0.4

What about the probability that M2 is good given a win on M1?
P(M2 good | win M1) = P(win M1 | M2 good) * P(M2 good)
                    ----------------------------------
                            P(win M1)

                    = 1/3 * 1/2 / (5/12)
                    = 12/30
                    = 0.4

For the first question, the probabilities are as follows: 
    Posterior - P(M1 is Good | Win on M1) 
    Prior - P(M1 is Good) 
    Likelihood - P(Win on M1 | M1 is Good)

Bayesian updating uses the posterior probability from a previous calculation as the prior probability to the next calculation. It will yield the same result whether you provide the data all at once or one by one.

Given two wins out of two plays on M1 and two wins out of three plays on machine 2, the relative probabilities of each being the good machine are: P(M1 is good | data)=0.571, P(M2 is good | data)=0.429
