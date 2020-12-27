---
layout: post
title: Spam Riddle
---

Another [probability riddle](https://fivethirtyeight.com/features/can-you-catch-the-free-t-shirt/) from FiveThirtyEight: 

> Last week’s Riddler column garnered six comments on Facebook. However, every single one of those comments was spam. Sometimes, spammers even reply to other spammers’ comments with yet more spam. This got me thinking.<br><br>Over the course of three days, suppose the probability of any spammer making a new comment on this week’s Riddler column over a very short time interval is proportional to the length of that time interval. (For those in the know, I’m saying that spammers follow a Poisson process.) On average, the column gets one brand-new comment of spam per day that is not a reply to any previous comments. Each spam comment or reply also gets its own spam reply at an average rate of one per day.<br><br>For example, after three days, I might have four comments that were not replies to any previous comments, and each of them might have a few replies (and their replies might have replies, which might have further replies, etc.).<br><br>After the three days are up, how many total spam posts (comments plus replies) can I expect to have?

# Solution

This question boils down to finding the mean of a [linear birth process](http://www.damtp.cam.ac.uk/user/st321/CV_&_Publications_files/STpapers-pdf/BDP.pdf), which models population growth — très à la mode! Every derivation I found for the distribution of this process was fairly messy, while the formula for the distribution $$$$\mathbb{P}(k\text{ spams after t days}) = \mathbb{P}(S_t=k)=(1 - e^{-t})^ke^{-t}$$$$ begs an intuitive explanation connecting it to the geometric distribution. So I decided to find it myself!

# Description of the Linear Birth Process

$$(1 - e^{-t})^ke^{-t}$$ is the probability that a coin that comes up heads with probability $$e^{-t}$$ will get k tails before its first heads. This is the geometric distribution with parameter $$e^{-t}$$. $$e^{-t}$$ is the probability that an entity will take longer than t days to produce its first spam.

This shows that, **but doesn't explain why**, you can model $$S_t$$ by for instance counting how many times in a row the post births a new top-level spam comment (not a reply) in less than t days. 

# Connection to Coin Flipping

The main trick is to connect the linear birth process to the linear death process, $$D^k$$, which models a population starting with $$D^k_0 = k$$ spams that each take an average of one day to die.

When $$D^k=j$$ the death rate is j, since there are $$j$$ spams dying. Meanwhile, when $$S=j-1$$ the birth rate is $$j$$ (there are $$j - 1$$ spams plus the original post). So the time it takes $$D^k$$ to go from $$j$$ to $$j-1$$ has the same distribution as the time it takes $$S$$ to go from $$j - 1$$ to $$j$$. Based on their starting points then, the extinction time of $$D^k$$ has the same distribution as the time of the $$k^{th}$$ birth of $$S$$. In fact, running $$S + 1$$ (the population of spams plus the post) until the $$k^{th}$$ birth is essentially the reverse of $$D^k$$, with the exception that $$S + 1$$ will be one bigger than $$D^k$$ at the jumps since a birth just occurred instead of a death.

$$D^k$$ will go extinct by time t, i.e. $$D^k_t=0$$, if all $$k$$ spams die before $$t$$ days. If we think of $$f_j$$ as a coin that's heads when the $$j^{th}$$ spam lives longer than $$t$$ days, this is like having $$f_j=T$$ for $$1 \le j \le k$$. Then \begin{align*}
\mathbb{P}(S_t = k) &= \mathbb{P}(S_t \ge k) - \mathbb{P}(S_t \ge k + 1) \\
&= \mathbb{P}(D^k_t = 0) - \mathbb{P}(D^{k+1}_t = 0) \\
&= P(f_1, \ldots, f_k = T) - P(f_1, \ldots, f_{k+1}=T) \\
&= P(f_1, \ldots, f_k = T, f_{k+1}= H)\\
&= (1 - e^{-t})^ke^{-t}
\end{align*}

# Riddle Solution from Coin Flipping

The mean of $$G_p$$, a geometric random variable with parameter p, is $$\frac{1}{p} - 1$$, so $$E[S_t] = e^t - 1$$. To see this, imagine simulating $$G_p$$ N times for large N by flipping the coin until there have been N heads. Then $$\frac{\sum_{i=1}^N G_p^i}{N} = \frac{N_T}{N_H} = \frac{N_T + N_H}{N_H} - 1 \to \frac{1}{p} - 1$$.
