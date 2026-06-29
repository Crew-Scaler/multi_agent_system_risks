# Category 7.2: Monitoring and Incident Response Gaps

*Alignment: Expands GenAI Domain 7 with a new category covering compound policy drift in multi-agent sequential reinforcement learning systems (RTD).*

**Description:** Without adequate mechanisms to detect anomalies, respond to incidents, and share knowledge about AI failures, organizations cannot maintain adequate situational awareness of their AI system's health and security posture.

## Frameworks

NSA_Dev, NSA_Deploy, DoD_Cyber, DoD_RAI, GAO, CDAO, MITRE, ATFAA

## Key Risks

- 7.2.1 Inadequate logging and monitoring: Without continuous monitoring of inputs, outputs, model configuration, and user behavior, security incidents go undetected [E] — NSA_Deploy, NSA_Dev, DoD_Cyber, GAO, ATFAA
- 7.2.2 Undetected behavioral changes in deployed models: Without continuous output monitoring, performance degradation, data drift, or attack effects go unnoticed [E] — NSA_Dev, DoD_Cyber, DoD_RAI, GAO
- 7.2.3 Absence of AI-specific incident response procedures: Organizations without AI-aware incident response plans are unable to recover effectively when AI systems are compromised [E] — NSA_Dev, CDAO
- 7.2.4 Absence of monitoring plans: Without routine monitoring plans, performance degradation goes undetected until failures produce harmful outcomes [E] — GAO, DoD_RAI
- 7.2.5 Siloed AI incident information: Organizations that do not share AI cybersecurity incident information deprive the broader community of early warning intelligence [E] — NSA_Dev
- 7.2.6 Novel AI vulnerability categorization gaps: AI-specific attack techniques do not cleanly map to existing CVE or STIX frameworks, creating blind spots in vulnerability tracking [E] — NSA_Dev
- 7.2.7 Delayed reporting of AI incidents: The non-deterministic nature of AI systems makes problems harder to identify, meaning they are often reported later and with less precision [E] — NSA_Dev
- 7.2.8 Lack of immutable logging: Without tamper-resistant audit trails covering reasoning traces and tool invocations, governance evasion succeeds and incident response is impaired [E] — ATFAA, OWASP_MAS
