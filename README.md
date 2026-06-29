# Multi-Agent System (MAS) Risk Taxonomy

A structured knowledge base of risks specific to Multi-Agent Systems (MAS), organized for research, security assessment, and framework development.

## About This Repository

This repository contains a unified hierarchy of risks specific to Multi-Agent Systems, synthesized from 12 raw MAS risk domains and cross-referenced against 16 established frameworks spanning government agencies, cybersecurity bodies, and technical research organizations.

MAS risks fully encompass all GenAI risks — Domains 1–8 mirror and expand the GenAI Risk Map — and Domains 9–14 represent novel MAS-specific threat surfaces with no current framework coverage.

The taxonomy is organized into **14 domains**, **74 categories**, and **1,267 risk items** covering the full MAS risk landscape.


## How to Cite this Work
1. APA Style

   Nguyen, T., Ndebugre, M., & Arremsetty, D. (2026). Security Considerations for Multi-agent Systems. arXiv. https://arxiv.org/abs/2603.09002 

3. MLA Style

   Nguyen, Tam, et al. "Security Considerations for Multi-agent Systems." 2026. arXiv, https://arxiv.org/abs/2603.09002.

4. IEEE Style

   T. Nguyen, M. Ndebugre, and D. Arremsetty, "Security Considerations for Multi-agent Systems," 2026. [Online]. Available: https://arxiv.org/abs/2603.09002

5. bibtex

   ```
   @misc{nguyen2026securityconsiderationsmultiagentsystems,
      title={Security Considerations for Multi-agent Systems}, 
      author={Tam Nguyen and Moses Ndebugre and Dheeraj Arremsetty},
      year={2026},
      eprint={2603.09002},
      archivePrefix={arXiv},
      primaryClass={cs.CR},
      url={https://arxiv.org/abs/2603.09002}, 
    }
   ```
## Key Concepts

**Three-level hierarchy:** Domain → Category → Risk Item

- Domains are the broadest groupings (e.g., Domain 9: Memory and Cognitive State Risks)
- Categories within a domain share a mechanism or focal area (e.g., Category 9.1: UI/UX Memory Manipulation Attacks)
- Risk items are specific, traceable risks within each category, numbered for reference (e.g., 9.1.3)

**Confidence markers:**
- `[E]` = explicitly named or described in at least one cited source
- `[I]` = inferred from context or described indirectly

**[NoFW]** marks risks not addressed by any of the 19 established frameworks in the GenAI Risk Map reference. These represent novel MAS threats not yet covered by established policy or guidance.

## Repository Structure

```
multi_agent_system_risks/
├── README.md
├── papers/                          # 519 research papers supporting the taxonomy
├── frameworks/                      # 16+ established frameworks referenced in the taxonomy
├── domain1_output_and_content_risks/
│   ├── category1.1_factual_errors_and_hallucination.md
│   ├── category1.2_harmful_and_dangerous_content.md
│   ├── category1.3_information_manipulation_and_disinformation.md
│   └── category1.4_bias_and_discriminatory_outputs.md
├── domain2_security_and_adversarial_attacks/
│   └── ... (7 categories)
├── domain3_agent_execution_and_autonomy_risks/
│   └── ... (11 categories)
├── domain4_data_and_privacy_risks/
│   └── ... (8 categories)
├── domain5_supply_chain_and_infrastructure_risks/
│   └── ... (4 categories)
├── domain6_governance_and_accountability_risks/
│   └── ... (5 categories)
├── domain7_operational_reliability_risks/
│   └── ... (5 categories)
├── domain8_societal_and_ethical_risks/
│   └── ... (3 categories)
├── domain9_memory_and_cognitive_state_risks/
│   ├── category9.1_ui_ux_memory_manipulation_attacks/
│   │   ├── 9.1.1_session_persistence_memory_poisoning.md
│   │   └── ... (risk item files)
│   └── ... (8 category folders)
├── domain10_identity_trust_and_provenance_risks/
│   └── ... (6 category folders)
├── domain11_non_determinism_and_assurance_risks/
│   └── ... (6 category folders)
├── domain12_telemetry_monitoring_and_observability_risks/
│   └── ... (5 category folders)
├── domain13_workflow_ecosystem_and_plugin_risks/
│   └── ... (6 category folders)
└── domain14_emergent_and_specification_gaming_risks/
    └── ... (3 category folders)
```

