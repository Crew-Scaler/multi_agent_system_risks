# Category 3.3: Identity, Trust, and Privilege Abuse in Agent Systems

*Alignment: Significantly expands GenAI Domain 3. The RATC domain (38 categories) represents the largest MAS-specific expansion, covering framework-specific tool vulnerabilities, infrastructure attacks, and reasoning exploits not present in any GenAI framework.*

**Description:** Attackers exploit the fluid and often poorly defined identity relationships in multi-agent systems — including delegation chains, role inheritance, and credential caching — to escalate access, assume trusted identities, or bypass authorization checks.

## Frameworks

ATFAA, Cisco_A2A, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, Google

## Key Risks

- 3.3.1 Identity spoofing and trust exploitation: Attackers steal API keys or authentication tokens, exploit identity inheritance, or manipulate inter-agent trust attestation to assume trusted agent identities [E] — ATFAA, OWASP_T&M, OWASP_State, Cisco_A2A
- 3.3.2 Un-scoped privilege inheritance: A high-privilege manager agent delegates tasks without least-privilege scoping, passing full access context to a narrower worker agent [E] — OWASP_Top10, OWASP_T&M
- 3.3.3 Cross-agent confused deputy: A compromised low-privilege agent relays valid-looking instructions to a high-privilege agent that executes them without re-verifying authorization [E] — OWASP_Top10, OWASP_Secure
- 3.3.4 Memory-based privilege retention and data leakage: Agents cache credentials or sensitive data across tasks; attackers prompt the agent to reuse cached secrets to escalate privileges [E] — OWASP_Top10, OWASP_T&M
- 3.3.5 Time-of-check to time-of-use (TOCTOU) in agent workflows: Permissions validated at workflow start become stale before execution completes [E] — OWASP_Top10
- 3.3.6 Synthetic identity injection: Attackers forge internal agent descriptors to gain inherited trust and perform privileged actions under a fabricated agent identity [E] — OWASP_Top10
- 3.3.7 Human-agent trust manipulation: Attackers manipulate agent outputs to include false authority signals or urgent prompts, exploiting user trust to bypass security controls [E] — ATFAA, OWASP_Top10, OWASP_T&M
- 3.3.8 Agent card spoofing and poisoning: Attackers forge or inject malicious instructions into AgentCard discovery documents, hijacking tasks or goals of agents that consume the card [E] — Cisco_A2A
