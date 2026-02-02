---
SIP: 002
Title: Unified Computing Provider Role and Contribution-Based Rewards
Author: Swan DAO Community
Status: Draft
Category: Economic / Protocol
Created: 2026-01-30
Discussions-To: https://github.com/swanchain/governance/discussions/16
Requires: SIP-001
Replaces: None
---

> **⚠️ DRAFT PROPOSAL - NOT FINALIZED**
>
> This SIP is under active development. All parameters, formulas, thresholds, and timelines are subject to change based on community feedback and technical review. Nothing in this document should be considered final until the proposal moves to **Review** status.
>
> **🧪 Try Swan Inference (Dev)**: https://inference-dev.swanchain.io/
>
> We encourage providers and community members to test the inference platform and provide feedback on the contribution tracking mechanisms before finalization.

---

## Summary

Transition Swan Network's incentive model from Universal Basic Incentives (UBI) "sampling mode" to a contribution-based rewards system, while simultaneously merging Edge Computing Providers (ECPs) and Fog Computing Providers (FCPs) into a unified **Computing Provider (CP)** role. Under this model, providers earn proportionally to their measurable contributions: inference requests processed, tokens generated, uptime maintained, and service quality delivered. Rewards are paid in **SWAN tokens** at launch, with quarterly governance reviews to evaluate adding stablecoin options (USDC/USDT/OP) based on protocol revenue and provider needs. **SWAN token collateral** is required initially, with community governance able to approve stablecoin collateral alternatives in the future. This proposal leverages Swan Inference (Swan 2.0) infrastructure to track, score, and reward real compute work, with an accelerated 3-month transition timeline.

## Motivation

### Current State

The existing UBI model distributes rewards based on random sampling tasks and collateral staking. While effective for bootstrapping the network, this approach has several limitations:

1. **Misaligned Incentives**: Legacy providers (ECPs/FCPs) receive subsidies primarily for staking collateral, not for providing useful compute. Data shows many providers contribute mostly as "collateral staking participants" with limited real workload.

2. **Sell Pressure Without Value Creation**: Subsidies distributed without corresponding compute work translate into market sell pressure without strengthening network utility.

3. **Growing AI Inference Demand**: Swan ecosystem products (Aimo, NebulaBlock, MissionHub, SwanFi) face increasing demand for GPU resources. The incentive model should direct rewards toward providers who serve this demand.

4. **Swan 2.0 Capabilities**: Swan Inference now provides comprehensive contribution tracking, making merit-based rewards technically feasible.

### Desired Outcome

- Providers earn rewards proportional to actual compute contributions
- Subsidy efficiency increases as rewards flow to active, high-quality providers
- Network capacity aligns with real market demand
- Reduced token sell pressure through more effective distribution

## Specification

### 1. Contribution Metrics

Swan Inference tracks the following metrics per provider, which form the basis for contribution scoring:

| Metric | Description | Data Source |
|--------|-------------|-------------|
| `total_inferences` | Total inference requests processed | Usage records |
| `successful_inferences` | Requests completed successfully | Usage records |
| `total_input_tokens` | Input tokens processed | Usage records |
| `total_output_tokens` | Output tokens generated | Usage records |
| `uptime_7d` | 7-day uptime percentage | WebSocket heartbeats |
| `uptime_30d` | 30-day uptime percentage | WebSocket heartbeats |
| `models_served` | Number of distinct models supported | Provider registration |
| `avg_latency_ms` | Average response latency | Request traces |
| `success_rate` | Ratio of successful to total requests | Usage records |

### 2. Contribution Score Formula

Each provider receives a daily Contribution Score calculated as:

