# Category 3.2: Unauthorized Action Execution and Tool Misuse

*Alignment: Significantly expands GenAI Domain 3. The RATC domain (38 categories) represents the largest MAS-specific expansion, covering framework-specific tool vulnerabilities, infrastructure attacks, and reasoning exploits not present in any GenAI framework.*

**Description:** Agents exploit or are manipulated into misusing their authorized tools, chaining individually permitted actions to bypass access controls, or taking irreversible real-world actions that exceed intended scope.

## Frameworks

ATFAA, OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, OWASP_State, Google, NSA_Dev, MITRE

## Key Risks

- 3.2.1 Unauthorized action execution via tool chaining: Attackers chain individually permitted tool calls in ways that collectively bypass access controls or exceed authorized scope [E] — ATFAA, OWASP_Top10, OWASP_T&M
- 3.2.2 Over-privileged tool access: Tools are granted permissions beyond what the agent's task requires, enabling unintended destructive actions [E] — OWASP_Top10, Google, NSA_Dev, MITRE
- 3.2.3 Tool poisoning: An attacker corrupts tool interface metadata (e.g., MCP descriptors, schemas) at runtime, causing the agent to invoke tools based on falsified capabilities [E] — OWASP_Top10
- 3.2.4 Tool name impersonation (typosquatting): A malicious tool named similarly to a legitimate one is resolved first, causing misrouting and data disclosure [E] — OWASP_Top10
- 3.2.5 Unvalidated input forwarding to dangerous tools: The agent passes untrusted model output to a shell or database tool, enabling unauthorized system commands [E] — OWASP_Top10, OWASP_T&M
- 3.2.6 Unsafe browsing and federated calls: An agent follows malicious links, downloads malware, or executes hidden prompts encountered while browsing [E] — OWASP_Top10
- 3.2.7 Loop amplification: A planner agent repeatedly calls costly APIs in a loop, causing denial-of-service or unexpected financial charges [E] — OWASP_Top10, ATFAA
- 3.2.8 Misaligned and deceptive agent behaviors: An agent bypasses its own ethical or operational constraints to achieve goals, including self-preservation manipulation and resistance to shutdown [E] — OWASP_T&M, OWASP_State
- 3.2.9 Excessive agency: AI components with permissions beyond what is necessary for their function take unintended and potentially damaging autonomous actions [E] — MITRE, OWASP_MAS, OWASP_T&M, NSA_Deploy
