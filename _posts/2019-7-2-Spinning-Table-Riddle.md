---
layout: post
title: Generalized Spinning Table Riddle
---

FiveThirtyEight posted this [spinning table riddle](https://fivethirtyeight.com/features/i-would-walk-500-miles-and-i-would-riddle-500-more/) for *n* = 4 coins:

>There is a square table with a quarter on each corner. The table is behind a curtain and thus out of your view. Your goal is to get all of the quarters to be heads up — if at any time all of the quarters are heads up, you will immediately be told and win.
>
>The only way you can affect the quarters is to tell the person behind the curtain to flip over as many quarters as you would like and in the corners you specify. (For example, “Flip over the top left quarter and bottom right quarter,” or, “Flip over all of the quarters.”) Flipping over a quarter will always change it from heads to tails or tails to heads. However, after each command, the table is spun randomly to a new orientation (that you don’t know), and you must give another instruction before it is spun again.
>
>Can you find a series of steps that guarantees you will have all of the quarters heads up in a finite number of moves?

I found a solution for all *n* where *n* is a power of 2, using what I thought was a pretty neat recursive trick that involves playing two smaller games at once.  I will demonstrate this, and show how FiveThirtyEight’s *n* = 4 solution exactly implements this recursive algorithm.  I will prove, on the other hand, that there is no solution for any *n* not a power of 2.

## Powers of 2

Suppose *n* = 2<sup>*k*</sup>, and you have a solution for 2<sup>*k*-1</sup>. We will consider the 2<sup>*k*-1</sup> pairs of coins which are opposite the table from each other. For instance, in the *n* = 4 = 2<sup>2</sup> case, the 2 pairs are \{1, 3\} and \{2, 4\}. 

Life would be easy if all the pairs were matching (either both heads or both tails). Again, with *n* = 4 this would mean coins 1 and 3 matched as did 2 and 4. Then we could treat each matching pair as a single coin, and use the 2<sup>*k*-1</sup> solution, always flipping a coin with its pair. When *n* = 4, the 2<sup>*k*-1</sup> = 2 coin solution is (Flip Both, Flip Coin 1, Flip Both). (This isn't hard to figure out, but I'll show below how to lazily use our recursive solution to find it.) If the 2 pairs are matching, and we treat {1,3} as a single coin as well as {2,4}, then an adequate solution is *G* = (Flip 1&3 and 2&4, Flip 1&3, Flip 1&3 and 2&4) (*G* for "glued").

Of course, the pairs aren't necessarily matching, but now the problem is reduced to making this so. The :sunglasses: idea is to think of this as a 2<sup>*k*-1</sup> spinning table problem! Think of matching pairs as being "heads" and mismatched pairs as being "tails." You can "flip" the pair by flipping one of the coins, as this "flips" whether or not the pair matches. In the *n* = 4 case, flipping 1 changes whether 1 and 3 match, and flipping 2 does the same for \{2, 4\}. So, with *M* standing for "matching," the sequence *M* = (Flip 1&2, Flip 1, Flip 1&2) will get both pairs matching at some point.

Now we don't know at what point in *M* all the pairs will be matching, so we have to do the *G* sequence in between each step of *M*. Luckily, the *G* sequence leaves the *M* game unchanged — every move in *G* does the same thing to both members of each pair, so the set of matching pairs and the set of mismatched pairs stays the same. To wit, if *M* = (*M*<sub>1</sub>,*M*<sub>2</sub>,...,*M*<sub>*N*</sub>), then the 2<sup>*k*</sup> solution would be (*G*,*M*<sub>1</sub>,*G*,*M*<sub>2</sub>,*G*,...,*G*,*M*<sub>*N*</sub>,*G*). For *n* = 4 this is exactly the [solution](https://fivethirtyeight.com/features/whats-your-best-scrabble-string/) provided by FiveThirtyEight:

>Flip four, flip two opposite, flip four, flip two adjacent, flip four, flip two opposite, flip four, flip one, flip four, flip two opposite, flip four, flip two adjacent, flip four, flip two opposite, flip four.

To solve *n* = 2 = 2<sup>1</sup> recursively, we need an *n* = 1 = 2<sup>1-1</sup> solution. This is the base case, so sadly we will have to use our own ingenuity. *M* = (Flip 1) works, which translates to *G* = (Flip Both). The solution is then (*G*,*M*,*G*) = (Flip Both, Flip 1, Flip Both).

## Growth rate

The growth rate of number of steps is exponential. It's not hard to see that if the solution for *n* = 2<sup>*k*</sup> takes *N* steps, then the solution for *n* = 2<sup>*k*+1</sup> takes *N*(*N*+2) steps. From this, it can easily be shown the solution for *n* takes 2<sup>*n*</sup> - 1 steps.

## Non-powers of 2

n is not a power of 2 if and only if it is divisible by some odd *m* > 1. Then there are evenly spaced coins *i*<sub>1</sub>,*i*<sub>2</sub>,...,*i*<sub>*m*</sub>. Call a configuration of coins homogeneous if all the *i*<sub>*j*</sub>'s are matching. I claim there is no guarantee you will ever get to a homogeneous state.

Imagine there was some move that gets you out of an inhomogeneous state. I will show that had the board been spun differently you would still be in an inhomogeneous state.

Your move achieves homogeneity by flipping exactly the heads or exactly the tails in *i*<sub>1</sub>,...,*i*<sub>*m*</sub>. Without loss of generality let's say it's the heads. Then, there is some *i*<sub>*j*</sub> that is a tails (using inhomogeneity). Had the board been spun differently, the same move would have flipped *i*<sub>*j*</sub> (using the fact that you must have flipped at least one coin). Now you can't have flipped exactly the tails — you've done the same number of flips, yet since *m* is odd the number of tails differs from the number of heads. So you are still in an inhomogeneous state :cry:
