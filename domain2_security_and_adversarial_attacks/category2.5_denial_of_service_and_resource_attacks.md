# Category 2.5: Denial of Service and Resource Attacks

**Description:** Attackers disrupt AI system availability through resource exhaustion, flooding, or computationally expensive queries, causing service degradation or forcing systems into reduced-security operational modes.

## Frameworks

NIST_RMF, DoD_Cyber, ATFAA, OWASP_MAS, OWASP_T&M, Cisco_A2A, MITRE, NSA_Deploy

## Key Risks

- 2.5.1 Compute resource exhaustion (energy-latency attacks): Attackers send specially crafted queries to degrade or exhaust model compute resources [E] — NIST_RMF, MITRE, DoD_Cyber
- 2.5.2 Denial of service via task flooding: Attackers flood AI systems or agent servers with excessive requests to exhaust resources and cause service degradation [E] — Cisco_A2A, OWASP_MAS, OWASP_T&M, DoD_Cyber, MITRE
- 2.5.3 Cost harvesting: Adversaries exploit AI APIs to generate excessive compute or financial costs for the operating organization [E] — DoD_Cyber, MITRE, OWASP_Top10
- 2.5.4 Oversight saturation attacks: Attackers deliberately generate excessive audit events and low-significance alerts to create blind spots and exhaust monitoring resources [E] — ATFAA
- 2.5.5 SSE resource exhaustion: Attackers open excessive persistent streaming connections to exhaust server memory [E] — Cisco_A2A
- 2.5.6 Computational resource manipulation by agents: Specially crafted inputs trigger disproportionately resource-intensive agent processing to degrade performance or force reduced-security modes [E] — ATFAA
