# Ethereum Sidechains vs Layer 2s: What’s the Difference?

# 以太坊侧链（Sidechain）和layer2的区别到底是什么？

```With millions of users joining and new applications launched daily, Ethereum is severely limited by the number of its transactions! Ethereum’s capacity to process transactions, its transaction throughput, is limited to 15 transactions/second, leading it to become increasingly expensive and often too congested for many people to use.```

以太坊上的应用和用户与日俱增，交易越来越多，以太坊的交易处理能力严重地限制了整个生态的发展。目前以太坊的交易处理能力为每秒能处理15笔交易，由于处理能力低，导致以太坊上的交易昂贵，并且经常出现拥堵。

The Ethereum network is the main chain, and all transactions that occur directly on it are “on-chain”, while anything else is considered “off-chain”. It’s some of these off-chain solutions like sidechains and layer 2s that could help Ethereum scale, increasing transaction speed and increasing the amount of transaction data the network can handle. In this article, we’ll show you what sidechains and layer 2 solutions are and how they can help with scalability.   

通常认为，发生在以太坊主链上的交易是链上交易（on-chain）,其他的交易则是发生在链下的交易（off-chain）。这些基于off-chain的解决方案可以帮助以太坊扩容，从而加快交易确认速度，提高网络吞吐量。侧链（Sidechain）和layer2都是基于off-chain的解决方案。这篇文章将介绍这两种方案，并且说明这两种方案的工作机制。

Sidechains and layer 2 Ethereum solutions tackle the problem of helping Ethereum scale. Attempts to try to scale up performance on-chain often lead to trading off either the decentralization or scalability of Ethereum - this is known as the Scalability Trilemma. 

侧链和layer2的方案都力图解决以太坊的扩容问题，但是这两种方案都是以牺牲了一定的去中心化为代价来换取吞吐量的提高。这里会涉及到一个不可能三角，即安全性、去中心化和吞吐量的不可能三角。如下如所示：





Increasing the sophistication of Ethereum layer 1 is not ideal as it would heighten the level of governance overhead for the platform to continuously debate, decide and implement new improvements. Sidechains and layer 2 solutions allow for constant and incremental innovation that improves Ethereum for everyone while maintaining security and decentralization. 

The main difference between sidechains and Ethereum layer 2 solutions is that while layer 2 inherits the security of the main Ethereum network,  sidechains rely on their own security. 


An Ethereum sidechain is a separate blockchain network that runs in parallel to the Ethereum main chain. Sidechains connect to the main chain via a two-way peg system allowing assets to be exchanged between the chains. 

There are two basic types of sidechains, one in which a chain is dependent on the other and another where they are independent. 

When one chain is dependent on another chain like Ethereum, it can be considered the child chain of this parent chain. Typically, the child chain doesn’t create its own assets and derives any assets from transfers from the parent chain.