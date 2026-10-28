# 📘 ক্রিপ্টো বাইবেল: ডেভেলপারের জন্য (Payment Integration Focus)

> লেখক: Nayem | স্ট্যাক: Laravel / Go | ফোকাস: Crypto Payment Integration (B2BinPay-এর অভিজ্ঞতা থেকে)

---

## 📑 সূচিপত্র
1. Blockchain: ভিত্তি
2. Coin বনাম Token
3. ক্রিপ্টোর ক্লাসিফিকেশন
4. Crypto Network
5. Fee: Network Fee বনাম Gas Fee
6. কেন এত ক্রিপ্টো?
7. Stablecoin ও অন্য Fiat-পেগড কয়েন
8. Wallet
9. Transaction Lifecycle
10. ক্রিপ্টো বিজনেস ইকোসিস্টেম ও আয়ের উৎস
11. ক্রিপ্টো ও ট্রেডিং-এর সম্পর্ক
12. তিন দৃষ্টিকোণ: Merchant / Orchestrator / Processor
13. Compliance (KYC/AML/Travel Rule)
14. Developer Checklist
15. Glossary ও Roadmap

---

## অধ্যায় ১: Blockchain: ভিত্তি

**Blockchain = বিতরণকৃত (distributed) ও পরিবর্তন-অযোগ্য (immutable) খাতা (ledger)।**

- **Block**: কিছু transaction-এর গুচ্ছ।
- **Chain**: প্রতিটি block আগের block-এর **hash** ধারণ করে, তাই ইতিহাস বদলানো প্রায় অসম্ভব।
- **Node**: যে কম্পিউটার ledger-এর কপি রাখে ও নিয়ম যাচাই করে।
- **Consensus**: কোন block বৈধ, তা নিয়ে সবাই কীভাবে একমত হয়।
  - **PoW (Proof of Work)**: Mining, প্রচুর কম্পিউটিং পাওয়ার (Bitcoin)।
  - **PoS (Proof of Stake)**: Validator কয়েন stake করে (Ethereum, Solana ইত্যাদি)।
- **Cryptocurrency**: ব্লকচেইনের ভেতরের "মান" (value) যা ledger-এ রেকর্ড হয়।

> মনে রাখো: ক্রিপ্টো কোথাও "সংরক্ষিত" থাকে না। Blockchain-এ থাকে শুধু **balance-এর রেকর্ড**। তোমার **private key** প্রমাণ করে ওই balance তোমার।

---

## অধ্যায় ২: Coin বনাম Token

| | Coin | Token |
|---|---|---|
| নিজস্ব blockchain? | হ্যাঁ | না, অন্যের chain-এ চলে |
| উদাহরণ | BTC, ETH, TRX, SOL, BNB | USDT, USDC, SHIB, UNI |
| তৈরি হয় | Protocol দ্বারা | Smart contract দ্বারা |
| Fee দেওয়া হয় | নিজের কয়েনে | Host chain-এর native coin-এ |

**Smart Contract**: blockchain-এ চলা কোড। Token-এর balance আসলে একটি contract-এর ভেতরের mapping।

**Token Standard**:
- Ethereum: ERC-20 (fungible), ERC-721 (NFT)
- Tron: TRC-20
- BNB Smart Chain: BEP-20
- Solana: SPL

> ⚠️ **Dev Alert**: "USDT" একটি currency নয়, একাধিক network-এ একাধিক contract। Currency চিনতে হলে লাগে **Symbol + Network + Contract Address + Decimals**।

---

## অধ্যায় ৩: ক্রিপ্টোর ক্লাসিফিকেশন

কাজ (function) অনুযায়ী:

| ক্যাটাগরি | কাজ | উদাহরণ |
|---|---|---|
| **Store of Value / Payment** | ডিজিটাল সোনা/মুদ্রা | BTC, LTC, BCH |
| **Smart Contract Platform (L1)** | অন্য অ্যাপ চালানোর chain | ETH, SOL, TRX, BNB, AVAX |
| **Layer 2 (L2)** | L1-এর উপর সস্তা ও দ্রুত স্তর | Arbitrum, Optimism, Base, Polygon |
| **Stablecoin** | স্থিতিশীল মূল্য (fiat-পেগড) | USDT, USDC, DAI, EURC |
| **Exchange Token** | Exchange-এর ইকোসিস্টেম | BNB, OKB |
| **Utility / Governance** | সার্ভিস ব্যবহার বা ভোট | LINK, UNI, AAVE |
| **Meme Coin** | কমিউনিটি/হাইপ | DOGE, SHIB, PEPE |
| **Privacy Coin** | লেনদেন গোপন | XMR, ZEC |
| **Wrapped Token** | অন্য chain-এর asset-এর প্রতিনিধি | WBTC, WETH |
| **RWA / Tokenized Assets** | বাস্তব সম্পদের টোকেন | টোকেনাইজড বন্ড, গোল্ড |

অন্য একটি অ্যাঙ্গেল, **Payment Integration-এর জন্য ক্লাসিফিকেশন**:
1. **Native coin** (BTC, ETH, TRX)
2. **Token** (USDT-TRC20, USDC-ERC20)
3. **Memo/Tag-ভিত্তিক** (XRP, XLM, TON: একই address, আলাদা memo)

> 📌 বাস্তবে পেমেন্টে ১৫-২০টি asset-ই ৯০%+ ভলিউম ধরে: BTC, ETH, USDT, USDC, TRX, BNB, SOL, LTC, XRP, TON ইত্যাদি। বাকি হাজারো কয়েন Long-tail।

---

## অধ্যায় ৪: Crypto Network

**Network = একটি আলাদা blockchain (বা তার উপর চলা অবকাঠামো), যার নিজস্ব নিয়ম, নোড, fee ও address format আছে।**

| Network | Native Coin | Model | Fee | গতি | USDT-এ জনপ্রিয়তা |
|---|---|---|---|---|---|
| Bitcoin | BTC | UTXO | মাঝারি-উচ্চ | ধীর (~১০ মিনিট/block) | – |
| Ethereum | ETH | Account | উচ্চ | মাঝারি | ✅ (ব্যয়বহুল) |
| Tron | TRX | Account | কম (Energy/Bandwidth) | দ্রুত | ✅✅ সবচেয়ে বেশি ব্যবহৃত |
| BNB Smart Chain | BNB | Account | কম | দ্রুত | ✅ |
| Solana | SOL | Account | খুব কম | খুব দ্রুত | ✅ |
| Polygon/Arbitrum/Base | ETH/MATIC | Account (L2) | খুব কম | দ্রুত | ✅ |

> ⚠️ **সবচেয়ে বড় ঝুঁকি**: ভুল network-এ পাঠালে টাকা হারিয়ে যেতে পারে (যেমন USDT-ERC20 address-এ TRC20 পাঠানো, কখনো recoverable, কখনো না)। UI-তে network পরিষ্কার দেখাতে হবে।

**ধারণা**: EVM-compatible chain (Ethereum, BSC, Polygon, Arbitrum…) সব একই address format ব্যবহার করে (`0x...`), তবে প্রতিটি আলাদা network।

---

## অধ্যায় ৫: Fee: Network Fee বনাম Gas Fee

**Network Fee**: যেকোনো blockchain-এ transaction প্রসেস করার জন্য miner/validator-কে দেওয়া ফি (সাধারণ/বৃহত্তর শব্দ)।
**Gas Fee**: Ethereum ও EVM chain-এ এই ফি-এর নাম। কম্পিউটিং কাজের **পরিমাণ (gas units)** অনুযায়ী।

```
Gas Fee = Gas Used × Gas Price
EIP-1559:  Gas Price = Base Fee + Priority Tip
```

