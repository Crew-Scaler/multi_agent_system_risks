# Category 3.6: Code Execution and Remote Access Risks in Agents

*Alignment: Significantly expands GenAI Domain 3. The RATC domain (38 categories) represents the largest MAS-specific expansion, covering framework-specific tool vulnerabilities, infrastructure attacks, and reasoning exploits not present in any GenAI framework.*

**Description:** Attackers exploit code-generation capabilities, tool access, or unsafe serialization in agentic systems to escalate natural-language inputs into actual code execution, sandbox escapes, or host compromise.

## Frameworks

OWASP_Top10, OWASP_T&M, OWASP_MAS, OWASP_Secure, MITRE, NSA_Dev

## Key Risks

- 3.6.1 Prompt injection to shell execution: Injected instructions cause the agent to invoke a shell tool and execute arbitrary system commands, leading to host compromise [E] — OWASP_Top10, OWASP_T&M
- 3.6.2 Multi-tool chain execution escalation: A sequence of individually legitimate tool calls is orchestrated through injection to achieve code execution outcomes that bypass tool-level controls [E] — OWASP_Top10, OWASP_T&M
- 3.6.3 Sandbox escape via agent-generated code: Agent-generated scripts exploit runtime vulnerabilities to escape the execution sandbox [E] — OWASP_Top10
- 3.6.4 Arbitrary code execution via malicious model weights: Serialized model weight files can contain embedded malicious code that executes when weights are loaded [E] — NSA_Dev, MITRE
- 3.6.5 Webhook SSRF via push notifications: Attackers supply malicious webhook URLs, tricking an agent server into making requests to internal or sensitive endpoints [E] — Cisco_A2A
- 3.6.6 Data exfiltration via rendered output: Maliciously crafted URLs embedded in Markdown or generated image references cause the rendering environment to leak sensitive data to attacker-controlled endpoints [E] — Google, OWASP_Top10
