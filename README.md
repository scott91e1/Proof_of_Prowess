# Bitcoin Improvement Proposal

Proof of Prowess

# Preamble

```
BIP: ?
Title: Proof-of-Prowess Consensus
Author: Scott McCallum <enpap.x@hey.com>
Twitter: @enpap_x
Comments-URI: https://reddit.com/r/Proof_of_Prowess
Status: Draft 
Type: Standards Track
Created: 2022-04-20
License: CC0
```

# Abstract
Imagine a new consensus that honors investments in the SHA256 ecosystem while cutting energy consumption by over 99.5%. This new low energy consensus builds a bright bridge to long-term fee stability in the face of the ever-decreasing block rewards.

Proof-of-Prowess uses a digital sled that miners pull for 15 seconds on the hour. The sled is exponential and can "measure " how much equipment the miner has at their disposal. The results run a lottery which determines what mining identity produces the next block.  Metadata is stored in additional blocks referenced by hash hiding them from older code.

# Specification
Proof-of-Prowess uses "hash-pulled" blocks; miners spend 15 seconds on the hour iteratively increasing difficulty on a block while recording the sequence of nonces in OP_RETURN transactions. The block begins with the transactions released by as many "starter transactions" created by the 100 largest registered miners at a pre-determined wall time, such as 42 minutes past the hour. These transactions ensure nobody can start to mine early, gaining an unfair advantage.

The PoP block is then broadcast together with an immortal UXTO referencing the block's hash. The other miners then use this to build a table of all the miners by virtual hash power. This information is averaged over the last two weeks of blocks to determine the number of lottery tickets for each miner. The hash of the hashes of these historical blocks becomes the seed of the random number generator used to draw the winner. On 0, 10, 20, 30, 40, and 50 minutes past every hour, PoP produces a block.

Proof-of-work supports the miners being anonymous; however, most miners tag their blocks. Proof-of-prowess requires the miner to create an identity by crafting a specific transaction that burns dust to a sentinel address. The transaction contains an OP_RETURN output containing the virtual miner tag. A 3rd, 4th, and 5th outputs become rolling UXTOs, representing the miner's identity. The miners do not charge a fee for these special UXTOs allowing them to be "spent" indefinitely.

Where the miner does something to attack the network, such as broadcasting faulty or delayed blocks, the honest miners can cooperate to issue demerits to the attacking miner's virtual identity. The honest miners record a negative hash power against the miners' identity to move the average by significant percentages. A reward of a percentage of the punishment goes to the virtual miner producing the punishment transactions.

Currently, no indication from the miners is transmitted to the network when a minder has accepted a block as valid. With PoP, when the 100 largest honest miners receive a correct block, they create a transaction using their rolling identity UXTO with a 16-byte snippet of the approved block's hash using OP_RETURN. Are these transactions included as transactions in a sub-block and create a rolling checkpoint.

The PoP's rolling checkpoint radically enhances the confirmation process. As soon as the block with your transaction is published and the largest 100 miners release their confirmation transaction, you have 100% confidence in its finality.

In the rare event of a network connectivity split, it is possible to determine what percentage of the network is on "your transactions side." Rather than the longest chain, PoP selects the chain with the highest tally of virtual hash-power based on the rolling checkpoints.

It is essential for the protocol that miners only create a single checkpoint for a given chain height. The miner needs to record the evidence of this misbehavior to collect the reward of 30% of the miner's virtual horsepower. 

Since the miner's identity tasked with producing the next block is known, the likelihood of it coming under a Distributed Denial of Service (DDoS) attack increases. The defense for this is having extensive connections to dozens, if not hundreds of nodes using redundant internet providers and routinely changing the public IP addresses.

Many things happen when the winning miner has not produced the block within 15 seconds of the designated time. The 100 largest honest miners would release a punishment transaction for the missing block, and these punishment transactions form the permission of the block mined by the fallback miner(s) for the given time slot.  

There are many deterministic ways to select who the fallback miners will be, such as giving it to the miner of the previous blocks or another lottery drawing with a modified PRNG seed. Miners that create these punishment blocks collect rewards that increase the miner's virtual horsepower as some proportion of the fine against the faulty miner. This "civil forfeiture" of mining horsepower ensures that Sybil attacks are ineffective when contrasted to a fixed value reward.

Where the internet fragments and no random nodes can bridge, PoP is well behaved. The miners can only produce a single confirmation transaction for a given height without being punished. When the network re-joins, the chain with the highest virtual horsepower tally is selected, no matter the designated miner's side. Since each miner declares the first valid block they received, the proportions of the network split would become calculatable.  

Should a network split occur, the chain's confidence can be determined and published. When the number is 100% or any number over 51%, the user is confident their transaction is in the winning chain.

## Motivation
Motivation is critical for BIPs that want to change the Bitcoin protocol. It should clearly explain why the existing protocol is inadequate to address the problem that the BIP solves.

Energy consumption.

## Rationale
The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work. The rationale should provide evidence of consensus within the community and discuss important objections or concerns raised during discussion.

The design deliberately changes as little as possible. No new concepts are needed; just transactions, burn addresses, OP_RETURNS, blocks, hashes, and difficulties.

The data needed for PoP lives in the OP_RETURN area within transactions that include no mining fees. These immortal UXTOs ensure that no private key is used multiple times.

In the primary block, a single transaction (following the coinbase transaction) with an OP_RETURN of the hash of the secondary control block is present. This secondary block contains the 100 largest miners' positive confirmations of the previous block. This secondary block can also contain references to more blocks that contain additional control information, honoring the 1MB block size cap.

## Backwards compatibility
All BIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The BIP must explain how the author proposes to deal with these incompatibilities.

The most significant backward compatibility issue is random difficulty on the main chain. Once PoP kicks in, the blocks will have a magic nonce producing the random difficulty as measured by the leading zeros for the block's hash.

The second transaction following coinbase will trace back to the identity transaction (one that includes dust to a sentinel burn address) of the miner winning the lottery to produce the block.

## Reference implementation
The reference implementation must be completed before any BIP is given status "Final", but it need not be completed before the BIP is accepted. It is better to finish the specification and rationale first and reach consensus on it before writing code. The final implementation must include test code and documentation appropriate for the Bitcoin protocol.

None

# Benifits

## Energy Consumption
PoP running for 15 seconds on the hour reduces energy consumption by 99.5%.

## Hardware Longentivity
Older, less energy-efficient equipment can still be profitable when only used for 15 seconds per hour. PoP will reduce the amount of equipment sent to landfills and allow the equipment depreciation over many years.

## Bursty Power Consumption
Systems that supply intermittent power, such as UPS and super capacitors, can be used to run the miners. Electric cars are one example as they contain high-capacity batteries. Adding SHA256 chips to the die for CPU and GPU is another viable option to take advantage of the energy and cooling systems.

## Massive CPU Mining Systems
Where a company has millions of CPU-based machines, they can together create a large virtual miner since the load is only present for 15 seconds.

## Block Production
Blocks are created precisely at 0, 10, 20, 30, 40, and 50 minutes past the hour. The 100 largest miners will generate a punishment transaction if the block does not appear within 15 seconds. The fallback miner includes these transactions in a sub-sub-block as evidence to be allowed to produce the block.

## Dynamic Checkpoints
The 100 largest miners produce the checkpoint transaction as soon as they have downloaded and verified the block. Blockchain users can then be confident in the finality of the transactions contained in the block, as a fork is not possible.
