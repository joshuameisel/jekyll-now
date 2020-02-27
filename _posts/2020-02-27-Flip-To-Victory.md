---
layout: post
title: Can You Flip Your Way To Victory?
---

FiveThirtyEight posted this
[probability riddle](https://fivethirtyeight.com/features/can-you-flip-your-way-to-victory/):


>You have two fair coins, labeled A and B. When you flip coin A, you get 1 point if it comes up heads, but you lose 1 point if it comes up tails. Coin B is worth twice as much — when you flip coin B, you get 2 points if it comes up heads, but you lose 2 points if it comes up tails.
>
>To play the game, you make a total of 100 flips. For each flip, you can choose either coin, and you know the outcomes of all the previous flips. In order to win, you must finish with a positive total score. In your eyes, finishing with 2 points is just as good as finishing with 200 points — any positive score is a win. (By the same token, finishing with 0 or −2 points is just as bad as finishing with −200 points.)
>
>If you optimize your strategy, what percentage of games will you win? (Remember, one game consists of 100 coin flips.)
>
>_Extra credit:_  What if coin A isn’t fair (but coin B is still fair)? That is, if coin A comes up heads with probability  _p_  and you optimize your strategy, what percentage of games will you win?

Somewhat improbably, the answer is the same as Prob($M_{100}$ is not a multiple of 3), where $M_n$ is the running maximum of a simple symmetric random walk! (A simple symmetric random walk is the process starting at 0 whereby after each step in time you move either left or right by 1 with equal probability.)

## Bet Big When You're Down

My first insight was that you should probably bet big when you're losing, maximizing the probability of regaining your losses, and bet small when you're up, minimizing the probability of going in the hole again. I wasn't able to find a simple proof for this (let me know if you do!), but will demonstrate it later inductively using some fancy random walk mumbo jumbo I develop first.

## Running Maximum

Assuming the above strategy, we start off losing (and betting 2) until we have more heads than tails. This takes as long as a random walk $W_n$  takes to hit 1 for the first time, or for its maximum $M_n$ to reach 1. At this point we'll be up two points, start betting 1, and be winning until we end up back at 0. This is like $W_n$ going from 1 to -1, or considering its reflection across 1 over this time period, going from 1 to 3 and having $M_n$ reach 3.

More concretely, say that $W_n$ is a random walk that moves in the same direction as our points when have 0 or fewer points, and the opposite direction when we have positive points (to convince yourself this is a proper random walk, note that regardless of the previous path of $W_n$, it's next move has even odds of being a left or right). We can see that we're only losing when $M_n$ is a multiple of 3, so our win probability $p(100) = P(M_{100} \not\equiv 0 \pmod  3)$.

There's a very [pretty derivation](https://web.ma.utexas.edu/users/gordanz/notes/lecture4.pdf) of the distribution of $M_n$ that  uses a similar reflection trick. It's nearly the distribution of $|W_n|$. For $0 \le k \le n$, $$P(M_n = k) = \begin{cases}
    P(W_n = k) & \text{if } n \equiv k \pmod 2\\
    P(W_n = k+1) ,              & \text{otherwise}
\end{cases} $$ or, equivalently, ${n \choose \lfloor \frac{n + k + 1}{2} \rfloor}2^{-n}$. 

So $p(100) = 1 - \underset{0 \le 3k \le 100}{\sum}P(M_{100} = 3k)  \approx 0.640$.

## Asymptotic Behavior
You would guess that as $n \to \infty$, $P(M_n \equiv 0 \pmod 3) \to \frac{1}{3}$, so $p(n) \to \frac{2}{3}$.  To see this, note that $P(M_{n} = k)$ is non-increasing, so $p(n) =  \underset{0 \le 3k \le n}{\sum}P(M_n = 3k + 1) + P(M_n = 3k + 2) \le 2\underset{0 \le 3k \le n}{\sum}P(M_n = 3k)  = 2(1 - p(n)) \implies p(n) \le  \frac{2}{3}$.

Also, $p(n) \ge 2\underset{3 \le 3k \le n}{\sum}P(M_n = 3k + 3) 
= 2(1 - p(n) - P(M_n = 0)) \implies p(n) \ge \frac{2(1 - P(M_n = 0))}{3} \to \frac{2}{3}$.

## Proof of Winning Strategy
Now I'll show inductively that betting 1 if and only if you're ahead is a winning strategy, and that it doesn't matter what you bet when you have 0 or 1 points. When there's only one flip left, your strategy only matters if you're up 2 or down 1, and the strategy works. 

Now Call $p(k, n)$ your win probability with k points and n  flips left. At k points 