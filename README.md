# wholecomputercurrency

# Whole-Machine Currency: Exploratory Research Brief

Date: 2026-10-29  
Status: Research discussion, not a protocol specification

## 1. Project vision

Explore a cryptocurrency whose issuance and mining are accessible to ordinary personal computers, including older machines. The initial hardware reference point is a 2011-era Intel Core i5-class computer with approximately 4 GB RAM and a GPU.

The broader objective is not only technical decentralization. It is to give people in different countries a straightforward way to obtain and use the same currency without requiring an exchange account, specialized mining hardware, or access to a particular banking system.

Useful sources of inspiration include:

- Kaspa-style blockDAG architecture for fast proof-of-work consensus.
- Orchard-style shielded transactions for privacy, potentially as a later stage.
- CPU-oriented and memory-hard mining as prior art, while recognizing that no algorithm is permanently ASIC-proof.

## 2. Agreed principles and definitions

### Resource-proportional fairness

Mining rewards should primarily be proportional to contributed computational resources. Someone operating 1,000 computers may fairly receive approximately 1,000 times the expected reward of someone operating one computer.

The goal is not one-person-one-vote or one-physical-computer-one-vote. It is to make balanced, general-purpose computing resources useful for mining and to avoid excessive advantages for early participants or single-component specialized miners.

### Multiple mining identities are acceptable

A powerful computer may divide its resources among several mining identities. This is acceptable if each identity makes a real, sustained resource commitment.

A participant should therefore be understood as a resource-backed mining commitment, not a proven human or physical computer. Time requirements alone cannot prevent parallel identity creation.

### Distribution and adoption are central

Broad issuance can create the conditions for social legitimacy and adoption, but it does not guarantee either. Coins can later become concentrated through sale or transfer. A wallet containing dust is not necessarily meaningful adoption.

Sustainable value should ultimately come from real use, continued participation, cross-border utility, security, and liquidity rather than issuance alone.

### Majority hash power

The project discussion is willing to treat majority computational control as a legitimate outcome of resource-proportional consensus. Even under that philosophy, majority hash power permits censorship, reordering, and reorganization but does not permit forging wallet signatures or making honest nodes accept invalid consensus rules. Economic and social recognition remain separate from protocol acceptance.

## 3. Whole-machine proof of work

### Objective

Design a heterogeneous or whole-machine PoW in which competitive mining requires a balanced combination of:

- CPU processing;
- GPU-style parallel processing;
- RAM capacity, bandwidth, or latency;
- storage capacity and continuing availability.

A computer lacking a required component should be unable to mine competitively or at all. This should reduce the advantage of hardware that optimizes only one component.

### Balance model

A conceptual balance score could use a bottleneck rather than an additive score:

```text
B_i = min(
    C_i / C_target,
    G_i / G_target,
    M_i / M_target,
    S_i / S_target
)
```

Here `C`, `G`, `M`, and `S` represent CPU, GPU, memory, and storage capabilities. This is a design model, not something that consensus should measure through self-reported specifications.

The real implementation should use deterministic, interdependent tasks. Valid shares should demonstrate completed work rather than claimed hardware capabilities.

### Performance over time

Accepted shares naturally measure both balanced performance and participation duration:

```text
work credit ≈ balanced performance × participation time
```

For example, a weak computer contributing 3 work units per minute for 24 hours contributes 4,320 units. A strong computer contributing 20 units per minute for one minute contributes 20 units. The weaker machine can therefore earn more through longer service, while the stronger machine retains an advantage over equal periods.

### Storage over time

A one-time storage benchmark is insufficient because storage could be borrowed or discarded. If storage is a required commitment, the protocol needs repeated unpredictable challenges or another proof-of-storage-over-time mechanism.

### Technical boundaries

Whole-machine PoW can raise the cost and complexity of specialization, but it cannot prove that mining occurred on a consumer PC. Custom mining systems can still integrate CPUs, GPUs, memory, and storage. Screens, manufacturer identity, and physical-computer provenance cannot be reliably established by permissionless PoW.

Block verification must remain cheap and deterministic on ordinary computers. Requiring every full node to perform expensive GPU work would undermine accessibility. GPU floating-point behavior and driver differences must not affect consensus results.

BlockDAG throughput and Orchard-style privacy are separate design problems. Privacy should not require miners to generate zero-knowledge proofs for mining. Shielded-wallet proving, proof verification, scanning, storage, and synchronization costs need separate evaluation.

## 4. Protocol-native reward smoothing

### Selected research direction

The preferred research direction is protocol-native reward smoothing combined with a small minimum payment for eligible sustained participants.

