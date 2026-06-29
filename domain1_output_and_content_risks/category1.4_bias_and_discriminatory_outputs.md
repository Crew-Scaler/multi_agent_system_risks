# Category 1.4: Bias and Discriminatory Outputs

*Alignment: Equivalent to GenAI Domain 1 — all GenAI D1 risks apply to MAS without modification.*

In multi-agent pipelines, output risks are amplified because one agent's hallucination or harmful output becomes input to downstream agents, compounding errors before any human review point is reached.

**Description:** GenAI systems inherit and often amplify biases present in training data, producing outputs that disadvantage specific demographic groups and that can drive inequitable decisions at scale when used in consequential contexts.

## Frameworks

NIST_GenAI, NIST_RMF, DoD_RAI, CDAO, GAO, ENISA, NSA_Data, NSA_Deploy, MITRE

## Key Risks

1. 1.4.1 Representational bias in outputs: Text-to-image and other GenAI models underrepresent women, racial minorities, and people with disabilities in professional contexts, producing biased or stereotyped outputs [E] — NIST_GenAI, NIST_RMF, ENISA
2. 1.4.2 Performance disparities across demographic groups: GenAI models perform less well for non-English languages or certain dialects, contributing to discriminatory decision-making and unequal access [E] — NIST_GenAI, NIST_RMF
3. 1.4.3 Statistical bias from sampling artifacts: Training data that is not representative of real-world distributions causes systematic inaccuracies in AI outputs [E] — NSA_Data, NIST_RMF, GAO, ENISA
4. 1.4.4 Undesired output homogenization and model collapse: GenAI systems can produce overly uniform outputs; overreliance on synthetic training data causes model collapse, where data points disappear from the model's distribution [E] — NIST_GenAI, NIST_RMF
5. 1.4.5 Algorithmic monoculture risk: Repeated use of the same model in consequential decision-making increases susceptibility to correlated failures across multiple actors and systems [E] — NIST_GenAI
6. 1.4.6 Intersectional and context-specific inequity: Harms from bias may affect different sub-groups differently, and standard population-level metrics miss targeted harms [E] — NIST_RMF, DoD_RAI, CDAO
7. 1.4.7 Exacerbation of digital divide: AI systems may widen existing disparities in access to services or opportunities based on digital access or literacy [E] — NIST_RMF
