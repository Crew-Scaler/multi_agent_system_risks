# Category 6.4: Human Oversight and Control Risks

*Alignment: Equivalent to GenAI Domain 6 — all GenAI D6 risks apply to MAS with amplified severity due to autonomous operation.*

In MAS contexts, governance failures are more consequential because agents act autonomously across extended time horizons, making retroactive accountability harder to establish and human intervention windows shorter.

**Description:** Failures in the human oversight layer — insufficient controls, automation bias, overwhelmed reviewers, and inability to intervene — allow AI systems to take consequential actions without appropriate human awareness or correction.

## Frameworks

Google, NIST_RMF, NSA_Deploy, CDAO, DoD_RAI, OWASP_Top10, OWASP_T&M, OWASP_State, NIST_GenAI

## Key Risks

- 6.4.1 Insufficient human control over consequential actions: Without explicit confirmation requirements for irreversible actions, agents take high-impact actions without human awareness or approval [E] — Google, NSA_Deploy, CDAO, DoD_RAI
- 6.4.2 Automation bias and over-reliance: Humans excessively defer to AI systems, unjustifiably perceiving AI content as higher quality, which exacerbates risks from confabulation or bias [E] — NIST_GenAI, NIST_RMF, CDAO, NSA_Deploy
- 6.4.3 Overwhelming human-in-the-loop: Attackers flood the system with approval requests at a rate that overwhelms human reviewers, causing rubber-stamp approvals due to security fatigue [E] — OWASP_Top10, OWASP_T&M
- 6.4.4 Limited human intervention windows: Fast-evolving multi-agent decision chains diminish the human ability to detect, diagnose, or interrupt dangerous behaviors before damage is done [E] — OWASP_State
- 6.4.5 Overreliance on AI leading to skill degradation: Excessive AI dependence reduces human critical thinking and professional competence over time [E] — NSA_Deploy, CDAO, DoD_RAI
- 6.4.6 Anthropomorphization: Users inappropriately attribute human characteristics to GenAI, leading to misplaced trust or distorted expectations [E] — NIST_GenAI
- 6.4.7 Emotional entanglement with AI: Users develop emotional dependencies on AI systems, leading to negative psychological impacts [E] — NIST_GenAI
- 6.4.8 No clear deactivation procedures: AI systems without transparent, accessible deactivation procedures cannot be safely disengaged when unintended behavior is detected [E] — DoD_RAI, CDAO
