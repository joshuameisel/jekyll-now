---
layout: post
title: Can You Flip Your Way To Victory?
---

{% include lib/mathjax.html %}

FiveThirtyEight posted this
[probability riddle](https://fivethirtyeight.com/features/can-you-flip-your-way-to-victory/):


>You have two fair coins, labeled A and B. When you flip coin A, you get 1 point if it comes up heads, but you lose 1 point if it comes up tails. Coin B is worth twice as much — when you flip coin B, you get 2 points if it comes up heads, but you lose 2 points if it comes up tails.
>
>To play the game, you make a total of 100 flips. For each flip, you can choose either coin, and you know the outcomes of all the previous flips. In order to win, you must finish with a positive total score. In your eyes, finishing with 2 points is just as good as finishing with 200 points — any positive score is a win. (By the same token, finishing with 0 or −2 points is just as bad as finishing with −200 points.)
>
>If you optimize your strategy, what percentage of games will you win? (Remember, one game consists of 100 coin flips.)
>
>_Extra credit:_  What if coin A isn’t fair (but coin B is still fair)? That is, if coin A comes up heads with probability  _p_  and you optimize your strategy, what percentage of games will you win?

Somewhat improbably, the answer is the same as the probability that the running maximum of a simple symmetric random walk (the process starting at 0 by which you move either left or right by 1 with equal probability at each step in time) is not a multiple of 3 at time 100! 

## Bet Big When You're Down

My first insight was that you should probably bet big when you're losing, maximizing the probability of regaining your losses, and bet small when you're up, minimizing the probability of going in the hole again. I wasn't able to find a simple proof for this (let me know if you do!), but will demonstrate it later inductively using some fancy random walk mumbo jumbo I develop first.

## Running Maximum

Assuming the above strategy, notice that we're losing (and betting 2) until we have more heads than tails (this takes as long as a random walk W<sub>n</sub> takes to hit 1 for the first time, or for its maximum M<sub>n</sub> to hit 1). At this point we'll be up two points, start betting 1, and winning until we end up back at 0. This is like W<sub>n</sub> going from 1 to -1, or considering its reflection over this time period, going from 1 to 3 and having M<sub>n</sub> hit 3).

More concretely, say that W<sub>n</sub> is a random walk that moves in the same direction as our money pot when we're losing, and the opposite direction when we're winning (to convince yourself this is a proper random walk, note that regardless of the previous path of W<sub>n</sub>, it still has 1/2 odds of moving left or right). We can see that we're only losing when M<sub>n</sub> is a multiple of 3, so our win probability equals Prob(M<sub>100</sub> is not multiple of 3).

try again

This $$\subset$$ latex 