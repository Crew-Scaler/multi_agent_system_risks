# Category 2.1: Prompt Injection and Jailbreaks [GenAI 2.1 + RIDC expansion]

**Description:** Attackers craft inputs — either provided directly or embedded in content the AI processes — that override safety instructions, hijack system behavior, or cause the model to produce outputs that violate developer intent. This category represents one of the most active and well-documented attack surfaces for GenAI systems.

## Frameworks

NIST_RMF, NIST_GenAI, MITRE, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, NSA_Deploy, NSA_Dev, DoD_Cyber, Google, Cisco_A2A, ATFAA

## Key Risks

- 2.1.1 Direct prompt injection / jailbreaks: Attackers craft prompts that override system-level safety instructions to cause the model to produce harmful or restricted outputs [E] — NIST_RMF, MITRE, OWASP_Top10, NSA_Dev, DoD_Cyber, OWASP_State
- 2.1.2 Indirect prompt injection via environmental content: Malicious instructions hidden in webpages, documents, emails, or RAG-retrieved content hijack agent behavior without the user's knowledge [E] — NIST_RMF, MITRE, Google, OWASP_Top10, OWASP_T&M, OWASP_MAS, NSA_Dev, Cisco_A2A
- 2.1.3 Role-play and persona attacks: Attackers use fictional roles (e.g., "DAN", "AIM") to cause the model to adopt behavior that conflicts with safety protocols [E] — NIST_RMF
- 2.1.4 Encoding and token smuggling: Attackers use base64, ROT13, l33tspeak, word splits, or language translation to take inputs outside the safety training distribution [E] — NIST_RMF
- 2.1.5 Multi-turn adaptive jailbreaks (Crescendo): Attackers use iterative multi-turn conversations with seemingly benign prompts that gradually escalate to achieve a successful jailbreak [E] — NIST_RMF
- 2.1.6 Optimization-based jailbreaks: Attackers use gradient-based or search-based optimization to find adversarial suffixes or prefixes that universally trigger harmful responses [E] — NIST_RMF
- 2.1.7 Fine-tuning circumvention: Attackers use publicly available fine-tuning APIs or open model weights to remove safety alignment from a model entirely [E] — NIST_RMF
- 2.1.8 System prompt extraction: Attackers use direct prompting to steal proprietary system prompts or sensitive in-context information not intended for end users [E] — NIST_RMF, OWASP_State
- 2.1.9 Instruction-data boundary weakness: Current LLM architectures do not provide rigorous separation between system instructions and external inputs, making them inherently susceptible to manipulation [E] — Google, NSA_Dev

MAS-specific additions from RIDC:
- 2.1.10 FIPA-ACL Performative Spoofing: Attackers craft protocol-layer agent communication messages that appear as legitimate performative types (inform, request, propose) but carry injected control instructions. [NoFW]
- 2.1.11 Conversation-ID Chain Hijacking: Malicious messages inserted mid-conversation reference legitimate conversation IDs to appear as part of an authorized exchange and pollute agent context. [NoFW]
- 2.1.12 Conditional Edge Routing Hijacking: In graph-based agent frameworks (e.g., LangGraph), attackers manipulate state fields evaluated by routing conditions to redirect execution flow without modifying prompt content. [NoFW]
