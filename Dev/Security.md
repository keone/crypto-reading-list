Smart contract security
========================

## Best practices
* [Eth smart contract security best practices](https://consensys.github.io/smart-contract-best-practices/)
  (see especially: [known attacks](https://consensys.github.io/smart-contract-best-practices/known_attacks/))

## Aggregators
* [Rekt leaderboard](https://rekt.news/leaderboard/)
* [SlowMist stats](https://hacked.slowmist.io/en/) - brief summaries of each hack
* [web3isgoinggreat.com](https://web3isgoinggreat.com/) - a (slanted) aggregator of recent hacks

## Specific incidents explained
* [Poly network hack](https://slowmist.medium.com/the-analysis-and-q-a-of-poly-network-being-hacked-8112a35beb39)
  (more commentary [here](https://mudit.blog/poly-network-largest-crypto-hack/))
* [Cream Finance 130M hack](https://medium.com/@AndyPavia/swissblock-post-mortem-cream-finance-hack-7c1caff4335c) -
  oracle attack on a lending protocol due to a flawed custom oracle for yearn assets
  (see also: [cream hack analysis](https://mudit.blog/cream-hack-analysis/))
* [Compound overdistribution of governance token](https://twitter.com/Mudit__Gupta/status/1443454935639609345?t=sznwoTmp3KMDffaI1vpINA&s=19)
  (see also [this](https://cybavo.medium.com/defi-protocol-compound-suffers-a-90m-reverse-rugpull-after-1-letter-bug-6168071497e2))
* [BadgerDAO Cloudflare exploit](https://badger.com/technical-post-mortem) -
  frontend attack arising from Cloudflare bug which allowed attackers to preregister API keys by email address
  without email verification.  Attacker used access to inject malicious scripts that prompted users to authorize
  tokens via MetaMask.
* [Spartan Protocol hack](https://medium.com/amber-group/exploiting-spartan-protocols-lp-share-calculation-flaws-391437855e74) -
  mechanical flaw in calculation of LP share value in a synthetic asset protocol
* [bZx 2020 hack](https://peckshield.medium.com/bzx-hack-full-disclosure-with-detailed-profit-analysis-e6b1fa9b18fc) - 
  a notable attack against a lending protocol that was fooled into taking a negative-value position