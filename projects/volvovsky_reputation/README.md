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

  - simple success
  - simple fail
  - complex success
  - complex fail

- pull names of images from the labeling/recall round for when buyer is on your team.
