# Category 6.5: Regulatory and Acquisition Governance Risks

*Alignment: Equivalent to GenAI Domain 6 — all GenAI D6 risks apply to MAS with amplified severity due to autonomous operation.*

In MAS contexts, governance failures are more consequential because agents act autonomously across extended time horizons, making retroactive accountability harder to establish and human intervention windows shorter.

**Description:** Failures in the procurement, contracting, and regulatory compliance processes that govern how AI systems are acquired and authorized create gaps between what was approved and what is actually deployed or operated.

## Frameworks

DoD_RAI, DoD_Cyber, GAO, NIST_RMF, CDAO, ENISA

## Key Risks

- 6.5.1 AI risk considerations absent from acquisition processes: Failure to assess AI-related risks early in acquisition means mitigation opportunities are missed [E] — DoD_RAI, CDAO
- 6.5.2 No standard RAI contract language: Acquisition contracts without RAI-specific requirements enable vendors to deliver AI systems that do not meet responsible AI standards [E] — DoD_RAI
- 6.5.3 Inadequate intellectual property protection in acquisition: Failure to preserve government or organizational IP rights limits the ability to maintain, update, or audit acquired AI systems [E] — DoD_RAI
- 6.5.4 Operating without valid authorization: AI systems that bypass authorization steps lack an adequate body of evidence, exposing the organization to unmanaged risk [E] — DoD_Cyber
- 6.5.5 Regulatory model inadequacy for self-modifying AI: Most compliance frameworks assume AI remains fixed after deployment; agentic AI violates this assumption, creating liability gaps [E] — OWASP_State
- 6.5.6 Vulnerability disclosure and remediation gaps: Organizations without AI-specific vulnerability disclosure policies leave identified model vulnerabilities unaddressed and users uninformed [E] — NSA_Dev, CDAO
