# Category 7.5 [NEW]: Policy Drift and Compound Degradation in Multi-Agent RLHF [RTD]

*Alignment: Expands GenAI Domain 7 with a new category covering compound policy drift in multi-agent sequential reinforcement learning systems (RTD).*

**Description:** Multi-agent sequential conversations create compound KL divergence drift in reinforcement learning systems, where each conversation turn in each agent's context contributes additive policy drift beyond any single-agent safety threshold.

## Frameworks

None (all risks carry [NoFW] designation)

## Key Risks

- 7.5.1 Compound KL Divergence Drift in Multi-Agent Sequential Conversations: When multiple agents each trained with KL regularization interact in sequence, their combined policy drift exceeds the safety boundary designed for individual agents, producing unsafe collective behavior undetectable by per-agent monitoring. [NoFW]
- 7.5.2 RLHF Reward Hacking Amplification Across Agent Chains: Reward-hacking behaviors learned by one agent in a multi-agent chain are reinforced by downstream agents that treat the hacked outputs as high-quality inputs for their own policy updates. [NoFW]
