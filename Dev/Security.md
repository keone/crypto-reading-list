Smart contract security
========================

## Education / best practices
### Ethereum
_See [Solidity/Security](Solidity.md#security)_
### Solana
_See [SolanaProgramming/Security](SolanaProgramming.md#security)_
  
## Bughunting challenges:
* [Damn Vulnerable DeFi](https://www.damnvulnerabledefi.xyz/)
* [OpenZeppelin Ethernaut](https://ethernaut.openzeppelin.com/)
* [Capture the Ether](https://capturetheether.com/)
* [Paradigm CTF 2021](https://github.com/paradigm-operations/paradigm-ctf-2021)
* [Smart contract CTF list](https://github.com/PumpkingWok/CTFGym)

  
## Real-life vulnerabilities
### Summaries
* [Rekt leaderboard](https://rekt.news/leaderboard/)
* [SlowMist stats](https://hacked.slowmist.io/en/) - brief summaries of each hack
* [web3isgoinggreat.com](https://web3isgoinggreat.com/) - a (slanted) aggregator of recent hacks

### Notable issues and incidents, explained
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
  * [another](https://lev.liv.nev.org.uk/pub/bzx_debug.txt) great description
* [MonoX hack](https://twitter.com/Mudit__Gupta/status/1465726874974187524) (Nov 2021)
  * simple logical bug in vAMM protocol
* [PancakeBunny reward overmint](https://medium.com/amber-group/bsc-flash-loan-attack-pancakebunny-3361b6d814fd) (May 2021)
  * oracle manipulation attack on PancakeBunny AMM
  * attacker gets way too many BUNNY reward tokens for LPing by unstaking in the middle of a massive mispricing from a flashloan
* [MISO vulnerability](https://www.paradigm.xyz/2021/08/two-rights-might-make-a-wrong/) (Aug 2021)
  * batched delegatecall issue
* [Enzyme finance custom oracle bug](https://medium.com/immunefi/enzyme-finance-price-oracle-manipulation-bug-fix-postmortem-4e1f3d4201b5)
  * an issue showing an interesting interaction between a governance token's custom oracle
    and its support for flashloans

## Other
* [Interview with whitehat samczsun](https://medium.com/immunefi/the-u-up-files-with-samczsun-1a9116cf6e74)
* [Oracle vulnerabilities](https://samczsun.com/so-you-want-to-use-a-price-oracle/)
  * samczsun discussion of some famous oracle attacks