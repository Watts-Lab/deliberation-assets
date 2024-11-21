Project to assess the impact of age and gender on talk-time balance

## Intro steps

- Answer a panel of yes/no questions
- Demographic survey

## Dispatch

- ideally, get partnered with someone who disagrees with you on an issue
- but initially, just get paired and given a random topic

## Game

#### Stage 1

Attitude dimensions
Asked to write arguments about your position
How compelling are these arguments?
Asked to write arguments against your position
How compelling are these arguments?
Tolerance for people who disagree
How much do you think you would enjoy discussing this issue with someone else?

#### Stage 2: Discussion

Conversation

1. Hello
2. Open discussion
3. Joint policy writing

## Post

Ask position again
How much do you support the statement you wrote? (behavioral)
Write arguments about your position
How compelling are these arguments?
Write arguments against your position
How compelling are these arguments?
Tolerance for people who disagree
Feeling heard
Willingness to have another discussion (behavioral)

## Inferred DVs

- Talk time balance

Start with a baseline, no interventions, describe:

- effect of age, gender, race on outcomes

# Todo:

- [x] make RME questions mandatory
- [x] remove back button in rme
- [x] Make "republicans" more emphatic in trait ratings? Previously, "them" had refered to the discussion partner.
- [x] Display nickname in partner ranking
- [x] emphasize that headphones prevent sound from your computer being picked up by your microphone.
- [ ] require all topic opinion questions to proceed

In this study. we're using an interesting knockdown strategy. Our goal is to uniformly sample over topics and levels of disagreement between partners, but we don't actually specify levels of disagreement as part of the treatment. Instead, we specify the absolute positions of each of the participants, using a 7 point scale. So, while there are 7 levels of disagreement (0-6), there are actually 49 different treatments. In order to make sure we sample evenly over the levels of disagreement, we use a knockdown matrix that reduces the reward for every treatment having the same level of payoff as the selected treatment (for the same topic). For example, if the Republican selects "Strongly No" and the democrat selects "Slightly yes", this is a difference of 4 scale points. We want to make sure we sample differences of 3, 2, etc. before we sample a difference of 4 scale points again, if possible. so, we knock down the payout not only for "Strongly No"/"Slightly Yes", but also for "Mostly No"/"Mostly Yes", which also has a difference of 4 scale points, and the other combinations with the same absolute difference.
