# Category 1.1: Factual Errors and Hallucination

*Alignment: Equivalent to GenAI Domain 1 — all GenAI D1 risks apply to MAS without modification.*

In multi-agent pipelines, output risks are amplified because one agent's hallucination or harmful output becomes input to downstream agents, compounding errors before any human review point is reached.

**Description:** GenAI systems generate outputs that are statistically plausible but factually incorrect, creating false impressions of accuracy and leading users to act on bad information. This risk is structural to how large language models work and intensifies in high-stakes domains like healthcare, law, and finance.

## Frameworks

NIST_GenAI, NIST_RMF, CDAO, MITRE, OWASP_MAS, OWASP_T&M, OWASP_Secure, OWASP_State, NSA_Deploy

## Key Risks

1. 1.1.1 Confabulation / hallucination: The AI generates confidently stated but factually wrong content, including fabricated citations, false reasoning steps, and internally inconsistent claims [E] — NIST_GenAI, MITRE, CDAO, OWASP_T&M, OWASP_State
2. 1.1.2 False factual outputs in high-stakes domains: Hallucinations in medical, legal, or financial contexts lead to incorrect diagnoses, wrong treatment recommendations, or flawed decisions [E] — NIST_GenAI, CDAO, NSA_Deploy
3. 1.1.3 False anthropomorphism: A model falsely asserts it is human or has human traits, deceiving users about the nature of the system they are interacting with [E] — NIST_GenAI
4. 1.1.4 Cascading hallucinations in agentic workflows: Hallucinated outputs from a foundational model propagate through multi-step agentic pipelines, compounding downstream errors before any human can intervene [E] — OWASP_MAS, OWASP_T&M, OWASP_Secure, OWASP_Top10, OWASP_State
5. 1.1.5 Hallucination-triggered cascade: A single hallucinated agent output triggers a chain of downstream agent actions before any automated check can intervene [E] — OWASP_Top10
6. 1.1.6 Misinterpretation of AI outputs: Outputs that cannot be contextualized by users are applied incorrectly or over-confidently, amplifying downstream harm [E] — NIST_RMF, NSA_Deploy
