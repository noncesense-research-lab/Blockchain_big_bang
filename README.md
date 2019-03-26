# Please see the [Big Bang Notebook](https://github.com/noncesense-research-lab/Blockchain_big_bang/blob/master/models/Isthmus_Bx_big_bang_model.ipynb) for the full story with code & plots.

--- 
--- 

### Some information (without plots) is provided below for readers that cannot view the notebook.

Numerical simulation for upper bound on dynamic blocksize expansion (Blockchain Big Bang)

Isthmus - Sept 2018, updated Jan 2019

## Abstract:

The current bound on expansion of the Monero block(/chain) is that each block cannot be larger than the median size of the previous 100 blocks. Given the 2-minute block time, this means that the protocol has a few-hour memory, which performs well for most use cases and conditions. 

However, it is prudent to consider edge cases as well, and the current absence of medium or long-term memory comes with a significant risk. Under the current protocol, sustained high transaction volume (due to rapid adoption or a well-funded spammer) induces a rapid exponential increase in resource requirements (disk space, bandwidth, etc) that would exceed the capacities of the extant Monero infrastructure on the scale of hours

This notebook quantifies the scale of damage in the worst-case-scenarios that could arise from overwhelming adpotion or a deliberate spam attack. The current system (short-term memory only) is compared to two proposed mitigation strategies: a two point long/short-term memory, and an additive limit.

## Background Reading:

-  Jolly Mort's [Monero Dynamic Block Size and Dynamic Minimum Fee](https://github.com/JollyMort/monero-research/blob/master/Monero%20Dynamic%20Block%20Size%20and%20Dynamic%20Minimum%20Fee/Monero%20Dynamic%20Block%20Size%20and%20Dynamic%20Minimum%20Fee%20-%20DRAFT.md)
-  ArticMine's [BitcoinTalk discussion](https://bitcointalk.org/index.php?topic=753252.msg13591241#msg13591241) about oversize block attacks.

The topic of blockchain bloat has been discussed in many communities for many years. This notebook is not the first mention of the idea, it simply seeks to quantify the rate and scale of increasing resource demand, and test strategies proposed to protect the Monero cryptocurrency.

## Risk:

The block size can be maximized by any set of transactions in the memory pool whose:
-  Net fee > coinbase
-  Net size ~ 2*median(last 100 blocks)

*Note: depending on assumptions about miner software and transaction selection methodology, a net fee of 4x coinbase may be necessary to induce a maximum-size block.*

We'll consider "Icarus adoption" to be any scenario (whether benignly or maliciously induced) that causes a flood of transactions that increases resource requirements beyond the capacity of the Monero infrastructure (e.g. exceeding the disk space or bandwidth of most nodes and miners).

## Results:

Under the current system, the *block size* doubles every 51 blocks (i.e. doubling 14 times each day) indefinitely. The *blockchain size* doubles in less than a day and grows catastrophically large shortly thereafter.

```
*************************
At the end of the 6th hour:
-
# Current algorithm
Blocksize = 4800.0 kB
>>> Blockchain size = 65.3489 GB
-
# Two-point short/long-term algorithm
Blocksize = 1152.48 kB
Blockchain size = 65.12532128 GB
-
# Two-point short/long-term algorithm
Blocksize = 700.0 kB
Blockchain size = 65.1164 GB
-
# Additive cap
300 kB additive allowance
Blocksize = 1500.0 kB
Blockchain size = 65.1791 GB
-
Net fees ~ 594.0 XMR
Net fees ~ 29700.0 EUR
stake ~ 0.004%

*************************
At the end of the 12th hour:
-
# Current algorithm
Blocksize = 76800.0 kB
>>> Blockchain size = 69.3473 GB
-
# Two-point short/long-term algorithm
Blocksize = 4427.367168 kB
Blockchain size = 65.5347079916 GB
-
# Two-point short/long-term algorithm
Blocksize = 750.0 kB
Blockchain size = 65.2476 GB
-
# Additive cap
300 kB additive allowance
Blocksize = 2700.0 kB
Blockchain size = 65.5478 GB
-
Net fees ~ 1188.0 XMR
Net fees ~ 59400.0 EUR
stake ~ 0.006999999999999999%

*************************
At the end of the 18th hour:
-
# Current algorithm
Blocksize = 614400.0 kB
>>> Blockchain size = 117.5009 GB
-
# Two-point short/long-term algorithm
Blocksize = 12148.695509 kB
Blockchain size = 66.9059521508 GB
-
# Two-point short/long-term algorithm
Blocksize = 800.0 kB
Blockchain size = 65.384 GB
-
# Additive cap
300 kB additive allowance
Blocksize = 3600.0 kB
Blockchain size = 66.11075 GB
-
Net fees ~ 1782.0 XMR
Net fees ~ 89100.0 EUR
stake ~ 0.011000000000000001%

*************************
At the end of the 24th hour:
-
# Current algorithm
Blocksize = 9830400.0 kB
>>> Blockchain size = 689.2001 GB
-
# Two-point short/long-term algorithm
Blocksize = 15000.0 kB
Blockchain size = 69.5648348384 GB
-
# Two-point short/long-term algorithm
Blocksize = 800.0 kB
Blockchain size = 65.528 GB
-
# Additive cap
300 kB additive allowance
Blocksize = 4800.0 kB
Blockchain size = 66.86525 GB
-
Net fees ~ 2376.0 XMR
Net fees ~ 118800.0 EUR
stake ~ 0.013999999999999999%

*************************
At the end of the 36th hour:
-
# Current algorithm
Blocksize = 1258291200.0 kBhttps://github.com/noncesense-research-lab/Blockchain_big_bang/edit/master/README.md
>>> Blockchain size = 88145.3537 GB
-
# Two-point short/long-term algorithm
Blocksize = 15000.0 kB
Blockchain size = 74.9648348384 GB
-
# Two-point short/long-term algorithm
Blocksize = 850.0 kB
Blockchain size = 65.8188 GB
-
# Additive cap
300 kB additive allowance
Blocksize = 6900.0 kB
Blockchain size = 68.95235 GB
-
Net fees ~ 3564.0 XMR
Net fees ~ 178200.0 EUR
stake ~ 0.021%
```

