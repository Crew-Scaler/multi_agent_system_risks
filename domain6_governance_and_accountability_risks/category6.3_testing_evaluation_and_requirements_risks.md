# Category 6.3: Testing, Evaluation, and Requirements Risks

*Alignment: Equivalent to GenAI Domain 6 — all GenAI D6 risks apply to MAS with amplified severity due to autonomous operation.*

In MAS contexts, governance failures are more consequential because agents act autonomously across extended time horizons, making retroactive accountability harder to establish and human intervention windows shorter.

**Description:** AI systems deployed without adequate testing, verification, and validation have unknown failure modes, reliability gaps, and unintended behaviors that may not be discovered until significant harm has occurred in deployment.

## Frameworks

DoD_RAI, NIST_RMF, NIST_GenAI, NSA_Dev, GAO, CDAO

## Key Risks

- 6.3.1 Absence of comprehensive TEVV framework: Without test, evaluation, verification, and validation tailored for AI, system failure modes and unintended behaviors may not be identified before fielding [E] — DoD_RAI, NIST_RMF, CDAO
- 6.3.2 Non-testable AI requirements: Requirements that cannot be validated through testing make it impossible to confirm an AI system behaves as intended [E] — DoD_RAI, GAO
- 6.3.3 Insufficient AI-specific testing coverage: Standard software testing methods do not adequately address AI model vulnerabilities; AI requires adversarial testing and red-teaming [E] — NSA_Dev, NIST_RMF
- 6.3.4 Real-world vs. lab gap: Risks measured in controlled settings may differ significantly from risks that emerge in operational deployment [E] — NIST_RMF, DoD_RAI, GAO
- 6.3.5 Irresponsible model release without security evaluation: Releasing AI models without red-teaming or benchmarking means exploitable vulnerabilities reach production users [E] — NSA_Dev, CDAO
- 6.3.6 Insufficient robustness assessment: Performance testing that does not probe edge cases or adversarial conditions leaves brittleness undiscovered [E] — GAO, NIST_RMF
- 6.3.7 Evaluation and benchmarking inadequacy: Current evaluation methods may underestimate vulnerabilities and become outdated as new attacks or model capabilities emerge [E] — NIST_RMF