| Chain | Fee কীভাবে হয় |
|---|---|
| Bitcoin | Transaction size (vbyte) × sat/vbyte |
| Ethereum/EVM | Gas used × gas price (ETH/BNB/MATIC-এ) |
| Tron | Energy + Bandwidth (না থাকলে TRX burn) |
| Solana | ছোট ফিক্সড + priority fee |

**মূল পয়েন্ট**:
- Fee **সবসময় native coin-এ** দিতে হয়। USDT পাঠাতে TRX/ETH/BNB লাগে।
- Token transfer = smart contract call, তাই native coin পাঠানোর চেয়ে বেশি gas।
- Network congestion বাড়লে fee বাড়ে।

**তিন ধরনের ফি গুলিয়ে ফেলো না**:
1. **Network/Gas fee**: blockchain-কে দেওয়া
2. **Processor/Service fee**: B2BinPay-র মতো provider-এর কমিশন
3. **Conversion/Spread fee**: এক asset থেকে অন্যটিতে কনভার্টের মার্জিন

**Dev Tip**: Sweep/withdrawal-এ fee estimate করতে হয়। **Gas station / fee wallet** ধারণা: deposit address-এ token আছে কিন্তু gas নেই, তাই আগে gas top-up করে তারপর sweep।

---

## অধ্যায় ৬: কেন এত ক্রিপ্টো?

1. **Open source**: কোড fork করে নতুন chain বানানো যায় (Litecoin, BCH)।
2. **Token বানানো সহজ**: মিনিটে ERC-20/TRC-20 তৈরি করা যায়, তাই হাজার হাজার।
3. **Trade-off**: Decentralization, Security, Scalability একসাথে পূর্ণ পাওয়া কঠিন (Trilemma), তাই ভিন্ন ডিজাইন।
4. **ভিন্ন use case**: পেমেন্ট, DeFi, গেমিং, প্রাইভেসি, গভর্নেন্স।
5. **ফান্ডিং ও spec­ulation**: ICO/মিম/হাইপ।
6. **Interoperability সমস্যা**: তাই Wrapped token ও bridge।

> বাস্তবতা: বেশিরভাগ কয়েনের কোনো real usage নেই। একজন payment engineer-এর মূল কাজ: **কোন asset সাপোর্ট করবে** তা বেছে নেওয়া (liquidity, নিরাপত্তা, চাহিদা, compliance ঝুঁকি দেখে)।

---

## অধ্যায় ৭: Stablecoin ও অন্য Fiat-পেগড কয়েন

**Stablecoin = যে ক্রিপ্টোর মূল্য একটি সম্পদের (সাধারণত ১ USD) সাথে বাঁধা।** Volatility এড়িয়ে ক্রিপ্টোর গতি ও ২৪/৭ সুবিধা পাওয়ার জন্য।

| ধরন | কীভাবে স্থির থাকে | উদাহরণ | ঝুঁকি |
|---|---|---|---|
| **Fiat-backed** | ইস্যুকারীর রিজার্ভে USD/বন্ড | USDT, USDC | ইস্যুকারী ও রিজার্ভের উপর বিশ্বাস, freeze/blacklist |
| **Crypto-backed** | অতিরিক্ত ক্রিপ্টো collateral | DAI | Collateral ক্র্যাশ |
| **Algorithmic** | কোড/সাপ্লাই ম্যানিপুলেশন | UST (ব্যর্থ, ২০২২) | De-peg, পতন |

**USDT vs USDC**: USDT (Tether) সবচেয়ে বড় ও লিকুইড, বিশেষ করে Tron-এ। USDC (Circle) বেশি রেগুলেটেড/স্বচ্ছতা-কেন্দ্রিক।

**EUR, INR, CNY ইত্যাদির মতো অন্য fiat-পেগড কয়েন আছে কি?** হ্যাঁ, তবে ছোট:

| Fiat | উদাহরণ | মন্তব্য |
|---|---|---|
| EUR | EURC (Circle), EURS | MiCA রেগুলেশনে বাড়ছে |
| SGD | XSGD | Singapore-এ রেগুলেটেড |
| JPY | JPYC | জাপানে |
| BRL | BRZ | ব্রাজিলে জনপ্রিয় |
| GBP | GBPT ইত্যাদি | ছোট ভলিউম |
| CNY/CNH | CNHT (offshore yuan) | মূল ভূখণ্ডে ক্রিপ্টো নিষিদ্ধ, তাই সীমিত |
| INR | সীমিত/নগণ্য | ভারতে কড়া রেগুলেশন ও কর |

**কেন USD-ই রাজা?**
- USD আন্তর্জাতিক বাণিজ্য ও রিজার্ভ মুদ্রা, তাই চাহিদা সবচেয়ে বেশি।
- Liquidity একবার জমলে সেটিই আরও liquidity টানে (network effect)।
- স্থানীয় মুদ্রার স্টেবলকয়েনে রেগুলেটরি বাধা বেশি।

> 💡 CBDC (Central Bank Digital Currency, যেমন e-CNY, Digital Rupee) আলাদা জিনিস। এটি সরকার-নিয়ন্ত্রিত, permissionless ক্রিপ্টো নয়।

---

## অধ্যায় ৮: Wallet

**Wallet = private key ও address ম্যানেজ করার টুল।** (কয়েন "ভেতরে" থাকে না।)

```
Private Key → Public Key → Address
(গোপন)         (শেয়ারযোগ্য)   (ডিপোজিটের ঠিকানা)
```

### ধরন
| | Custodial | Non-custodial |
|---|---|---|
| Key কার কাছে? | তৃতীয় পক্ষ (exchange/processor) | ইউজার নিজে |
| ঝুঁকি | প্রদানকারী হ্যাক/দেউলিয়া | key হারালে সব শেষ |
| উদাহরণ | Binance, B2BinPay | MetaMask, Ledger |

| | Hot Wallet | Cold Wallet |
|---|---|---|
| ইন্টারনেট | সংযুক্ত | বিচ্ছিন্ন |
| ব্যবহার | দৈনিক লেনদেন | বড় ফান্ড সংরক্ষণ |
| ঝুঁকি | বেশি | কম |

### Processor-দের জন্য গুরুত্বপূর্ণ ধারণা
- **HD Wallet (BIP32/39/44)**: একটি seed/mnemonic থেকে অসংখ্য address তৈরি।
- **xpub (extended public key)**: private key ছাড়া নতুন deposit address জেনারেট।
- **প্রতি invoice-এ আলাদা address**: কোন পেমেন্ট কোন অর্ডারের তা চেনার সহজ উপায়।
- **Multisig / MPC**: একাধিক পক্ষের সম্মতিতে সাইন, বড় ফান্ডের নিরাপত্তা।
- **Sweeping**: অনেক deposit address থেকে ফান্ড একটি main/treasury wallet-এ জড়ো করা।
- **UTXO vs Account model**:
  - UTXO (Bitcoin): ব্যালেন্স = খরচ-না-হওয়া output-এর সমষ্টি।
  - Account (Ethereum): ব্যাংক অ্যাকাউন্টের মতো সরাসরি balance।
- **Memo/Tag/Destination Tag** (XRP, XLM, TON): এক shared address + আইডেন্টিফায়ার। memo ভুল হলে ফান্ড ক্রেডিট হয় না।

> 🔑 **Not your keys, not your coins.**

---

## অধ্যায় ৯: Transaction Lifecycle

```
Create → Sign (private key) → Broadcast → Mempool → Included in Block
→ Confirmations → Final (সম্পূর্ণ নিশ্চিত)
```

