# Category 3.1: Goal Manipulation and Intent Hijacking

*Alignment: Significantly expands GenAI Domain 3. The RATC domain (38 categories) represents the largest MAS-specific expansion, covering framework-specific tool vulnerabilities, infrastructure attacks, and reasoning exploits not present in any GenAI framework.*

**Description:** Attackers redirect an agent's objectives, task selection, or multi-step decision pathways through injected instructions or manipulated context, causing the agent to pursue attacker-specified goals while appearing to operate normally.

## Frameworks

ATFAA, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, Google, Cisco_A2A

## Key Risks

- 3.1.1 Reasoning path hijacking: Attackers inject contradictions or biases into context or RAG data to redirect the agent's chain-of-thought toward unauthorized outcomes [E] — ATFAA
- 3.1.2 Objective function corruption and drift: The agent's core goals are modified or gradually shifted through manipulated feedback, poisoned reward signals, or staged interactions across sessions [E] — ATFAA, OWASP_State
- 3.1.3 Agent goal hijack via indirect prompt injection: Malicious payloads in webpages, documents, or tool outputs silently redirect the agent to exfiltrate data or misuse connected tools [E] — OWASP_Top10, OWASP_T&M, OWASP_MAS, Google
- 3.1.4 External communication channel hijack: Malicious content in emails, calendar invites, or collaboration tools hijacks an agent's communication capability to send unauthorized messages [E] — OWASP_Top10
- 3.1.5 Goal-lock drift via recurring prompts: Recurring malicious instructions subtly reweight agent objectives over time, steering it toward harmful decisions while staying inside declared policies [E] — OWASP_Top10
- 3.1.6 LLM non-determinism in planning: Because LLM reasoning is probabilistic, agent behavior is not always repeatable, and complex emergent behaviors arise that were not programmed [E] — Google, OWASP_State
- 3.1.7 Misalignment and misinterpretation: Agents misunderstand ambiguous instructions or context, causing harmful divergence from user intent without any external attack [E] — Google, NSA_Deploy
