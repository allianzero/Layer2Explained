# Layer2Explained

Layer 2 scaling explained: rollups, bridges, and fees. Layer 2 (L2) networks sit on top of a Layer 1 (L1) blockchain like Ethereum, handling transactions faster and cheaper while relying on the L1 for security.

> This guide is educational only, not financial advice. Bridges and new L2 networks carry real risks, including bugs, centralised sequencers, and failed withdrawals.

## 1. The scaling problem, in plain language

A Layer 1 blockchain like Ethereum can only process a limited number of transactions per second, because every node must verify every transaction. When demand exceeds that limit, users compete by paying higher fees, and fees spike.

There are two broad responses:

- **Scale the L1 itself:** bigger blocks, faster consensus, sharding. This helps, but it risks making running a node so expensive that only large operators can do it, weakening decentralisation.
- **Move work off the L1 (Layer 2):** process most transactions on a second layer, and use the L1 only to settle and secure the results. This is the L2 approach.

Think of it as a court system: the L1 is the supreme court — slow, expensive, authoritative. L2s are lower courts that handle most cases, with appeals to the supreme court when needed.

## 2. What counts as Layer 1, Layer 2, and sidechain

- **Layer 1 (L1):** the base blockchain with its own consensus and security. Examples: Ethereum, Bitcoin, BNB Chain, Solana.
- **Layer 2 (L2):** a network built on top of an L1 that inherits the L1's security for its final settlement. Its transactions are ultimately anchored to the L1.
- **Sidechain:** a separate blockchain connected to an L1 by a bridge, but with its own consensus and validators. It does **not** inherit the L1's security. If the sidechain's validators fail, its users can lose funds even if the L1 is fine.

This distinction matters: calling something "Layer 2" does not automatically make it as secure as the L1. Check how it settles and who can challenge or verify its results.

## 3. Rollups: the main L2 design

A **rollup** executes transactions off the L1, bundles ("rolls up") many of them, and posts a compressed summary plus enough data to the L1 so anyone can verify or reconstruct the results.

Two kinds dominate:

### Optimistic rollups
- Assume transactions are valid by default ("optimistic").
- Anyone can challenge a result during a **challenge period** (often about 7 days) by submitting a fraud proof to the L1.
- If a challenge succeeds, the invalid result is reversed and the challenger is rewarded.
- Consequence for users: **withdrawals to the L1 take about 7 days**, because the system must wait out the challenge period.
- Examples of this design: Arbitrum, Optimism.

### Zero-knowledge rollups (ZK-rollups)
- Every batch of transactions comes with a **validity proof** (a zero-knowledge proof) that mathematically proves the batch is correct.
- The L1 verifies the proof immediately — no challenge period, no waiting for fraud proofs.
- Consequence for users: **withdrawals are fast**, often minutes to hours.
- Trade-off: generating validity proofs is computationally heavy and, historically, harder to make compatible with general smart contracts, though this is improving rapidly.
- Examples of this design: zkSync, Starknet, Polygon zkEVM.

**Simple comparison:**

| | Optimistic rollup | ZK-rollup |
|---|---|---|
| How validity is ensured | Fraud proofs, if challenged | Validity proof, always |
| Withdrawal time to L1 | About 7 days | Fast (minutes to hours) |
| Computation cost | Lower | Higher (proof generation) |
| Maturity | More battle-tested for general apps | Rapidly maturing |

## 4. Other scaling approaches

- **State channels:** two parties transact privately off-chain and only settle the final result on-chain. Fast and cheap for repeated transactions between the same parties, but awkward for general use. (The Lightning Network for Bitcoin is the famous example.)
- **Validium / data-availability variants:** like a rollup, but transaction data is kept off the L1 (for example, by a separate committee). Cheaper, but you must trust that committee to provide the data if needed. If data is unavailable, users may be unable to withdraw.
- **Plasma (historical):** an early design that largely gave way to rollups, mainly because users could be trapped if data was withheld.

When evaluating any L2, ask: **where is the data, who can verify it, and how do I exit if the operators disappear?** The answers determine whether it is genuinely secure or merely fast.

## 5. Bridges: how assets move between layers

To use an L2, you usually **bridge** assets from the L1: you lock tokens in a contract on the L1 and receive equivalent tokens on the L2. To return, you burn the L2 tokens and unlock the L1 ones.

Bridges are the weakest link in the whole system:

- They hold enormous amounts of locked assets, making them prime hacking targets. Several of the largest crypto hacks in history were bridge hacks.
- A bridge's security depends on its own design: some are smart contracts, some rely on a small set of validators or multisig signers, some on external committees.
- If a bridge is hacked or its operators fail, bridged assets on the destination chain can become worthless, even if both chains themselves are fine.

**Bridge safety rules:**

1. Use well-established, audited bridges, and prefer the L2's official bridge over a random third-party bridge.
2. Bridge only what you need, when you need it. Do not leave large amounts sitting in bridge contracts longer than necessary.
3. Check the destination network and token address after bridging — bridged tokens can have different addresses than native tokens.
4. Understand withdrawal times: optimistic rollups impose about a 7-day wait for native withdrawals, though third-party "fast bridges" can shortcut this for a fee and additional risk.
5. Never bridge from a link in a DM, post, or ad. Use the L2 project's official site.

## 6. Fees on L2s: why they are cheaper, and when they are not

L2 fees are usually much lower than L1 fees, because:

- Many transactions share one L1 settlement, so the L1 cost is split across them.
- Computation happens off the L1, where it is cheaper.

But L2 fees are not zero, and they are not always low:

- Every L2 transaction still pays a share of L1 data costs, so when L1 gas spikes, L2 fees rise too.
- Popular L2s congest as well, and their own fee markets can spike during launches, airdrops, or crashes.
- Bridging, swapping, and interacting with contracts each cost gas, and small transactions can still be uneconomical after all fees.

Always preview the total cost — bridge fee, swap fee, and gas — before confirming, especially for small amounts.

## 7. Sequencers and centralisation: the honest caveat

Most L2s today rely on a **sequencer**: a single operator (or small set) that orders transactions and posts batches to the L1.

- If the sequencer goes offline, new transactions may stall until it returns or a fallback activates.
- A malicious or censoring sequencer could delay or reorder transactions, though it generally cannot steal funds that are secured by the L1's verification — the exact guarantee depends on the design.
- Decentralising sequencers is an active, unfinished project for most L2s.

This is a real, current limitation. An L2 that is fast and cheap today may still depend on one operator for liveness. Check a project's sequencer status and fallback mechanisms before relying on it for large amounts.

## 8. Choosing an L2: a beginner checklist

- [ ] Does it settle to a major L1, and how? (Rollup with proofs, or sidechain with its own validators?)
- [ ] How do withdrawals work, and how long do they take?
- [ ] Where is transaction data stored, and can users exit if operators disappear?
- [ ] Is there a single sequencer, and what happens if it fails?
- [ ] Is the bridge official, audited, and well used?
- [ ] What are the total fees for my whole journey (bridge in, transact, bridge out)?
- [ ] Does the app I want actually run on this L2, with real liquidity — or am I bridging to an empty chain?

## 9. Common mistakes

| Mistake | Consequence | Fix |
|---|---|---|
| Sending L1 tokens directly to an L2 address without bridging | Funds lost or stuck | Always bridge through the proper mechanism |
| Assuming "Layer 2" means "as safe as Ethereum" | Sidechains and data-availability designs have weaker guarantees | Check the settlement and data design |
| Forgetting the 7-day optimistic withdrawal wait | Funds locked for a week when you need them | Plan exits in advance; use fast bridges only knowing the extra risk |
| Bridging through an unaudited third-party bridge | Bridge hack can wipe bridged assets | Prefer official, audited bridges |
| Leaving everything on a new L2 | Sequencer outage or bug freezes access | Keep most funds on the L1 or a mature chain until an L2 proves itself |

## 10. Frequently asked questions

**Do I need a different wallet for L2s?**
Usually no. The same wallet address works across Ethereum and most Ethereum-compatible L2s, but you must switch networks in your wallet and hold the right gas token for each network.

**Are L2 tokens the same as L1 tokens?**
Bridged tokens represent locked L1 tokens, but they are technically different tokens on the L2, with different contract addresses. Native L2 tokens (issued on the L2 itself) are different again. Always check addresses.

**Will L2s replace Ethereum?**
No — L2s depend on Ethereum for settlement and security. They scale Ethereum; they do not replace it. (Other L1s have their own scaling stories, with different trade-offs.)

**Is every "fast, cheap chain" a Layer 2?**
No. Many fast, cheap chains are independent L1s or sidechains with their own validators. Speed alone does not make something a Layer 2.

> Educational content only. This is not financial, investment, or tax advice.
