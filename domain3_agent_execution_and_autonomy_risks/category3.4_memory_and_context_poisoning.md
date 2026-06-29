# Category 3.4: Memory and Context Poisoning

*Alignment: Significantly expands GenAI Domain 3. The RATC domain (38 categories) represents the largest MAS-specific expansion, covering framework-specific tool vulnerabilities, infrastructure attacks, and reasoning exploits not present in any GenAI framework.*

**Description:** Adversaries corrupt or seed an agent's stored context — conversation history, memory tools, embeddings, and RAG stores — with malicious or misleading data, causing future reasoning, planning, and tool invocations to become biased or unsafe across sessions.

## Frameworks

ATFAA, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, Google, NIST_RMF

## Key Risks

- 3.4.1 Persistent prompt injection via memory: Malicious data processed and stored in memory influences agent behavior in future unrelated interactions [E] — Google, OWASP_T&M, ATFAA
- 3.4.2 Conversation history injection: An attacker seeds false entries into an agent's conversation history, causing decisions based on fabricated prior context [E] — OWASP_Top10
- 3.4.3 RAG store poisoning: Malicious documents or data injected into the vector store cause the agent to retrieve and act on poisoned knowledge during future tasks [E] — OWASP_Top10, OWASP_MAS, NIST_RMF, ATFAA
- 3.4.4 Embedding corruption: An attacker corrupts stored embeddings to cause the agent to associate queries with incorrect or malicious knowledge chunks [E] — OWASP_Top10
- 3.4.5 Peer-agent context poisoning: A compromised peer agent injects malicious data into shared context or memory stores accessed by other agents [E] — OWASP_Top10, OWASP_MAS
- 3.4.6 Cross-user memory contamination: Without strict memory isolation, one user's stored data or a malicious injection affects another user's agent sessions [E] — Google, OWASP_Secure
- 3.4.7 Delayed exploitability: Memory and goal-targeting attacks may not manifest until days or weeks after initial compromise, making root-cause tracing difficult [E] — ATFAA
