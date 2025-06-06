# Simulating First Price Sealed Bid and Second Price Sealed Bid Auctions
In this model, a planner or auctioneer simultaneously allocates several goods to set of people.

Here, a single good is allocated to one person within a set of people and we explore and simulate two classic auctions:

- a First-Price Sealed-Bid Auction (FPSB)  
- a Second-Price Sealed-Bid Auction (SPSB) created by William Vickrey

We would also explore the 
- Revenue Equivalence Therorem

My code draws heavily from the works of Thomas J. Sargent and John Stachurski in their course on **Intermediate Quantitative Economics with Python**

## First-Price Sealed-Bid Auction (FPSB)

**Protocols:**

- A single good is auctioned.  
- Prospective buyers  simultaneously submit sealed bids.  
- Each bidder knows only his/her own bid.  
- The good is allocated to the person who submits the highest bid.  
- The winning bidder pays price she  has bid.

**Detailed Setting:**

There are $n > 2$ prospective buyers named $i = 1, 2, \ldots, n$.

Buyer $i$ attaches value $v_i$ to the good being sold.

Buyer $i$ wants to maximize the expected value of her **surplus** defined as $v_i - p$, where  $p$ is the price that she pays, conditional on her winning the auction.

Evidently,

- If $i$ bids exactly $v_i$, she pays what she thinks it is worth and gathers no surplus value.  
- Buyer $i$ will never want to bid more than $v_i$.  
- If buyer $i$ bids $b < v_i$ and wins the auction, then she gathers surplus value $v_i - b > 0$.  
- If buyer $i$ bids $b < v_i$ and someone else bids more than $b$, buyer $i$ loses the auction and gets no surplus value.  
- To proceed, buyer $i$ wants to know the probability that she wins the auction as a function of her bid $b$.  
  - This requires that she know a probability distribution of bids $v_j$ made by prospective buyers $j \neq i$.  
- Given her idea about that probability distribution, buyer $i$ wants to set a bid that maximizes the mathematical expectation of her surplus value.

Bids are sealed, so no bidder knows bids submitted by other prospective buyers.

This means that bidders are in effect participating in  a game in which players do not know  **payoffs** of  other players.

This is   a **Bayesian game**, a Nash equilibrium of which is called a **Bayesian Nash equilibrium**.

To complete the specification of the situation, we’ll  assume that  prospective buyers’ valuations are independently and identically distributed according to a probability distribution that is known by all bidders.

Bidder optimally chooses to bid less than $v_i$.

### Characterization of FPSB Auction

A FPSB auction has a unique symmetric Bayesian Nash Equilibrium.

The optimal bid of buyer $i$ is

```math
\mathbf{E}[y_{i} \mid y_{i} < v_{i}] 
```
where $v_{i}$ is the valuation of bidder $i$ and $y_{i}$ is the maximum valuation of all other bidders:

```math
y_{i} = \max_{j \neq i} v_{j}
```

