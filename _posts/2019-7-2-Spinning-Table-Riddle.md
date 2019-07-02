---
layout: post
title: Generalized Spinning Table Ridde
---

FiveThirtyEight posted this spinning table riddle for n=4:

>There is a square table with a quarter on each corner. The table is behind a curtain and thus out of your view. Your goal is to get all of the quarters to be heads up — if at any time all of the quarters are heads up, you will immediately be told and win.
>
>The only way you can affect the quarters is to tell the person behind the curtain to flip over as many quarters as you would like and in the corners you specify. (For example, “Flip over the top left quarter and bottom right quarter,” or, “Flip over all of the quarters.”) Flipping over a quarter will always change it from heads to tails or tails to heads. However, after each command, the table is spun randomly to a new orientation (that you don’t know), and you must give another instruction before it is spun again.
>
>Can you find a series of steps that guarantees you will have all of the quarters heads up in a finite number of moves?

<!--more-->

([fivethirtyeight](https://fivethirtyeight.com/features/i-would-walk-500-miles-and-i-would-riddle-500-more/))

I found a solution
for all n where n is a power of 2, using what I thought was a pretty neat recursive
trick that involves playing two smaller games at once. I will demonstrate this,
and show how FiveThirtyEight’s n=4 solution exactly implements this recursive
algorithm. I will prove, on the other hand, that there is no solution for any n
not a power of 2.

## Powers of 2

Suppose $$n = 2^k$$
, and you have a solution for 2k−1
. Life would be easy if
every pair of opposite coins matched (either both heads or both tails). Then
you could think of the matching pairs as single coins, and use the 2k−1
solution,
always flipping a coin with its pair.
So, the problem is reduced to getting the pairs all matching. This turns out
to also be a 2k−1
spinning table problem! Think of matching pairs as being
“heads” and mismatched pairs as being “tails.” “Flipping” a pair involves flipping one of the coins, as this “flips” whether or not the pair matches.
At this point you may be saying to yourself, “Hell yeah! We just have to
do the sequence to get all the pairs matching (M), and then the sequence to
get all the matching pairs to heads (H).” And you’re mostly right, with one
caveat. While M gets all the pairs matching at some point, it’s not necessarily
at the end. So we actually have to do the H sequence in between every move in
M. To wit, if M consists of flips m1, m2, . . ., mN , then the 2k
solution would
be Hm1Hm2H . . . HmN H. One final saving grace keeping everything kosher is
that each of the flips in H leaves the M game unchanged, since pairs are always
flipped together (they’re essentially non-flips in M).
1
2 Deriving the original solution from the generalized
To solve the n=4 case, we need an n=2 solution. It’s not hard to figure out
that M=(Flip 1,2; Flip 1; Flip 1,2) works, but I will show below how you could
be lazy and find this recursively. In H, 1 and 3 form a pair as do 2 and 4, so
coin 1 maps to {1, 3} and 2 to {2, 4}. Thus H=(Flip both pairs; Flip {1,3}; Flip
both pairs). Then the solution is Hm1Hm2Hm3H=(H; Flip 1,2; H; Flip 1; H;
Flip 1,2; H), which is exactly the solution provided by FiveThirtyEight.
To solve n=2 in the same way, we need an n=1 solution. This is the base
case, so sadly we will have to use our own ingenuity. M=(Flip 1) works, which
translates to H=(Flip both). The solution is then HMH=(Flip both; Flip 1;
Flip both).
3 Growth rate
The growth rate is exponential. It’s not hard to see that if the solution for
n = 2k
takes N steps, then the solution for n = 2k+1 takes m(m + 2) steps.
From this, it can easily be shown the solution for n takes 2n − 1 steps.
4 Non-powers of 2
n is not a power of 2 if and only if it’s divisible by some odd m > 1. Then
there are evenly spaced coins i1, i2, . . . , im. There is no solution because if you
start in a configuration where i1, . . . , ım are not homogeneous (not all heads or
all tails), there is no guarantee you will ever escape this state.
To see this, consider a choice of flips. You could luck out and flip exactly
the heads or exactly the tails in i1, . . . , im. W.L.O.G. let’s say it’s the heads.
Then, there is some ij that is a tails (using inhomogeneity), and because of the
spinning, you could have flipped ij . But you won’t have flipped all the tails,
because m is odd so the number of tails differs from the number of heads, so
you will still be in the inhomogeneous state.
