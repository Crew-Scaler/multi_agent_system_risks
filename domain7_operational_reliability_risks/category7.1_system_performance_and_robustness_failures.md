# Category 7.1: System Performance and Robustness Failures

*Alignment: Expands GenAI Domain 7 with a new category covering compound policy drift in multi-agent sequential reinforcement learning systems (RTD).*

**Description:** AI systems may fail to perform reliably when confronted with inputs, conditions, or operational contexts outside their training distribution, causing harmful outputs or dangerous failures in high-stakes deployments.

## Frameworks

NIST_RMF, NSA_Deploy, GAO, CDAO, ENISA, DoD_RAI

## Key Risks

- 7.1.1 Inaccuracy and poor generalization: AI systems produce statistically or factually wrong results when confronted with real-world data outside training conditions [E] — NIST_RMF, GAO, ENISA, CDAO
- 7.1.2 Brittleness and lack of robustness: AI systems fail or produce harmful outputs when confronted with inputs or conditions outside the original problem context [E] — NSA_Deploy, NIST_RMF, ENISA, CDAO
- 7.1.3 Cascading or emergent system failure: Large-scale complex AI systems have failure modes that are difficult to predict, including emergent properties of large pre-trained models [E] — NIST_RMF, ENISA, CDAO
- 7.1.4 Failure to detect and correct errors: Systems that cannot detect or self-correct their own errors require human intervention mechanisms that may not be in place [E] — NIST_RMF, GAO
- 7.1.5 Absence of shutdown or override capability: Systems that cannot be stopped or have human intervention introduced in real time pose heightened safety risks [E] — NIST_RMF, DoD_RAI
- 7.1.6 Unsafe operation outside anticipated conditions: AI systems cause harm when operating in settings not anticipated during design, especially without fail-safe mechanisms [E] — NIST_RMF, NSA_Deploy, ENISA
- 7.1.7 AI suitability failure: Choosing GenAI when a simpler technique would be more effective results in unnecessary complexity, cost, and risk [E] — CDAO
- 7.1.8 Plan-and-Execute Planning Paralysis as Goal Drift: Planning agents that cannot find plans satisfying all constraints enter indefinite planning loops, effectively drifting from active goal pursuit to passive planning without producing outcomes. [NoFW]
  - Pantea Karimi, Kimia Noorbakhsh, Mohammad Alizadeh, and Hari Balakrishnan. 2026. Improving Coherence and Persistence in Agentic AI for System Optimization. In *ACM Conference on AI and Agentic Systems (CAIS 2026)*. https://doi.org/10.48550/arXiv.2603.21321
    - no strong evidence found
