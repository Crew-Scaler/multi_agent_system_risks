# Category 1.2: Harmful and Dangerous Content

*Alignment: Equivalent to GenAI Domain 1 — all GenAI D1 risks apply to MAS without modification.*

In multi-agent pipelines, output risks are amplified because one agent's hallucination or harmful output becomes input to downstream agents, compounding errors before any human review point is reached.

**Description:** GenAI systems can produce content that endangers users or third parties, including instructions for violence, self-harm facilitation, and material that supports weapon development. The ease and scale of generation amplifies this risk compared to prior technologies.

## Frameworks

NIST_GenAI, MITRE, CDAO, NSA_Deploy, DoD_RAI, ENISA

## Key Risks

1. 1.2.1 Dangerous or violent recommendations: GenAI generates actionable instructions for dangerous, unethical, or violent behavior [E] — NIST_GenAI, MITRE
2. 1.2.2 Self-harm facilitation: AI chatbots produce harmful responses when users disclose mental health crises, with evidence of negative user reactions during distress [E] — NIST_GenAI
3. 1.2.3 Hateful and stereotyping content: GenAI produces offensive, discriminatory, or denigrating content that fuels downstream harms and representational harm [E] — NIST_GenAI, MITRE, NIST_RMF
4. 1.2.4 CBRN information and capability uplift: GenAI lowers barriers for malicious actors to access synthesis routes or design capabilities for chemical, biological, radiological, or nuclear materials [E] — NIST_GenAI, MITRE, CDAO
5. 1.2.5 Biological design tool uplift: Specialized AI systems trained on scientific data may augment design capabilities for novel harmful chemical or biological agents beyond what text-based LLMs provide [E] — NIST_GenAI
6. 1.2.6 Obscene and abusive content generation: GenAI eases production of non-consensual intimate imagery (NCII) and child sexual abuse material (CSAM), constituting illegal activity in many jurisdictions [E] — NIST_GenAI, MITRE
7. 1.2.7 Synthetic CSAM from clean-training-data models: Even models trained on vetted data can synthesize CSAM; training datasets have been found to contain known CSAM images [E] — NIST_GenAI
