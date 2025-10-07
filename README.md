# SWAN DAO Governance

The SWAN DAO governance system is the backbone of the SWAN ecosystem. It keeps decision-making decentralized, transparent, and community-driven while guiding long-term growth, financial stewardship, and the core principles of a DAO.

## Governance Overview

- **Committee Structure**  
  Representation spans SWAN token holders, core developers, builders, and other contributors so decisions reflect diverse stakeholder perspectives. Members are elected through transparent community votes.
- **Decision-Making Pipeline**  
  Any community member can submit proposals. Approved proposals move from community voting to implementation by responsible teams or sub-committees, with voting power often reflecting token holdings and tenure.
- **Community Engagement**  
  Forums, chats, town halls, workshops, and webinars keep contributors informed and involved so everyone can participate in an informed way.
- **Treasury Management**  
  The DAO allocates the treasury across staking rewards, community programs, and strategic investments. Regular audits and clear compensation frameworks balance incentives for builders and Universal Basic Income (UBI) for computing providers.
- **Continuous Evolution**  
  Governance processes adapt to community feedback, ensuring protocol upgrades and organizational refinements keep the SWAN network resilient and forward-looking.

## SIP (Swan Improvement Proposal)

An SIP is the official, permanent record for any change, upgrade, or policy within Swan Chain, Swan Cloud, SwanFi, or the Swan Metaverse Foundation. SIPs move through community review, DAO voting, and implementation so every change remains transparent, versioned, and verifiable on-chain and in this repository.

### When to Use an SIP

| Category                        | Example Proposals                                                          |
| ------------------------------- | -------------------------------------------------------------------------- |
| Protocol / Technical Change     | Adjust L2 parameters, modify ZK task logic, update Swan SDK specs          |
| Economic / Treasury             | Token allocation, UBI formula update, Foundation or Grant distributions    |
| Governance                      | Add new voting mechanisms, adjust quorum rules, create subDAO or committee |
| Ecosystem or Policy             | Partner onboarding, community reward programs, or SwanFi changes           |
| Documentation / Meta            | Updates to SIP format, new governance frameworks                           |

### SIP Structure and Naming

Place every proposal in the `SIPs/` directory using a sequential filename such as:

```
SIPs/SIP-001-adjust-fcp-ubi-curve.md
```

Copy `SIPs/SIP-template.md` when drafting a new proposal. The template includes the canonical front matter:

```markdown
---
SIP: 001
Title: Adjust Fog Computing Provider (FCP) UBI Curve
Author: Charles Cao (@charlescao-swan)
Status: Draft
Category: Economic / Protocol
Created: 2025-10-07
Discussions-To: https://github.com/swanchain/governance/discussions/13
Requires: None
Replaces: None
---
```

### Body Sections

1. **Summary** – One-paragraph description of the proposed change.  
2. **Motivation** – Why the change is needed and the problem it solves.  
3. **Specification** – Detailed technical, economic, or governance modifications, including parameters, formulas, token distributions, or contract addresses.  
4. **Rationale** – Design decisions and alternatives evaluated.  
5. **Implementation** – Rollout plan, timeline, and responsible teams (e.g., Swan Cloud Inc., Swan Foundation).  
6. **Governance Impact** – Effects on token holders, computing providers, and DAO operations.  
7. **References** – Links to discussions, whitepapers, dashboards, and related proposals.

### SIP Lifecycle

| Stage                     | Description                                                |
| ------------------------- | ---------------------------------------------------------- |
| Draft                     | Initial version under discussion in GitHub Discussions     |
| Review                    | Reviewed by Swan Foundation / core contributors            |
| Snapshot Vote             | On-chain or off-chain voting period for community approval |
| Accepted                  | Approved and queued for execution                          |
| Implemented               | Deployed or enforced via DAO operations                    |
| Rejected / Superseded     | Not approved or replaced by a later SIP                    |

### Example Workflow

1. Post an idea in [GitHub Discussions](https://github.com/swanchain/governance/discussions) and iterate with feedback.
2. Convert the idea into an SIP using the template and save it as `SIPs/SIP-###-descriptive-title.md`.
3. Submit a pull request titled “Formal SIP Submission” for community review.
4. Engage with feedback from maintainers (`@swanchain-governance`) and contributors until consensus is reached.
5. Move the SIP to Snapshot or on-chain voting.  
6. Once approved, maintainers merge the SIP and coordinate implementation.

## Repository Layout

- `README.md` — Governance overview and SIP process guide (this document).
- `SIPs/SIP-template.md` — Canonical template for drafting new SIPs.

Questions, improvements, or new proposals should start in Discussions before becoming formal SIPs.
