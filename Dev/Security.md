Smart contract security
========================

## Best practices
### Ethereum
* [Eth smart contract security best practices](https://consensys.github.io/smart-contract-best-practices/)
  (see especially: [known attacks](https://consensys.github.io/smart-contract-best-practices/known_attacks/))
* [Most common smart contract bugs](https://medium.com/solidified/most-common-smart-contract-bugs-of-2020-c1edfe9340ac)

### Solana
* [Solana common pitfalls](https://blog.neodyme.io/posts/solana_common_pitfalls) (Neodyme, Aug 2021)
* [Auditing Solana smart contracts - Part 1 (checklist)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-1-a-systematic-approach-56a434f6c9ed) (Nov 2021)
  * [Part 2 (autoscanning)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-2-automated-scanning-ceb88830ae6d)
  * [Part 3 (penetration testing)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-3-penetration-testing-a315b3bbb2d3)
  * [Part 4 (anchor)](https://medium.com/coinmonks/how-to-audit-solana-smart-contracts-part-4-the-anchor-framework-ef42d944f086)

## Aggregators
* [Rekt leaderboard](https://rekt.news/leaderboard/)
* [SlowMist stats](https://hacked.slowmist.io/en/) - brief summaries of each hack
* [web3isgoinggreat.com](https://web3isgoinggreat.com/) - a (slanted) aggregator of recent hacks

## Notable issues and incidents, explained
* [Poly network hack](https://slowmist.medium.com/the-analysis-and-q-a-of-poly-network-being-hacked-8112a35beb39) (Aug 2021)
  * more commentary [here](https://mudit.blog/poly-network-largest-crypto-hack/)
* [Polygon PoS bridge withdrawal bug](https://medium.com/immunefi/polygon-double-spend-bug-fix-postmortem-2m-bounty-5a1db09db7f1) (Oct 2021)
  * bug allowing repeat withdrawals from the bridge contract
* [Cream Finance 130M hack](https://medium.com/@AndyPavia/swissblock-post-mortem-cream-finance-hack-7c1caff4335c) (Oct 2021)
  * oracle attack on a lending protocol due to a flawed custom oracle for yearn assets
    (see also: [cream hack analysis](https://mudit.blog/cream-hack-analysis/))
* [Compound overdistribution of governance token](https://twitter.com/Mudit__Gupta/status/1443454935639609345?t=sznwoTmp3KMDffaI1vpINA&s=19) (Sep 2021)
  * (see also [this](https://cybavo.medium.com/defi-protocol-compound-suffers-a-90m-reverse-rugpull-after-1-letter-bug-6168071497e2))
* [BadgerDAO Cloudflare exploit](https://badger.com/technical-post-mortem) (Dec 2021)
  * frontend attack arising from Cloudflare bug which allowed attackers to preregister API keys by email address
    without email verification
  * attacker used access to inject malicious scripts that prompted users to authorize
    tokens via MetaMask.
* [Spartan Protocol hack](https://medium.com/amber-group/exploiting-spartan-protocols-lp-share-calculation-flaws-391437855e74) (May 2021)
  * mechanical flaw in calculation of LP share value in a synthetic asset protocol
* [bZx 2020 hack](https://peckshield.medium.com/bzx-hack-full-disclosure-with-detailed-profit-analysis-e6b1fa9b18fc) (Feb 2020)
  * a notable attack against a lending protocol that was fooled into taking a negative-value position
  * [another](https://www.palkeo.com/en/projets/ethereum/bzx.html) great description of this issue
* [MonoX hack](https://twitter.com/Mudit__Gupta/status/1465726874974187524) (Nov 2021)
  * simple logical bug in vAMM protocol
* [MISO vulnerability](https://www.paradigm.xyz/2021/08/two-rights-might-make-a-wrong/) (Aug 2021)
  * batched delegatecall issue
* [Enzyme finance custom oracle bug](https://medium.com/immunefi/enzyme-finance-price-oracle-manipulation-bug-fix-postmortem-4e1f3d4201b5)
  * an issue showing an interesting interaction between a governance token's custom oracle
    and its support for flashloans

## Other
* [Interview with whitehat samczsun](https://medium.com/immunefi/the-u-up-files-with-samczsun-1a9116cf6e74)
* [Oracle vulnerabilities](https://samczsun.com/so-you-want-to-use-a-price-oracle/)
  * samczsun discussion of some famous oracle attacks