```
Contribution_Score = (
    W_inf  × normalize(inference_count) +
    W_tok  × normalize(total_tokens) +
    W_up   × uptime_score +
    W_qual × quality_score +
    W_div  × diversity_score
)

Where:
  W_inf  = 0.30  (Inference volume weight)
  W_tok  = 0.25  (Token throughput weight)
  W_up   = 0.20  (Uptime weight)
  W_qual = 0.15  (Quality weight)
  W_div  = 0.10  (Model diversity weight)

  normalize(x) = x / max(x_all_providers)

  uptime_score = uptime_30d / 100

  quality_score = success_rate × (1 - normalize(avg_latency_ms))

  diversity_score = models_served / max_models_in_catalog
```

### 3. Reward Distribution

#### Phase 1: Hybrid Mode (Month 1)

Maintain both UBI and contribution rewards during transition:

```
Daily_Reward = UBI_Component + Contribution_Component

UBI_Component = UBI_Base × (1 - Phase1_Reduction × utilization_factor)
  Where:
    Phase1_Reduction = 0.25
    utilization_factor = paid_gpu_hours / total_online_hours

Contribution_Component = (Provider_Score / Sum_All_Scores) × Daily_Contribution_Pool
  Where:
    Daily_Contribution_Pool = 25% of current daily UBI allocation
```

#### Phase 2: Contribution-Weighted UBI (Month 2)

Shift majority of rewards to contribution-based distribution:

```
Daily_Reward = UBI_Component + Contribution_Component

UBI_Component = UBI_Base × 0.50 × availability_factor
  Where:
    availability_factor = min(1.0, uptime_30d / 95)  # Must maintain 95%+ uptime

Contribution_Component = (Provider_Score / Sum_All_Scores) × Daily_Contribution_Pool
  Where:
    Daily_Contribution_Pool = 50% of current daily UBI allocation
```

#### Phase 3: Pure Contribution Mode (Month 3+)

Eliminate UBI baseline; all rewards based on contribution:

```
Daily_Reward = Contribution_Reward + Availability_Bonus

Contribution_Reward = (Provider_Score / Sum_All_Scores) × Daily_Reward_Pool

Availability_Bonus = Availability_Pool × (online_hours / 24) × hardware_tier_multiplier
  Where:
    Availability_Pool = 10% of Daily_Reward_Pool (incentivizes standby capacity)
    hardware_tier_multiplier:
      - RTX 3090 / A4000: 1.0x
      - RTX 4090 / A5000 / A6000: 1.5x
      - A100: 2.5x
      - H100: 4.0x
```

#### Payout Structure

**Initial Launch**: 100% SWAN Token rewards

All contribution rewards are paid in SWAN tokens at launch. This simplifies implementation and aligns provider incentives with network growth.

| Component | Allocation |
|-----------|------------|
| Liquid SWAN | 80% |
| Locked SWAN (3-month vesting) | 20% |

**Future Stablecoin Option** (Subject to Governance Review):

Governance may introduce stablecoin payment options (USDC/USDT/OP) based on:
- Protocol revenue growth from paid inference requests
- Provider feedback on operational cost challenges
- Treasury sustainability assessment

Quarterly reviews will evaluate whether to introduce hybrid payout plans:

| Review Trigger | Potential Action |
|----------------|------------------|
| Paid inference revenue > $50K/month | Consider 20-30% stablecoin option |
| Provider churn due to OpEx concerns | Evaluate stability-heavy plan for large clusters |
| Treasury stablecoin reserves sufficient | Enable provider choice between token/stable mix |

**Inference Revenue Share** (When Applicable):

When paid inference requests generate protocol revenue, providers receive direct payment:

```
Inference_Revenue_Share = Request_Fee × Provider_Cut
  Where:
    Provider_Cut = 70% (provider, paid in request currency)
    Protocol_Cut = 20% (treasury)
    Burn_Cut = 10% (SWAN buyback & burn)
```

This creates a sustainable path where contribution rewards transition from subsidies to real revenue as paid inference demand grows.

### 4. Minimum Contribution Thresholds

To prevent reward fragmentation and gaming:

