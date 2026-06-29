# Category 6.2: Transparency and Explainability Failures

*Alignment: Equivalent to GenAI Domain 6 — all GenAI D6 risks apply to MAS with amplified severity due to autonomous operation.*

In MAS contexts, governance failures are more consequential because agents act autonomously across extended time horizons, making retroactive accountability harder to establish and human intervention windows shorter.

**Description:** Failure to make AI systems understandable to operators, users, oversight bodies, and affected parties undermines the ability to detect problems, assign accountability, and provide redress to those harmed.

## Frameworks

NIST_RMF, NIST_GenAI, GAO, ENISA, CDAO, DoD_RAI, NSA_Deploy

## Key Risks

- 6.2.1 Opacity and lack of transparency: Users and affected parties cannot determine how an AI system works or why it produced a specific output, impeding redress [E] — NIST_RMF, GAO, ENISA, CDAO
- 6.2.2 Inexplicable model behavior: Systems that cannot describe how they reached a conclusion impede debugging, auditing, and governance [E] — NIST_RMF, ENISA, NSA_Deploy
- 6.2.3 Overconfidence in AI decisions: Humans perceive AI systems as more objective than they are, creating risk of over-reliance on incorrect or biased outputs [E] — NIST_RMF, NIST_GenAI
- 6.2.4 Inadequate disclosure to end users: Users interacting with AI systems without knowing it, or without understanding system limitations, are exposed to unacknowledged risks [E] — NIST_RMF, NIST_GenAI, GAO
- 6.2.5 Opaque agent operations: If agent actions, tool invocations, and reasoning steps are not logged and auditable, security auditing, incident response, and debugging become impossible [E] — Google, OWASP_T&M, ATFAA
- 6.2.6 Missing data cards and model cards: Without standardized documentation of datasets and models, developers and users lack visibility into how AI capabilities were built and their limitations [E] — DoD_RAI, CDAO
- 6.2.7 Repudiation and untraceability: AI agents operating without sufficient logging make it impossible to audit decisions, attribute accountability, or detect malicious activity [E] — OWASP_T&M, OWASP_State, ATFAA
