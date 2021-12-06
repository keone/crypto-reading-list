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

## Programming notes

*(Please feel free to edit and correct any mistakes. Also don't hesitate to remove content if you find it overly verbose / repetitive)*

Goal of this section: condense the above resources and compile useful tidbits (tips, gotchas) to reference in the future.

Let's also take a look at the [Serum](https://github.com/project-serum/serum-dex) code itself.
The escrow tutorial is excellent, but it would be interesting to look at a directly relevant real-world program.

### Framing

It might be useful to begin by mapping new technologies to familiar concepts.
Doing this will help us form the mental model of what exactly we're programming.

At the highest-level, you have two distinct notions of a "program" in Solana:
- the on-chain program (i.e. the smart contract and what is literally known as a **program** in Solana parlance)
- the client or "dApp" that interacts with that Solana program

With respect to tooling:
- `solana-program` is the crate you use for writing on-chain programs
- `solana-client` for clients
- `solana-sdk` for shared utilities

I find this analogous to any client-server interaction model, where the **server** is the on-chain program and the **client** is a dApp that interacts with it.
In fact, **writing Solana programs is a lot like implementing RPC servers, REST endpoints, [insert distributed / IPC thing here]**.

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

### Taking a look at Serum (WIP)

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

One thing that stands out here is the `NewOrder`, `NewOrderV2`, `NewOrderV3` definitions, with the first two `unimplemented`.
It looks a little funky. It appears to be handling for API versioning / compatibility.

I don't know enough to determine whether that's idiomatic or not. Perhaps this is the best way to deprecate operations in an on-chain program?

### Highlights (WIP)

> Solana programs are stateless

> If the program needs to store state between transactions, it does so using accounts

> Programs are constrained to run deterministically, so random numbers are not available

> the basic operational unit on solana is an instruction. an instruction is one call into a program.
> one or more instructions can be bundled into a message. a message plus an array of signatures constitutes a transaction

It's worth reviewing the [on-chain programming docs](https://docs.solana.com/developing/on-chain-programs/overview)
or at least the [FAQ](https://docs.solana.com/developing/on-chain-programs/faq).
It'll save you a debugging headache.

### Gotchas (WIP)
- Don't use std::collections::HashMap. You'll get an obscure error because of the "no-randomness" constraint
  - Reason: `HashMap<K, V, S = RandomState>`. Notice the generic type `S` defaults to `RandomState`
  - It may be possible use by substituting a different, non-random `S` - see the `with_hasher` constructor (*I have not tried this myself*)
- Relatedly, don't use `rand` crate. If a crate you depend on transitively depends on `rand`, follow [this guide](https://docs.solana.com/developing/on-chain-programs/developing-rust#depending-on-rand)
