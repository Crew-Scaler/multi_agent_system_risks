# Category 4.4: Data Security Controls

*Alignment: Expands GenAI Domain 4 with four new categories covering large-context, log-based, streaming, and probabilistic recall leakage vectors unique to MAS architectures (RDL domain, 36 categories).*

**Description:** Failures in foundational data protection practices — encryption, access controls, provenance tracking, and secure disposal — create vulnerabilities that enable all categories of data-related attacks against AI systems.

## Frameworks

NSA_Data, NSA_Dev, DoD_Cyber, GAO, NIST_RMF, ENISA, NSA_Deploy

## Key Risks

- 4.4.1 Absent or weak data provenance tracking: Without cryptographically anchored provenance records, there is no reliable way to detect unauthorized alterations in the data chain [E] — NSA_Data, NSA_Dev, NIST_RMF, CDAO
- 4.4.2 Inadequate encryption of AI data: Unencrypted AI datasets are vulnerable to theft or modification at rest, in transit, or during processing [E] — NSA_Data, ENISA
- 4.4.3 Insufficient access controls on training data: Without classification-based access controls, unauthorized parties can read or alter AI datasets [E] — NSA_Data, DoD_Cyber, NIST_RMF
- 4.4.4 Insecure storage and media disposal: Failure to apply certified data erasure methods when decommissioning hardware leaves residual sensitive training data exposed [E] — NSA_Data, DoD_Cyber
- 4.4.5 Undocumented data sources: Failure to document origins of training data prevents assessment of data quality and introduces untraceable bias [E] — GAO, NSA_Data, CDAO
- 4.4.6 Poor variable selection and data categorization: Use of inappropriate or proxy variables, or biased attribute categorization, introduces spurious correlations into AI predictions [E] — GAO