A proof for this assertion is available  at the [Wikepedia page](https://en.wikipedia.org/wiki/Vickrey_auction) about Vickrey auctions.

## Second-Price Sealed-Bid Auction (SPSB)

**Protocols:** In a  second-price sealed-bid (SPSB) auction,  the winner pays the second-highest bid.

## Characterization of SPSB Auction

In a  SPSB auction  bidders optimally choose to bid their  values.

Formally, a dominant strategy profile in a SPSB auction with a single, indivisible item has each bidder  bidding its  value.

A proof is provided at [the Wikepedia page](https://en.wikipedia.org/wiki/Vickrey_auction) about Vickrey auctions.

## Uniform Distribution of Private Values

We assume valuation $v_{i}$ of bidder $i$ is distributed $v_{i} \stackrel{\text{i.i.d.}}{\sim} U(0,1)$.

Under this assumption, we can analytically compute probability  distributions of  prices bid in both  FPSB and SPSB.

We’ll  simulate outcomes and, by using  a law of large numbers, verify that the simulated outcomes agree with analytical ones.

We can use our  simulation to illustrate   a  **Revenue Equivalence Theorem** that asserts that on average first-price and second-price sealed bid auctions  provide a seller the same revenue.

To read about the revenue equivalence theorem, see [this Wikepedia page](https://en.wikipedia.org/wiki/Revenue_equivalence)

## Setup

There are $n$ bidders.

Each bidder knows that there are $n - 1$ other bidders.

## First price sealed bid auction

An optimal bid  for bidder $i$ in a **FPSB**  is described by the above equations. 

When bids are i.i.d. draws from a uniform distribution, the CDF of $y_{i}$ is

```math
\begin{aligned}
\tilde{F}_{n-1}(y) = \mathbf{P}(y_{i} \leq y) &= \mathbf{P}(\max_{j \neq i} v_{j} \leq y) \\
&= \prod_{j \neq i} \mathbf{P}(v_{j} \leq y) \\
&= y^{n-1}
\end{aligned}
```

and the PDF of $y_i$ is $\tilde{f}_{n-1}(y) = (n-1)y^{n-2}$.

Then bidder $i$’s optimal bid in a **FPSB** auction is:

```math
\begin{aligned}
\mathbf{E}(y_{i} \mid y_{i} < v_{i}) &= \frac{\int_{0}^{v_{i}} y_{i}\tilde{f}_{n-1}(y_{i})\,dy_{i}}{\int_{0}^{v_{i}} \tilde{f}_{n-1}(y_{i})\,dy_{i}} \\
&= \frac{\int_{0}^{v_{i}}(n-1)y_{i}^{n-1}\,dy_{i}}{\int_{0}^{v_{i}}(n-1)y_{i}^{n-2}\,dy_{i}} \\
&= \frac{n-1}{n}y_{i}\bigg|_{0}^{v_{i}} \\
&= \frac{n-1}{n}v_{i}
\end{aligned}
```

## Second Price Sealed Bid Auction

In a  **SPSB**, it is optimal for bidder $i$ to bid $v_i$.

## Revenue Equivalence Theorem

We now compare  FPSB and a SPSB auctions from the point of view of the  revenues that a seller can expect to acquire.

**Expected Revenue FPSB:**

The winner with valuation $y$ pays $\frac{n-1}{n} \cdot y$, where $n$ is the number of bidders.

Above we computed that the CDF is $F_{n}(y) = y^{n}$ and the PDF is $f_{n}(y) = n y^{n-1}$.

```math
\mathbf{R} = \int_{0}^{1} \frac{n-1}{n} v_{i} \times n v_{i}^{n-1} \, dv_{i} = \frac{n-1}{n+1}
```

**Expected Revenue SPSB:**

The expected revenue equals $n \times$ expected payment of a bidder.

```math
\begin{aligned}
\mathbf{TR} &= n \mathbf{E}_{v_i}\left[\mathbf{E}_{y_i}[y_{i} \mid y_{i} < v_{i}] \mathbf{P}(y_{i} < v_{i}) + 0 \times \mathbf{P}(y_{i} > v_{i})\right] \\
&= n \mathbf{E}_{v_i}\left[\mathbf{E}_{y_i}[y_{i} \mid y_{i} < v_{i}] \tilde{F}_{n-1}(v_{i})\right] \\
&= n \mathbf{E}_{v_i}\left[\frac{n-1}{n} \times v_{i} \times v_{i}^{n-1}\right] \\
&= (n-1) \mathbf{E}_{v_i}[v_{i}^{n}] \\
&= \frac{n-1}{n+1}
\end{aligned}
```

Thus, while probability distributions of winning bids typically differ across the two types of auction, we deduce that  expected payments are identical in FPSB and SPSB.

**Summary of FPSB and SPSB results with uniform distribution on $[0,1]$**

|Auction: Sealed-Bid|First-Price|Second-Price|
|:-------------------------------:|:-------------------------------:|:-------------------------------:|
|Winner|Agent with highest bid|Agent with highest bid|
|Winner pays|Winner’s bid|Second-highest bid|
|Loser pays|0|0|
|Dominant strategy|No dominant strategy|Bidding truthfully is dominant strategy|
|Bayesian Nash equilibrium|Bidder $i$ bids $\frac{n-1}{n}v_{i}$|Bidder $i$ truthfully bids $v_{i}$|
|Auctioneer’s revenue|$\frac {n-1}{n+1}$|$\frac {n-1}{n+1}$|

**Detour: Computing a Bayesian Nash Equilibrium for FPSB**

The Revenue Equivalence Theorem lets us find an optimal bidding strategy for a FPSB auction from outcomes of a SPSB auction.

Let $b(v_{i})$ be the optimal bid in a FPSB auction.

The revenue equivalence theorem tells us that a bidder agent with value $v_{i}$ on average receives the same **payment** in the two types of auction.

Consequently,

```math
b(v_{i}) \mathbf{P}(y_{i} < v_{i}) + 0 \times \mathbf{P}(y_{i} \ge v_{i}) = \mathbf{E}_{y_{i}}[y_{i} \mid y_{i} < v_{i}] \mathbf{P}(y_{i} < v_{i}) + 0 \times \mathbf{P}(y_{i} \ge v_{i})
```

## Calculation of Bid Price in FPSB

We have already displayed formulas for optimal bids in a symmetric Bayesian Nash Equilibrium of a FPSB auction.

$$
\mathbf{E}[y_{i} \mid y_{i} < v_{i}]
$$

where

- $v_{i} =$ value of bidder $i$  
- $y_{i} =$ maximum value of all bidders except $i$, i.e., $y_{i} = \max_{j \neq i} v_{j}$  

Above, we computed an optimal bid price in a FPSB auction analytically for a case in which private values are uniformly distributed.

For most probability distributions of private values, aFor most probability distributions of private values, analytical solutions aren’t  easy to compute.

Instead, we can  compute  bid prices in FPSB auctions numerically as functions of the distribution of private values.