| Threshold | Requirement | Effect if Not Met |
|-----------|-------------|-------------------|
| Minimum Uptime | 80% over 7 days | Excluded from contribution pool |
| Minimum Inferences | 100/week | Reduced to 50% contribution weight |
| Minimum Success Rate | 90% | Reduced to 75% contribution weight |
| Minimum Online Hours | 120 hours/week | Pro-rated availability bonus |

### 5. Collateral Requirements

Providers must stake SWAN tokens as collateral to participate in contribution rewards.

**Initial Collateral** (SWAN Token):

| Hardware Tier | Minimum Collateral | Slashing Conditions |
|---------------|-------------------|---------------------|
| RTX 3090 / A4000 | 5,000 SWAN | Uptime < 50% for 7 days |
| RTX 4090 / A5000 / A6000 | 10,000 SWAN | Repeated failed requests (>20% failure rate) |
| A100 | 25,000 SWAN | Malicious behavior or gaming detected |
| H100 | 50,000 SWAN | Unauthorized hardware changes |

**Collateral Rules**:
- Collateral locked for duration of active provider status
- 7-day unbonding period upon exit
- Slashed collateral goes to treasury for ecosystem development
- Collateral amount may be adjusted by governance based on SWAN price

**Future Stablecoin Collateral Option**:

Community may vote to allow stablecoin (USDC/USDT) as collateral alternative when:
- Contribution-based system proves stable (6+ months operation)
- Sufficient provider adoption demonstrates model viability
- Governance proposal passes with standard quorum

Until then, SWAN token collateral remains mandatory to ensure provider commitment and network security.

### 6. New Provider Ramp-Up Period

New providers receive a 30-day ramp-up bonus to bootstrap their contribution history:

```
Ramp_Up_Bonus = Base_Bonus × (1 - days_since_registration / 30)
  Where:
    Base_Bonus = Average daily reward of bottom 25% providers
```

### 7. Anti-Gaming Measures

- **Latency Penalties**: Artificially slow responses reduce quality_score
- **Success Rate Verification**: Failed requests audited; pattern abuse leads to suspension
- **Sybil Prevention**: Contribution scores capped per unique hardware attestation
- **Collusion Detection**: Unusual request patterns between consumer/provider pairs flagged

## Rationale

### Why Contribution-Based Over Pure UBI?

1. **Value Alignment**: Rewards flow to providers creating network value, not passive stakers
2. **Market Efficiency**: Prices reflect actual compute costs and quality
3. **Sustainable Economics**: Revenue from real usage can eventually replace subsidies
4. **Quality Incentive**: Success rate and latency metrics encourage service excellence

### Why Start with Pure SWAN Token Rewards?

1. **Simplicity**: Single token payout simplifies implementation and accounting
2. **Alignment**: Providers are fully aligned with SWAN token value and network success
3. **Treasury Conservation**: Preserves stablecoin reserves for critical operational needs
4. **Flexibility**: Quarterly governance reviews can introduce stablecoin options when justified by real data (provider feedback, revenue growth, churn metrics)

### Why Accelerated 3-Month Transition?

1. **Market Urgency**: Growing AI inference demand requires rapid capacity alignment
2. **Swan 2.0 Readiness**: Infrastructure already supports contribution tracking
3. **Reduce Prolonged Uncertainty**: Faster transition minimizes provider confusion
4. **Monthly Checkpoints**: Each phase still allows course correction based on data

### Alternatives Considered

| Alternative | Reason Rejected |
|-------------|-----------------|
| Immediate UBI elimination | Too disruptive; may cause provider exodus |
| Pure staking rewards | Doesn't incentivize useful compute |
| Fixed per-request payment | Ignores quality and availability factors |
| Auction-based allocation | Too complex; favors large providers |
| 7-month gradual transition | Too slow given market demand; prolongs uncertainty |
| Maintain separate ECP/FCP roles | Creates unnecessary complexity; both serve same AI inference workloads |

### Industry Precedents

