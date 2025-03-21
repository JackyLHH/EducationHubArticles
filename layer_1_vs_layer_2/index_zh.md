---
title: 'Layer 1 与 Layer 2 区块链网络有哪些差异？'
coverImage: 'images/image2.png'
category: 'Blockchain, Nervos'
date: '2023-05-25T16:00:00.000Z'
---

![alt_text](images/image1.png 'image_tooltip')

在区块链网络中，Layer 1 是指交易最终结算的基础层区块链。而 Layer 2 则是构建在 Layer 1 区块链之上的可扩展性解决方案，旨在提高 Layer 1 的交易吞吐量。常见的 Layer 1 区块链包括比特币、以太坊和 [CKB（Common Knowledge Base，共同知识库）](https://www.nervos.org/knowledge-base/nervos_overview_of_a_layered_blockchain)，而 Layer 2 网络则包括闪电网络、Optimism 和 Nervos 生态的 [Godwoken](https://www.nervos.org/godwoken)。

可以用现实世界中的 Fedwire 和支付处理商（如PayPal或Stripe）作为类比，来解释 Layer 1 和 Layer 2 的区别。与 Layer 1 一样，Fedwire 对银行转账进行最终的结算，而支付处理商只是处理许多付款的中介，需要将处理后的付款转给银行进行最终的结算。

## Layer 1 区块链和可扩展性解决方案

Layer 1 和 “区块链” 这两个术语有时可以互换，因为它们描述的是同一个概念：一个公共的、不可篡改的分布式交易账本，在分散的、分布式的计算机节点网络之间复制和维护。

与由单个可信机构维护的中心化数据库不同，区块链是由许多不相关实体以无需信任的方式维护的分布式数据库。区块链执行的三个主要任务包括执行交易、保证数据可用性以及在相关方之间就区块链的真实状态达成共识。

然而，不谈太多细节，区块链在执行这些任务时会面临通常被称为区块链的 “[不可能三角](https://www.nervos.org/knowledge-base/blockchain_trilemma)” 问题。这意味着区块链网络无法同时实现安全性、可扩展性和去中心化。因为区块链的可扩展性与节点的硬件和带宽需求呈负相关，要实现更高的交易吞吐量和数据可用性就需要更昂贵的硬件，从而导致参与节点减少和中心化现象加强。

换句话说，试图通过增加每个区块的数据量或加快区块确认速度来扩展链上规模的区块链，必然会牺牲去中心化，损害其安全性和其他重要属性，包括抗审查能力、抗捕获能力，甚至是不可篡改性。另一种扩展 Layer 1 的方法是通过分片，将区块链状态分成不同的数据集，称为 “分片”，使网络可以并行处理。目前，多个区块链项目，如以太坊和 Tezos，正在探索这种 Layer 1 可扩展性解决方案，但其开发仍处于实验阶段。

所以，在不牺牲安全性和去中心化的情况下，将部分交易转移到单独的 Layer 2 网络是扩展区块链的有效方法。 

## 什么是 Layer 2 区块链网络？

Layer 2 是一个专业术语，用于描述建立在现有区块链协议之上的网络或技术。Layer 2 网络的目标是通过将一些交易转移到另一个系统架构从而提高区块链的可扩展性和效率。Layer 2 网络可以批量处理交易，然后将它们作为单个交易发布到底层区块链进行最终的结算。这种解决方案减少了主区块链上的处理负荷，提高了可扩展性。

目前存在多种类型的 Layer 2 解决方案，其中包括 Optimistic Rollup、ZK Rollup 和状态通道。

### Optimistic Rollup

[Optimistic Rollup](https://www.nervos.org/knowledge-base/what_are_optimistic_rollups) 是一种区块链 Layer 2 解决方案，用于提高可扩展性。其原理是将多个交易捆绑成一个交易，在主区块链之外进行处理。这些交易被”乐观地”处理，即在所有捆绑交易都有效的假设下进行。这样可以加快处理时间并减轻主区块链的计算负载。

要使用 Optimistic Rollup，用户需要通过跨链桥合约将其原始资产锁定在 Layer 1 上。然后，在 Layer 2 上的智能合约里创建相应的 wrapped token，以 1:1 的比例映射到之前在 Layer 1 上锁定的资产。用户在 Layer 2 上使用这些 wrapped token 进行交易时，交易会捆绑在一起进行批量处理，只有交易数据会回传到 Layer 1。

当用户想要把原始资产从 Layer 1 上取回时，他们需要将之前在 Layer 2 上铸造的 wrapped token 发回智能合约，并等待数天或数周的挑战期。这个挑战期是必要的，以便验证者有足够的时间在主区块链进行不可逆转的结算之前对 rollup 上的可疑交易提出异议。

Nervos 的 Layer 1 区块链，即 CKB，使用名为 Godwoken 的 Optimistic Rollup 来提高 CKB 的可扩展性。

### ZK Rollup

[ZK Rollup](https://www.nervos.org/knowledge-base/zk_rollup_vs_optimistic_rollup) 也是一种区块链 Layer 2 可扩展性解决方案，它利用了称为零知识证明的加密技术来确保交易的有效性。零知识证明可以让证明者向验证者证明某个声明的有效性，而无需透露额外信息。

在 ZK Rollup 上进行交易时，这些交易会与其他交易捆绑在一起，组成一笔交易。然后，将这笔交易连同有效性证明发布到 Layer 1 进行最终结算。这意味着交易不受等待期或争议解决程序的限制，因为它们在数学上已被证明有效。与 Optimistic Rollup 相比，ZK Rollup 更安全，因为它不依赖其他参与者的诚实来确保交易有效性。

简而言之，Optimistic Rollup 计算成本更低，但需要挑战期来确保交易的有效性；ZK Rollup 使用零知识证明验证交易，提供了更高的安全性和隐私性，但计算成本更高。

### 状态通道

状态通道（State Channel）是一种 Layer 2 可扩展性解决方案，让用户能够在不涉及主区块链的情况下进行链下交易。其原理是通过在两方之间创建虚拟通道来进行工作，每次双方进行交易时通道的状态都会更新。这些交易是在链下进行的，所以不会立即记录在主区块链上。交易由通道中涉及的各方进行存储和验证，并且仅在通道关闭时或者一方想要从通道中提取资金时，才会记录到主区块链上。这种方法减少了主区块链需要处理的交易数量，让交易速度更快、成本更低。

其中最知名的状态通道实践是闪电网络，它是基于比特币的 Layer 2 可扩展性解决方案。闪电网络通过建立支付通道网络在用户之间实现即时且成本极低的比特币交易。

## Layer 1 与 Layer 2，哪个更好？

对于 Layer 1 与 Layer 2 哪个更好，并没有直接的答案，因为它们各自为不同的目的提供服务，并具有各自的优势和局限性。

Layer 1 区块链提供了卓越的安全性和最终或者说不可逆的交易结算，使其成为需要安全性和透明度的用例的理想选择。与此同时，Layer 2 网络旨在解决 Layer 1 区块链的可扩展性问题。Layer 2 解决方案，如状态通道、[侧链](https://www.nervos.org/knowledge-base/sidechains_unlocking_the_potential)和各种 rollup 方案，通过将一些交易从 Layer 1 转移到 Layer 2 来提供更快速、更便宜的交易。此外，它们还可以支持更复杂的交易，比如原子交换和跨链互操作性。

总之，Layer 1 和 Layer 2 各有优势和劣势，它们的适用性取决于具体的用例和需求。某些应用可能需要 Layer 1 的极高安全性和去中心化特性，而其他应用可能会从 Layer 2 解决方案的速度和灵活性中受益。

# 那 Layer 3 呢？

Layer 3 网络，也称为应用链，它运行在 Layer 2 解决方案之上，为特定应用程序或用例提供特定功能和服务。Layer 3 网络可以被看作是 Layer 2 网络的子集，其中 Layer 2 解决了 Layer 1 区块链的可扩展性问题，而 Layer 3 网络构建在 Layer 2 之上，并且提供了额外的服务和功能。

![alt_text](images/image3.png 'image_tooltip')

来源：[https://vitalik.ca/general/2022/09/17/layer_3.html](https://vitalik.ca/general/2022/09/17/layer_3.html)

Layer 3 网络可用于各类应用，例如 DeFi、NFT、供应链管理和游戏。Layer 3 网络搭建在 Layer 2 解决方案之上，因此可以充分利用 Layer 2 解决方案带来的可扩展性、互操作性和成本优势，同时还为特定用例提供更专业、定制化的功能。

## Nervos 的分层架构

Nervos 是一个模块化的区块链网络，采用分层架构，利用多种可扩展性解决方案来实现高吞吐量。具体来说，Nervos 利用名为 Axon 的专有侧链解决方案，使开发人员能够在几分钟内在 [CKB](https://medium.com/nervosnetwork/nervos-ckb-in-a-nutshell-7a4ac8f99e0e) 上推出自己的高吞吐量、可互操作且安全的侧链。得益于 CKB 的灵活性，Axon 侧链支持所有共识机制和密码原语。此外，CKB 还支持各种 Layer 2 可扩展性解决方案，包括各种 Rollup 和状态通道。其中，名为 Godwoken 的 Optimistic Rollup 已经上线，而状态通道仍在进行中。

