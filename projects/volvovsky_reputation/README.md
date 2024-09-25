# Text for Buyer training video

# Text for Agent training video

# Text for observer training video

This game has one more role, an observer. The observer watches the art gallery game, and then at the end will have the opportunity to hire the agent to buy a piece of art for themselves.

Since all the observer does is observe and take notes, there is no need for a training round for this role.

<!--
Unfortunately we couldn't match you in the first game. But don't worry, this actually gives you an advantage: you get to observe an agent before deciding to hire them.
You will observe a game between the following buyer and agent:
<avatar and name of buyer and agent>

At the end of the game, you will have the opportunity to hire the agent if you wish.

Hiring an agent who buys the right art will earn you 30 points per piece (after accounting for the agent's wages). However, hiring an agent who buys the wrong art will earn you nothing, and you will lose the 50 points you paid the agent.

You will get 25 points for every round you observe. -->

Unfortunately we couldn't match you in the first game. But don't worry, this actually gives you an advantage: you get to observe an agent before deciding to hire them.

At the end of the game, you will have the opportunity to hire the agent if you wish.

Hiring an agent who buys the right art will earn you 30 points per piece (after accounting for the agent's wages). However, hiring an agent who buys the wrong art will earn you nothing, and you will lose the 50 points you paid the agent.

You will get 25 points for every round you observe.

You will observe a game between the following buyer and agent:
<avatar and name of buyer and agent>

# Conditions

1. Simple images, Buyer your team, Agent other team, Success (need group's name for this)
2. Simple images, Buyer your team, Agent other team, Fail (need group's name for this)
3. Simple images, Buyer other team, Agent other team, different teams, Success
4. Simple images, Buyer other team, Agent other team, different teams, Fail

5. Complex images, Buyer your team, Agent other team, Success (need group's name for this)
6. Complex images, Buyer your team, Agent other team, Fail (need group's name for this)
7. Complex images, Buyer other team, Agent other team, different teams, Success
8. Complex images, Buyer other team, Agent other team, different teams, Fail

We never test an agent from your team, because you don't need a reputational signal for that, you already know them.

signal is success-failure
difference in signal is (7-8) - (5-6)

(5-7) difference in singnal for success cases

Research question: How much affordance do I give to people who make mistakes if I don't have high expectations for them to succeed?

Why is it that in some markets (buying books) we can use review sites and ratings to make decisions (ie, book sellers), but other markets (hiring babysitters, recommendations on the content of books) we only trust signals from people we know.

WHen there are no established frameworks, reputational signals (of whom) are more informative when they come from someone close to you in the network (same whom?). Equivalently, the informativeness of reputational signals decays with network distance.

Two mechanisms why that might be true:

1. I trust the judgement of people I know better than the judgement of people I don't.
2. I believe that I am more similar to people i know (on my team) so that if they were able to succeed with the agent, I am more likely to be able to succeed with them too.

Focussing mostly on the second mechanism.
Using simple images, the primary issue is whether the agent is competent, engaged
Using complex images, the issue is whether the agent is a good match/fit for someone like me.

Reputational signal always comes from the buyer
Object of the reputation is the Agent
Manipulation is how far away in your network is the buyer, plus content of signal.
Reputational signal is used to decide whether to hire an agent (from another team).

Question:
Does the observer plausibly believe that an agent can get the right art piece even if they were on a different team, which may have used different names for the images? If not, then if there is a negative signal, they may interpret it to be due to the fact that NO agent could have gotten it right under those conditions, and so instead of being a reputational signal, it just tells you about the game. A positive signal will maybe surprise them, because

If you see a game where you and I agree that an image is karate, and then i see you play as the buyer, and you say "buy karate" and the other person buys the right or wrong image, there is a lot of information in whether I want to hire the agent.

However, if the agent is from another team, so that they are not expected to know the name, then i don't know if the agent was lazy, or if they just didn't have the information that they needed.

# Notes

When we show them a buyer/agent combination in the observer round, we need to make sure that if the buyer is from their team, we don't show them their own avatar. so, we need 4 combinations:

- Bird, Other1
- Bear, Other1
- Fish, Other1
- Other1, Other2

# where we are:

- we have built up training for agent and buyer, and also have instructions for observer (who needs no training)
- we set up the avatar combinations for the observer rounds.

# Next time

- create a template of the principal agent game that they will observe
- plug in the image, the name and the outcome/number of rounds

# Todo:

- work out the sequence of observations for each of the following:

  - simple success (five rounds of success)
  - simple fail (two rounds of failures)
  - complex success (five rounds of success) five rounds indicates that the game can continue if you want it to.
  - complex fail (two rounds of failure)

- pull names of images from the labeling/recall round for when buyer is on your team.

# Observer rounds must contain:

The buyer is deciding whether to hire the agent (10 second timer)

The buyer asked for:

- if it's your team, show name from your team
- otherwise show other name

<!-- The agent purchased: Not going to show images, because we don't want the observer to learn the agent's schema and then think they can use it.
- correct image
- incorrect image -->

This was the

- right piece, they spent n points to earn n points
- wrong piece, they spent n points with no return

The buyer decided:

- not to hire the agent to purchase another piece
- to hire the agent to purchase another piece

The buyer and agent are waiting for you to complete the round.

Whatever timers we put in here, we should also put into the training rounds for consistency.

The number of rounds in the game is randomly selected by a computer.

# July 25

Where are we:

- Created simple images
- Decided and mocked up templates for the observer stages

Need to do:

- markdown files for each other buyer stage with quoted names
- markdown files for success and failure with decisions to hire agent
- instantiate templates with the right recallIndexes and build up the exit sequences.

# July 31

where we are:

- implemented most of the interventions and manipulation

Need to do:

- post-decision debrief
- debug

# Aug 28

where we are:

- prototyped the whole major experiment

need to:

- exit surveys
- consider the two remaining teammates,, answer perception of others survey about them
- create solo images for these teammates
- debug treatments
- debug timings

# Sept 12

Need to:

- fix timer for exit stages for kitchenTimer, also display and hide times
- [x] check that images are properly displayed in labeling game
- check that stage observer intro stage displays properly
- check that play as agent task displays properly
- check submit button in observer round advances
- labels on manipulation check need to be different, no submit button
- need comprehension checks for after training rounds.

# Sept 25

- [x] bug on the rendering of the outcomeFile
- [x] need to double check the comprehension checks
- [x] confusing that you observe multiple rounds and they aren't labeled as multiple rounds. We should say how many rounds.
- [x] check that the name assigned to the original images are set in the various conditions

# for Oct 3

- check that generic names for images work
- [ ] still having a bug where the stage isn't re-rendering after the timer elapses - James
- [ ] binary choice or continuous choice for willingness to hire the agent. - Hagay
- [ ] other team observe stage isn't rendering asksforfile properly, just gets loading message
