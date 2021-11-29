_Not financial or legal advice.  Please contact [crypto-research@jumptrading.com](crypto-research@jumptrading.com)
for suggestions or to report errors._

# MEV

MEV is a measure of the profit a block producer (miner, validator, etc) can make due to their ability to
arbitrarily include, exclude, or re-order transactions within a block.

Recommended:
* [Interview with a Searcher](https://uncommoncore.co/29-interview-with-a-searcher-with-mev-senpai-and-hasu/) - 
  the single best discussion of MEV and how searchers (arbitrageurs) uncover arbs and submit them to the
  flashbots private relay.  Includes discussion of the role flashbots plays in 'democratizing' MEV and
  providing DOS protection for miners.
* [awesome-MEV-resources](https://github.com/0xalpharush/awesome-MEV-resources) - curated list of MEV articles/podcasts/etc
  
History:
* [Ethereum is a dark forest](https://www.paradigm.xyz/2020/08/ethereum-is-a-dark-forest/) (Dan Robinson, Aug 2020)
  an example of a frontrunning incident to illustrate the adversarial nature of arbitrage on the blockchain
* [Flash Boys 2.0](http://www.pdaian.com/flashboys2.pdf) - 
  Phil Daian's seminal paper on frontrunning and backrunning prior to the introduction of flashbots
* [MEV SOTW Feb 2021](https://research.paradigm.xyz/MEV) - Charlie Noyes' assessment of the state of MEV
* [MEV Roast](https://www.youtube.com/watch?v=krlAqKsdLkw) - virtual conference on MEV

Tools
* [explore.flashbots.net](http://explore.flashbots.net) - 
  dashboard of recent transactions going thru flashbots + estimate of profit (not very accurate)