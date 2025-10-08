---
SIP: 001
Title: Stage 1 Funding for Computing Providers (FCP) Subsidy Program
Author: Flyworker (@flyworker)
Status: Draft
Category: Economic / Ecosystem
Created: 2025-10-07
Discussions-To: https://github.com/swanchain/governance/discussions/11
Requires: None
Replaces: None
---

## Summary

Launch Stage 1 of the Funding for Computing Providers (FCP) program to subsidize high-performance GPU operators, secure three-month minimum availability commitments, and pilot a hybrid payout model that blends stablecoins with Swan Token incentives.

## Motivation

Swan Network demand for AI training and inference on 7B-70B parameter models is growing faster than the current supply of premium GPUs. Existing Swan Token rewards attract contributors, but additional financing is required to: (1) onboard new top-tier providers, (2) keep advanced clusters online for sustained workloads, and (3) validate a mixed payout structure that supports provider operating costs without eroding the Swan Token economy.

## Specification

### Program Objectives

- Strengthen Swan's AI Superchain capacity ahead of confirmed enterprise and protocol user demand.
- Prioritize GPUs capable of reliably serving 7B-70B parameter models.
- Provide supplemental subsidies in addition to Swan Token rewards.
- Require a minimum three-month commitment to guarantee resource stability.

### Eligibility and Priority

- **Baseline Hardware**: RTX 3090, RTX 4090, RTX 5090, A4000, A4500, A5000, A6000, A100, H100, or higher.
- **Cluster Bonus**: Multi-GPU rigs (>=8 GPUs) and dense server cards receive additional weighting.
- **Model-to-Hardware Guidance**:

| Model Scale | Parameters | Recommended GPU | Deployment Config | FCP Subsidy Priority |
| --- | --- | --- | --- | --- |
| 7B | 7 Billion | RTX 3090 / RTX 4090 | Single GPU | Medium |
| 14B | 14 Billion | RTX 4090 / A6000 / H100 | Single or dual GPU (tensor parallel) | High |
| 24B | 24 Billion | RTX 4090 / RTX 5090 / H100 / A100 | 2-4 GPUs | High |
| 70B | 70 Billion | H100 / A100 / 4090 or 5090 cluster | 8-16 GPUs | Highest |

- **Pilot Inventory Targets**: 8 x A4000, 4 x RTX 3090, and 2 x RTX 3090 rigs confirmed for initial deployment; additional A6000 or RTX 5090 units may be sourced subject to subsidy approval.

### Subsidy Mechanics

1. **Base Subsidy**: Every qualifying provider receives a fixed Swan Token stipend.
2. **Model-Oriented Multiplier**: Higher multipliers for providers that unlock 14B, 24B, or 70B model capacity.
3. **Cluster Bonus**: Extra allocation for >=8 GPU configurations or dense server cards that improve utilization.
4. **Commitment Requirement**: Subsidies vest monthly across the three-month lease term; early withdrawal forfeits the remaining subsidy.

### Payout Structure (Stage 1 Pilot)

- **Plan A - Balanced Mix (default)**: 30% stable-value assets (USDC/USDT/OP) to cover fixed costs, 50% Swan Token direct reward, 20% Swan Token collateral locked for three months.
- **Plan B - Higher Swan Incentive (optional)**: 20% stable-value assets, 60% Swan Token, 20% locked collateral.
- **Plan C - Stability-Heavy (for large clusters)**: >=50% stable-value assets, <=40% Swan Token, >=10% locked collateral.
- For all plans, OP token may be substituted one-for-one with stablecoins within the stable-value allocation. Providers can opt into Plan C if justified by cluster scale or operating costs; governance will evaluate requests above the default 30% stable portion to balance treasury risk with market stability feedback.

### Monitoring

- Providers must submit uptime metrics, hardware attestations, and workload readiness evidence each month.
- Swan Foundation and delegated node operators will audit compliance before releasing payouts.

## Rationale

Stage 1 focuses on securing compute supply ahead of anticipated customer onboarding for the A2A protocol rollout. Community feedback highlighted the need to include RTX 5090 support and to offer higher stablecoin coverage for large clusters to mitigate market volatility. The three-plan payout approach keeps Plan A as the recommended default while giving governance levers to accommodate providers with heavier operating costs. Requiring three-month commitments ensures that subsidies translate into steady availability rather than short-lived bursts of capacity, aligning with Swan's enterprise-facing service levels.

## Implementation

- **Timeline**: Prepare onboarding materials in September 2025; formally launch the pilot in October 2025 with a three to six month evaluation window.
- **Administration**: Swan Foundation treasury team disburses stablecoins and coordinates token and collateral flows via SwanFi.
- **Budgeting**: Monthly subsidy pool reviewed by governance; unused allocations roll over or revert to the treasury.
- **Reporting**: Publish monthly program updates and hardware utilization metrics in GitHub Discussions; propose adjustments or expansion in a follow-on SIP if KPIs are met.

## Governance Impact

- Allocates treasury stablecoins and Swan Tokens to sustain compute supply, temporarily increasing monthly outflows.
- Introduces collateralized Swan Token locks that can reduce circulating supply during the pilot.
- Requires a delegated reviewer group to verify hardware attestations, increasing operational overhead but strengthening accountability.
- Sets precedent for future subsidy structures; subsequent SIPs may refine or sunset the program based on performance data.

## References

- Swan Governance Discussion #11 - Subsidy Program for Computing Providers (CPs): https://github.com/swanchain/governance/discussions/11
- Community feedback from @ThomasBlock, @gugege521, @fangjun18484, and @LoveTh9-cyber within Discussion #11.