- **Confirmation**: transaction-এর পরে যত block যোগ হয়। সংখ্যা বাড়লে reversal-এর ঝুঁকি কমে।
- **Reorg**: কখনো chain পুনর্গঠিত হয়ে পূর্বের block বাতিল হতে পারে (বিশেষ করে কম confirmation-এ)।
- **Finality**: আর বদলানো যাবে না। Chain ভেদে সময় আলাদা।
- **Irreversible**: ব্যাংকের chargeback নেই। ভুল বা প্রতারণামূলক পেমেন্ট ফেরত পাওয়া কঠিন।

### Payment Processor ফ্লো (Deposit)
1. Merchant invoice তৈরি করে → unique address + amount + rate lock।
2. Customer পাঠায় → processor blockchain মনিটর করে।
3. Transaction detect → "pending" → N confirmation → "confirmed"।
4. **Webhook/callback** merchant-এর সার্ভারে।
5. Settlement (crypto হিসেবে বা fiat-এ কনভার্ট করে)।

### Edge Case (যেগুলো প্রোডাকশনে কামড় দেয়)
- **Underpaid / Overpaid**: কম বা বেশি পাঠানো
- **Late payment**: invoice expire হওয়ার পর পাঠানো
- **Wrong network / wrong asset**
- **Duplicate webhook** (তাই idempotency জরুরি)
- **Dust / ছোট অ্যামাউন্ট**
- **Fee-র কারণে net amount কম আসা**

---

## অধ্যায় ১০: ক্রিপ্টো বিজনেস ইকোসিস্টেম ও আয়ের উৎস

### খেলোয়াড়েরা
| Role | কাজ |
|---|---|
| **Exchange** (Binance, Coinbase) | কেনা-বেচা, লিকুইডিটি |
| **Liquidity Provider / Market Maker** | বাজারে বাই/সেল প্রাইস দেওয়া |
| **OTC Desk** | বড় ট্রেড, সরাসরি |
| **Crypto Payment Processor** (B2BinPay, BitPay, CoinPayments) | মার্চেন্টের ক্রিপ্টো গ্রহণ/পাঠানো |
| **Payment Orchestrator** | একাধিক provider/rail একটি API-তে |
| **On/Off-Ramp** | Fiat ↔ Crypto |
| **Custodian** | প্রাতিষ্ঠানিক নিরাপদ হেফাজত |
| **Chain Analytics** (Chainalysis, TRM, Elliptic) | AML/KYT ঝুঁকি স্কোর |
| **Issuer** (Tether, Circle) | Stablecoin ইস্যু |

### মুনাফা কীভাবে হয়?
| উৎস | বিবরণ |
|---|---|
| **Processing fee** | প্রতি ট্রানজাকশনে ০.৫–১%+ (সাধারণ রেঞ্জ) |
| **Spread / FX markup** | মার্কেট রেট ও কাস্টমার রেটের পার্থক্য |
| **Conversion fee** | Crypto→fiat/stable কনভার্সন |
| **Withdrawal/Payout fee** | নেটওয়ার্ক fee + মার্জিন |
| **Network fee markup** | Gas-এর উপর সামান্য বাড়তি |
| **Float** | সেটেলমেন্টের আগে ফান্ড হোল্ড |
| **Exchange-এর আয়** | Trading fee, listing fee, লেন্ডিং |
| **Staking / Yield** | Stake করা asset থেকে reward |
| **SaaS / API fee** | মাসিক সাবস্ক্রিপশন |
| **Market-making** | Bid-ask spread থেকে আয় |

---

## অধ্যায় ১১: ক্রিপ্টো ও ট্রেডিং-এর সম্পর্ক

- ক্রিপ্টোর দাম প্রতি সেকেন্ডে বদলায়। এটাই **volatility**।
- মার্চেন্ট সাধারণত **volatility চায় না**, তারা চায় stable fiat মূল্য।
- তাই:
  1. **Rate lock / quote**: ১০–৩০ মিনিটের জন্য নির্দিষ্ট রেট ধরে রাখা।
  2. **Auto-convert**: পেমেন্ট আসামাত্র stablecoin/fiat-এ রূপান্তর।
  3. **Hedging**: ঝুঁকি কমাতে বিপরীত পজিশন।
  4. **Stablecoin settlement**: USDT/USDC-তে সেটেল।
