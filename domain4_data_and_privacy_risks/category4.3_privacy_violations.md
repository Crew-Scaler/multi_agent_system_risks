# Category 4.3: Privacy Violations

*Alignment: Expands GenAI Domain 4 with four new categories covering large-context, log-based, streaming, and probabilistic recall leakage vectors unique to MAS architectures (RDL domain, 36 categories).*

**Description:** AI systems create multiple categories of privacy risk through their need for large training data volumes, their ability to memorize and leak personal information, and their capacity to infer sensitive attributes from seemingly innocuous inputs.

## Frameworks

NIST_GenAI, NIST_RMF, CDAO, GAO, DoD_RAI, ENISA, NSA_Data

## Key Risks

- 4.3.1 Training data privacy violations: Use of personal data for GenAI training raises risks to transparency, consent, and purpose specification [E] — NIST_GenAI, NIST_RMF, CDAO
- 4.3.2 Data memorization and leakage: Adversarial attacks can cause LLMs to reveal sensitive information memorized from training data [E] — NIST_GenAI, NIST_RMF
- 4.3.3 PII inference from disparate sources: GenAI models may correctly infer PII or sensitive data not present in training data by combining information from multiple sources [E] — NIST_GenAI, NIST_RMF
- 4.3.4 Identity inference from anonymized data: AI systems can infer the identity of individuals from data that appears anonymous [E] — NIST_RMF
- 4.3.5 Sensitive attribute inference: AI systems infer sensitive personal attributes (health, demographics, behavior) not explicitly provided in input data [E] — NIST_RMF, NIST_GenAI
- 4.3.6 Secondary harms from incorrect inferences: Incorrect PII inferences (including confabulations) result in dignitary harm, extortion risk, or discriminatory adverse decisions [E] — NIST_GenAI
- 4.3.7 Unauthorized aggregation of personal data: AI systems' data aggregation capabilities enable privacy violations at scale not possible with traditional software [E] — NIST_RMF
- 4.3.8 Data residency and compliance violations: AI systems transfer or process data in ways that violate data privacy regulations or geographic data residency requirements [E] — OWASP_MAS, ENISA, CDAO
