# Numerical simulation for upper bound on dynamic blocksize expansion

Isthmus - Sept 2018

Organization:
-  Abstract
-  Background reading
-  Adversary model
-  Threat model
-  Mitigation strategies
-  Numeric simulations
-  Observations
-  FAQ

## Abstract:
The current bound on expansion of the Monero block(/chain) is that a block cannot be larger than the median size of the previous 100 blocks. Given the 2-minute block time, this means that the protocol has a 3-hour memory, which performs well for most use cases. However, it is prudent to consider edge cases as well - the current absence of medium or long-term memory leaves the network susceptible to block size inflation from a well-funded adversary. This notebook quantifies the scale of damage and costs of a worst-case-scenario attack, and suggests some starting points for developing mitigation strategies.

## Background Reading:
-  Jolly Mort's [Monero Dynamic Block Size and Dynamic Minimum Fee](https://github.com/JollyMort/monero-research/blob/master/Monero%20Dynamic%20Block%20Size%20and%20Dynamic%20Minimum%20Fee/Monero%20Dynamic%20Block%20Size%20and%20Dynamic%20Minimum%20Fee%20-%20DRAFT.md)
-  ArticMine's [BitcoinTalk discussion](https://bitcointalk.org/index.php?topic=753252.msg13591241#msg13591241) about oversize block attacks.

## Adversary model:
A well-funded (\$\$\$\$\$+) adversary wishes to disrupt the Monero cryptocurrency/network/community. The adversary could be a crypto whale with a trading position against Monero, a 3-letter agency that decides breaking Monero is easier than cracking it, powers that benefit from the incumbent financial/economic systems, or even a careless Monero trader who wishes to sow chaos and reap cheap coins.

## Threat model:
The attacker creates a set of transactions whose:
-  Net fee > coinbase
-  Net size ~ 2*median(last 100 blocks)

Monero's dynamic blocksize includes a coinbase penalty function to incentivize miners to properly manage block size; the attacker overrides these economic safeguards by including a net fee greater than the coinbase. Given all of the possible subsets of transactions in the memory pool (and taking into account the coinbase penalty), the attacker's transactions must be the most profitable combination.

**Result: The *block size* doubles every 3 hours, indefinitely. The *blockchain size* doubles within 8 hours and grows catastrophically large.**

# Please see Jupyter notebook for results and FAQ

# See issues for conversation/brainstorming around scenarios and algorithms to test