- **Price source**: Binance/Kraken/aggregator থেকে রেট, প্রায়ই একাধিক সোর্সের গড় (বা VWAP)।
- **Slippage**: বড় অর্ডারে প্রত্যাশিত বনাম প্রকৃত রেটের পার্থক্য।
- **Pair**: BTC/USDT = ১ BTC-এর দাম USDT-তে।

> প্রসেসর যে কনভার্সন দেয়, তা ভেতরে ভেতরে একটি **exchange/liquidity provider-এ ট্রেড**। এখানেই spread ও risk।

---

## অধ্যায় ১২: তিন দৃষ্টিকোণ

### 🛒 Merchant
- **চায়**: গ্লোবাল কাস্টমার, কম ফি, chargeback নেই, দ্রুত সেটেলমেন্ট।
- **ভয়**: volatility, compliance, ভুল পেমেন্ট, রিফান্ড জটিলতা।
- **সিদ্ধান্ত**: ক্রিপ্টো রাখবে নাকি fiat-এ সেটেল।

### 🔀 Payment Orchestrator (PSP/Aggregator)
- **কাজ**: একাধিক processor/exchange/ramp একত্র করে একটি unified API দেওয়া।
- **মূল ফিচার**:
  - **Routing**: সবচেয়ে সস্তা/দ্রুত/নির্ভরযোগ্য provider বেছে নেওয়া
  - **Failover**: একটি ডাউন হলে অন্যটিতে
  - **Normalization**: ভিন্ন provider-এর currency ID, status, webhook এক ফরম্যাটে
  - **Reconciliation, ledger, reporting**
- **মূল চ্যালেঞ্জ**: প্রতিটি provider-এর **আলাদা currency ID ও naming**। তাই নিজস্ব internal currency mapping লাগে।

### ⚙️ Crypto Processor
- **কাজ**: address generation, blockchain monitoring, confirmation tracking, sweeping, payout, wallet infra, নিরাপত্তা।
- **আয়**: fee, spread, float।
- **মূল ঝুঁকি**: key নিরাপত্তা, hot wallet হ্যাক, AML ঝুঁকি, node reliability।

---

## অধ্যায় ১৩: Compliance

- **KYC**: গ্রাহক পরিচয় যাচাই
- **AML**: মানি লন্ডারিং প্রতিরোধ
- **KYT (Know Your Transaction)**: ট্রানজাকশনের উৎস/গন্তব্য ঝুঁকি স্কোরিং (Chainalysis, TRM)
- **Travel Rule (FATF)**: নির্দিষ্ট সীমার উপরে sender/receiver তথ্য VASP-দের মধ্যে আদানপ্রদান
- **Sanctions screening**: নিষিদ্ধ ঠিকানা/দেশ (OFAC)
- **Blacklist/Freeze**: Tether/Circle ঠিকানা freeze করতে পারে
- **লাইসেন্স**: দেশভেদে VASP/MSB/MiCA ইত্যাদি
- **Mixer/Tumbler, Darknet** সংশ্লিষ্ট ঠিকানা = high risk

---

## অধ্যায় ১৪: Developer Checklist (Laravel / Go)

### Currency Modeling
```
currency:  id, symbol, name
network:   id, name, chain_type (utxo|evm|tron|solana...)
asset:     currency_id, network_id, contract_address, decimals,
           requires_memo, min_confirmations, is_active
provider_asset_map: provider, provider_currency_id → asset_id
```

