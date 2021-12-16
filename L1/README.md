# L1 Details

## Child pages
* [Bitcoin](Bitcoin.md)
* [Ethereum](Ethereum.md)
* [Solana](Solana.md)
* [Cosmos](Cosmos.md)
* [Terra](Terra.md)

## Historical / academic background

### Cryptography
* [Awesome cryptography papers](https://github.com/pFarb/awesome-crypto-papers) -
  a curated list of papers on various cryptographic techniques
* [Cryptographic hash function](https://en.bitcoinwiki.org/wiki/Cryptographic_hash_function) -
  the mechanism behind generating a hash
* [Digital signature](https://en.bitcoinwiki.org/wiki/Digital_signature) -
  how signing works. key ideas: asymmetric (public key) cryptography, hard to compute but easy to verify.
* [Merkle tree](https://en.bitcoinwiki.org/wiki/Merkle_tree) -
  important data structure optimizing the storage of hashes in a blockchain

### Distributed consensus
* [Practical Byzantine Fault Tolerance](https://pmg.csail.mit.edu/papers/osdi99.pdf) (Castro and Liskov, 1999) -
  a seminal algorithm for state machine replication / transaction ordering in a distributed system with up to 1/3 malicious nodes
* [Distributed Systems](https://www.cl.cam.ac.uk/teaching/2021/ConcDisSys/dist-sys-notes.pdf) -
  excellent 101-level lecture notes. presumes some comfort with math notation, particularly predicate logic.
  distributed systems concepts can help answer questions like "why is Solana's "Proof of History" an innovation?"
* [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem) -
  fundamental trade-off between staying consistent vs. staying available when there's a network partition, i.e. nodes can't communicate
* [FLP Result](https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf) -
  why distributed consensus is not guaranteed when a node goes down. in practice, this is solved by relaxing the constraints of their model
* [Proof of useful work](https://primecoin.io/about.php) -
  using collective computing power to search for prime number chains. an academic application but interesting concept

### Bonus
* [P vs. NP](https://en.wikipedia.org/wiki/P_versus_NP_problem) -
  deep theoretical question on the very nature of computation: whether easy to verify implies (`=>`) easy to solve
* [Elliptic curve cryptography](https://hackernoon.com/what-is-the-math-behind-elliptic-curve-cryptography-f61b25253da3) -
  in-depth blog post with plenty of diagrams on how elliptic curve cryptography works