- **Bittensor**: Pure contribution model based on AI model quality
- **Akash Network**: Market-based pricing with provider competition
- **Render Network**: Per-job payment for GPU render work
- **Filecoin**: Hybrid model with storage proofs and deal revenue

## Implementation

### Timeline (Accelerated 3-Month Transition)

| Phase | Duration | Key Actions |
|-------|----------|-------------|
| **Phase 0** | Week 1 | Deploy contribution scoring API; publish informational scores; announce ECP/FCP merger |
| **Phase 1** | Month 1 | Hybrid mode (25% contribution-weighted); begin unified CP registration |
| **Phase 2** | Month 2 | Contribution-weighted UBI (50% contribution); complete CP migration |
| **Phase 3** | Month 3+ | Pure contribution mode (governance vote required) |
| **Evaluation** | Month 4 | Comprehensive review; adjust parameters or revert if needed |

### Responsible Parties

| Party | Responsibility |
|-------|----------------|
| **Swan Cloud Inc.** | Implement contribution scoring API in Swan Inference |
| **Swan Foundation** | Manage reward pool allocation and treasury disbursements |
| **ubi-server maintainers** | Integrate contribution scores into reward calculation |
| **Governance Committee** | Monitor metrics and propose parameter adjustments |

### Dependencies

- Swan Inference v0.2.0 with `/api/v1/rewards/contribution` endpoint
- ubi-server integration with Swan Inference metrics API
- Provider dashboard updates to display contribution scores
- Smart contract updates for contribution-weighted distribution (if on-chain)

### Unified Computing Provider (CP) Role

This proposal merges the legacy ECP (Edge Computing Provider) and FCP (Fog Computing Provider) roles into a single **Computing Provider (CP)** classification. All providers are evaluated equally based on contribution metrics.

### Migration Path for Existing Providers

1. **ECPs and FCPs running inference tasks**: Automatically converted to unified CP role; contribution tracked from Day 1
2. **Providers not running inference**: 30-day grace period to either:
   - Onboard to Swan Inference and begin serving AI workloads, OR
   - Migrate staked SWAN to SwanFi with **1.1x governance weight bonus**
3. **Hardware Requirements for CP**: ≥24GB VRAM recommended; ≥48GB VRAM for priority task routing

## Governance Impact

### Token Holders

- **Positive**: Reduced sell pressure from more efficient reward distribution
- **Positive**: Increased network utility drives demand for SWAN
- **Consideration**: May reduce staking APY for passive holders

### Computing Providers

- **Active Providers**: Increased earnings potential based on contribution
- **Passive Providers**: Reduced rewards; incentivized to increase utilization or exit
- **New Providers**: 30-day ramp-up bonus eases entry barrier

### DAO Operations

- **Increased Complexity**: Contribution scoring requires ongoing parameter tuning
- **Transparency Requirement**: Regular publication of scoring methodology and results
- **Governance Votes**: Phase transitions require community approval

### Ecosystem Products

- **SwanFi**: May see increased deposits from providers migrating out of compute role
- **Aimo/NebulaBlock**: Better GPU availability from incentivized CPs
- **MissionHub**: More reliable compute for mission execution

## References

- [GitHub Discussion #11](https://github.com/swanchain/governance/discussions/11) - FCP Subsidy Program
- [GitHub Discussion #13](https://github.com/swanchain/governance/discussions/13) - ECP Role Transition
- [GitHub Discussion #15](https://github.com/swanchain/governance/discussions/15) - ECPs to AI Inference
- [GitHub Discussion #16](https://github.com/swanchain/governance/discussions/16) - UBI Mode Conversion
- [SIP-001](./SIP-001-fcp-subsidy-program.md) - Stage 1 FCP Subsidy Program
- Swan Inference Architecture: `docs/architecture/inference.md`
- Swan Inference Contribution Stats: `internal/module/service/provider_auth.go`
- [DePIN Tokenomics Best Practices](https://bingx.com/en/learn/what-are-the-top-depin-crypto-projects)