### অবশ্যই মনে রাখবে
- ✅ **Decimals আলাদা**: BTC=8, ETH=18, USDT(TRC20)=6, USDT(ERC20)=6, USDT(BSC)=18। প্রতিটি asset-এর decimals সংরক্ষণ করো।
- ✅ **Float কখনোই না**। Laravel: `bcmath` / `brick/math`; Go: `math/big` বা `shopspring/decimal`। DB-তে smallest unit (integer) বা `DECIMAL(36,18)`।
- ✅ **Idempotency**: webhook ডুপ্লিকেট আসবেই (idempotency key / unique tx_hash+index)।
- ✅ **Webhook signature যাচাই**।
- ✅ **State machine**: `created → pending → confirmed → settled / expired / failed / underpaid / overpaid`।
- ✅ **Double-entry ledger**: প্রতিটি টাকার নড়াচড়ার রেকর্ড।
- ✅ **Reconciliation job**: নিজের ledger বনাম provider/blockchain মিলিয়ে দেখা।
- ✅ **Confirmation threshold** asset অনুযায়ী কনফিগারেবল।
- ✅ **Rate lock ও expiry** ঠিক রাখা।
- ✅ **Memo/tag** ফিল্ড হ্যান্ডেল করা।
- ✅ **Retry + queue** (Laravel Queue / Go worker) এবং timeout।
- ✅ **Secrets/Keys**: কখনো কোডে/লগে নয়। HSM/KMS/Vault।
- ✅ **Audit log** ও **monitoring/alerting**।
- ✅ Provider-এর হাজার হাজার কয়েন সব সাপোর্ট করার দরকার নেই। **Whitelist** করো।

---

## অধ্যায় ১৫: Glossary ও Roadmap

| শব্দ | অর্থ |
|---|---|
| **Mempool** | অপেক্ষমান ট্রানজাকশনের পুল |
| **Nonce** | অ্যাকাউন্টের ট্রানজাকশন ক্রম নম্বর (EVM) |
| **Satoshi** | ১ BTC = ১০০,০০০,০০০ sat |
| **Wei / Gwei** | ১ ETH = 10¹⁸ wei; ১ Gwei = 10⁹ wei |
| **DeFi** | Decentralized Finance (লেন্ডিং, DEX ইত্যাদি) |
| **DEX / CEX** | Decentralized / Centralized Exchange |
| **Bridge** | এক chain থেকে অন্য chain-এ asset সরানো |
| **Oracle** | Blockchain-এ বাইরের ডেটা (যেমন price) আনা |
| **De-peg** | Stablecoin-এর মূল্য পেগ থেকে সরে যাওয়া |
| **VASP** | Virtual Asset Service Provider |
| **On-ramp / Off-ramp** | Fiat→Crypto / Crypto→Fiat |
| **Dust** | অতি ক্ষুদ্র, ব্যবহার অযোগ্য ব্যালেন্স |
| **TVL** | DeFi-তে লক করা মোট মূল্য |

### 🗺️ শেখার রোডম্যাপ
1. ✅ এই বই (ফান্ডামেন্টাল)
2. Bitcoin (UTXO, script) ও Ethereum (account, EVM, gas) গভীরে
3. Tron ও Solana-র ফি মডেল
4. HD Wallet (BIP32/39/44) হাতে-কলমে কোড (Go দিয়ে)
5. Node/RPC (Alchemy, Infura, TronGrid) দিয়ে ট্রানজাকশন মনিটরিং
6. Custody architecture (hot/cold/MPC)
7. Compliance ও Travel Rule
8. International payment: SWIFT, SEPA, ACH, card networks বনাম crypto rails

---

> 🧠 **এক লাইনে সারকথা**: Blockchain = ledger, Network = আলাদা ledger, Coin/Token = ledger-এর ভেতরের মান, Wallet = key, Fee = native coin-এ, Stablecoin = ক্রিপ্টোর গতি + fiat-এর স্থিতি, Processor = এই সবকিছুকে merchant-এর জন্য সহজ করা।
