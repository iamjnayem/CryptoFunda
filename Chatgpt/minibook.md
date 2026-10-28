# Crypto Bible for Payment Integration Engineers
## A practical book on crypto assets, blockchains, wallets, fees, trading, and payment businesses

**Author:** Md Jannatul Nayem  
**Focus:** Crypto payment integration, payment orchestration, merchant settlement, and processor engineering  
**Primary stack:** Laravel / PHP and Go  
**Version:** 1.0 — October 2026

> **Purpose of this book**
>
> This is a developer-first guide, not an investment guide. Its goal is to help a payment engineer understand what a crypto asset actually is, why thousands of assets exist, how a payment moves from a customer's wallet to a merchant's account, how fees and settlement work, and where a payment orchestrator or processor creates value.
>
> The chapter structure follows the 15-chapter outline in the supplied draft and expands it into a practical reference. Examples are illustrative; exact network rules, provider IDs, fees, confirmation policies, and legal obligations must be checked against the relevant chain, provider, and jurisdiction.

---

# Table of Contents

1. [Blockchain: The Foundation](#chapter-1--blockchain-the-foundation)
2. [Coin vs Token](#chapter-2--coin-vs-token)
3. [How to Classify Crypto Assets](#chapter-3--how-to-classify-crypto-assets)
4. [What Is a Crypto Network?](#chapter-4--what-is-a-crypto-network)
5. [Network Fee vs Gas Fee](#chapter-5--network-fee-vs-gas-fee)
6. [Why Are There So Many Cryptocurrencies?](#chapter-6--why-are-there-so-many-cryptocurrencies)
7. [Stablecoins and Fiat-Pegged Assets](#chapter-7--stablecoins-and-fiat-pegged-assets)
8. [Crypto Wallets and Custody](#chapter-8--crypto-wallets-and-custody)
9. [Transaction Lifecycle and Payment States](#chapter-9--transaction-lifecycle-and-payment-states)
10. [The Crypto Business Ecosystem](#chapter-10--the-crypto-business-ecosystem)
11. [Crypto and Trading](#chapter-11--crypto-and-trading)
12. [Three Business Perspectives: Merchant, Orchestrator, Processor](#chapter-12--three-business-perspectives)
13. [Compliance, Risk, and Operational Controls](#chapter-13--compliance-risk-and-operational-controls)
14. [Developer Handbook: Modeling, APIs, and Production Safety](#chapter-14--developer-handbook)
15. [Glossary and Learning Roadmap](#chapter-15--glossary-and-learning-roadmap)

---

# Chapter 1 — Blockchain: The Foundation

## 1.1 The fintech metaphor: a shared ledger

Imagine PayMid and several payment companies need to agree on a record of payments. In a traditional system, one company usually owns the authoritative database. Other parties trust its API, audit records, and settlement reports.

A public blockchain changes the trust model. Many independent computers maintain and verify a shared ledger according to a common protocol. Instead of asking one company, “What does your database say?”, a participant can verify the chain's public transaction history and rules.

A blockchain does **not** automatically make every business process trustworthy. It provides a particular way to record and validate state transitions. Wallet security, smart-contract correctness, exchange solvency, stablecoin reserves, and merchant operations are separate questions.

## 1.2 The core building blocks

- **Transaction:** A proposed state change, such as transferring BTC or calling a token contract.
- **Block:** A batch of transactions, plus metadata used by the protocol.
- **Block header:** Metadata that can include a timestamp, a reference to a previous block, a transaction commitment, and consensus-related data.
- **Hash:** A fixed-size digest computed from input data. A small change in input normally produces a very different digest.
- **Chain:** Blocks reference previous blocks, making changes to old history detectable and expensive to carry out.
- **Node:** Software that participates in a network. A node may validate transactions and blocks, relay data, or provide RPC access.
- **Consensus:** Rules and mechanisms by which network participants agree on the valid chain or state.
- **Validator / miner:** A participant that helps propose or secure blocks, depending on the consensus design.
- **Ledger state:** The current balances, unspent outputs, contract storage, and other information implied by valid transactions.

“Immutable” is useful shorthand, but not absolute magic. A chain can experience reorganizations, protocol upgrades, governance interventions, or exceptional recovery actions. For payment systems, treat finality as a chain-specific operational property rather than a universal yes/no promise.

## 1.3 How a transaction becomes part of history

A simplified flow is:

1. A wallet constructs a transaction.
2. The wallet signs it using a private key or another authorized signing mechanism.
3. The transaction is broadcast to a node.
4. Nodes check protocol rules, such as signatures, balances or inputs, nonce rules, and transaction format.
5. A valid transaction may wait in a pending pool, often called a **mempool**.
6. A miner or validator includes it in a block.
7. Other nodes validate the block and update their view of the ledger.
8. Additional blocks or chain-specific finality rules increase confidence that the transaction will remain canonical.

Not every network has a mempool that behaves exactly like Bitcoin or Ethereum. Not every chain defines “confirmation” in the same way. Some systems have probabilistic finality; others have explicit finality mechanisms.

## 1.4 Proof of Work and Proof of Stake

### Proof of Work (PoW)

Miners compete to produce a valid block by performing computational work. Rewriting history generally requires substantial resources and must compete with the honest chain. Bitcoin is the best-known example.

### Proof of Stake (PoS)

Validators lock or stake assets and participate in block production and/or finality. Protocol rules can penalize certain misbehavior. Ethereum and Solana use proof-of-stake-based designs, though their implementations differ significantly.

Do not reduce security to “PoW is secure” or “PoS is secure.” Security depends on incentives, client diversity, validator/miner distribution, economic costs, software quality, governance, and network assumptions.

## 1.5 What does a cryptocurrency represent?

A crypto asset is represented by protocol state, not by a physical coin stored inside a wallet.

- On a **UTXO** chain such as Bitcoin, ownership is represented through unspent transaction outputs that can be spent under specified conditions.
- On an **account-based** chain such as Ethereum, state includes account balances, nonces, and smart-contract storage.
- A token on a smart-contract chain is typically recorded by the token contract's state. A token transfer often updates balances inside that contract.

A wallet manages keys and helps construct transactions. The blockchain records the state transition.

## 1.6 Public, private, permissioned, and permissionless networks

- **Public:** The ledger is publicly readable, though addresses may not directly reveal real-world identity.
- **Permissionless:** Participants can generally join under protocol rules without approval from a central operator.
- **Permissioned:** Participation in validation or access may be restricted to approved parties.
- **Private / consortium chain:** Access or governance is limited to an organization or group.

These labels are not interchangeable. A public chain can have centralized points of influence; a permissioned chain can still distribute control among several organizations.

## 1.7 What a payment engineer should take away

A blockchain is the settlement rail and state machine. Your application still needs its own order database, ledger, payment state machine, reconciliation jobs, security controls, and customer support process.

**Never treat a blockchain transaction hash as the entire payment record.** A payment record also needs the expected asset and network, amount, invoice, sender/recipient details where available, confirmations/finality status, provider event history, and the business decision made from that evidence.

---

# Chapter 2 — Coin vs Token

## 2.1 The short definition

A **coin** is the native asset of a blockchain or protocol. A **token** is an asset represented on an existing blockchain, commonly through a smart contract or token program.

Examples:

- BTC is native to Bitcoin.
- ETH is native to Ethereum.
- TRX is native to Tron.
- SOL is native to Solana.
- USDT can be issued on several networks; on those networks it is a token rather than the native asset.
- USDC can also exist on multiple networks.

The terms are useful but not always perfectly consistent across marketing material. Model the actual asset and network, not just the label “coin” or “token.”

## 2.2 Why this distinction matters for fees

The native asset usually pays for the resources needed to submit a transaction.

For example:

- Sending ETH on Ethereum generally pays the network fee in ETH.
- Sending USDT as an ERC-20 token normally requires ETH for the transaction fee.
- Sending a token on BNB Smart Chain normally requires BNB for gas.
- Sending a token on Tron may require TRX resources or TRX expenditure, depending on available Energy/Bandwidth and the transaction.
- Sending a token on Solana requires SOL for transaction fees and may require additional SOL to create or fund associated token accounts.

A customer may therefore hold USDT but still be unable to send it if the wallet lacks the network's fee asset or resources.

## 2.3 Smart contracts and token balances

A smart contract is code deployed to a blockchain and executed according to that chain's rules. A fungible token contract commonly keeps a mapping between addresses and balances and implements functions such as transfer and approval.

A token transfer is therefore not necessarily a simple native-coin transfer. It is commonly a call to a token contract, which consumes execution resources.

Token standards provide shared interfaces:

- **ERC-20:** Fungible tokens on Ethereum-compatible environments.
- **ERC-721:** A common non-fungible token standard.
- **ERC-1155:** A standard supporting multiple token types.
- **TRC-20:** Fungible tokens on Tron.
- **BEP-20:** Common token standard on BNB Smart Chain.
- **SPL Token:** Solana's token-program model; token accounts and program details differ from EVM contracts.

Standards make integrations more predictable, but they do not guarantee that every token is safe, liquid, redeemable, or supported by every provider.

## 2.4 Why “USDT” is not enough

A payment integration should not identify an asset by symbol alone.

A robust identity normally includes:

- **Asset identity:** Your internal immutable asset ID.
- **Network identity:** Which blockchain or supported network environment.
- **Symbol and display name:** For UI only; symbols can collide.
- **Contract address or token mint:** Required for tokens on networks that use one.
- **Decimals:** How integer units map to displayed quantities.
- **Address format:** How destinations are represented and validated.
- **Memo/tag requirement:** Whether a shared address needs an additional identifier.
- **Provider mapping:** Provider-specific asset/network code.
- **Status and policy:** Enabled, disabled, deposit-only, payout-only, minimum amount, confirmation threshold, and jurisdiction restrictions.

For example, `USDT on Ethereum`, `USDT on Tron`, and `USDT on BNB Smart Chain` are not interchangeable payment destinations, even though the display symbol is identical.

## 2.5 Decimals and smallest units

A blockchain usually represents token amounts as integers. The token's decimal count controls how that integer is displayed.

If an asset has 6 decimals:

- On-chain integer `1` = `0.000001` displayed units.
- On-chain integer `1,000,000` = `1.000000` displayed units.
- On-chain integer `12,345,678` = `12.345678` displayed units.

Do not assume decimals based on the symbol. Read trusted token metadata and verify it against authoritative network/provider information. Decimals are part of the asset configuration.

## 2.6 Wrapped assets

A wrapped token represents an asset or claim associated with another network or asset. WBTC, for example, is designed to represent BTC on Ethereum. WETH is an ERC-20 representation of ETH used by applications that need token-style behavior.

Wrapped assets add extra assumptions: the quality of the backing, custodian or bridge, redemption process, smart contracts, and governance. “Same economic exposure” does not mean “same technical asset.”

## 2.7 Developer rule

Keep **currency**, **network**, and **asset-on-network** separate in your data model. The display currency is a user-facing concept; the network asset is what actually moves on-chain.

---

# Chapter 3 — How to Classify Crypto Assets

There is no single classification system. An asset can fit several categories at once. ETH, for example, is a native coin, a smart-contract execution asset, a staking asset, and a trading asset. BNB has multiple roles in its ecosystem. Classification should answer a question: “What does it do?”, “Where does it run?”, or “How will our payment platform support it?”

## 3.1 Classification by technical role

### A. Native coins

The protocol's native asset, used for transfers, network resources, and sometimes security or staking.

Examples: BTC, ETH, TRX, SOL, BNB, LTC.

### B. Fungible tokens

Interchangeable units defined on an existing chain. One unit of a standard fungible token is intended to be equivalent to another unit of the same token.

Examples: USDT, USDC, UNI, LINK, and many others, depending on network and contract.

### C. Non-fungible tokens (NFTs)

Tokens representing distinct items or identifiers. Their units are not normally interchangeable one-for-one in the same way as fungible tokens. They are usually not a first-priority payment asset for a typical checkout integration.

### D. Wrapped or bridged assets

Tokens intended to represent assets moved or made usable across networks. They introduce bridge, custodian, and redemption risks.

### E. Layer 1 (L1) assets

Assets native to independent base-layer blockchain ecosystems, such as BTC, ETH, SOL, and TRX. “L1” describes an architectural role, not a guarantee of decentralization, speed, or security.

### F. Layer 2 (L2) ecosystems

Systems built on or connected to another chain to improve throughput, cost, or user experience. Examples include Arbitrum, Optimism, and Base. Polygon's architecture and product names have evolved; always identify the exact network, not just a broad brand name.

An L2 may use ETH or another specified asset for gas, and its deposit/withdrawal relationship with the underlying chain can differ from an independent L1. Do not assume every L2 has the same bridge or finality behavior.

## 3.2 Classification by economic purpose

| Category | Primary purpose | Examples / notes | Payment-integration concern |
|---|---|---|---|
| Payment / store-of-value asset | Transfer or hold value | BTC, LTC, BCH | Volatility, confirmation policy, liquidity |
| Smart-contract platform asset | Pay for execution and/or secure a chain | ETH, SOL, TRX, BNB | Fee model and chain-specific transaction rules |
| Stablecoin | Target a reference value | USDT, USDC, DAI, EURC | Peg, issuer, redemption, freeze and reserve risk |
| Exchange/ecosystem token | Used in a particular platform ecosystem | BNB, OKB and others | Platform dependence and regulatory risk |
| Utility token | Access a service or protocol function | Depends on the project | Real utility, liquidity, contract risk |
| Governance token | Voting or governance rights | UNI, AAVE and others | Governance does not necessarily imply legal ownership |
| Meme/community coin | Primarily community, culture, or speculation | DOGE, SHIB, PEPE | Volatility, liquidity, operational support risk |
| Privacy-focused asset | Enhances transaction privacy | XMR, ZEC | Compliance and exchange-support restrictions |
| Tokenized real-world asset (RWA) | Represents a claim or exposure to an off-chain asset | Tokenized funds, bonds, gold | Legal claim, issuer, custody, transfer restrictions |
| DeFi receipt / liquid-staking token | Represents a deposit, position, or staking claim | Varies by protocol | Redemption, smart-contract, depeg, and liquidity risk |

These categories overlap. A stablecoin is also a token on many networks; a wrapped asset may be used for DeFi; a governance token may also have utility.

## 3.3 Classification for payment integration

For a payment platform, a more operational taxonomy is often more useful:

1. **Native-coin transfer:** Send or receive the network's native asset.
2. **Token transfer:** Send or receive a contract/program-defined asset.
3. **Memo/tag-based deposit:** An address plus a destination identifier, where supported.
4. **Stablecoin settlement asset:** Used to reduce price volatility between receipt and payout.
5. **Conversion-only asset:** Accepted by a provider but immediately converted into another asset.
6. **Payout-only asset:** Supported for outgoing transfers but not for deposits, or vice versa.
7. **Unsupported/high-risk asset:** Excluded due to liquidity, legal, security, or operational reasons.

A provider may expose thousands of assets because it aggregates multiple chains and tokens. That does not mean every asset is appropriate for your merchants.

## 3.4 How to decide whether to support an asset

Evaluate at least:

- Merchant/customer demand.
- Real liquidity and reliable conversion venues.
- Deposit and payout availability.
- Network reliability and finality behavior.
- Minimum transfer and withdrawal constraints.
- Fee predictability.
- Confirmation and reorg risk.
- Address/memo complexity.
- Contract and issuer risk.
- Sanctions, AML, and local legal constraints.
- Provider support quality and incident history.
- Refund and reconciliation feasibility.

**Practical principle:** Support a curated whitelist, not a provider's entire catalog by default.

---

# Chapter 4 — What Is a Crypto Network?

## 4.1 A network is more than a name

A crypto network is the set of nodes, protocol rules, transaction formats, consensus or sequencing mechanisms, address conventions, and state history that determine which transactions are valid and how state changes.

In payment products, “network” often means the particular blockchain environment selected for a transfer. For example, a token called USDT may be available on Ethereum, Tron, and BNB Smart Chain. Those are distinct transaction environments.

A network usually defines:

- Valid transaction format and signing rules.
- Address or account representation.
- Native fee asset and fee calculation.
- Block production and finality model.
- Token standards or programs.
- RPC interfaces and node behavior.
- Confirmation/finality signals.
- Network identifiers, such as chain IDs where applicable.

## 4.2 Same symbol, different network

Suppose a customer is asked to pay 100 USDT. The checkout must specify the supported network clearly.

- USDT on Ethereum is a token contract on Ethereum.
- USDT on Tron is a token contract on Tron.
- USDT on BNB Smart Chain is a token contract on BNB Smart Chain.

The recipient may see similar-looking addresses on EVM-compatible networks. That does not make those networks the same. A transfer made on one chain will not automatically appear as a transfer on another chain.

A wrong-network deposit may sometimes be recoverable if the recipient controls the relevant key and can access the correct chain. In other situations, recovery may be impossible or commercially uneconomic. Do not promise recovery.

## 4.3 Common network families

| Network / family | Native fee asset | Broad model | Typical integration notes |
|---|---|---|---|
| Bitcoin | BTC | UTXO | Transaction size, fee rate, confirmations, change outputs |
| Ethereum mainnet | ETH | Account-based / EVM | Gas, nonce, token contracts, chain congestion |
| Tron | TRX | Account-based | Energy/Bandwidth resource model; token transfers may consume resources |
| BNB Smart Chain | BNB | EVM-compatible | EVM-style transactions and gas; separate chain state |
| Solana | SOL | Account/program model | Signatures, account state, token accounts, priority fees |
| Arbitrum / Optimism / Base | Commonly ETH for gas | L2 / rollup ecosystems | L2 execution fees and L1-related data costs; bridge/finality specifics |
| XRP Ledger | XRP | Ledger/account model | Destination tags may be required by custodians |
| Stellar | XLM | Ledger/account model | Memo/identifier requirements vary by recipient |
| Litecoin | LTC | UTXO | Bitcoin-family transaction patterns with its own network rules |
| TON | TON | Account/contract model | Message-based semantics and provider-specific memo/address handling may matter |

This table is an orientation guide, not a current fee or performance ranking. Fees and throughput vary with congestion, transaction complexity, protocol changes, and provider infrastructure.

## 4.4 EVM compatibility does not mean network equivalence

Ethereum, BNB Smart Chain, and many other networks use EVM-compatible addresses that commonly look like `0x...`. They can share wallet tooling and transaction concepts, but each network has its own:

- Chain ID.
- Native coin balance.
- Nonce/state.
- Block history.
- Token contract deployments.
- RPC endpoints.
- Fee market.
- Security and finality assumptions.

A token contract address on one EVM chain does not necessarily identify the same asset on another. Even if the same hexadecimal address appears on two chains, it may be a different contract or no contract at all.

## 4.5 Network identifiers in your system

Do not identify a network by a display name alone. Maintain a stable internal network ID and map provider-specific identifiers to it.

Store, where applicable:

- Internal network ID.
- Canonical name and aliases.
- Chain family / transaction model.
- Chain ID or equivalent network identifier.
- Mainnet/testnet environment.
- Native fee asset.
- Address validation rules.
- RPC/provider configuration.
- Confirmation/finality policy.
- Deposit/payout enablement.
- Provider-specific network codes.

Keep testnet assets separate from production assets. A testnet transaction hash is not a production payment.

## 4.6 Network-selection UX

A checkout should show the asset **and** network together, such as:

> USDT — Tron network (TRC-20)

Do not make customers infer the network from an address. Show warnings before the customer copies an address, explain whether a memo/tag is required, and avoid offering networks that the merchant cannot monitor and reconcile.

---

# Chapter 5 — Network Fee vs Gas Fee

## 5.1 The distinction

**Network fee** is the broad term for the cost of getting a transaction processed by a blockchain network.

**Gas** is the execution-resource accounting model used by Ethereum and EVM-compatible chains, and the phrase **gas fee** commonly refers to the fee paid for consuming those resources.

Therefore, a gas fee is a kind of network fee, but not every network fee is a gas fee.

Also keep three separate charges distinct:

1. **Blockchain network fee:** Paid to the network's transaction-processing/security mechanism.
2. **Processor/service fee:** Charged by a payment provider for its service.
3. **Conversion spread or exchange fee:** Charged or embedded when converting between assets or currencies.

A merchant may see one combined fee on an invoice, but your internal accounting should distinguish the components.

## 5.2 Ethereum-style gas

On Ethereum, a transaction consumes gas units. A simple transfer and a smart-contract interaction consume different amounts of execution resources.

A simplified EIP-1559-style formula is:

`Transaction fee = gas used × effective gas price`

The effective gas price is influenced by the base fee and the transaction's priority fee, subject to the transaction's fee parameters and inclusion conditions.

- **Gas used:** The execution resources actually consumed.
- **Base fee:** Protocol-determined fee per gas unit for a block; it changes with demand.
- **Priority fee / tip:** An additional amount intended to incentivize inclusion.
- **Max fee:** The transaction's maximum allowed total fee per gas unit, in the relevant transaction model.

A transaction can fail while still consuming gas, so a failed smart-contract transaction may still cost money.

## 5.3 Different networks, different fee models

### Bitcoin

Fees are typically related to transaction virtual size and the fee rate chosen by the sender, often expressed as sat/vbyte.

`Approximate fee = transaction size in vbytes × fee rate in sat/vbyte`

The actual size depends on inputs, outputs, and transaction type. Consolidating many small UTXOs can make a later transaction larger.

### EVM networks

Fees are generally based on gas consumed and gas price, paid in the network's native fee asset. L2s may include additional components related to data posted to an underlying chain.

### Tron

Tron uses resources including **Energy** and **Bandwidth**. A transaction may use available resources and/or consume TRX according to the network's rules. Contract interactions such as token transfers can consume Energy. The cost depends on resource availability and current protocol parameters.

### Solana

Solana has a base transaction fee and can include a priority fee. Transactions require SOL for fees. Some operations may also require account creation or rent-exempt funding; that funding is not the same thing as the transaction fee.

### Other networks

Each network may have its own fee denomination, resource accounting, and priority mechanism. Do not build one universal formula and apply it to every chain.

## 5.4 Why token transfers need native assets

A token is recorded by a contract or token program, but the network still has to execute the transaction. The token itself generally does not pay for that execution unless the system has a special sponsored-fee or account-abstraction arrangement.

Examples:

- USDT on Ethereum: typically needs ETH for gas.
- USDT on BNB Smart Chain: typically needs BNB.
- USDT on Tron: may need TRX resources or TRX expenditure.
- SPL token transfer on Solana: requires SOL for fees and may require account setup resources.

Some wallets, processors, or relayers offer fee sponsorship. This changes who pays; it does not mean the network has no fee.

## 5.5 Deposit fees vs payout fees

A common integration mistake is assuming the merchant's incoming amount will be reduced by the network fee in the same way for every transfer.

- On many chains, the sender pays the fee separately from the transfer value.
- A recipient may receive the full amount specified in a standard transfer while the sender's balance is reduced by amount plus fee.
- Some assets or transaction models may deduct fees from the transferred value, or a provider may impose its own accounting policy.
- A processor can charge its service fee separately or net it during settlement.

Always document whether a quoted amount means gross amount, on-chain transfer amount, processor fee, conversion amount, or net merchant settlement.

## 5.6 Sweep transactions and gas funding

A processor may generate many deposit addresses and later sweep funds to a treasury wallet.

For token deposits, a deposit address may hold tokens but have no native fee asset. The processor may need to fund the address with a small amount of the native coin/resources before it can move the token. This is sometimes called gas funding or a gas-station process.

Operational implications:

- Funding thousands of addresses can be expensive.
- Gas prices may change between estimation and broadcast.
- Funding transactions can become stuck.
- Dust balances may cost more to sweep than they are worth.
- The system must avoid funding an address based only on an unverified deposit event.
- Sweep fee costs must be included in unit economics.

## 5.7 Fee estimation and user communication

A robust payout flow should:

1. Estimate the fee using current network/provider data.
2. Apply a safety margin where appropriate.
3. Check the maximum fee and business policy.
4. Make clear whether the recipient gets the exact requested amount or a net amount.
5. Record estimated and actual fee separately.
6. Reconcile after broadcast and confirmation.
7. Handle fee spikes, replacement transactions, and stuck transactions according to the chain.

Never promise a fixed network fee unless your product actually guarantees it and the commercial risk is priced in.

---

# Chapter 6 — Why Are There So Many Cryptocurrencies?

Thousands of assets exist because creating a token is relatively easy, protocols can be copied or forked, and projects have different technical, economic, political, and speculative goals.

## 6.1 Different goals and design trade-offs

Projects may optimize for:

- Payments and store of value.
- Smart-contract execution.
- Faster or cheaper transactions.
- Privacy.
- Governance and protocol incentives.
- Gaming and digital ownership.
- Lending, trading, and other decentralized finance (DeFi).
- Stable-value transfers.
- Representation of off-chain assets.
- Community identity or speculation.

No design is best for every use case. Systems make trade-offs among decentralization, security, throughput, cost, usability, and governance. The “blockchain trilemma” is a popular framing of these tensions, not a precise law proving that exactly two properties are possible.

## 6.2 Forks, copies, and token creation

There are several ways new assets appear:

- **Protocol fork:** A new network or chain is created from existing software or history.
- **New blockchain:** A team designs a new protocol and launches its own network.
- **Smart-contract token:** A project deploys a token contract on an existing network.
- **Wrapped/bridged asset:** A system issues a representation of an asset from another environment.
- **Protocol receipt:** A DeFi protocol issues a token representing a deposit or claim.
- **Meme/community launch:** A token is created largely around community identity, attention, or speculation.

Creating a token is technically much easier than creating a secure, liquid, trusted financial ecosystem.

## 6.3 Why projects create tokens

A token may be intended to fund development, coordinate incentives, pay protocol fees, govern a system, represent collateral, or provide a tradable asset. Sometimes the stated utility is weak or mostly promotional.

A token's existence does not prove that it has:

- Sustainable demand.
- A reliable market.
- A legal claim on assets.
- Redemption rights.
- Independent security.
- A trustworthy issuer.
- Long-term utility.

## 6.4 Liquidity and network effects

A crypto asset becomes easier to use when many users, exchanges, market makers, wallets, processors, and merchants support it. Liquidity can attract more liquidity. This network effect helps explain why a relatively small group of assets and stablecoins dominate many payment flows while thousands of other tokens remain thinly traded.

A provider may list an asset because it can technically detect or route it, not because the asset is liquid enough for reliable merchant settlement.

## 6.5 How an integration engineer should think

You do not need to support every asset a provider exposes. Your support decision is a product, risk, and unit-economics decision.

Ask:

1. Who needs to pay with this asset?
2. Can we reliably identify it and its network?
3. Can we convert or settle it at a known cost?
4. What happens if the market becomes illiquid?
5. Is the issuer or contract trusted enough for our risk policy?
6. Can the processor monitor deposits and execute payouts?
7. What is the refund path?
8. Are we allowed to support it in the relevant jurisdiction?

---

# Chapter 7 — Stablecoins and Fiat-Pegged Assets

## 7.1 What is a stablecoin?

A stablecoin is designed to track the value of a reference asset, commonly a fiat currency such as the US dollar or euro. It aims to combine digital transferability with a more stable unit of account than a volatile crypto asset.

“Stable” describes an objective, not a guarantee. A stablecoin can trade below or above its target, become illiquid, lose redemption access, or face issuer, reserve, contract, bridge, and regulatory risks.

## 7.2 Main stablecoin designs

### A. Fiat-backed

An issuer claims to maintain reserves or backing assets intended to support redemption.

Examples include USDT and USDC. The exact reserve structure, redemption eligibility, transparency, and regulatory status depend on the issuer and jurisdiction.

Key risks:
- Issuer or custodian failure.
- Reserve quality or liquidity.
- Redemption restrictions.
- Address freezing or blacklisting where supported.
- Regulatory action.
- Banking partner disruption.
- Depeg during market stress.

### B. Crypto-collateralized

A protocol holds crypto collateral, often more than the value of the stablecoins issued, and uses liquidation mechanisms to manage risk. DAI is a well-known example of a crypto-collateralized stablecoin design, although its collateral composition and governance have evolved over time.

Key risks:
- Collateral price collapse.
- Congestion during liquidations.
- Oracle failure.
- Smart-contract bugs.
- Governance decisions.
- Insufficient collateral or impaired redemption.

### C. Algorithmic / undercollateralized

These designs attempt to maintain a peg through incentives, supply changes, or linked assets without the same level of direct, redeemable reserve backing. They can be fragile during confidence shocks. The TerraUSD (UST) collapse in 2022 is a major cautionary example.

Do not assume every stablecoin fits neatly into one category. Read the actual mechanism and disclosures.

## 7.3 USDT and USDC

USDT (Tether) and USDC (Circle) are widely used dollar-referenced stablecoins. Their availability, liquidity, fees, and support vary by chain, exchange, and processor.

Do not treat them as identical merely because both target one US dollar. Compare:

- Issuer and legal structure.
- Reserve disclosures and attestations/audits as applicable.
- Redemption access and eligibility.
- Supported networks and contract addresses.
- Freeze/blacklist capabilities.
- Liquidity in your payout and conversion venues.
- Provider support and regional restrictions.
- Historical depeg behavior and operational track record.

“More regulated” or “more transparent” is not a substitute for a current, jurisdiction-specific review.

## 7.4 Are there EUR-, INR-, CNY-, or other fiat-linked stablecoins?

Yes, there are or have been stablecoin projects linked to currencies other than USD, but their market depth, availability, and legal status differ significantly. Examples historically include EURC and EURS for euro exposure, XSGD for Singapore dollars, BRZ for Brazilian reais, and projects targeting other currencies. Availability and issuer status can change, so verify before integrating.

A few important distinctions:

- **EUR stablecoin:** May be useful for euro-denominated settlement, but liquidity and chain availability may be lower than dollar stablecoins.
- **GBP / JPY / SGD-linked assets:** Exist in varying forms and sizes; assess the specific issuer and market.
- **INR-linked assets:** The market is much smaller and more constrained than USD stablecoins; do not assume broad exchange or processor support.
- **CNY/CNH-linked assets:** Offshore yuan-linked token projects have existed, but availability, regulatory treatment, and liquidity require particular care.
- **CBDC:** A central bank digital currency, such as China's e-CNY or India's digital rupee pilot, is not the same thing as a permissionless stablecoin. It is a digital form of sovereign money operated under central-bank rules.

For every non-USD asset, verify that it is an actual live product, that your provider supports the exact network/contract, and that its redemption/liquidity route is viable. A symbol found in a provider list is not enough.

## 7.5 Why USD dominates

USD stablecoins benefit from several reinforcing factors:

- USD is widely used in global trade and crypto trading pairs.
- Major exchanges and liquidity providers support dollar pairs.
- Users often want dollar-denominated savings or settlement.
- A large installed base attracts more merchants, wallets, and providers.
- Network effects make the dominant asset more convenient to use.

Local-currency stablecoins face smaller addressable markets, fewer liquidity venues, regulatory constraints, banking challenges, and redemption limitations.

## 7.6 Stablecoins in a payment flow

Imagine a merchant sells a product for EUR 100:

1. The checkout calculates a quote for the chosen crypto asset.
2. The customer pays the quoted amount.
3. The processor detects and confirms the transaction.
4. The merchant's contract may allow the processor to hold the crypto, convert it to USDT/USDC, convert to fiat, or settle in the received asset.
5. The final settlement amount reflects the quote, actual execution price, fees, spread, and the merchant's agreement.

The merchant must know which party bears price movement between quote, receipt, conversion, and settlement.

---

# Chapter 8 — Crypto Wallets and Custody

## 8.1 What is a wallet?

A crypto wallet is software or hardware that manages keys and helps create, sign, and broadcast transactions. The assets themselves are represented in blockchain state; they are not literally stored inside the wallet app.

A simplified relationship is:

`Private key / signing authority → public key or derived key material → address`

The exact derivation and address construction differ by chain.

## 8.2 Private key, public key, address, and seed phrase

- **Private key:** Secret material that authorizes specific actions. Anyone who obtains sufficient key control may be able to move the associated assets.
- **Public key:** Derived from a private key and used in verification or address derivation, depending on the protocol.
- **Address:** A destination identifier derived according to chain-specific rules. It is not always a direct encoding of the public key.
- **Seed phrase / mnemonic:** A human-readable representation used to derive wallet key material in common wallet systems.
- **Derivation path:** Rules that derive particular keys/addresses from a seed.
- **Signature:** Cryptographic proof that a transaction or message was authorized by the relevant key.

Never log seed phrases, private keys, signing payload secrets, or recovery material. Never ask customers to send them to support.

## 8.3 Custodial vs non-custodial

### Custodial

A provider controls or manages the private keys or signing authority on behalf of customers.

Advantages:
- Easier recovery and customer support.
- Can support enterprise controls, policies, and compliance workflows.
- Users do not have to manage keys directly.

Risks:
- Provider compromise or insolvency.
- Insider threats.
- Withdrawal restrictions.
- Counterparty and legal risk.
- Concentrated custody risk.

### Non-custodial

The user retains key control and authorizes transactions.

Advantages:
- More direct user control.
- Less dependence on a custodian for transaction authorization.

Risks:
- Lost seed phrase or key can mean permanent loss.
- Phishing and malicious approvals.
- User mistakes and wrong-network transfers.
- Recovery is difficult or impossible.

A processor can provide a custodial service, a non-custodial integration, or a hybrid model. Determine who actually controls signing authority; product labels alone are not sufficient.

## 8.4 Hot, warm, and cold storage

- **Hot wallet:** Connected to systems that can sign or initiate transactions online. Useful for operational liquidity, but exposed to online threats.
- **Warm wallet:** A middle category with additional operational controls or limited connectivity; definitions vary by organization.
- **Cold wallet:** Keys are kept offline or in a strongly isolated signing environment. Often used for long-term or high-value storage.

A sound custody architecture separates the funds needed for daily operations from the bulk of treasury assets. It also uses limits, approvals, monitoring, recovery procedures, and independent audits.

## 8.5 HD wallets and deposit addresses

Hierarchical Deterministic (HD) wallet standards allow a seed to derive many keys and addresses. BIP32, BIP39, and BIP44 are widely referenced in Bitcoin-family and multi-coin wallet ecosystems, but exact support and paths vary by chain and wallet.

An **extended public key (xpub)** or equivalent public derivation material can allow a service to generate receiving addresses without holding the corresponding private keys, when the derivation scheme permits it. Public derivation material is still sensitive from a privacy perspective: it may reveal address relationships and transaction history.

### Unique address per invoice

A processor may create a distinct deposit address for each invoice/customer. This can simplify attribution and reconciliation.

Benefits:
- Clearer mapping from deposit to invoice.
- Easier automated monitoring.
- Less reliance on customer-entered memos.

Costs:
- More address management.
- Sweep and fee-funding complexity.
- Address derivation and key-management requirements.
- Privacy and operational considerations.

### Shared address plus memo/tag

Some networks or custodians support a shared destination address and an additional identifier, such as a destination tag or memo. This reduces address-generation overhead but increases the risk that a customer omits or mistypes the identifier.

The application must model `address` and `memo/tag` separately. A memo is not part of every network's address, and it must not be silently discarded.

## 8.6 UTXO vs account model

### UTXO model

Bitcoin-style systems use unspent transaction outputs. A transaction consumes existing outputs and creates new outputs. The available balance is the sum of spendable outputs, but a transaction's practical spendability depends on input selection, fee, and script conditions.

### Account model

Account-based systems maintain account state, balances, and often a nonce or sequence number. A transaction changes account or contract state.

Developer consequences:
- UTXO systems need input selection, change outputs, and transaction-size-aware fees.
- Account systems need nonce management, fee estimation, and careful handling of concurrent transactions from the same account.
- Both require confirmation tracking, safe signing, and reconciliation.

## 8.7 Multisig and MPC

- **Multisignature (multisig):** The protocol or script requires a specified combination of keys/signatures.
- **Multi-Party Computation (MPC):** Key control or signing is distributed among participants so that a single complete private key may not exist in one place in the ordinary form.

Both can reduce single-key risk, but implementation, recovery, governance, and vendor trust still matter. They are not interchangeable, and neither removes the need for policy controls.

## 8.8 Sweeping and treasury management

**Sweeping** moves funds from many deposit addresses into a controlled treasury or operational wallet. A typical process:

1. Detect a deposit.
2. Verify the deposit against the chain/provider.
3. Wait for the configured confirmation/finality threshold.
4. Check asset and network policy.
5. Estimate sweep costs.
6. Ensure the source address can pay the fee or arrange resource funding.
7. Sign under the appropriate policy.
8. Broadcast and track the sweep.
9. Reconcile source and destination balances.
10. Record the network fee and internal ledger entries.

Do not sweep merely because a webhook says “paid.” Verify the event and ensure the transaction meets your policy.

---

# Chapter 9 — Transaction Lifecycle and Payment States

## 9.1 Blockchain transaction lifecycle

A typical lifecycle is:

`Construct → Sign → Broadcast → Seen by node → Included → Confirmed/finalized`

Not every chain exposes every stage in the same way. A provider may report a transaction as “detected” before it is included in a block.

### Important terms

- **Broadcast:** Transaction submitted to a node or provider.
- **Pending:** Not yet included or not yet considered final under your policy.
- **Mempool:** A node's pool of transactions waiting for inclusion; there is no single universal mempool shared perfectly by all nodes.
- **Block inclusion:** Transaction appears in a block.
- **Confirmation count:** Commonly the number of blocks including the transaction's block and/or blocks built on top of it, depending on convention.
- **Reorganization (reorg):** A chain reorganizes and a previously observed block may no longer be canonical.
- **Finality:** The point or confidence threshold at which the network's rules consider a transaction finalized. Definitions differ by chain.
- **Replacement / cancellation:** Some transaction systems allow a pending transaction to be replaced under specific rules; behavior is chain-specific.

## 9.2 Why confirmation policies differ

A high-value Bitcoin deposit may require more confirmations than a small payment because the economic cost of a reversal attempt and the merchant's risk tolerance differ. Other networks use different finality signals and may have different operational risks.

A confirmation policy should consider:
- Asset and network.
- Transaction value.
- Reorg/finality model.
- Historical network reliability.
- Merchant product and fraud exposure.
- Processor/provider evidence.
- Compliance holds.
- Whether goods or services are delivered immediately.

Never hard-code “one confirmation is always enough” for every asset.

## 9.3 The invoice lifecycle is not the blockchain lifecycle

The blockchain says whether an on-chain state change occurred. Your payment application decides what that event means for an order.

Example payment states:

- `created`
- `awaiting_payment`
- `detected`
- `confirming`
- `confirmed`
- `settling`
- `settled`
- `expired`
- `underpaid`
- `overpaid`
- `wrong_asset`
- `wrong_network`
- `manual_review`
- `refunded`
- `failed`

Not every provider uses these labels. Normalize external statuses into your own state machine, and preserve the original provider status for troubleshooting.

A payment can be confirmed on-chain but still not be settled to the merchant. Settlement may require conversion, compliance review, a payout, or internal ledger posting.

## 9.4 Deposit flow: from invoice to merchant balance

A robust simplified flow:

1. **Create invoice:** Store merchant, order, quote, expected asset/network, amount, expiry, and fee/settlement policy.
2. **Assign destination:** Generate a unique address or address-plus-memo.
3. **Show instructions:** Display network, amount, address, memo, expiry, and warnings.
4. **Observe payment:** Monitor chain events or receive provider webhooks.
5. **Verify evidence:** Confirm transaction hash, network, asset contract/mint, destination, amount, and relevant memo/tag.
6. **Update state:** Mark as detected/pending; do not credit prematurely.
7. **Wait for policy threshold:** Confirm enough blocks/finality or provider evidence.
8. **Evaluate amount and timing:** Underpaid, overpaid, late, duplicate, or wrong-network deposits need defined handling.
9. **Post ledger entries:** Record the asset received and any liability or receivable created.
10. **Settle:** Convert, retain, or pay out according to the merchant agreement.
11. **Notify merchant:** Send an authenticated, idempotent webhook.
12. **Reconcile:** Compare your records against provider and blockchain evidence.

## 9.5 Payout flow

1. Merchant requests payout.
2. Validate merchant balance, permissions, destination, asset, network, memo, and limits.
3. Run risk/compliance checks.
4. Quote fees and determine gross/net payout semantics.
5. Reserve funds in the internal ledger.
6. Create a payout request with an idempotency key.
7. Obtain required approvals or signing authorization.
8. Build and sign the transaction.
9. Broadcast and store the transaction hash.
10. Track pending, included, confirmed/finalized, or failed states.
11. Reconcile the final on-chain amount and fee.
12. Release or adjust reservations and notify the merchant.

The system must handle a timeout after broadcast. A network timeout does not prove the transaction failed; retrying blindly can create duplicate payouts.

## 9.6 Underpaid, overpaid, and late payments

### Underpaid
The received amount is below the invoice amount. Policies may allow partial payment, request a top-up, or route to manual review. Do not silently mark paid unless the business contract permits it.

### Overpaid
The received amount exceeds the expected amount. Decide whether to credit the extra amount, refund it, or hold it for review. Refunds cost network fees and may require the sender to provide a verified destination.

### Late payment
A customer sends funds after invoice expiry or after the exchange quote expires. The on-chain transaction may be valid, but the amount's fiat value or order validity may have changed. Define whether to re-quote, accept at market value, refund, or review manually.

### Wrong asset/network
A deposit can arrive at a similar-looking address on a different network or in an unsupported token. Never infer the intended asset from symbol alone. Recovery needs a documented process and may be impossible.

### Dust
Very small balances may be uneconomical to sweep or refund because fees exceed their value. Define dust thresholds and customer communication.

## 9.7 Webhooks and idempotency

Provider webhooks may be duplicated, delayed, retried, or delivered out of order.

A reliable consumer should:
- Verify the webhook signature or authenticated delivery mechanism.
- Persist the event before performing irreversible business actions.
- Deduplicate by provider event ID where available.
- Use idempotent state transitions.
- Reject illegal transitions or record them for investigation.
- Re-fetch the transaction from the provider/chain when evidence is insufficient.
- Return success quickly and process heavy work asynchronously.
- Maintain a dead-letter/retry strategy.
- Never equate a webhook retry with a new payment.

A unique transaction hash alone may not be enough for all chains. UTXO chains can have multiple outputs; contract/event-based systems may require log index or transfer index. Use a chain-appropriate unique transaction/event identity.

---

# Chapter 10 — The Crypto Business Ecosystem

## 10.1 The main participants

### Exchanges

Centralized exchanges (CEXs) match or facilitate trades, manage customer accounts, and provide liquidity or conversion services. They may also offer custody, staking, or other products. Their risk includes custody, solvency, market, operational, and regulatory risk.

### Decentralized exchanges (DEXs)

DEX protocols enable on-chain asset swaps, often through smart contracts and automated market makers or order-book systems. They introduce smart-contract, liquidity, MEV, oracle, and transaction-execution risks.

### Liquidity providers and market makers

They quote buy/sell prices or supply liquidity. A payment processor may use them to convert a received asset into a settlement asset. The processor must account for spread, depth, slippage, execution latency, and counterparty risk.

### OTC desks

Over-the-counter desks facilitate larger trades, often with negotiated prices and settlement terms. They may be useful when a market order would move the price significantly.

### Crypto payment processors

A processor provides some combination of deposit addresses, chain monitoring, invoice management, conversion, custody, payout, settlement, and reporting. Providers differ: one may be a hosted checkout, another a custody/wallet service, and another primarily an API aggregator.

### Payment orchestrators

An orchestrator integrates multiple processors or payment rails behind a common interface. It may route transactions, normalize statuses, manage retries/failover, and reconcile provider results. It does not automatically own the underlying blockchain infrastructure.

### On-ramp and off-ramp providers

- **On-ramp:** Converts fiat into crypto.
- **Off-ramp:** Converts crypto into fiat or a bank payout.
- Some providers do both.

### Custodians

Custodians safeguard assets or signing authority, often for institutions. They may provide policy controls, key management, reporting, and withdrawal workflows.

### Stablecoin issuers

Issuers create and manage stablecoin supply under their rules. Depending on the asset, they may maintain reserves and support redemption. Some issuers can freeze or blacklist addresses under contract or legal powers.

### Chain analytics and KYT vendors

These services assess blockchain addresses and transaction flows for risk indicators. Their outputs are signals, not infallible proof of criminality. Policies need escalation, review, and a way to handle false positives.

## 10.2 How a processor makes money

Potential revenue sources include:

- **Processing fee:** A percentage or fixed charge for processing deposits/payouts.
- **Conversion spread:** Difference between the rate offered to the customer and the processor's execution rate.
- **Exchange/market fee:** A fee charged by a trading venue.
- **Payout fee:** A service charge for sending funds.
- **Network fee handling:** A transparently disclosed charge related to network costs. Markups can create legal, trust, and commercial issues if hidden.
- **Subscription/API fee:** Monthly or usage-based platform pricing.
- **Custody/service fee:** Fees for safeguarding assets or operational services.
- **Treasury/float economics:** Earnings or costs associated with holding assets before settlement, subject to contract, regulation, liquidity, and risk. It should not be assumed that holding customer funds is permitted or profitable.
- **Value-added services:** Reconciliation, reporting, risk screening, merchant tools, and enterprise support.

These are possible business models, not guaranteed margins. Actual fees depend on volumes, competition, jurisdiction, chain costs, liquidity, fraud, support, and capital requirements.

## 10.3 A simple unit-economics example

Assume a merchant accepts the equivalent of USD 1,000 in crypto.

Illustrative figures:
- Merchant processing fee: 0.8% = USD 8.00.
- Conversion spread retained by provider: 0.2% = USD 2.00.
- Direct provider revenue: USD 10.00.
- Processor's own exchange, network, custody, compliance, support, and infrastructure costs: USD 4.50.
- Approximate contribution before fixed overhead and other risks: USD 5.50.

This is only a teaching example. Real economics may be very different. The same transfer can be unprofitable if network costs, failed payouts, support, or liquidity slippage are high.

## 10.4 Why float is not free money

If a processor holds merchant funds for a period, it may appear to have a balance-sheet benefit. But customer funds can create custody, safeguarding, liquidity, reconciliation, insolvency, and regulatory obligations. Settlement delays can also damage merchant trust. Never design a business model around using customer funds without explicit legal and contractual analysis.

## 10.5 What creates a defensible processor business?

A provider is not valuable merely because it has a long coin list. Strong capabilities include:

- Reliable chain monitoring and transaction attribution.
- Secure key management and withdrawal controls.
- Clear, predictable fees.
- Good liquidity and execution quality.
- Accurate settlement and reconciliation.
- Low operational downtime.
- Good developer documentation and sandbox quality.
- Reliable webhooks and idempotent APIs.
- Fraud/AML controls appropriate to the business.
- Good support for refunds, disputes, and exceptions.
- Transparent asset/network support policies.

---

# Chapter 11 — Crypto and Trading

## 11.1 Why payment engineers need trading basics

A merchant may sell goods in EUR but receive BTC, ETH, or USDT. The processor may need to quote a price, hold the received asset, convert it, or settle a different asset. This creates an interaction between payments, market prices, and execution.

You do not need to become a trader to integrate payments, but you do need to understand exchange rates, volatility, spreads, slippage, liquidity, and settlement timing.

## 11.2 Trading pairs

A pair such as `BTC/USDT` expresses the price of BTC in USDT. If the pair is quoted at 60,000, the market convention generally means one BTC is priced at 60,000 USDT. Always check base/quote conventions for the specific API.

If an invoice is EUR 100 and a quote source gives EUR/USD = 1.08 and BTC/USD = 60,000, a rough BTC amount is:

`EUR 100 × 1.08 USD/EUR ÷ 60,000 USD/BTC = 0.0018 BTC`

Real payment quotes need fees, source timestamps, spread, rounding, and a validity period.

## 11.3 Quote and rate lock

A quote is a price offer valid under defined conditions. A rate lock can protect the customer or merchant from price changes for a short period.

A quote record should include:
- Base and quote assets/currencies.
- Rate and source(s).
- Quote creation time and expiry.
- Amount before and after fees.
- Rounding rules.
- Spread or markup.
- Provider or venue used.
- Execution and settlement policy.

If a quote expires, the payment may still arrive later. Your business rules must define whether the payment is accepted at the old quote, revalued at the current market rate, or reviewed/refunded.

## 11.4 Spread, fees, and slippage

- **Spread:** Difference between bid and ask, or between a provider's quoted price and its execution price, depending on context.
- **Trading fee:** Fee charged by an exchange or execution venue.
- **Slippage:** Difference between expected price and actual execution price.
- **Market depth:** Available liquidity at different prices.
- **VWAP:** Volume-weighted average price over a set of trades or levels; useful in some price calculations but not a guarantee of executable price.
- **Volatility:** How much the price fluctuates over time.
- **Hedging:** Taking an offsetting position or using another risk-control mechanism to reduce price exposure.

A spot price displayed by an API is not necessarily the price at which your processor can execute a large conversion.

## 11.5 Conversion paths

A processor may convert:

`BTC → USDT → fiat`

or:

`BTC → fiat`

The best path depends on liquidity, fees, supported trading pairs, limits, compliance, and settlement needs. Multiple conversions can create additional costs and execution risk.

## 11.6 Merchant risk choices

A merchant can choose among different policies:

1. **Keep the crypto:** Merchant bears price volatility and custody risk.
2. **Settle in a stablecoin:** Reduces exposure to BTC/ETH price movements but retains stablecoin and network risk.
3. **Auto-convert to fiat:** Reduces crypto price exposure but adds conversion, banking, and payout dependencies.
4. **Hedge exposure:** More sophisticated and requires controls, liquidity, and clear accounting.

The merchant agreement must state who bears price movement and fees at each stage.

## 11.7 Payment quote design

For a good checkout:
- Use a trusted price source or a controlled aggregation method.
- Store the quote, not only the final amount.
- Include quote expiry and asset/network details.
- Apply deterministic rounding.
- Define underpayment and overpayment tolerance.
- Distinguish the expected amount from the amount actually received.
- Reconcile conversion execution against the quoted rate.
- Monitor stale or divergent price sources.

Do not use binary floating-point for financial amounts or rates when precision matters.

---

# Chapter 12 — Three Business Perspectives

The same crypto transaction looks different to a merchant, an orchestrator, and a processor. Understanding the boundary between these roles helps you design cleaner APIs and avoid assigning responsibilities to the wrong system.

## 12.1 Merchant perspective

A merchant primarily wants to get paid for goods or services.

### Merchant priorities
- Easy checkout and clear instructions.
- High payment success rate.
- Predictable fees.
- Fast and reliable settlement.
- Minimal volatility exposure.
- Refund capability.
- Accurate reporting and reconciliation.
- Compliance and reputation protection.

### Merchant questions
- Which assets and networks can customers use?
- Does the merchant receive crypto, stablecoin, or fiat?
- Who pays network fees?
- What happens if the customer underpays or pays late?
- How are refunds handled?
- When is the payment considered final?
- What happens if the processor is unavailable?

The merchant's order status should not be tied blindly to a provider's ambiguous string. Define exactly what `paid`, `confirmed`, and `settled` mean in the merchant contract.

## 12.2 Payment orchestrator perspective

An orchestrator provides a normalized layer over multiple providers. In a PayMid-like architecture, it may connect several crypto processors alongside card, bank, and alternative payment methods.

### Core responsibilities
- Unified API and data model.
- Provider selection and routing.
- Health monitoring and failover.
- Currency/asset/network mapping.
- Status normalization.
- Webhook verification and delivery.
- Idempotency and retry policies.
- Internal ledger and reconciliation.
- Fee/FX quote normalization.
- Reporting, observability, and support tooling.

### Routing is more than “cheapest provider”

Routing can consider:
- Merchant eligibility.
- Asset/network support.
- Provider uptime and latency.
- Expected fees and FX spread.
- Liquidity and payout capability.
- Transaction amount and risk.
- Regional restrictions.
- Historical success rates.
- Settlement currency and speed.

A low-cost provider is not useful if it cannot reliably monitor the chosen network or settle the merchant's required currency.

### Normalize carefully

A provider may call an asset `USDT_TRON`, another may use a numeric currency ID, and another may expose separate asset and network fields. Maintain your own canonical model and explicit provider mapping.

Never assume that two provider IDs refer to the same on-chain asset just because the display symbols match.

## 12.3 Crypto processor perspective

A processor may operate or integrate the lower-level crypto infrastructure:

- Address generation and assignment.
- Chain node/RPC connectivity.
- Deposit monitoring.
- Transaction validation and confirmation tracking.
- Wallet and key management.
- Fee estimation and transaction broadcasting.
- Sweeping and treasury management.
- Payout execution.
- Conversion and liquidity access.
- Compliance screening.
- Settlement and reporting.

The processor must answer operational questions such as:
- What if an RPC provider returns stale data?
- What if two nodes disagree?
- What if a webhook arrives before the transaction is indexed?
- What if the transaction is replaced or reorged?
- What if a deposit address has tokens but no gas asset?
- What if a payout broadcasts but the API times out?
- What if the key-management system is unavailable?

## 12.4 Responsibility matrix

| Concern | Merchant | Orchestrator | Processor |
|---|---|---|---|
| Product/order decision | Owns | May relay/normalize | Usually not |
| Customer checkout | Owns or delegates | May provide unified flow | May provide hosted checkout |
| Provider routing | Usually not | Often owns | Not applicable for its own execution |
| Chain monitoring | Usually delegates | May verify/aggregate | Often owns or contracts |
| Private-key control | Depends on model | Usually should not need keys | May own if custodial |
| Asset/network mapping | Chooses supported options | Canonical mapping | Provider-specific support |
| Merchant ledger | Owns order/accounting | May own platform ledger | Owns its operational records |
| Conversion | Policy decision | May route/normalize | May execute or source liquidity |
| Compliance | Responsible according to role/law | Role-dependent | Role-dependent |
| Reconciliation | Reconciles orders and settlement | Cross-provider reconciliation | Chain/wallet/processor reconciliation |

This is a common pattern, not a universal legal allocation. Contracts and actual custody/control determine responsibilities.

## 12.5 Internal currency mapping

A useful conceptual model:

- `currency`: human-facing unit, e.g. USDT.
- `network`: blockchain environment, e.g. Tron mainnet.
- `asset`: USDT token on Tron, with its token identifier and decimals.
- `provider_asset_map`: provider's external code mapped to your internal asset ID.
- `payment_method`: the merchant-facing option enabled for a transaction.
- `settlement_asset`: the asset the merchant will actually receive.

The exact schema can vary, but do not collapse all of these concepts into one `currency` table.

---

# Chapter 13 — Compliance, Risk, and Operational Controls

## 13.1 Compliance is not one feature

Crypto compliance may involve customer identity, transaction monitoring, sanctions screening, licensing, recordkeeping, reporting, travel-rule requirements, and jurisdiction-specific restrictions. Which obligations apply depends on the business model, where the service operates, who its customers are, and whether it holds or transfers assets for others.

This chapter is an engineering orientation, not legal advice. Have qualified compliance and legal professionals review the actual product and jurisdictions.

## 13.2 KYC, KYB, AML, and KYT

- **KYC (Know Your Customer):** Identity verification for individuals where required.
- **KYB (Know Your Business):** Verification of business customers and ownership/control information.
- **AML (Anti-Money Laundering):** Controls designed to prevent and detect money laundering and related financial crime.
- **KYT (Know Your Transaction):** Analysis of transaction and address risk indicators, often using blockchain analytics.
- **Sanctions screening:** Checking relevant parties or addresses against applicable sanctions restrictions.

A risk score is not a verdict. It can be incomplete, delayed, or incorrect. Define review thresholds, escalation paths, evidence retention, and customer communication.

## 13.3 Travel Rule

The FATF Travel Rule framework concerns certain originator and beneficiary information accompanying qualifying virtual-asset transfers between covered service providers. Specific implementation, thresholds, exemptions, and local legal obligations vary by jurisdiction.

Engineering considerations:
- Identify which transactions fall within the rule.
- Collect and validate required data securely.
- Exchange information through approved mechanisms.
- Preserve audit trails.
- Handle missing or mismatched information.
- Avoid putting sensitive personal data in public blockchain transactions or ordinary application logs.

## 13.4 Issuer controls and address freezes

Some centralized stablecoin issuers have technical or legal mechanisms to blacklist or freeze addresses. This means an asset can be transferable on-chain in general but still subject to issuer controls. Other tokens have different governance and control models.

Your risk review should ask:
- Can the issuer freeze tokens?
- Can the contract be upgraded?
- Who controls admin keys?
- Are transfers restricted?
- Is redemption available, and to whom?
- What happens if an issuer suspends service?

## 13.5 Key and transaction security

Use layered controls:
- Hardware Security Modules (HSM), secure MPC custody, or equivalent hardened key-management systems where appropriate.
- Least-privilege access.
- Separation of duties and multi-person approval for high-risk payouts.
- Withdrawal allowlists and velocity limits.
- Hot-wallet balance limits.
- Independent transaction policy checks before signing.
- Secure backup and recovery procedures.
- Secret rotation and incident response.
- Immutable or tamper-evident audit logs.
- Monitoring for unusual destinations, amounts, and transaction patterns.

Never store private keys in source code, ordinary environment files committed to Git, application logs, or support tickets.

## 13.6 Risk categories for an integration platform

- **Address risk:** Invalid, unsupported, or malicious destination.
- **Network risk:** Congestion, reorg, outage, RPC inconsistency.
- **Asset risk:** Depeg, illiquidity, issuer failure, contract exploit.
- **Custody risk:** Key compromise, insider abuse, provider insolvency.
- **Counterparty risk:** Exchange, liquidity provider, bank, or processor failure.
- **Compliance risk:** Sanctions, prohibited customer, missing required data.
- **Operational risk:** Duplicate payout, lost webhook, bad mapping, stale quote.
- **Accounting risk:** Incorrect fee, decimals, settlement rate, or ledger entry.
- **Customer-experience risk:** Wrong network, omitted memo, late payment, unclear refund.

## 13.7 Incident readiness

Before production, define playbooks for:
- Provider or RPC outage.
- Chain halt or major reorg.
- Stablecoin depeg.
- Token contract exploit or issuer freeze.
- Compromised signing credentials.
- Duplicate or unknown payout.
- Delayed deposits and webhook backlog.
- Incorrect currency/network mapping.
- Suspected sanctions or illicit-flow alert.
- Treasury shortfall or reconciliation mismatch.

The objective is not to predict every incident; it is to make detection, containment, communication, and recovery repeatable.

---

# Chapter 14 — Developer Handbook: Modeling, APIs, and Production Safety

This chapter translates the concepts into a practical integration design. The examples are conceptual and should be adapted to the actual provider API and chain libraries.

## 14.1 Recommended domain model

Avoid a single table that treats `USDT`, `USDT-TRC20`, and a provider's numeric ID as the same concept.

A starting model:

### `currencies`
Human-facing units or asset family labels.

Suggested fields:
- `id`
- `code` or `symbol`
- `name`
- `kind` (native, token, stablecoin, etc., if useful)
- `is_active`

Do not assume `symbol` is globally unique.

### `networks`
Network environments.

Suggested fields:
- `id`
- `code`
- `name`
- `family` (utxo, evm, tron, solana, etc.)
- `chain_id` or equivalent
- `environment` (mainnet/testnet)
- `native_asset_id`
- `is_active`

### `assets`
A specific asset on a specific network.

Suggested fields:
- `id`
- `currency_id`
- `network_id`
- `asset_type` (native/token)
- `contract_address` or token mint, nullable for native assets
- `decimals`
- `address_format`
- `requires_memo`
- `min_deposit`
- `min_payout`
- `confirmation_policy_id`
- `deposit_enabled`
- `payout_enabled`
- `risk_status`

Add uniqueness constraints appropriate to your data, for example one canonical native asset per network and a unique token identifier per network. Normalize addresses before comparing them where the chain's rules allow it.

### `provider_asset_maps`
Maps a provider's asset/network representation to your canonical asset.

Suggested fields:
- `id`
- `provider_id`
- `provider_asset_code`
- `provider_network_code`
- `asset_id`
- `deposit_supported`
- `payout_supported`
- `metadata_json`
- unique provider mapping constraints

Provider IDs can change or be reused across environments, so scope them correctly.

### `payments` and `payment_events`
Store business state separately from raw external events. Keep event history append-only where practical.

### `payouts`
Store destination, memo, requested amount, fee policy, idempotency key, provider request ID, transaction hash, and state transitions.

### `ledger_accounts` and `ledger_entries`
Represent balances and movements using a double-entry model. A payment status field is not a ledger.

## 14.2 Amount precision: never use float

Binary floating-point cannot represent many decimal fractions exactly. This can create rounding errors in money calculations.

Use:
- Integer smallest units when the asset's precision and range fit the chosen integer type.
- Arbitrary-precision integer arithmetic for on-chain quantities.
- Decimal arithmetic for fiat quotes and calculations.
- Explicit rounding rules at system boundaries.

In PHP/Laravel, use BCMath or a well-tested decimal library such as Brick\Math. In Go, use `math/big.Int` for raw integer units or a carefully selected decimal library when decimal calculations are needed.

Avoid converting a large on-chain integer to a native floating-point number.

### Example: integer units

For a token with 6 decimals, the displayed amount `12.345678` corresponds to integer units `12345678`.

When parsing a user amount:
1. Validate the decimal string.
2. Reject more fractional digits than the asset permits, unless a documented rounding rule exists.
3. Convert to integer smallest units exactly.
4. Validate min/max limits.
5. Store both the exact integer units and a display representation if needed.

Never trust a client-supplied `decimals` value; use your canonical asset configuration.

## 14.3 Idempotency and duplicate protection

Every create-payment or create-payout operation should have a defined idempotency policy.

A request idempotency key should:
- Be scoped to the merchant or authenticated principal.
- Be stored with a request fingerprint.
- Return the original result for a valid retry.
- Reject reuse with a materially different payload.
- Have a documented retention period.

For chain events, deduplication keys should be chain-aware. Depending on the chain, identity may require transaction hash plus output index, event/log index, or another unique event identifier.

## 14.4 Webhook security

- Verify signatures using the provider's documented algorithm.
- Validate timestamp and replay window if supported.
- Compare signatures in constant time where relevant.
- Parse the raw request body exactly as required by the signature scheme.
- Persist event IDs and payload hashes.
- Do not trust a `status=confirmed` field without validating the sender and provider semantics.
- Use asynchronous processing for heavy work.
- Keep a replay/reprocessing tool for operations teams.

Do not invent a generic signature formula. Providers differ in whether they sign raw bytes, canonical JSON, a timestamp plus body, or another payload.

## 14.5 State machines and illegal transitions

A state machine prevents contradictory business states. For example, a payout should not transition from `settled` back to `pending` just because a delayed webhook arrives.

Define:
- Allowed transitions.
- Conditions required for each transition.
- Which events are authoritative.
- How to handle out-of-order events.
- Whether states are terminal.
- How manual review and reversal are represented.

Preserve the provider's raw status separately from your normalized status. This makes it possible to update mapping logic without losing evidence.

## 14.6 Ledger and reconciliation

Use a double-entry ledger so each movement has balancing entries. The exact accounts depend on the accounting model, but a ledger should make it possible to explain:
- What the customer paid.
- What asset was received.
- What fees were charged.
- What conversion occurred.
- What the merchant is owed.
- What was paid out.
- What remains reserved or under review.

Reconciliation should compare at least:
- Internal payment/payout records.
- Provider reports and balances.
- Blockchain transaction evidence where accessible.
- Exchange execution and settlement records.
- Bank payout records, if fiat settlement is involved.

Differences should create actionable exceptions, not be silently overwritten.

## 14.7 Retries, queues, and timeouts

Use queues for monitoring, webhook processing, conversion, payout tracking, and reconciliation. Retry transient failures with bounded exponential backoff and jitter. Do not retry every error.

Classify errors:
- **Transient:** Timeout, temporary rate limit, temporary RPC/provider failure.
- **Permanent:** Invalid address, unsupported asset, malformed request.
- **Unknown outcome:** Request may have succeeded but response was lost.
- **Business decline:** Risk policy, insufficient balance, limit exceeded.
- **Manual review:** Ambiguous or potentially irreversible case.

For unknown outcomes, query the provider or chain using the existing idempotency key/request ID before submitting a new transaction.

## 14.8 Laravel-oriented architecture

A practical Laravel service might separate:

- `AssetRegistry`: canonical currencies, networks, and asset metadata.
- `ProviderAssetMapper`: provider code to internal asset mapping.
- `PaymentService`: invoice creation and business validation.
- `BlockchainProviderClient`: provider API calls.
- `WebhookController`: authentication, persistence, and quick acknowledgement.
- `WebhookProcessor`: asynchronous event normalization and state transitions.
- `LedgerService`: balanced ledger postings.
- `PayoutService`: validation, reservation, submission, and tracking.
- `ReconciliationJob`: scheduled comparison and exception creation.
- `RiskScreeningService`: sanctions/KYT policy integration.

Keep controllers thin. Use database transactions for internal state changes, but remember that a database transaction cannot atomically include an external blockchain broadcast. Use an outbox or durable job pattern to coordinate internal writes and external side effects.

## 14.9 Go-oriented architecture

In Go, separate interfaces for:
- Asset/network registry.
- Provider API client.
- Chain/RPC client.
- Transaction signer.
- Fee estimator.
- Event store.
- Ledger.
- Queue/worker.
- Metrics and tracing.

Use `context.Context` for cancellation and deadlines. Set explicit HTTP timeouts. Reuse clients and connections. Avoid unbounded goroutine creation. Protect nonce allocation and treasury spending with appropriate coordination. Use a database uniqueness constraint as a final line of defense against duplicate processing.

The critical optimization for payment systems is correctness and bounded resource use first. Zero allocations are valuable in hot paths, but they do not compensate for unsafe key handling, duplicate payouts, or incorrect financial accounting.

## 14.10 Observability

Monitor:
- Deposit detection delay.
- Time from detection to confirmation.
- Webhook delivery and retry rates.
- Payout success/failure rate.
- Pending payout age.
- RPC/provider error rates.
- Fee estimate vs actual fee.
- Quote age and price-source divergence.
- Ledger reconciliation mismatches.
- Sweep backlog and gas-funding cost.
- Asset/network-specific incident rates.

Include correlation IDs across invoice, provider request, webhook event, transaction hash, conversion, and settlement. Do not put secrets or unnecessary personal data into logs.

## 14.11 Production checklist

Before enabling an asset/network:

- [ ] Confirm exact network and mainnet/testnet.
- [ ] Confirm token contract/mint and decimals from a trusted source.
- [ ] Confirm deposit and payout support separately.
- [ ] Validate address and memo/tag rules.
- [ ] Define min/max amounts and dust policy.
- [ ] Configure confirmation/finality thresholds.
- [ ] Understand network fee denomination and estimation.
- [ ] Test wrong-network, underpaid, overpaid, late, and duplicate events.
- [ ] Verify webhook authentication and idempotency.
- [ ] Test provider timeout after request submission.
- [ ] Test chain/RPC outage and reorg behavior where applicable.
- [ ] Confirm ledger entries and reconciliation reports.
- [ ] Review custody, key, and withdrawal controls.
- [ ] Review compliance and geographic availability.
- [ ] Confirm refunds and customer-support procedures.
- [ ] Add monitoring, alerts, and incident runbooks.
- [ ] Enable via a controlled feature flag or whitelist.

---

# Chapter 15 — Glossary and Learning Roadmap

## 15.1 Essential glossary

| Term | Practical meaning |
|---|---|
| Address | Destination/account identifier used by a network |
| AML | Anti-Money Laundering controls |
| Asset | A specific transferable or representational value on a network |
| Base fee | EIP-1559-style minimum per-gas fee component |
| Block | A batch of transactions and related metadata |
| Blockchain | A ledger/state system secured by a protocol and consensus mechanism |
| Bridge | System intended to transfer assets/messages between networks |
| CEX | Centralized exchange |
| Chain ID | Identifier distinguishing an EVM chain/network |
| Confirmation | Evidence of a transaction's inclusion and/or subsequent blocks |
| Consensus | Rules/mechanisms for agreeing on valid state/history |
| Custody | Control or safeguarding of assets/signing authority |
| Depeg | Stablecoin price moving away from its target |
| DeFi | Decentralized finance applications and protocols |
| DEX | Decentralized exchange |
| Decimals | Number of fractional digits used to display integer token units |
| Destination tag / memo | Extra identifier needed by some deposit systems |
| Finality | Chain-specific point or confidence level at which a transaction is treated as finalized |
| Gas | Execution-resource accounting on EVM-style networks |
| Gwei | 10^9 wei; commonly used for Ethereum gas prices |
| Hash | Fixed-size digest of data |
| HD wallet | Wallet deriving many keys/addresses from a seed |
| Hot wallet | Wallet/signing setup connected to online systems |
| KYB | Know Your Business |
| KYC | Know Your Customer |
| KYT | Know Your Transaction |
| L1 | Layer 1 / base blockchain |
| L2 | Layer 2 scaling or execution system |
| Ledger | Record of balances and movements |
| Liquidity | Ability to trade or convert without excessive price impact |
| Mempool | A node's pool of pending transactions |
| Memo | Extra payment identifier, when supported by the network/provider |
| MPC | Multi-Party Computation, used in some distributed signing systems |
| Multisig | Transaction authorization requiring multiple signatures/keys |
| Native coin | The protocol's own asset |
| Nonce | Sequence value used by some account-based transaction systems |
| Off-ramp | Service converting crypto to fiat or a bank payout |
| On-ramp | Service converting fiat into crypto |
| Oracle | System supplying external data to a blockchain application |
| Private key | Secret signing material |
| Public key | Key material used in verification or address derivation |
| Reorg | Reorganization of the canonical chain history |
| RWA | Real-world asset tokenization |
| Satoshi | 10^-8 BTC |
| Seed phrase | Human-readable representation used to derive wallet keys |
| Slippage | Difference between expected and actual execution price |
| Smart contract | Code executed according to blockchain rules |
| Stablecoin | Asset designed to track a reference value |
| Sweep | Moving funds from deposit addresses to a treasury/operational wallet |
| Token | Asset represented on an existing network |
| Travel Rule | Rules for transmitting specified originator/beneficiary information for qualifying transfers |
| UTXO | Unspent transaction output |
| Validator | Participant helping validate/propose/finalize blocks, depending on chain |
| VASP | Virtual Asset Service Provider, a regulatory term used in some frameworks |
| VWAP | Volume-weighted average price |
| Wei | 10^-18 ETH |
| Wrapped token | Token representing another asset or claim |
| xpub | Extended public key material used in certain HD wallet systems |

## 15.2 The most important mental model

For a developer, keep these layers separate:

1. **Asset:** What value is being transferred?
2. **Network:** On which ledger is it transferred?
3. **Wallet/key:** Who can authorize spending?
4. **Transaction:** What state change was submitted?
5. **Fee:** What does the network charge to process it?
6. **Provider:** Which service monitors, converts, or sends it?
7. **Payment:** Which business invoice/order does the transfer satisfy?
8. **Ledger:** What does the merchant/customer owe or own in your platform?
9. **Settlement:** What final asset or fiat amount reaches the merchant?
10. **Compliance/risk:** Is the transaction permitted and safe under policy?

Many production bugs happen when two of these layers are collapsed into one field or one status.

## 15.3 A practical learning roadmap

### Stage 1 — Asset and network fundamentals
- Understand coin vs token.
- Learn asset identity, contract addresses, decimals, and network IDs.
- Compare Bitcoin UTXO transactions with Ethereum account-based transactions.
- Understand native fee assets and memo/tag rules.

**Build:** An asset registry with currencies, networks, assets, and provider mappings.

### Stage 2 — Transaction observation
- Learn block explorers and RPC APIs.
- Query transaction details and block status.
- Learn confirmations, finality, logs/events, and reorg handling.
- Build webhook signature verification and idempotent event processing.

**Build:** A small service that tracks a testnet deposit and advances a payment state machine.

### Stage 3 — Wallet and payout concepts
- Study key derivation, address generation, and signing.
- Learn custody, HD wallets, hot/cold separation, multisig/MPC concepts.
- Understand nonce management and UTXO selection.
- Never use real customer funds while learning.

**Build:** A testnet-only payout simulator with fee estimates, approval controls, and transaction tracking.

### Stage 4 — Payment accounting
- Learn quote/rate locks, fees, spreads, and settlement.
- Implement exact decimal/smallest-unit calculations.
- Design a double-entry ledger.
- Build reconciliation between your database and provider events.

**Build:** An end-to-end simulated checkout with underpayment, overpayment, duplicate webhooks, late payment, and refund scenarios.

### Stage 5 — Production operations
- Learn queue retry policies, rate limits, observability, incident response.
- Test RPC outages, provider timeouts, stuck transactions, and reorgs.
- Add alerting for ledger mismatches and payout anomalies.
- Review key management and compliance responsibilities.

**Build:** A provider adapter with a normalized API, health checks, idempotency, audit logs, and a controlled asset whitelist.

### Stage 6 — Business and risk
- Understand stablecoin issuer models and depeg risk.
- Learn liquidity, bid/ask, spread, slippage, and conversion paths.
- Compare merchant, orchestrator, processor, exchange, custodian, and ramp roles.
- Learn the compliance obligations for the actual jurisdictions and product model.

**Build:** A settlement simulator comparing crypto retention, stablecoin settlement, and fiat conversion under different fee and volatility assumptions.

## 15.4 Suggested technical references

Use primary documentation whenever possible, because protocol details and provider support can change.

- Bitcoin developer documentation: https://developer.bitcoin.org/
- Ethereum developer documentation: https://ethereum.org/developers/
- Ethereum JSON-RPC documentation: https://ethereum.org/developers/docs/apis/json-rpc/
- EIP-1559 specification: https://eips.ethereum.org/EIPS/eip-1559
- Tron developer documentation: https://developers.tron.network/
- Solana developer documentation: https://solana.com/docs
- BIP32: https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki
- BIP39: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki
- FATF virtual asset resources: https://www.fatf-gafi.org/
- Always consult the specific processor's official API documentation for provider IDs, webhook signing, status meanings, supported networks, and settlement behavior.

---

# Appendix A — Example: A USDT Payment from Start to Finish

Suppose a customer buys a product priced at USD 100 and chooses USDT on Tron.

1. The merchant creates an invoice for USD 100.
2. The pricing service obtains a current USDT/USD quote and applies its quote policy.
3. The payment service selects the exact asset record: USDT on Tron, with the verified token identifier and decimals.
4. The platform creates a deposit destination and displays the Tron network explicitly.
5. The customer sends the specified USDT amount. The customer's wallet must have enough TRX/resources to submit the token transfer.
6. The processor detects a token transfer to the expected destination.
7. The platform verifies network, token identifier, destination, amount, and transaction evidence.
8. The payment moves from `detected` to `confirming`.
9. Once the configured confirmation/finality policy is satisfied, the platform marks the on-chain payment as confirmed.
10. The ledger records the asset received and the amount owed to the merchant.
11. Depending on the agreement, the processor retains USDT, converts it, or initiates settlement.
12. The system records the actual conversion rate, service fee, network fee where applicable, and net settlement.
13. A signed/verified provider event is normalized and sent to the merchant using an idempotent webhook.
14. Reconciliation later compares the invoice, chain evidence, provider records, and settlement records.

Notice the layers: the customer paid on a blockchain; the processor confirmed that evidence; the merchant's order was updated; and settlement was accounted for separately.

# Appendix B — Example: Why a Token Deposit Can Be Hard to Sweep

A customer sends a token to a unique deposit address. The token balance increases, but the address has no native fee asset. The processor cannot simply move the token to treasury because the network still requires transaction resources.

A safe operational flow is:

1. Verify the token deposit from trusted chain/provider evidence.
2. Check that the asset is supported and not under a risk hold.
3. Estimate the token sweep cost.
4. Compare the sweep cost with the balance and business policy.
5. If worthwhile, fund the address with the minimum safe fee resources according to the network's rules.
6. Submit the sweep under controlled signing policy.
7. Track the transaction until it meets the sweep finality policy.
8. Reconcile the source and treasury balances.
9. Record fee-funding and sweep costs against the appropriate internal accounts.

If the deposit is tiny, the sweep may cost more than the balance. That is a product/accounting policy question, not merely a coding problem.

# Appendix C — Five Rules Worth Memorizing

1. **Symbol is not identity.** Always identify the asset and network together.
2. **A wallet is key management.** The asset is represented by blockchain state.
3. **A network fee is not a processor fee.** Keep network cost, service fee, and conversion spread separate.
4. **Confirmed is not always settled.** Blockchain evidence, payment status, ledger posting, and merchant settlement are different layers.
5. **Never float money math.** Use exact integer or decimal arithmetic, idempotency, and reconciliation.

---

## Final Summary

A blockchain is a ledger and execution environment. A network defines the rules under which transactions are accepted. A native coin belongs to that network; a token is represented on an existing network. A wallet manages signing authority. A network fee pays for transaction processing, while a processor fee and conversion spread are business charges. Stablecoins target a reference value but still carry issuer, liquidity, network, and depeg risks.

For payment integration, the hard work is not merely sending a transaction. It is identifying the exact asset/network, producing a safe quote, monitoring the chain, handling confirmations and exceptions, protecting signing authority, posting balanced ledger entries, reconciling settlement, and making the entire process reliable for the merchant.

**The engineering principle:** model each layer explicitly, verify external evidence, make side effects idempotent, and reconcile every movement of value.
