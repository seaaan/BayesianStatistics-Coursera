# Bayesian Statistics: From Concept to Data Analysis

## Week 1: Introduction

### Module 1: Probability
Three frameworks for probability: 
    Classical = equally likely outcomes have equal probabilities
                E.g. 1/6 likelihood of rolling a given number
                Difficult to apply if we don't have equally likely probabilities 
    Frequentist = Requires infinite or hypothetically infinite sequence of events, compare relative frequency of events
                E.g. expect 1/6 rolls of a die to be a given face
                Probability of a die being fair, however, doesn't change with infinite rolls, so probability is either 0 or 1 which is unintuitive
    Bayesian = personal perspective, probability represents what you know about a particular problem
                Is this a fair die? Your probability may differ from someone else depending on what you/they know
                One example is the odds you place on an event. 
                    Odds of 1:4 for rain implies that you think the probability of rain is 1/5. 
                    Put in terms of betting, this means you should accept these bets: 
                        if rain, win $4; if no rain, lose $1
                        if rain, lose $4, if no rain, win $1 (flipped probabilities)
                    Expected return = $0
Coherence = probabilities must follow all the rules of probability; if something is incoherent, then it doesn't follow all of the rules of probability and then someone can construct a series of bets where you're guaranteed to lose

### Module 2: Bayes' theorem
Conditional probability: what is the probability of A given B? 
    P(A|B) = P(A&B) / P(B)
    Example: 30 students in a class, 9 females, 12 CS majors (4 female)
            P(F) = 9/30 = 3/10
            P(CS) = 12/30 = 2/5
            P(F&CS) = 4/30
            P(CS|F) = 4/9
            P(F|CS) = P(F&CS) / P(CS) = 4/30 / 12/30 = 4/12 = 1/3
Independence = one event doesn't depend on the other, implies P(A|B) = P(A) because B doesn't matter
    Similarly, P(A&B) = P(A) * P(B) 
    *not* true for being female and computer science major
Want P(A|B) but have opposite, use: 
    P(B|A) * P(A) / [P(B|A) * P(A) + P(B|!A) * P(!A)]

Example: probability of CS given female = 
    P(F|CS) * P(CS) / [P(F|CS)*P(CS) + P(F|!CS)*P(!CS)]

ELISA HIV example: 
    P(+|HIV) = 0.977
    P(-|no HIV) = 0.926
    P(HIV) = 0.0026 (North America)

    So what is P(HIV|+)? 
    
    P(HIV|+) = P(+|HIV) * P(HIV) / [P(+|HIV) * P(HIV) + P(+|no HIV) * P(no HIV)]
             = 0.977 * 0.0026 / [0.977 * 0.0026 + (1-0.926) * (1-0.0026)]
             = 0.033
        tiny! only 3%! because it's a rare disease

    P(HIV|+) is not the same as P(+|HIV)!
