# Category 1.3: Information Manipulation and Disinformation

*Alignment: Equivalent to GenAI Domain 1 — all GenAI D1 risks apply to MAS without modification.*

In multi-agent pipelines, output risks are amplified because one agent's hallucination or harmful output becomes input to downstream agents, compounding errors before any human review point is reached.

**Description:** GenAI dramatically lowers the cost and effort needed to generate convincing false or misleading content at scale, enabling sophisticated influence operations, deepfake media, and erosion of trust in legitimate information sources.

## Frameworks

NIST_GenAI, MITRE, CDAO, NSA_Deploy, DoD_RAI, OWASP_State, ATFAA, NIST_RMF

## Key Risks

1. 1.3.1 Misinformation at scale: GenAI enables unintentional production and dissemination of false or inaccurate content, especially through confabulations [E] — NIST_GenAI, CDAO
2. 1.3.2 Deliberate disinformation campaigns: GenAI enables production of false or misleading content targeted at specific demographics, supporting sophisticated influence operations [E] — NIST_GenAI, MITRE, CDAO, NSA_Deploy
3. 1.3.3 Deepfake generation: Multimodal GenAI enables realistic synthetic audiovisual content and photorealistic images that can be indistinguishable from authentic material [E] — NIST_GenAI, MITRE, NSA_Deploy
4. 1.3.4 Fraudulent impersonation: GenAI assists in creating compelling imagery and content that impersonates real individuals or institutions for fraud or manipulation [E] — NIST_GenAI, MITRE
5. 1.3.5 Erosion of public trust in valid information: Widespread AI-generated misinformation erodes trust in true evidence, with downstream economic and social effects [E] — NIST_GenAI, CDAO, OWASP_State
6. 1.3.6 AI-enhanced social engineering and phishing: AI-generated deepfakes, voice clones, and highly personalized messages manipulate users into disclosing credentials or authorizing harmful actions [E] — NSA_Deploy, MITRE, OWASP_State
7. 1.3.7 Election and political integrity risk: AI-generated disinformation threatens political structures and election integrity [E] — CDAO
