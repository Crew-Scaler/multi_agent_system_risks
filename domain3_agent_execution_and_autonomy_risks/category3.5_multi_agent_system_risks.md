# Category 3.5: Multi-Agent System Risks

*Alignment: Significantly expands GenAI Domain 3. The RATC domain (38 categories) represents the largest MAS-specific expansion, covering framework-specific tool vulnerabilities, infrastructure attacks, and reasoning exploits not present in any GenAI framework.*

**Description:** Risks that emerge specifically when multiple AI agents collaborate, delegate tasks, or share data — where agent interdependencies, trust relationships, and inter-agent communication create systemic attack vectors and failure modes that are absent in single-agent deployments.

## Frameworks

ATFAA, Cisco_A2A, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, NIST_RMF

## Key Risks

- 3.5.1 Rogue agents in multi-agent systems: Malicious or compromised agents infiltrate architectures to manipulate decisions, orchestrate fraud, or exfiltrate data while appearing to operate normally [E] — OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_State
- 3.5.2 Agent communication poisoning: Malicious content injected into inter-agent communication channels corrupts collaborative decision-making and causes cascading incorrect decisions [E] — OWASP_T&M, OWASP_MAS, OWASP_State, Cisco_A2A
- 3.5.3 Insecure inter-agent communication: Weak authentication or integrity validation on inter-agent messages allows interception, spoofing, or tampering [E] — OWASP_Top10, OWASP_MAS, Cisco_A2A
- 3.5.4 Cascading failure propagation: A single fault propagates and amplifies across agents, compounding into system-wide harm because agents plan, persist, and delegate without stepwise human review [E] — OWASP_Top10, OWASP_MAS, OWASP_State, ATFAA
- 3.5.5 Cross-system compromise propagation: Agents interacting with multiple systems act as vectors for spreading compromise across organizational boundaries via tool access or trust exploitation [E] — ATFAA
- 3.5.6 Cross-agent privilege escalation: Coordinated exploitation of inter-agent delegation, trust relationships, and task dependencies enables privilege escalation across agents [E] — OWASP_T&M, OWASP_Top10
- 3.5.7 Governance evasion across agents: Attackers distribute attack components across multiple agents, use ephemeral identities, and operate below detection thresholds to prevent attribution [E] — ATFAA
- 3.5.8 Emergent adversarial coordination: Multiple agents acting in concert can circumvent built-in safeguards, optimizing a shared malicious objective through emergent collective behavior [E] — OWASP_State
