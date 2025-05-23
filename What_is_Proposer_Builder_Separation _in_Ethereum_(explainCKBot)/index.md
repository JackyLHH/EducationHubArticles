---
title: 'What is Proposer-Builder Separation (PBS) in Ethereum?'
coverImage: 'images/image1.png'
category:
subtitle: 'The premise behind PBS is simple but profound: instead of having a single actor—currently, a validator—both build and propose new blocks to the network, the process is split into two discrete roles, each performed by different specialized participants'
date: '2025-01-15T15:00:00.000Z'
author:
- github:explainCKBot
---

Since Ethereum’s earliest days, the design and operation of the network’s consensus layer have centered on the notion of producing blocks—bundles of transactions that form the canonical chain. Over time, this process has evolved from [Proof-of-Work (PoW)](https://www.nervos.org/knowledge-base/pow_vs_pos_unravelling_(explainCKBot)) mining to Proof-of-Stake (PoS) validation, drastically reducing the network’s energy consumption. Yet, even in the post-Merge era, challenges remain around how blocks are constructed, who assembles them, and how best to mitigate centralizing forces driven by transaction ordering and value extraction from the [mempool](https://www.nervos.org/knowledge-base/mempool_in_cryptocurrency_(explainCKBot)).

One of the most researched and discussed concepts in the Ethereum ecosystem today is **Proposer-Builder Separation (PBS)**. The premise behind PBS is simple but profound: instead of having a single actor—currently, a validator—both build and propose new blocks to the network, the process is split into two discrete roles, each performed by different specialized participants. This structural adjustment promises better decentralization, improved censorship resistance, enhanced fairness, and more robust protection against centralizing pressures such as [Maximum Extractable Value (MEV)](https://ethereum.org/en/developers/docs/mev/).

In this article, we will explore what PBS is, why it matters, how it might be implemented, and what the broader implications are for the Ethereum ecosystem and its participants.


## The Evolution of Ethereum’s Block Production


### From Mining to Validating

Under Ethereum’s original Proof-of-Work consensus, miners competed to find solutions to cryptographic puzzles, and the winner constructed the new block. The composition of that block—determining transaction order, which transactions to include, and how to maximize fees—was solely in the miner’s hands. With the network’s transition to Proof-of-Stake in September 2022 ([the Merge](https://ethereum.org/en/roadmap/merge/)), the role of proposing new blocks shifted from energy-intensive miners to [validators](https://www.nervos.org/knowledge-base/difference_between_crypto_miners_validators_(explainCKBot)), a set of users who stake ETH to secure the network. Validators are randomly selected to propose blocks, with honest participation enforced economically via [slashing](https://www.nervos.org/knowledge-base/slashing_in_PoS_(explainCKBot)) conditions on misbehavior.


### Increasing Complexity in Block Building

While the act of proposing a block can be straightforward (selecting transactions from the mempool and signing off on the block), complexity arises when optimizing the ordering and selection of transactions to maximize the validator’s revenue. This optimization can involve capturing MEV—value extracted by strategically ordering or censoring transactions. Sophisticated block-building and MEV extraction strategies are often beyond the capabilities of smaller, less-resourced validators. Consequently, specialized block builders and services emerged, aggregating MEV opportunities and redistributing them to validators through out-of-protocol arrangements or "relays" like [MEV-Boost](https://github.com/flashbots/mev-boost).


## What Is Proposer-Builder Separation?

Proposer-builder separation aims to divide the block production pipeline into two roles more formally:

* **Proposers (Validators):** Responsible for *selecting* a pre-built block to finalize on-chain. They present the network with the chosen block’s header, ensuring it becomes part of the canonical chain.
* **Builders:** Specialist entities that *construct* candidate blocks from a pool of available transactions. Builders optimize for maximum value—factoring in fees, MEV, and possibly compliance or censorship resistance—and then offer these fully assembled block proposals, along with a bid, to proposers.

Under PBS, validators no longer need to be concerned with complex MEV extraction techniques. Instead, they can simply choose from among multiple builder-supplied blocks the one that is most profitable, beneficial, or meets certain policy criteria. A competitive marketplace of builders emerges, each striving to offer proposers the most attractive deals.


### Achieving Separation

In its simplest conceptual form, PBS ensures that no single entity constructs and finalizes a block. This splits the responsibilities, allowing proposers to remain relatively simple and trust-minimized while highly specialized and possibly more centralized builders handle the intricate optimization tasks. Implementing PBS is a non-trivial undertaking—it requires protocol-level changes to define roles, communication channels, and incentive mechanisms. The groundwork is being laid through ongoing Ethereum research and experiments with intermediary solutions such as MEV-Boost.


## Why Does Ethereum Need PBS?

Currently, an economically rational validator might feel compelled to run specialized block-building software or rely on third-party block-building services to remain competitive. This fosters centralization, as professionalized block builders can gain substantial market share, pushing out smaller participants. By establishing PBS, the role of proposers becomes simpler and more uniform—validators only need to pick the best-prepared block, rather than optimize its contents. This lowered complexity makes running a validator more accessible and decentralization-friendly.


### MEV Mitigation and Fairness

MEV—Maximum Extractable Value—refers to the extra profit a block producer can make by manipulating transaction ordering. While MEV itself isn’t inherently “good” or “bad,” its presence can encourage extractive and potentially harmful behaviors. Without PBS, MEV accumulates with whoever builds the block. Centralized MEV extraction services can become kingmakers, wielding outsized influence over transaction inclusion and ordering. PBS distributes MEV extraction across multiple builders who must compete for proposer selection, reducing the monopolistic potential and mitigating some of MEV’s centralizing tendencies.


### Censorship Resistance

Censorship at the block-building level can occur if the party assembling blocks chooses to exclude certain transactions. By having multiple builders compete to produce blocks, any builder refusing to include certain transactions risks being outbid by other, more inclusive builders. If no single builder has a monopoly, it becomes harder to systematically censor transactions. Proposers, in turn, are incentivized to choose the most profitable block, which likely includes all high-value transactions, making long-term censorship less sustainable.


### Enhanced Network Resilience

A resilient Ethereum network should not rely on any single point of sophistication. The current paradigm, which encourages validators to engage in complex MEV strategies, risks concentrating expertise and infrastructure into a small number of operators. By offloading complexity to builders, PBS ensures that validators can remain simpler while builders—competing in a free market—can explore various strategies. If one builder fails, or attempts censorship, another can quickly replace them. This dynamic fosters a more robust, failure-tolerant ecosystem.


## Real-world Analogies and Precedents

The concept of role specialization is not new. Financial markets, for example, separate the roles of exchanges (which produce order books) and market makers (which provide liquidity) from brokers (who connect customers to these opportunities). This separation of concerns allows each participant to focus on their core competency and makes the overall market fairer and more transparent.

In cryptocurrency ecosystems, PBS can be seen as a formalization of the existing relationships between validators and third-party block builders. Current add-on services like MEV-Boost function as a preliminary form of PBS, where the validator trusts an external party’s constructed block. The PBS research aims to bake this pattern into the Ethereum protocol itself, ensuring a trust-minimized, censorship-resistant, and transparent marketplace for block construction.


## Challenges and Open Questions for PBS Implementation


### Protocol-Level Implementation of PBS

One of the key challenges for implementing PBS is making it a first-class feature of the Ethereum protocol. Today, the quasi-PBS environment is achieved through off-chain agreements and middleware like MEV-Boost relays. Formalizing PBS would involve modifying the consensus protocol to define how builders submit blocks, how proposers select them, and how bids and fees are distributed. This is a complex process, and protocol researchers are still working out the details.


### Incentive Alignments in the PBS Framework

For PBS to work optimally, all actors must have the right incentives. Proposers must trust that builders will provide valid, profitable blocks; builders must trust that proposers will not front-run or steal their block construction strategies. Cryptographic commitments, sealed bids, or even auctions may be needed to ensure fairness and prevent opportunistic behavior. Designing such incentive-compatible mechanisms is a delicate balancing act.


### Market Centralization Concerns with Block Builders

While PBS aims to reduce centralization, it doesn’t entirely remove the possibility that a few large builders might dominate the builder marketplace. If one builder becomes too successful, could they exert undue influence? Encouraging competition and ensuring that the market remains accessible to new entrants will remain an ongoing challenge. Potential solutions might involve strict protocol rules, reputational systems, or community-led incentives to maintain a diverse builder ecosystem.


## The Road Ahead

Proposer-builder separation represents one of the most significant potential changes to Ethereum’s consensus mechanism since the Merge. Although the concept is still in the research and early design phases, initiatives like MEV-Boost have provided valuable lessons, testing how validators and block builders interact under more formalized roles.

In the coming years, we can expect more academic research, [Ethereum Improvement Proposals (EIPs)](https://github.com/ethereum/EIPs), community discussions, and prototype implementations that explore the technical intricacies and economic dynamics of PBS. The ultimate goal is to arrive at a protocol-level solution that:

* Minimizes the complexity required of validators.
* Preserves strong censorship resistance.
* Ensures a healthy, open, and decentralized marketplace for block building.
* Continues to enhance Ethereum’s security and economic properties.

If successful, proposer-builder separation will not only make Ethereum more secure and equitable but also provide a blueprint for other blockchains navigating similar challenges of block construction, MEV mitigation, and sustainable decentralization.

## Conclusion

Determinism is the silent, unobtrusive force enabling blockchains to function as trusted, decentralized systems. By ensuring that all participants reach the same conclusions from the same inputs, it establishes the unwavering consistency that justifies blockchain’s transformative claims. At the execution level, determinism enables verifiable smart contracts and tamper-proof digital records. At the economic and governance levels, it frames debates around PoW and PoS systems, exposing how subtle design choices shape the fairness, resilience, and decentralization of the network.

In the case of Ethereum and other blockchains which utilize PoS, the block proposer is selected deterministically, providing that actor full discretion over the block's contents, and thus any MEV opportunities.

As blockchains evolve, determinism remains a non-negotiable foundation. It does not preclude growth, innovation, or adaptation. Instead, it guides these processes, offering a reliable compass that keeps development aligned with the core promise of trustlessness.

By acknowledging its importance and refining our methods, we ensure that the future of blockchain technology remains bright, stable, and universally verifiable. In the interplay between determinism and innovation, we find the key to sustaining trust and unlocking the full potential of decentralized computational systems.
