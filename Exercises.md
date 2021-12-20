Exercises
=========

A collection of reading questions and exercises to help check for understanding.

## Blockchain mechanics

### Fundamentals

* What's a cryptographic hash? What are its important properties?
* What's a digital signature? Properties?
* They both seem to take some input and scramble it to some output. Can we reuse the same function for both?
* How does public-key cryptography relate?

These are important prereqs, as mentioned in [How the Bitcoin protocol actually works](https://michaelnielsen.org/ddi/how-the-bitcoin-protocol-actually-works/).
<details>

#### Can we reuse the same function?

The answer is no, we can't.
To start, a cryptographic hash function should be irreversible, whereas a digital signature has to be reversible (otherwise how would you determine what was "signed"?)

That's a bit literal, so a higher-level answer is that cryptographic hash functions and digital signature have different requirements,
which in turn requires the mathematical functions they use to have different properties.

Reversibility happens to be one property directly in conflict.
</details>

### Data structures

Describe a blockchain (data structure). What's the use-case?

<details>

A blockchain is a linked list with hash pointers. Its components:

* `.prev` pointer
* Hash of the previous node ("block")

The use-case is tamper detection. If any node in the blockchain is altered, we'll know because the hash will no longer match.
Therefore, you can always check the blockchain is valid by iterating from the head of the list, for every `curr`, hashing `prev`, and checking that equals `curr.prev_hash`
</details>

What's a Merkle tree? What's the use-case?

<details>

Merkle tree is a clever data structure to reduce time complexity by leveraging the fact hashes are composable.
You can combine two hashes `H(h1, h2)` to produce a third hash `h3`. If either `h1` or `h2` change, `h3` changes.

It takes `O(n)` time to verify a block is part of a blockchain, where `n` is the number of blocks.

This is because you have to start from the head and check the hashes until you get to that given block.

Overlay a tree on the blockchain such that all the leaves of the tree correspond to the original blocks.
Each parent is a composite hash, and the parent's parent is a composite of composite hashes, and so on.
[Diagram](https://en.bitcoinwiki.org/wiki/Merkle_tree)

Now, to prove a block is part of the chain, you only need a path through the tree. This is `O(h)`, where `h` is the height of the tree.
Then, by keeping the tree balanced, the complexity is logarithmic.
</details>


### Design a crypto protocol

#### Doxxcoin / Anoncoin
You are the designer of a currency `Doxxcoin`, and you want to implement a protocol `Anoncoin` that allows `Doxxcoin` holders to "anonymize" their coins.

#### Rules
* The definition of **anonymize** is to disassociate a given `Doxxcoin` from all its prior transactions and addresses.
* `Doxxcoin` works like `Bitcoin`, so you can trace all a given coin's addresses using the transaction ledger.
* You have the ability to mint and burn `Doxxcoin`. `Anoncoin` is just what we're calling the protocol.
* Users should still be able transact with `Doxxcoin` after anonymization.
* For the sake of simplicity, disregard units or amounts.

Assume also you have the means to construct a **zero-knowledge proof** that satisfies the predicate `Zk(f, x): ∃x, f(x) ∈ {s1, s2, ... , sn}`
* **Predicate:** "There exists an `x` such that `f(x)` is in the set `S = {s1, s2, ..., sn}`"
* **Zero-knowledge proof:** "I know `x` such that `f(x)` is in `S`, without giving away `x`"
* You can pick what `f` and `x` are
* The Zk-proof can be treated as a black-box and used anywhere in the scheme.

Can you come up with a cryptographic scheme `Anoncoin` that anonymizes `Doxxcoin`?

#### Hint
<details>

commitment, escrow pool, burn to mint, trust-less
</details>

#### Hint 2
<details>

* What if you pooled money together?
* How can you leverage your treasury superpowers? You can issue (mint) or remove (burn) currency from circulation.
* Can this scheme be completely trust-less? How might the Zk-proof help?

</details>

#### Solution
<details>

The key idea is to anonymize by pooling `Doxxcoin` together into a collective `Anoncoin` escrow pool then redeeming `Doxxcoin` from that pool.
The coin you get out is not the same coin you put in.
More importantly, the coin you get out cannot be associated in any way with the coin you put in.

Imagine you put physical cash in an envelope along with some proof of ownership (identity / public key), and add it to a pool of envelopes.
This physical cash has your fingerprints, as well as those of everyone before you (analogous to public keys and tx input / output addresses).

The safest way to ensure anonymity is to put the envelopes in a vault, taking that money out of circulation ("burning") and minting new money to replace it.
You can't just shuffle the envelopes and give back someone else's cash, because that still leaks information about history.

Now you have some freshly-minted money which has no transaction history.
It's still `Doxxcoin` so you can spend it just like you otherwise would.

In order for this to work, the owner / custodian of the pool needs some way to determine whether you have added money before handing out new `Doxxcoin`.

The obvious solution is that the custodian opens the envelope when you ask to redeem `Doxxcoin` from the pool (recall you stored your identity in the envelope to indicate that it belongs to you).
They do this to verify your ownership before crediting you with newly-minted `Doxxcoin`.
The problem is this requires trust: that owner now has linked the newly-minted `Doxxcoin` with your original, and it's no longer anonymous.
Centralization and trust is what we want to avoid!

This is where ZK-proofs come in. The ZK-proof allows you to prove you own one of the envelopes without ever opening it.

The other thing we need to do is model this notion of an "envelope" with cryptography. We can do this using a _commitment_.
Indeed, a commitment is often explained using this envelope analogy.

The `Anoncoin` scheme is as follows:
1. Anonymize:
   1. Create a transaction with two inputs `commit(identity)` and `Doxxcoin`.
   2. `commit(identity)` is added to the pool and the `Doxxcoin` is burned.
2. Redeem:
   1. Construct a Zk-proof `Zk(f, x)`, where `f = commit` and `x = identity`
   2. Then, the proof shows "I know `x`, where `f(x) = commit(identity)` and `f(x)` is in the pool of commitments `{s1, s2, ...}`" (see problem statement)
   3. The `Anoncoin` protocol (pool custodian) verifies `Zk(f, x)`. Because it's zero-knowledge, `Anoncoin` does not learn what `x` is at any point.
   4. `Anoncoin` outputs newly-minted `Doxxcoin` without ever knowing `identity`, thus anonymizing.

The beauty of Zk-proofs is everything can be done with pure cryptography. No trusted third-party needed.

_This scheme is based on the Zerocoin protocol, with details omitted for simplicity._
</details>

#### Follow-up
There is a problem with the scheme described in the solution above. Can you spot it?

#### Hint
<details>

UTXO
</details>

#### Solution
<details>

There's a double-spend problem. I can keep constructing Zk-proofs, and getting newly-minted `Doxxcoin`.

This is because the `Zk-proof` makes it such that my envelope is never opened: in fact the protocol doesn't even know which envelope is mine.
I could have multiple envelopes, so it can't just restrict me to redeem / spend once.

The solution is to include as part of the commitment a unique `mint_id`.
* Anonymize: the `mint_id` parameter is added to `commit` so `commit(mint_id, identity)`
* Redeem: the `mint_id` is also included with the `Zk-proof`. `Anoncoin` protocol keeps track `mint_id` has never been used in a prior `redeem` operation
</details>
