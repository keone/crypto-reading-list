crypto reading list
====================
_WIP; draft 2021-12-21_

A curated list for getting up to speed on crypto and decentralized networks.

The content on the toplevel page contains what we consider essential reading.
Child pages contain deeper, topic-specific information to review afterward.

The lists here are a work in progress.  Please open a PR/issue here or
reach out to [crypto-research@jumptrading.com](mailto:crypto-research@jumptrading.com)
with any suggestions, or to report any errors.

_Nothing in this repo constitutes financial or legal advice._


Contents
=========
* [Why is crypto important?](#why-is-crypto-important)
* [Blockchain mechanics & innovations](#blockchain-mechanics--innovations)
* [DeFi primitives](#defi-primitives)
* [NFTs & digital identity](#nfts--digital-identity)
* [DAOs](#daos--governance)
* [L1s](#l1s)
* [L2s](#l2s)
* [Trading mechanics](#trading-mechanics)
* [Smart contract programming](#smart-contract-programming)
* [Tools/Analytics](#toolsanalytics)
* [Exercises](#exercises)
* [Directories](#directories)
* [Online courses](#online-courses)


Why is crypto important?
===========================
We'd recommend starting your exploration by trying to understand what problems crypto is trying to solve.

In a few words, we'd say it is:
* enabling a decentralized ledger-based currency system
  * decentralized means, extremely difficult for bad actors to forge transactions taking your holdings
* enabling a decentralized network of computation / decentralized state transition machine
  * decentralized means, extremely difficult for bad actors for enact state changes not defined in sourcecode
* enabling an open network of APIs that can be leveraged to build increasingly advanced apps
* enabling an incentive model for these open networks to grow via crypto tokens

Here's the list:
* [Crypto tokens: a breakthrough in open design](https://cdixon.org/2017/05/27/crypto-tokens-a-breakthrough-in-open-network-design) (2017)
  * tokens as an enabler for alignment between networks and their participants
* [Why decentralization matters](https://cdixon.org/2018/02/18/why-decentralization-matters) (2018)
  * discusses the pattern of new technology progressing from innovation to extraction (from cooperation with their 
    ecosystem to eventual competition). For example consider Apple's transition from early days of encouraging 
    developers to build on iOS, to now charging 30% on all in-store purchases.
    Discusses how crypto solves this by aligning the network with its participants.
* [The true power of DeFi composability](https://medium.com/coinmonks/the-true-power-of-defi-composability-14fe8355e0d0) (Apr 2021)
* [Why I have changed my mind on tokens](https://insights.deribit.com/market-research/why-i-have-changed-my-mind-on-tokens/) (Dec 2020)
  * noted researcher Hasu weighs in on the merits of protocol tokens, during a time when many cynics questioned the need for each project to have its own token
* Optional: pieces discussing arguments for _Bitcoin_, generally as a decentralized store of value:
  * [Bitcoin for the open-minded skeptic](https://www.matthuang.com/bitcoin_for_the_open_minded_skeptic) (2020)
  * [7 things to read about bitcoin](https://www.paradigm.xyz/2020/05/7-things-to-read-about-bitcoin-for-institutional-investors/) (2020)

_More: see in-depth page: [Why](Why.md)_

Blockchain mechanics & innovations
====================================
We think it's essential reading to understand how bitcoin works,
and how smart contracts (pioneered by Ethereum) work.

* Bitcoin:
  * [How the Bitcoin protocol actually works](https://michaelnielsen.org/ddi/how-the-bitcoin-protocol-actually-works/) (2013)
    * describing Bitcoin's mechanics, building it up from first principles
  * OR for fundamentalists, see the [Bitcoin whitepaper](https://bitcoin.org/bitcoin.pdf) (2009)
* Ethereum:
  * [Ethereum whitepaper](https://ethereum.org/en/whitepaper/)
    * Vitalik Buterin's original whitepaper building on bitcoin to get smartcontracts; easier to read than the
      Bitcoin whitepaper; also happens to be good explanation of _Bitcoin_
  * OR for another clear description, see [How Ethereum and Smart Contracts work](https://vas3k.com/blog/ethereum/)

DeFi primitives
===========================
_In-depth page: [DeFi](DeFi.md)_

Next, let's try to understand the major kinds of financial dApps on the blockchain. 
Although there are many types, we'd say the two most common are:
1. **Lending protocol** (a decentralized bank, i.e. a smart contract where you can loan your assets for yield, or
   do borrow while paying interest).  Example: Aave
2. **Decentralized exchange** (most commonly an Automated Market Maker (AMM), a smart contract with two pools of 
   assets that allows swapping from one asset to the other).  Example: Uniswap

A third, which can be thought of as a competitor to (1) of sorts, is:

3. **Decentralized stablecoin issuer** (a protocol allowing you to deposit assets (e.g. Eth)
   and borrow a decentralized stablecoin (minted by the protocol) against it).  We say that it is a
   competitor of sorts to (1) where the lender is the protocol.  Example: MakerDAO

Initial reading material on these categories:
* Lending protocols
  * [Lending and borrowing in DeFi](https://finematics.com/lending-and-borrowing-in-defi-explained/) (Finematics, Nov 2020)
* AMMs
  * [AMMs](https://medium.com/dragonfly-research/what-explains-the-rise-of-amms-7d008af1c399) (Haseeb Qureshi, Jul 2020)
    * explaining the mechanics of the constant-product AMM popularized by Uniswap V2
  * [StableSwap AMMs](https://curve.fi/files/stableswap-paper.pdf) (2019)
    * explaining another AMM formulation for stable pairs of assets, pioneered by Curve.fi
  * [Uniswap V3 whitepaper](https://uniswap.org/whitepaper-v3.pdf)
    * explaining the 'concentrated liquidity' innovation in UniswapV3
    * see also [Uniswap v3: The Universal AMM](https://www.paradigm.xyz/2021/06/uniswap-v3-the-universal-amm/)
      for illustrations of UniV3 emulating specific other functions
* Decentralized stablecoin issuance
  * [Wikipedia article on Dai](https://en.wikipedia.org/wiki/Dai_(cryptocurrency)) provides a good, terse 
    description of MakerDAO and Dai, the stablecoin it issues
* Other essential reading:
  * [Bridges](https://blog.makerdao.com/what-are-blockchain-bridges-and-why-are-they-important-for-defi/)

For much more, see our [in-depth page on DeFi](DeFi.md)

NFTs & digital identity
===========================
_In-depth page: [NFT](NFT.md)_
* [NFTs and a Thousand True Fans](https://future.a16z.com/nfts-thousand-true-fans/) (Chris Dixon, Feb 2021) -
  an argument for NFTs as enabling a better creator economy

DAOs / Governance
===================
_In-depth page: [DAO](DAO.md)_
* [Beginner's guide to DAOs](https://linda.mirror.xyz/Vh8K4leCGEO06_qSGx-vS5lvgUqhqkCz9ut81WwCP2o) (Mar 2021) 
  * examples of what DAOs can do (e.g. shared ownership of a valuable asset, governance)
* [The DAO of DAOs](https://www.notboring.co/p/the-dao-of-daos-5b9) (Mar 2021)

Byzantine Fault Tolerance and Proof-of-Stake algos
===================================================
At this point, we'd recommend learning about alternative smart contract blockchains.

A fundamental design decision in blockchains is the mechanism by which block producers (miners
in Bitcoin and Eth 1.0) come to consensus on the next block.  This problem of doing so in a
distributed system with a variety of actors--some of whom may be sending intentionally confusing
or destabilizing messages to their peers--is the key to establishing consensus and progressing the
blockchain.

Bitcoin and Eth 1.0 accomplish this by proof of work ("Nakamoto consensus"),
but most other blockchains use variants of a different family of algorithms referred to as 
**Byzantine Fault Tolerant (BFT)** algorithms.

* [Understanding Blockchain Fundamentals: Byzantine Fault Tolerance](https://medium.com/loom-network/understanding-blockchain-fundamentals-part-1-byzantine-fault-tolerance-245f46fe8419)
* [Nakamoto Consensus vs BFT](https://medium.com/@yaoshiang/the-interplay-of-nakamoto-consensus-and-byzantine-fault-tolerance-9aeed9d102f5)
* [Tendermint: Byzantine Fault Tolerance](https://knowen-production.s3.amazonaws.com/uploads/attachment/file/1814/Buchman_Ethan_201606_Msater%2Bthesis.pdf)
  * Overview of BFT algorithms and how proof of stake chains can work, in the form of a masters thesis from the co-founder of Cosmos.

L1s
=====

_In-depth page: [L1](L1)_

At this point you can dig deeper into different L1 blockchains.  See in-depth pages below:

* Historical/academic background: [L1/README.md](L1/README.md)_
* [Bitcoin](L1/Bitcoin.md)
* [Ethereum](L1/Ethereum.md)
* [Solana](L1/Solana.md)
* [Terra](L1/Terra.md)
* [Avalanche](L1/Avalanche.md)
* [Polkadot](L1/Polkadot.md)
* [Polygon](L1/Polygon.md)
* [Cosmos](L1/Cosmos.md)


L2s
===============
_In-depth page: [L2](L2.md)_
* [L2 for Beginners](https://gourmetcrypto.substack.com/p/layer-2-for-beginners) - 
  describing a mental model of an L2 as a chain which writes enough state back to Ethereum that
  no one (including the L2's miners/validators) can send back a fraudulent state
* [Almost everything you need to know about optimistic rollups](https://www.paradigm.xyz/2021/01/almost-everything-you-need-to-know-about-optimistic-rollup/) (Jan 2021) -
  builds up the design for optimistic rollups from first principles, addressing various perceived issues as they arise
* [Optimistic rollups: Arbitrum vs Optimism](https://insights.deribit.com/market-research/making-sense-of-rollups-part-2-dispute-resolution-on-arbitrum-and-optimism/)
* [Optimistic rollups vs ZK-rollups](https://limechain.tech/blog/optimistic-rollups-vs-zk-rollups/) -
  a recent assessment of the state of various rollup projects


Trading mechanics
===============
_In-depth page: [TradingDynamics](TradingDynamics.md)_

_In-depth page: [MEV/Arbitrage](MEV.md)_


Smart contract programming
============================
_In-depth page: [Development](Dev)_


Tools/Analytics
===============
_In-depth page: [Tools](Tools.md)_


Exercises
===========
Check your understanding with _[these thought questions and exercises](Exercises.md)._


Directories
===============
_In-depth page: [Other Lists](OtherLists.md)_
* [Skill tree](https://thedailyape.notion.site/Skill-Tree-f5d7691421024090b66f9b07f7384314) -
  Darren Lau's minimal list of things to learn about crypto
* [The Daily Ape - directory](https://thedailyape.notion.site/thedailyape/Directory-c96c0b6727c0433a962e897ef43efb7e) -
  Darren Lau's excellent categorical directory
* [Pentacle](https://pentacle.ai/): directory of Ethereum, Solana, and NFT projects
* [Everest.link](https://everest.link/) - project registry


Online courses
===============
* [Berkeley DeFi](https://berkeley-defi.github.io/f21)