## Conclusions:

Most cryptoeconomic schemes include fee/reward mechanisms designed to discourage undesired behavior from financially-rational parties under economic equilibrium conditions. However all protocols must include secondary fail-safe mechanisms to protect the network against literally "overwhelming" adoption and/or adversaries whose external agendas override economic motivations.

## FAQ

Q: Won't miners voluntarily avoid mining large blocks?

A1: The timescale of this attack can be mind-bogglingly FAST! In 36 hours, the blockchain explodes to 30 TB. To stop an in-progress attack, it would be necessary to modify major mining and pool software, and roll out updates across the ecosystem and dozens of platforms. The permanent damage would be on the order of tens of terabytes, even if every Monero engineer mobilized instantly with deft DevOps ability. I don't know if every random botnet operator is going to be following Reddit 24/7 and race to fix their mining pools in the middle of the night.

A2: Voluntarily avoiding large blocks means voluntarily mining blocks with less profit. Some software/people are greedy.

A3: Cryptocurrencies are designed to be resistant to block censorship, so any ideas to have the nodes or users reject large blocks will run into a lot of practical issues.

--- 
Q: Why would somebody do this intentionally? Isn't this more expensive than a 51% attack?

A1: A 51% is a totally different threat/consequence model. The blockchain big bang knocks nodes off the network, leaves permanent scars in the blockchain, and wreaks obvious major havoc in everybody's face with severe service disruptions. In contrast, a 51% attack on Monero is totally unnoticable to the rest of the ecosystem, causing no disruption to Monero's services, network, or community. 

A2: While the two attacks may cost the same *on paper*, there is a big logistical difference. Converting 200,000 EUR into purchasing half of the mining equipment / hashrate in the world is a huge endeavor. Converting 200,000 EUR into a blockchain big bang spam attack requires a few hours of coding open source software, and a laptop.

---
Q: We need a dynamic blocksize! What about an influx of legitimate transactions?

A: We will definitely not remove dynamic blocksizes. Dynamic protocols can be designed to allow short-term flexibility without having to allow unbounded extremes.

Temporary high fees are better than a permanantly-broken network.

---
Q: What happens if we implement an upper bound, but no adoption/attacks ever cause the upper limit to be triggered?

A: That's an ideal outcome! In systems handling large amounts of money, it's better to be safe than sorry - properly implementing safeguards are always better than leaving any feature or process unbounded and up to fate. 

--- 
Q: How can we possibly adjust the blocksize algorithm?

A: The current block size algorithm should be subject to improvement just like every other aspect of the cryptocurrency. Belief that the first iteration of any system is perfect tends to be limiting, and often dangerous if it preserves vulnerabilities. 

---
Q: Isn't this a pretty big decision?

A: **YES, absolutely**. The discussion and development of mitigation strategies must include many different facets of the community - software engineers, economists, researchers, modelers, designers of the current algorithm, etc. This is a big undertaking that must be done deliberately. Each different strategy (or lack of one!) entails different consequences, and we must work together to make sure the community's solution does not introduce other issues / edge cases with financial incentives around mining, fees, etc. 

We should avoid both non-action and careless action.

---
Q: Have you thought about XXX mitigation strategy?

A: Not yet, so please share your ideas with us in Noncesense Research Lab ([website](https://noncesense-research-lab.github.io/), [IRC](https://www.irccloud.com/invite?channel=%23noncesense-research-lab&hostname=chat.freenode.net&port=6697&ssl=1)) or the Monero Research Lab ([website](https://www.getmonero.org/resources/research-lab/), [IRC](https://www.irccloud.com/invite?channel=%23monero-research-lab&hostname=chat.freenode.net&port=6697&ssl=1))