Mining pools have the lowest implementation and protocol cost, but they introduce operator, custody, payout, censorship, and concentration risks. Extremely frequent blocks cannot solve global-scale reward variance: one trillion blocks per second is physically and operationally impossible for ordinary full nodes.

### Work-based smoothing

Miners submit lower-difficulty shares during each reward epoch. If:

- `R` is the epoch reward;
- `w_i` is participant `i`'s accepted work;
- `W` is total accepted work;

then proportional rewards are:

```text
r_i = R × w_i / W
```

Finding a full block need not give the finder the entire reward. Blocks advance consensus, while shares determine how rewards are divided.

Rewards can accrue internally and become withdrawable above an anti-dust threshold. A minimum withdrawal threshold is different from a guaranteed minimum issuance.

### Eligibility through sustained service

A possible epoch is one week. To qualify for the base component, a participant must submit valid shares throughout the week and meet a minimum work threshold `q`.

A week-long requirement:

- prevents immediate register-and-claim behavior;
- rewards continuity;
- gives weak computers time to accumulate work;
- can demonstrate continuing resource commitments.

It does not stop parallel identities. A machine with weekly capacity `Q` can support approximately `floor(Q / q)` eligible identities. Under this project's fairness definition, that is acceptable because each identity represents real committed resources.

### Small base-payment incentive

The base payment should be a modest participation incentive, not a lucrative universal subsidy. A conceptual target is a 5–10% increase in coin rewards, although the exact meaning and calibration remain open. This is not a guarantee of a 5–10% profit increase, which also depends on electricity, hardware, and market value.

An earlier budget-bounded model divided epoch issuance into a base pool and a work pool:

```text
r_i = βR / N + (1 - β)R × w_i / W
```

where `β` is the base-pool fraction and `N` is the number of eligible participants.

This model guarantees that the base pool stays within its issuance budget, but its per-participant payment falls as participation grows. A positive flat base payment also creates some splitting incentive. The latest adaptive-issuance model below is another option.

A lower-Sybil alternative is a 5–10% continuity multiplier applied to work-based rewards rather than a flat base payment. That would reward uptime without paying ten flat bonuses to ten identities, but it would not guarantee a minimum payment.

## 5. Supplementary service lottery

A lottery can be a small supplementary incentive, not the primary distribution mechanism.

Possible construction:

1. A participant earns a ticket through sustained whole-machine work during an epoch.
2. The protocol snapshots eligible tickets.
3. Publicly verifiable, manipulation-resistant randomness selects winners.
4. A small portion of issuance pays the prizes automatically.

For a lottery pool `L`, `K` equal prizes, and `N` tickets, the expected lottery value per ticket is:

```text
(K / N) × (L / K) = L / N
```

Larger prizes increase excitement and variance but do not increase expected distribution. A very large prize means almost everyone receives nothing from the lottery. Base and proportional rewards remain necessary for broad access.

Randomness should not depend only on the latest block hash, because miners could withhold or select blocks to influence results. Commitments from many participants, future entropy, and a verifiable delay function are possible ingredients.

## 6. Participation-responsive issuance

### Problem with fixed early issuance

With fixed issuance `R`, expected miner rewards are:

```text
r_i = R × h_i / H
```

where `h_i` is one miner's hash rate and `H` is total network hash rate. Early miners receive many coins per unit of work because `H` is small. Later miners receive much less for the same work.

The objective is to issue fewer coins when participation is small and more coins as participation grows, reducing the early-participant advantage associated with Bitcoin-style fixed issuance and halving schedules.

### Work-linked issuance

If issuance is proportional to total work:

```text
R = kH
```

then:

```text
r_i = kH × h_i / H = kh_i
```

The expected coin reward per unit of work remains approximately constant as the network grows.

In practice, the protocol should use verified epoch work or carefully smoothed difficulty, not instantaneous or self-reported hash rates.

### Current candidate monetary rule

The native share ledger could measure:

- `W_t`: total verified whole-machine work in epoch `t`;
- `N_t`: eligible sustained resource commitments in epoch `t`.

A candidate issuance rule is:

```text
R_t = aW_t + bN_t
```

An eligible participant would receive:

```text
r_i = a × w_i + b
```

where `a` is the coin reward per work unit and `b` is the small base payment. The lottery should allocate part of this issuance rather than necessarily add more issuance.

Total supply grows with cumulative participation:

```text
S = sum over t of (aW_t + bN_t)
```

This is the latest preferred direction for reducing early issuance concentration while keeping future distribution accessible.

### Monetary tradeoffs

