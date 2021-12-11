Solana
=======

## Blockchain
* [Solana Summer](https://www.notboring.co/p/solana-summer) (Aug 2021) -
  an easy-to-read description of Solana's innovations from Packy McCormack (Not Boring newsletter)
* [8 Core Innovations](https://medium.com/solana-labs/7-innovations-that-make-solana-the-first-web-scale-blockchain-ddc50b1defda) (2019)
* [Solana Validator 101: transaction processing](https://jito-labs.medium.com/solana-validator-101-transaction-processing-90bcdc271143)

## Ecosystem
_see also: [DeFi/Serum](../DeFi.md#serum) and [NFT/Solana](../NFT.md#solana)_
* [pentacle.ai/solana](https://pentacle.ai/solana)
* [awesome-solana](https://github.com/paul-schaaf/awesome-solana)

## Serum
* [awesome-serum](https://github.com/project-serum/awesome-serum)
* [rust codebase](https://github.com/project-serum/serum-dex)
* [pyserum](https://github.com/serum-community/pyserum)

## Solana programming
* [ok so wtf is the deal with solana anyway](https://2501babe.github.io/posts/solana101.html) -
  helpful concise honest description of solana programming, good for formulating a mental map
* [Solana Programming (the "escrow tutorial")](https://paulx.dev/blog/2021/01/14/programming-on-solana-an-introduction/) -
  the canonical tutorial on Solana programming by building up an escrow contract.  Warning: somewhat out of date
* [Anchor](https://github.com/project-serum/anchor) - framework for reducing boilerplate by defining a IDL
  connecting your rust program to your typescript code.
  See also the [tutorial](https://github.com/project-serum/anchor/tree/master/examples/tutorial)
  * [angkor wat](https://2501babe.github.io/posts/anchor101.html) - another post from 2501babe.github.io, this time about anchor programming
* [Solana cookbook](https://solanacookbook.com/)
* [solana-program-library](https://github.com/solana-labs/solana-program-library) -
  a collection of examples and reference implementations
* [Metaplex](https://github.com/metaplex-foundation/metaplex) -
  defines the NFT standard on Solana; also standardized the minting process (candy machine).
  See also [community docs](https://docs.metaplex.com/community).
* [Rust programming on Solana](https://brson.github.io/2021/06/08/rust-on-solana) -
  thoughtful opinions and useful tips by a Solana beginner who's very experienced in Rust (core developer)
* [spl token-lending protocol exploit](https://blog.neodyme.io/posts/lending_disclosure) -
  walkthrough of an exploit in spl token-lending protocol identified by Neodyme, as an example of a flaw in a smart contract

## Incidents
_To understand what went wrong, why it went wrong, and how we can learn from it_
* [9-14 network outage: Solana postmortem](https://solana.com/news/9-14-network-outage-initial-overview) -
  official Solana post-mortem
* [9-14 Jump Crypto postmortem](https://jumpcrypto.com/reflections-on-the-sept-14-solana-outage/) -
  post-mortem by Leopold Schabel (Jump Crypto) with additional details
* [12-9 slowdown (no outage)](https://www.coindesk.com/tech/2021/12/10/solana-validators-engineers-grapple-with-blockchain-slowdown-on-public-call/) -
  filler coindesk article for now, will replace with a more useful information source once available


## Programming notes

*(Please feel free to directly edit: fix inaccuracies, expand content, condense sections, etc.)*

Goal of this section: condense the above resources and compile useful tidbits (tips, gotchas) to reference in the future.

Let's also take a look at the [Serum](https://github.com/project-serum/serum-dex) code itself.
The escrow tutorial is excellent, but it would be interesting to look at a directly relevant real-world program.

- [Framing](#framing)
- [On-chain programs](#on-chain-programs)
- [Taking a look at Serum](#taking-a-look-at-serum)
- [The importance of APIs and the bigger picture](#the-importance-of-apis-and-the-bigger-picture)
- [How Serum evolved their API](#how-serum-evolved-their-api)
- [Ownership](#ownership)
- [Highlights](#highlights)
- [Gotchas](#gotchas)

### Framing

It might be useful to begin by mapping new technologies to familiar concepts.
Doing this will help us form the mental model of what exactly we're programming.

At the highest-level, you have two distinct notions of a **program** in Solana:
- an on-chain program, i.e. a "smart contract" and what is literally known as a "program" in Solana parlance
- an off-chain program, i.e. a "client" or "dApp" that interacts with the chain

I find this analogous to any client-server interaction model, where the **server** is the on-chain program and the **client** is a dApp that interacts with it.
In fact, **writing Solana programs is a lot like implementing RPC servers, REST endpoints, [insert distributed / IPC thing here]**.

### Tooling

Solana supports any programming language that can compile to [BPF bytecode](https://docs.solana.com/developing/on-chain-programs/overview).
However, the vast majority of the ecosystem and tooling is in Rust. **All code in this tutorial is written in Rust, unless otherwise specified.**

Relevant Rust crates:
- `solana-program` for writing on-chain programs
  - in all likelihood, you will also depend on existing programs in the [Solana program library](https://github.com/solana-labs/solana-program-library)
  which are published as their own crates, e.g. `spl-token`
- `solana-sdk`, `solana-client` for off-chain programs

### On-chain programs

The `solana-program` crate exposes a `macro` aptly named `entrypoint!`

- It follows the framework over library model, i.e. [they call you](https://en.wiktionary.org/wiki/Hollywood_principle)
- So it expects you to pass in a function with a very specific signature. This function is where you will define your logic that ultimately runs on the chain once deployed (more on that later).
- The inputs to this deployed function are passed in via a [transaction](https://docs.solana.com/developing/programming-model/transactions)

- Solana program flow:
  - Process the input, including deserialization to determine the user's instructions
  - Do your logic / algorithm
  - Return the serialized outputs in a `ProgramResult`, a thin-wrapper around `std::result::Result`.
- See the [helloworld example](https://github.com/solana-labs/example-helloworld/blob/master/src/program-rust/src/lib.rs)

### Taking a look at Serum

Let's take a look at something more complicated than "hello world!". How about a real-world example? The [Serum dex](https://github.com/project-serum/serum-dex) itself!

To begin, let's examine the project structure. It looks like a monorepo: that is, both the on-chain program and client live in the same codebase.

Ok, now let's look for the entrypoint to the on-chain program. Remember that `entrypoint!` macro mentioned earlier? It's being called in `dex/src/lib.rs`:

```rust
#[cfg(all(feature = "program", not(feature = "no-entrypoint")))]
use solana_program::entrypoint;
#[cfg(feature = "program")]
use solana_program::{account_info::AccountInfo, entrypoint::ProgramResult, pubkey::Pubkey};

#[cfg(feature = "program")]
#[cfg(not(feature = "no-entrypoint"))]
entrypoint!(process_instruction);
#[cfg(feature = "program")]
fn process_instruction(
    program_id: &Pubkey,
    accounts: &[AccountInfo],
    instruction_data: &[u8],
) -> ProgramResult {
    Ok(state::State::process(
        program_id,
        accounts,
        instruction_data,
    )?)
}
```

Simple enough. Continuing to follow the white rabbit into the `process` method of `state.rs`:

```rust
#[cfg_attr(not(feature = "program"), allow(unused))]
impl State {
    #[cfg(feature = "program")]
    pub fn process(program_id: &Pubkey, accounts: &[AccountInfo], input: &[u8]) -> DexResult {
        let instruction = MarketInstruction::unpack(input).ok_or(ProgramError::InvalidArgument)?;
        match instruction {
            MarketInstruction::InitializeMarket(ref inner) => Self::process_initialize_market(
                account_parser::InitializeMarketArgs::new(program_id, inner, accounts)?,
            )?,
            MarketInstruction::NewOrder(_inner) => {
                unimplemented!()
            }
            MarketInstruction::NewOrderV2(_inner) => {
                unimplemented!()
            }
            MarketInstruction::NewOrderV3(ref inner) => {
                account_parser::NewOrderV3Args::with_parsed_args(
                    program_id,
                    inner,
                    accounts,
                    Self::process_new_order_v3,
                )?
            }
        ...
```
Interesting! So it looks like they deserialize the instructions then pattern match to figure out what the client wants to do.

As you might expect, these are standard instructions you'd run on an order book.

### The importance of APIs and the bigger picture

One thing that stands out here is the `NewOrder`, `NewOrderV2`, `NewOrderV3` definitions, with the first two `unimplemented`.
It looks a little funky. It appears to be handling for API versioning / compatibility.

Here is a key point: *a Solana program is ultimately an API*. As will all APIs, it's important to think carefully about the interface you're providing.
- What are you going to do when your program logic changes?
- How are you going to add features (API additions)?
- How are you going to deprecate functionality (breaking changes)?

**Ok why is this so important?**

When there is an entire ecosystem of other programs or client apps that depend on your on-chain program, it's _especially_ critical to consider these points.

Imagine a dApp developer building a UI on top of the Serum interface.
How would they feel if Serum was constantly changing the way they implemented order submission,
breaking their app? They'd probably stop using Serum.

In the spirit of decentralization, you should consider it _your_ responsibility to think carefully about your consumers everytime you make a change your program.
Did the public API change? Is it really encapsulated from the client?
A culture of constant breaking changes will damage morale and eventually drive developers away from the ecosystem.

All this said, as with anything there is a trade-off. Solana development is still very new.
Development is rapid and ongoing. That comes hand-in-hand with breaking changes.

In my opinion, the best thing to do is think for yourself: carefully consider the trade-offs and make an informed decision.
Don't blindly break APIs, thinking that's ok just because Solana development is new.
Don't chain yourself to previous iterations of your code either -- if your app has evolved, your codebase should evolve with it.
Solana explicitly spells out some [backwards compatibility guidelines](https://docs.solana.com/developing/backwards-compatibility).


### How Serum evolved their API

Take a look at this pull request releasing Dex V3, a **breaking change**:
https://github.com/project-serum/serum-dex/pull/97. Here's the description:

> The primary change is to immediately match incoming orders against the book,
> instead of first buffering them in the request queue.
> The request queue still exists to reduce breakage, but is always empty.
> Because of this, we're forced to remove support for the old order placement and cancellation instructions,
> since they don't provide the bids and asks accounts which would be necessary in order to process them in the new model.

Briefly, some more context:
- orders used to go into a `request queue`, not the order book directly.
- a "user" (i.e. either another on-chain program or client app) would explicitly send a "crank turn" instruction to Serum
- this "crank turn" pulled requests off the queue and matched them in the order book

As you might guess from both the description and the diff, this is a sizeable API change.

Observations on what they did:
- They implemented a corresponding "V2" or "V3", e.g. `MarketInstruction::CancelOrderV2`

### Stateless pros and cons
_LISP programmers know the value of everything and the cost of nothing_

Solana programs are "stateless". What does that mean? Programs cannot hold state their own state, so it has to be passed in.

The consequence is the same input leads to the same output, _always_. You have **determinism** (caveat: reality is not so simple).
If you're familiar with the "pure functional programming" paradigm, you'll feel right at home.

**Not every chain enforces statelessness. What are the pros / cons to Solana doing this?**

Pros:
- Facilitates parallel processing. Sealevel (Solana runtime) knows exactly what data a program depends on, and therefore what it can safely parallelize.
  - You can also specify whether a dependency is read-only. Readers don't block readers.
- Simplifies the programming model.
  - Referential transparency and the substitution model
  - I can substitute any expression `e` in my program with the result of evaluating `e`, _without changing my program's behavior_

Cons:
- Complicates the programming model
  - State has to be stored in separate accounts which don't live together with your program.
    - These accounts are opaque `Pubkey`s and have to be serialized / deserialized everytime you interact with them.
- APIs for working with accounts are somewhat primitive
  - Think about a function that takes 5 arguments. What goes in `arg #1` and what goes in `arg #5`, again?
  Rich Hickey calls this [place-oriented programming](https://www.youtube.com/watch?v=SxdOUGdseq4).
  - Even worse, it's `varargs` and there's no Rust type-checking because all the accounts are the same `AccountInfo` type.

I believe as Solana evolves, they can address the downsides with higher-level APIs and more user-friendly interfaces.
For example, just some ideas for the `accounts` problem: named arguments, dict-like structures, leveraging the type system (polymorphism of `Account`)

// TODO compare with programming on other chains?


### Accounts

- Accounts are just bytearrays `&[u8]`.
- Accounts have a `data` field for you to store arbitrary information
- Executable accounts are programs
- Go into how Serum parses the account array into the literal information it needs to execute the order instruction


### Highlights

> Solana programs are stateless

> If the program needs to store state between transactions, it does so using accounts

> Programs are constrained to run deterministically, so random numbers are not available

> the basic operational unit on solana is an instruction. an instruction is one call into a program.
> one or more instructions can be bundled into a message. a message plus an array of signatures constitutes a transaction


It's worth reviewing the [on-chain programming docs](https://docs.solana.com/developing/on-chain-programs/overview)
or at least the [FAQ](https://docs.solana.com/developing/on-chain-programs/faq).
It'll save you a debugging headache.

### Gotchas
- Don't use std::collections::HashMap. You'll get an obscure error because of the "no-randomness" constraint
  - Reason: `HashMap<K, V, S = RandomState>`. Notice the generic type `S` defaults to `RandomState`
  - It may be possible use by substituting a different, non-random `S` - see the `with_hasher` constructor (*I have not tried this myself*)
- Relatedly, don't use `rand` crate. If a crate you depend on transitively depends on `rand`, follow [this guide](https://docs.solana.com/developing/on-chain-programs/developing-rust#depending-on-rand)