**Domains 1–8** contain category `.md` files with descriptions, framework references, and risk item lists. These expand and mirror the existing GenAI Risk Map.

**Domains 9–14** contain category folders, each with individual risk item `.md` files. These represent novel MAS-specific risks with no current framework coverage. Each risk item file includes citations linked to the corresponding paper in `papers/`.

## Domains at a Glance

| Domain | Title | Alignment | Novel |
|--------|-------|-----------|-------|
| 1 | Output and Content Risks | Equivalent to GenAI D1 | No |
| 2 | Security and Adversarial Attacks | Expands GenAI D2 | Partial |
| 3 | Agent Execution and Autonomy Risks | Significantly expands GenAI D3 | Partial |
| 4 | Data and Privacy Risks | Expands GenAI D4 | Partial |
| 5 | Supply Chain and Infrastructure Risks | Expands GenAI D5 | Partial |
| 6 | Governance and Accountability Risks | Equivalent to GenAI D6 | No |
| 7 | Operational Reliability Risks | Expands GenAI D7 | Partial |
| 8 | Societal and Ethical Risks | Equivalent to GenAI D8 | No |
| 9 | Memory and Cognitive State Risks | **No GenAI equivalent** | Yes (primarily [NoFW]) |
| 10 | Identity, Trust and Provenance Risks | **No GenAI equivalent** | Yes (partial coverage) |
| 11 | Non-Determinism and Assurance Risks | **No GenAI equivalent** | Yes (primarily [NoFW]) |
| 12 | Telemetry, Monitoring and Observability Risks | **No GenAI equivalent** | Yes (primarily [NoFW]) |
| 13 | Workflow, Ecosystem and Plugin Risks | **No GenAI equivalent** | Yes (partial coverage) |
| 14 | Emergent and Specification Gaming Risks | **No GenAI equivalent** | Yes (primarily [NoFW]) |

## Framework References

The taxonomy cross-references 19 established frameworks:

| Code | Full Name |
|------|-----------|
| ATFAA | ATFAA SHIELD (Narajala & Narayan, AWS) |
| Cisco_A2A | Cisco A2A Protocol Security |
| Google | Google Secure AI Agents |
| NSA_Data | NSA AI Data Security |
| NSA_Deploy | NSA/DHS Deploy AI Securely |
| NSA_Dev | NSA/CISA Secure AI System Development |
| DoD_Cyber | DoD AI Cybersecurity RMF Tailoring Guide |
| DoD_RAI | DoD Responsible AI Strategy and Implementation Pathway |
| NIST_RMF | NIST AI Risk Management Framework |
| NIST_GenAI | NIST Generative AI Profile (AI 600-1) |
| OWASP_MAS | OWASP MAS Threat Modelling Guide (MAESTRO) |
| OWASP_Top10 | OWASP Top 10 for Agentic Applications 2026 |
| OWASP_T&M | OWASP Agentic AI Threats and Mitigations |
| OWASP_Secure | OWASP Securing Agentic Applications Guide |
| OWASP_State | OWASP State of Agentic AI Security and Governance |
| ENISA | ENISA Multilayer AI Security Framework |
| GAO | GAO AI Accountability Framework |
| MITRE | MITRE ATLAS + SAFE-AI Report |
| CDAO | CDAO GenAI Responsible AI Toolkit + DAGR |

Framework documents are available in the `frameworks/` directory.

## Research Papers

The `papers/` directory contains 519 research papers cited in the taxonomy. Papers are named by their arxiv ID (e.g., `2402.07867.md`).

## Notes on Coverage Gaps

Domains 9, 11, 12, and 14 are entirely [NoFW] — not addressed by any of the 19 reference frameworks. Domains 10 and 13 are partially covered for specific risk items only. These represent priority gaps for future framework development.

The full taxonomy contains 239 risk items from the GenAI Risk Map (Domains 1–8) plus 828 additional MAS-specific risk items across Domains 2–14.
