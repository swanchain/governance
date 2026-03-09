---
SIP: 003
Title: Swan 2.0 Inference Cloud - Model Catalog, Provider Incentives & Token Utility Redesign
Author: Swan Core Team
Status: Draft
Category: Economic / Protocol
Created: 2026-03-03
Discussions-To: https://github.com/swanchain/governance/discussions/21
Requires: SIP-002
Replaces: None
---

> **⚠️ DRAFT PROPOSAL - NOT FINALIZED**
>
> This SIP is under active development. All parameters, formulas, thresholds, and timelines are subject to change based on community feedback and technical review. Nothing in this document should be considered final until the proposal moves to **Review** status.
>
> **🧪 Try Swan Inference**: https://inference.swanchain.io/

---

## Summary

This proposal defines the Swan 2.0 Inference Cloud economics: a clean-slate transition from UBI subsidies to a market-driven AI inference marketplace. It introduces minimum hardware requirements for inference providers, a tiered model catalog with per-token pricing, a 95/5 provider-first revenue split, contribution-weighted SWAN rewards, and a Pay-with-SWAN mechanism that gives users a 20% inference discount while creating organic token demand.

The core principle: providers that cannot serve real inference workloads should not receive SWAN rewards. Only GPUs that generate value for end users deserve network incentives.

## Motivation

Swan 1.0 (UBI model) bootstrapped a registered network of 166 GPUs across 64 providers. However, the network faces a fundamental gap between registered capacity and operational reality:

- Of 64 registered providers, only 6 are genuinely operational (9.4% operational rate)
- The majority of active GPUs are legacy datacenter cards (TESLA P4, 2016) unsuitable for modern AI inference
- High-value GPU inventory (H100, A100, RTX 4090) is registered but shows 0% uptime — listed but not serving workloads
- Flat UBI rewards all registered providers equally, whether they serve inference or sit idle
- SWAN token lacks direct consumer utility beyond provider collateral and governance

The UBI cost analysis (March 3, 2026) quantifies the waste:

| Metric | Value |
|--------|-------|
| Daily UBI Pool | 58,369 SWAN ($29.28 at $0.0005016/SWAN) |
| Active Provider CU | 314.1 / 1,255.5 total (25%) |
| UBI to Active Providers | 14,603 SWAN ($7.32/day) — 25% |
| UBI to Idle Providers (0% uptime) | 43,766 SWAN ($21.95/day) — **75%** |
| Yearly Waste to Idle Providers | ~$8,013 |
| UBI Distribution Contract | [0xE7b1Dc0F5C33214c5dc213ea6215d5b10508aBc8](https://mainnet-explorer.swanchain.io/address/0xE7b1Dc0F5C33214c5dc213ea6215d5b10508aBc8) |

**75% of daily UBI goes to providers with 0% uptime doing zero work.** This is not sustainable.

The DePIN sector is shifting from narrative-driven valuation to revenue-based fundamentals. Projects like Venice.ai ($295M market cap, 1.3M users) demonstrate that privacy-focused, uncensored AI inference with well-designed token utility can achieve significant market traction. Swan 2.0 must close the gap between registered capacity and real service delivery.

## Specification

### 1. Relationship to SIP-002

SIP-003 **supersedes** the following sections of SIP-002:

| Aspect | SIP-002 | SIP-003 (supersedes) |
|--------|---------|---------------------|
| Revenue split | 70% provider / 20% treasury / 10% burn | 95% provider / 5% growth fund / 0% treasury |
| Payout currency | SWAN tokens (80% liquid / 20% locked) | Stablecoins (USDC/USDT) directly |
| Transition timeline | 3-phase hybrid UBI | Immediate 95/5 on inference revenue, UBI sunset over 3 months |

SIP-003 **inherits** from SIP-002:

- The five-factor contribution scoring framework (weights and formula unchanged)
- Collateral requirements (SWAN staking per hardware tier)
- Anti-gaming measures (sybil prevention, pattern detection)
- Unified Computing Provider (CP) role (ECP/FCP merger)

The rationale for the revenue split override: at current revenue levels (~$6/day), the difference between 70% and 95% provider share is negligible in dollar terms but sends a signal that matters for recruitment. A 20% protocol treasury cut on $6/day is $1.20 — not funding anything. The dynamic schedule in Section 5 brings this back toward 80/2+3% burn/15% as revenue scales past $1,000/day, which converges with SIP-002's intent.

### 2. Network Reset: From Phantom Capacity to Real Inference

#### 2.1 The Problem

The current dashboard reports 166 GPUs across 64 providers. In reality:

| Metric | Reported | Actual |
|--------|----------|--------|
| Active Providers | 64 | 6 genuinely operational |
| Operational GPUs | 166 | ~15 (in active providers) |
| Inference-capable GPUs | 166 | < 10 (most are TESLA P4 / GTX 1050 Ti) |
| 70B-capable GPUs | 73 (H100+A100+L40S) | 0 operational |
| Active Regions | 19 | 3 (California, Hunan, Tokyo) |

#### 2.2 The Solution: Clean Slate

Swan 2.0 redefines what it means to be a Computing Provider. Registration alone is not sufficient. To receive SWAN rewards, a provider must:

- Pass an initial inference benchmark (math accuracy, code generation, response latency)
- Maintain > 50% uptime over a trailing 7-day window
- Successfully serve real inference requests (not just ZK sampling tasks)
- Meet minimum hardware requirements for at least one model in the catalog

Providers that fail these criteria receive zero SWAN rewards. Their collateral is not slashed — they can upgrade hardware and re-qualify at any time. But idle registration no longer earns tokens.

#### 2.3 Minimum Hardware for Inference

| Tier | Min VRAM | Example GPUs | Models | Status |
|------|----------|-------------|--------|--------|
| S | 38GB+ | L40S, A100, H100 | 70B RP models | Recruit new providers |
| A | 24GB | RTX 4090, 3090, A6000 | 24B-32B Agent | Activate idle inventory |
| B | 12GB | RTX 4070 Ti, 3080 Ti | 8B-12B Free tier | Some current providers |
| C | 8GB | RTX 3070, 4060 | Embedding, Whisper | Lowest qualifying tier |
| Rejected | < 8GB or legacy | TESLA P4, GTX 1050 Ti | None | No rewards |

TESLA P4 (8GB, FP16 5.5 TFLOPS, 2016) and GTX 1050 Ti (4GB) cannot serve any model in the Swan 2.0 catalog at acceptable quality.

#### 2.4 Target Network Composition

| Tier | Current (Operational) | Target (Q3 2026) | Target (Q4 2026) |
|------|----------------------|-------------------|-------------------|
| S (70B) | 0 | 5-10 GPUs | 20+ GPUs |
| A (24B) | 1-2 (RTX 5090, 3080) | 10-20 GPUs | 40+ GPUs |
| B (8B-12B) | 3-5 (4070, TITAN RTX) | 20-30 GPUs | 60+ GPUs |
| C (Tools) | ~5 | 10+ GPUs | 20+ GPUs |
| **Total Inference-Ready** | **~10** | **45-70 GPUs** | **140+ GPUs** |

### 3. Model Catalog & Per-Token Pricing

#### 3.1 Tiered Model Catalog

All models are served via an OpenAI-compatible API. Pricing is set at 50-66% below comparable centralized providers.

**Tier S: Premium Roleplay (70B) — Swan Exclusive**

| Model | Params | Input /M tok | Output /M tok | Min VRAM |
|-------|--------|-------------|---------------|----------|
| Sapphira L3.3-70B | 70B | $0.20 | $0.30 | 38GB |
| Nevoria 70B | 70B | $0.20 | $0.30 | 38GB |
| Euryale v2.3 70B | 70B | $0.20 | $0.30 | 38GB |

**Tier A: Agent & Advanced (24B-32B)**

| Model | Params | Input /M tok | Output /M tok | Min VRAM |
|-------|--------|-------------|---------------|----------|
| Qwen3 32B | 32B | $0.08 | $0.12 | 18GB |
| Devstral 24B | 24B | $0.06 | $0.10 | 14GB |
| Mistral Small 3.2 24B | 24B | $0.06 | $0.10 | 14GB |

**Tier B: Free & Growth (8B-12B)**

| Model | Params | Input /M tok | Output /M tok | Min VRAM |
|-------|--------|-------------|---------------|----------|
| Violet Lotus 12B | 12B | $0.02 | $0.04 | 7GB |
| Stheno 8B | 8B | $0.01 | $0.03 | 5GB |
| Qwen3 8B | 8B | $0.01 | $0.03 | 5GB |
| Llama 4 Scout 17B | 17B MoE | $0.03 | $0.05 | 12GB |

**Tier C: Utility Models**

| Model | Type | Pricing | Unit | Min VRAM |
|-------|------|---------|------|----------|
| Whisper Large v3 | Audio | $0.003 | per minute | 4GB |
| BGE / E5 Large | Embedding | $0.005 | per M tokens | 2GB |
| FLUX.1 Schnell | Image Gen | $0.003 | per image | 12GB |
| SDXL | Image Gen | $0.002 | per image | 8GB |

#### 3.2 Free Tier

| Parameter | Specification |
|-----------|--------------|
| Available Models | Stheno 8B, Qwen3 8B (Tier B only) |
| Daily Token Limit | 50,000 tokens (~25 conversations) |
| Rate Limit | 5 requests per minute |
| Concurrency | 1 simultaneous request |
| Registration | API key only (free, no KYC) |
| Upgrade Path | USDC top-up or SWAN staking |

### 4. Provider-First Revenue Split: 95/5

#### 4.1 Bootstrap Phase (Current)

| Recipient | Share | Purpose |
|-----------|-------|---------|
| Computing Provider | 95% | Direct payout in payment currency (USDC/USDT) |
| Growth Fund | 5% | Provider recruitment, integrations, dev tooling, liquidity |
| Protocol Treasury | 0% | Deferred until network reaches sustainability threshold |

#### 4.2 Growth Fund Allocation

| Category | Target % | Examples |
|----------|----------|---------|
| Provider Recruitment | 40% | GPU onboarding bounties, hardware upgrade subsidies |
| Integrations & Partnerships | 30% | API integration grants, ecosystem project funding |
| Liquidity | 20% | SWAN/USDC DEX liquidity seeding |
| Dev Tooling | 10% | SDK improvements, dashboard features, monitoring |

Growth Fund spending is reported monthly with transaction hashes. Governance can vote to redirect Growth Fund allocation or introduce a burn component once daily revenue exceeds $1,000 (30-day rolling average).

#### 4.3 Dynamic Revenue Split Schedule

As network revenue grows, the split gradually adjusts through governance votes:

| Phase | Daily Revenue | Provider | Growth Fund | Treasury |
|-------|--------------|----------|-------------|----------|
| Bootstrap | < $100 | 95% | 5% | 0% |
| Growth | $100 - $1,000 | 90% | 5% | 5% |
| Maturity | $1,000 - $10,000 | 85% | 3% + 2% burn | 10% |
| Scale | > $10,000 | 80% | 2% + 3% burn | 15% |

Token burn is introduced only at the Maturity phase ($1,000+/day) when the burn amount becomes meaningful. Each phase transition requires a governance vote with a minimum 7-day voting period. Revenue thresholds are measured as a 30-day rolling average.

### 5. Quality Assurance & Provider Requirements

#### 5.1 Ongoing Quality Monitoring

| Level | Frequency | What It Tests |
|-------|-----------|---------------|
| **Heartbeat checks** | Every 30 minutes | Lightweight latency probe — confirms the provider is online and responsive |
| **Quality benchmarks** | 2-4 times daily (randomized intervals) | Accuracy and throughput — runs a standard inference test and verifies output quality |
| **Triggered audits** | Immediately upon event | Activated by user complaints or anomalous success rate drops — full diagnostic check |

Randomized benchmark timing prevents providers from gaming the system by only performing well during predictable test windows.

#### 5.2 Slashing Conditions

| Condition | Consequence |
|-----------|------------|
| Benchmark failure (1st) | Warning + 24h suspension from task routing |
| Consecutive failure (2nd) | 10% collateral slashed |
| Consecutive failure (3rd) | 30% collateral slashed + network removal |
| Inference success rate < 80% | Deprioritized in request routing |
| Uptime < 90% (30-day rolling) | Deprioritized in request routing |

#### 5.3 Current Provider Readiness

Applying SIP-003 hardware tiers to the 6 currently active providers:

| GPU | SIP-003 Tier | VRAM | Provider |
|-----|-------------|------|----------|
| TITAN RTX | A (Agent 24B-32B) | 24GB | nocall, 0x8B1 |
| RTX 5090 | A (Agent 24B-32B) | 32GB | 0xf95 (Tokyo) |
| A4000 | B (Free 8B-12B) | 16GB | nocall |
| RTX 3080 | B (Free 8B-12B) | 10GB | 0x5aC (myh) |
| RTX 4070 | B (Free 8B-12B) | 12GB | 0xf95 (Tokyo) |
| RTX 3070 | C (Utility) | 8GB | 0x8B1 |
| RTX 4060 | C (Utility) | 8GB | mqm |
| TESLA P4 | Rejected | 8GB legacy | 5 providers |
| GTX 1050 Ti | Rejected | 4GB | 3 providers |

**Per-Provider: UBI Now vs Inference Revenue (SIP-003)**

| Provider | Qualifying GPUs | UBI Now | Inference at $100/day | Inference at $500/day |
|----------|----------------|---------|----------------------|----------------------|
| nocall | TITAN RTX(A) + A4000(B) | $2.63/day | $23.75 | $118.75 |
| 0x8B1 (node2) | TITAN RTX(A) + RTX 3070(C) | $1.99/day | $23.75 | $118.75 |
| 0xf95 (Tokyo) | RTX 5090(A) + RTX 4070(B) | $0.20/day | $23.75 | $118.75 |
| 0x5aC (myh) | RTX 3080(B) | $0.53/day | $11.88 | $59.38 |
| mqm | RTX 4060(C) | $1.00/day | $11.88 | $59.38 |
| 0xE63 (node3) | None (TESLA P4 + 1050 Ti only) | $0.97/day | **$0.00** | **$0.00** |

The breakeven point where inference revenue matches current UBI is just **$25/day total network revenue**.

### 6. SWAN Token Utility: Pay-with-SWAN

#### 6.1 Dual Payment Options

| Payment Method | Discount | Settlement | Example (1M Tier B output tokens) |
|---------------|----------|------------|-----------------------------------|
| USDC/USDT | 0% (base price) | Instant | $0.03 |
| SWAN | 20% discount | Market swap at time of request | $0.024 (saves $0.006) |

#### 6.2 How It Works

1. Consumer sends an inference request with `payment: "SWAN"` in the API call
2. The required SWAN amount is calculated from the base USD price minus 20% discount
3. SWAN is deducted from the user's prepaid balance (topped up via on-chain deposit)
4. Provider receives 95% of the payment value in their preferred currency (USDC or SWAN)
5. The 5% platform share goes to the Growth Fund

#### 6.3 Why Pay-with-SWAN Over Stake-for-Inference

| | Stake-for-Inference | Pay-with-SWAN |
|--|---------------------|---------------|
| User friction | High — lock tokens for daily quota | Low — top up and use |
| Capital efficiency | Poor — $500 locked for $3/day | High — pay only for what you use |
| Token demand | Artificial (staking for yield) | Organic (discount incentive) |
| Complexity | Staking contracts, quota tracking | Simple balance + deduction |
| Price sensitivity | Locked users lose if price drops | Users buy SWAN when discount > spread |

#### 6.4 Token Value Flywheel

- Users buy SWAN on market to get 20% inference discount, creating organic buy pressure
- 5% of revenue funds Growth Fund → provider recruitment, integrations, liquidity seeding
- More providers + better integrations attract more users and inference demand
- Higher inference volume increases provider stablecoin earnings, attracting better GPUs
- Better GPUs enable better models and lower latency, further growing user base
- At Maturity phase ($1,000+/day revenue), governance can introduce token burn for deflationary pressure

#### 6.5 Supported Chains & Payment Roadmap

| Payment Method | Chain | Timeline | Notes |
|---------------|-------|----------|-------|
| SWAN (20% discount) | **Swan Chain** (ID: 254) | March 2026 | Native token, lowest friction |
| USDC | **Base** | Q2 2026 | Low fees (~$0.01), large user base |
| USDT | **Base** | Q2 2026 | Alongside USDC |
| USDC / USDT | **Ethereum** | Q2 2026 | For L1 stablecoin holders |
| Credit card (Stripe) | Off-chain | Q2 2026 | Fiat onramp for non-crypto users |

Provider payouts are always settled on **Swan Chain** via the MerkleDistributor contract, regardless of which chain the consumer paid on.

### 7. Three-Month Transition: UBI Shutdown & Inference Launch

#### 7.1 The Decision: Stop UBI Entirely

Current UBI emissions (58,369 SWAN/day = $29.28) are not meaningful enough to keep any provider online. The best provider earns $2.63/day — that doesn't cover electricity. Meanwhile, the emissions create 21.3M SWAN/year of dilution and sell pressure for near-zero return.

**Swan 2.0 eliminates UBI completely.** Providers earn solely from inference revenue (95% of fees).

After UBI stops, SWAN token utility is:
- **Pay-with-SWAN** — 20% inference discount for consumers
- **Provider collateral** — required to join the network
- **Governance** — vote on protocol parameters and revenue splits

#### 7.2 UBI Taper Schedule

| Period | Dates | UBI Level | SWAN/day | Notes |
|--------|-------|-----------|----------|-------|
| Swan 2.0 Launch | March 16 - April 9 | **100%** | 58,369 | Platform goes live, vote happens April 1-7 |
| Month 1 post-vote | April 10 - May 9 | **50%** | 29,185 | Contribution-weighted, legacy hardware earns zero |
| Month 2 post-vote | May 10 - June 9 | **20%** | 11,674 | Providers should earn primarily from inference |
| Month 3+ | June 10 onwards | **0%** | 0 | UBI permanently off. Inference revenue only. |

#### 7.3 Launch Month: Swan 2.0 Goes Live (March 16 - April 9, 2026)

**Goal:** Launch inference platform, onboard providers, seed initial demand. UBI unchanged.

| Action | Owner | Target |
|--------|-------|--------|
| Deploy inference API (OpenAI-compatible) to production | Core team | Week 1-2 |
| Onboard 6 active providers to inference platform | Core team + providers | Week 2-3 |
| Run inference benchmarks on all active GPUs | Core team | Week 2 |
| Classify providers into SIP-003 tiers (A/B/C/Rejected) | Automated | Week 2 |
| Launch free tier inference for early adopters | Core team | Week 3-4 |
| Publish OpenAI-compatible API docs and SDK examples | Core team | Week 2 |
| Seed SWAN/USDC liquidity on DEX | Growth Fund | Week 1 |
| Announce UBI sunset timeline to provider community | Core team | Week 1 |
| **Governance vote on UBI sunset (April 1-7)** | Community | Week 3 |

**UBI status:** 100% — full UBI continues (58,369 SWAN/day).

**Revenue target:** $1-10/day (internal testing + early adopters)

#### 7.4 Month 1 Post-Vote: Growth (April 10 - May 9, 2026)

**Goal:** Generate real inference revenue, attract external users. UBI begins tapering.

| Action | Owner | Target |
|--------|-------|--------|
| Public API launch — open registration for API keys | Core team | Week 1 |
| Free tier live (Stheno 8B, Qwen3 8B — 50K tokens/day) | Core team | Week 1 |
| OpenClaw AI Agent integration (recommended backend) | Partnerships | Week 2 |
| SillyTavern / JanitorAI ecosystem outreach | Partnerships | Week 2-3 |
| Provider recruitment: target 5-10 new Tier A/B GPUs | Growth Fund | Ongoing |
| Pay-with-SWAN discount live | Core team | Week 2 |
| Provider dashboard: real-time earnings, request logs | Core team | Week 3 |
| First monthly Growth Fund spending report published | Core team | Week 4 |

**UBI status:** 50% (29,185 SWAN/day). Contribution-weighted — only providers serving inference get the reduced pool.

**Revenue target:** $10-100/day

#### 7.5 Month 2 Post-Vote: Scale (May 10 - June 9, 2026)

**Goal:** Scale inference demand, expand provider fleet, prepare for full UBI cutoff.

| Action | Owner | Target |
|--------|-------|--------|
| Tier S models live (70B RP) if Tier S providers onboarded | Core team | Week 1-2 |
| Tier C utility models live (Whisper, FLUX.1, embeddings) | Core team | Week 2 |
| Full model catalog available | Core team | Week 3 |
| Provider self-service onboarding (no manual setup) | Core team | Week 2 |
| Second monthly Growth Fund spending report | Core team | Week 4 |

**UBI status:** 20% (11,674 SWAN/day). Contribution-weighted.

**Revenue target:** $50-500/day

#### 7.6 Month 3 Post-Vote: UBI Off (June 10+, 2026)

**Goal:** Full transition to revenue-driven model. UBI stops permanently.

**UBI status:** 0%. Permanently stopped. Requires governance vote to reinstate.

**Revenue target:** $100+/day

#### 7.7 Safety Valve

If by end of Month 3 the network cannot sustain minimum viable provider economics ($50/day revenue), governance can vote to:

1. **Extend UBI at 25%** for 3 more months (contribution-weighted only)
2. **Redirect collateral yield** — use locked collateral as temporary provider subsidies
3. **Increase Growth Fund** allocation toward demand generation

#### 7.8 UBI Emission Savings

| Metric | With UBI (status quo) | Without UBI (SIP-003) |
|--------|----------------------|----------------------|
| Annual SWAN emitted | ~21,300,000 SWAN | 0 |
| Annual dilution (USD) | ~$10,686 | $0 |
| Sell pressure from providers | Daily | None |
| Provider income source | SWAN dumps | Stablecoin revenue |
| Token holder impact | Diluted | Protected |

### 8. Transparency & On-Chain Verification

- Real-time public dashboard showing daily inference calls, token throughput, active providers, and model utilization rates
- On-chain settlement via MerkleDistributor with verifiable Merkle proofs for all provider payments
- Monthly Growth Fund spending reports published with transaction hashes and allocation breakdown
- Provider leaderboard with availability, success rate, and latency metrics visible to all participants
- Revenue split ratios and phase transition votes recorded on-chain through governance contracts

### 9. Growth Targets & Milestones

| Milestone | Daily Calls | Daily Revenue | Inference GPUs | Timeline |
|-----------|------------|---------------|----------------|----------|
| Current | < 1K (Swan) | < $1 | ~10 | Now |
| Foundation | 10K | $10 | 30+ | Q2 2026 |
| Traction | 100K | $100 | 70+ | Q3 2026 |
| Growth | 1M | $1,000 | 150+ | Q4 2026 |
| Scale | 10M | $10,000 | 300+ | 2027 |

## Rationale

### Why 95/5 Instead of SIP-002's 70/20/10

At $6/day network revenue, a 20% protocol treasury share yields $1.20/day — not enough to fund anything. The 95/5 split maximizes provider retention during bootstrap when every GPU matters. The dynamic schedule (Section 4.3) converges toward SIP-002's intent as revenue scales.

### Why Stablecoins Instead of SWAN Payouts

Providers have operational costs (electricity, bandwidth, hardware) denominated in fiat. Paying in SWAN forces providers to sell immediately, creating constant sell pressure. Stablecoin payouts eliminate this dynamic while Pay-with-SWAN (Section 6) creates organic buy pressure from the demand side.

### Why Eliminate UBI Entirely

75% of UBI flows to idle providers doing zero work. The remaining 25% yields $2.63/day for the best provider — not meaningful income. Inference revenue at just $25/day network volume already exceeds the best UBI payout. Every dollar spent on UBI emissions is a dollar not spent on demand generation.

## Implementation

| Phase | Timeline | Deliverables |
|-------|----------|-------------|
| Launch | March 16 - April 9, 2026 | Inference API live, provider onboarding, benchmarks |
| Governance Vote | April 1-7, 2026 | Community vote on UBI sunset |
| Growth | April 10 - May 9, 2026 | Public API, free tier, integrations, 50% UBI |
| Scale | May 10 - June 9, 2026 | Full model catalog, self-service onboarding, 20% UBI |
| Revenue-Only | June 10+, 2026 | UBI permanently off, inference revenue only |

**Responsible teams:**
- **Swan Cloud Inc.:** Platform development, API infrastructure, provider tooling
- **Swan Foundation:** Growth Fund management, governance facilitation
- **Community:** Governance votes, provider operations, integration development

## Governance Impact

- **Token holders:** Reduced dilution (21.3M SWAN/year emissions eliminated), organic buy pressure from Pay-with-SWAN
- **Active providers:** Higher per-GPU income (4-10x vs UBI at $100/day revenue), stablecoin payouts
- **Idle providers:** Zero rewards — must upgrade hardware and serve inference to earn
- **DAO operations:** Growth Fund spending requires monthly transparency reports; phase transitions require governance votes

## Risk Factors

- **Inference quality:** Decentralized GPU inference may not match centralized provider latency and reliability. Mitigation: benchmark-based quality assurance and health-aware routing.
- **Model obsolescence:** AI models evolve rapidly. Mitigation: modular model catalog updated without protocol changes.
- **Centralized competition:** Groq LPU and cloud providers are driving inference costs down. Mitigation: focus on uncensored RP models and privacy-sensitive use cases.
- **Token price dependency:** Pay-with-SWAN discount ties some demand to token price. Mitigation: SWAN is optional — providers earn 95% in stablecoins regardless.
- **Regulatory:** Uncensored AI models face evolving regulatory scrutiny. Mitigation: decentralized provider network with no single point of control.

## Voting Parameters

| Parameter | Value |
|-----------|-------|
| Swan 2.0 Launch | March 16, 2026 |
| Voting Start | April 1, 2026 |
| Voting Period | 7 days (April 1-7) |
| Quorum | 10% of staked SWAN |
| Approval Threshold | Simple majority (>50%) |
| Implementation Delay | 48 hours after vote passes |
| UBI Sunset Effective | April 10, 2026 |

## References

- [Discussion Thread: SIP-003](https://github.com/swanchain/governance/discussions/21)
- [SIP-002: Contribution-Based Rewards](https://github.com/swanchain/governance/blob/main/SIPs/SIP-002-contribution-based-rewards.md)
- [SIP-001: FCP Subsidy Program](https://github.com/swanchain/governance/blob/main/SIPs/SIP-001-fcp-subsidy-program.md)
- [Swan Inference Platform](https://inference.swanchain.io/)
- [UBI Distribution Contract](https://mainnet-explorer.swanchain.io/address/0xE7b1Dc0F5C33214c5dc213ea6215d5b10508aBc8)
- [SWAN Token (CoinGecko)](https://www.coingecko.com/en/coins/swan-chain)