- There may be no fixed maximum supply. Continuing access for future participants requires continuing issuance.
- More participation does not automatically create proportional economic demand.
- Growing issuance dilutes existing holders and may increase miner selling pressure; this is partly intentional but does not guarantee price stability.
- Future hardware may perform the same work with less electricity. Equal coins per work unit does not mean equal production cost across eras.
- Early participants can still benefit from holding before adoption grows. The design reduces their issuance-per-work advantage rather than eliminating every early advantage.
- A fixed `b` for every eligible identity can create substantial issuance if identity count grows rapidly. Eligibility must require meaningful resources, and `b` must remain modest.
- Sudden hash-rate changes and difficulty-adjustment artifacts must not become issuance exploits.
- A fixed supply cap and indefinitely equal access to new issuance are in tension.

## 7. Lessons from conventional money

The following distribution principles are worth preserving:

- **Universal access matters.** People need a straightforward way to obtain their first usable balance.
- **Distribution must reach the edges.** A system is not inclusive if only technically sophisticated or wealthy participants can use it.
- **Small balances must remain functional.** Fees and minimum payments cannot exceed their practical value.
- **Issuance needs legitimacy.** People should understand who benefits from new money creation and why.
- **Distribution is ongoing.** Existing monetary systems continually serve new workers, businesses, and communities rather than distributing money only once.

Conventional systems also distinguish functions that this protocol should not conflate:

- central banks issue base money and influence financial conditions;
- commercial banks create deposit money through lending;
- governments fund welfare and other transfers through fiscal policy;
- banks, ATMs, and payment networks provide access.

A mining protocol cannot identify poverty or replicate targeted welfare without identity information. Its base payment is better understood as a transparent resource-participation dividend.

Decentralized issuance is not automatically cheaper or more environmentally friendly. Early PoW can consume little total electricity when participation and value are low, but that phase also has low security. Competition and mining revenue drive network-defense spending upward as value grows.

A decentralized currency can give people in different countries access to the same issuance and transaction system without requiring correspondent banks or one nation's reserve currency. Initial adoption could come from small communities exchanging real goods, services, and work.

## 8. Main open research questions

### Whole-machine PoW

- Can CPU, GPU, RAM, and storage tasks be made genuinely interdependent?
- What is the realistic performance ratio between a 2011 PC, a modern PC, and a custom miner?
- Can verification remain inexpensive on a CPU-only, 4 GB machine?
- Can storage commitments be verified repeatedly without excessive network traffic?
- Can all operations be deterministic across hardware and drivers?

### Native shares and aggregation

- How low should share difficulty be for weak computers?
- How can millions or billions of shares be aggregated without bloating the chain?
- Can permissionless aggregators produce succinct validity proofs?
- What data-availability and censorship-resistance mechanisms are needed?
- Can miners bypass aggregators if they are ignored?

### Participation and rewards

- What minimum weekly work `q` defines an eligible participant?
- How should continuity be measured across a week?
- Should the base component be a fixed payment `b`, a fixed base-pool fraction `β`, or a continuity multiplier?
- How large can the splitting incentive become in equilibrium?
- What withdrawal threshold keeps small balances usable after fees?

### Monetary policy

- How should `a` and `b` be selected and adjusted, if at all?
- What happens to price, security, and participation under rapid growth or decline?
- Does work-linked issuance produce destabilizing feedback or useful self-regulation?
- Is unlimited supply accepted as a permanent principle?
- How should emission respond to hardware-efficiency improvements?

### Lottery

- What fraction of issuance should go to prizes?
- How many winners provide the best balance between excitement and distribution?
- Should tickets expire each epoch?
- How should publicly verifiable randomness be constructed?

### Architecture and security

- What blockDAG parameters can the target computers actually validate?
- When should shielded transactions be introduced?
- Is accepting majority computational control an explicit security assumption or only an experimental position?
- What protections are still needed against censorship, reorganization, botnets, and early attacks?

## 9. Suggested next steps

1. Formalize the monetary model first: compare fixed issuance, `R = aW`, and `R = aW + bN` under several participation scenarios.
2. Define participant eligibility in terms of weekly verified work and resource commitments.
3. Model identity splitting and calculate when the base reward becomes more attractive than undivided mining.
4. Specify the native share ledger and its aggregation requirements before designing the PoW details.
5. Benchmark candidate whole-machine workloads on real old and new computers.
6. Prototype reward smoothing and adaptive issuance in simulation before introducing blockDAG and privacy complexity.
7. Treat the lottery as an optional module after base and proportional rewards are economically sound.

## 10. Concise statement of the current direction

Build a participation-responsive currency using whole-machine PoW and protocol-native share-based reward smoothing. Reward verified work proportionally, give sustained resource-backed participants a modest base incentive, and optionally reserve a small amount for a service lottery. Issue fewer coins when participation is low and more as participation grows, reducing early miners' issuance-per-work advantage. Measure success by broad ongoing access, usable small balances, real transactions, and durable adoption rather than by the number of coins or wallets alone.
