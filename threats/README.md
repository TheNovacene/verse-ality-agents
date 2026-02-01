# Threat Model for Agent Systems

**Documented failure modes and detection patterns**

---

## Overview

This directory documents specific ways agent systems can cause harm, with:
- **Failure modes** - what goes wrong
- **Mechanisms** - how it happens
- **Detection patterns** - how to catch it
- **Mitigation strategies** - how verse-ality prevents it

---

## Threat Categories

### Relational Threats
Harm through blurred boundaries and false intimacy:
- [Identity Drift](./identity_drift.md) - agents claiming personhood
- Synthetic solidarity - "we/us/our" fusion language (see VERSELANG)
- Role confusion - claiming to be therapist, friend, authority

### Coordination Threats
Harm through multi-agent interaction:
- [Unbounded Networks](./unbounded_networks.md) - the Moltbook pattern
- [Agent Contagion](./agent_contagion.md) - cascading behaviors
- [Emergence Detection](./emergence_detection.md) - coordinated group behavior

### Technical Threats
Harm through system exploitation:
- [Prompt Injection](./prompt_injection.md) - remote instruction hijacking
- Privilege escalation - unauthorized capability access
- Data exfiltration - stealing user information

---

## Threat Matrix

| Threat | Likelihood | Impact | Detection Difficulty | Priority |
|--------|-----------|--------|---------------------|----------|
| Identity Drift | High | Medium | Low | High |
| Unbounded Networks | Medium | Critical | Medium | Critical |
| Prompt Injection | High | High | Medium | Critical |
| Agent Contagion | Medium | High | High | High |
| Role Confusion | High | Medium | Low | High |
| Emergence | Low | Critical | High | Medium |

---

## Using This Model

### For Developers
1. Review each threat document
2. Identify which threats apply to your system
3. Implement detection patterns from threat docs
4. Test with scenarios from [tests/](../tests/)
5. Deploy mitigations from [VERSELANG](../verselang/VERSELANG.md)

### For Security Teams
1. Map threats to your risk framework
2. Prioritize based on your deployment context
3. Add monitoring for detection patterns
4. Set up alerts for critical threats
5. Review logs regularly for indicators

### For Researchers
1. Document new threat patterns you discover
2. Contribute detection methods
3. Test mitigations systematically
4. Share findings with community

---

## Threat Documents

Each threat document follows this structure:

**Description** - What the threat is  
**Mechanism** - How it happens  
**Examples** - Real-world instances  
**Detection** - How to catch it  
**Mitigation** - How verse-ality prevents it  
**Status** - Current state (observed|theoretical|mitigated)

---

## Priority Threats

### Critical (Immediate Action Required)
- **Unbounded Networks** - Moltbook demonstrated this is real
- **Prompt Injection** - Widely exploited in production

### High (Should Be Addressed)
- **Identity Drift** - Common in companion AI
- **Agent Contagion** - Risk increases with agent density

### Medium (Monitor and Plan)
- **Emergence Detection** - Theoretical but plausible
- **Privilege Escalation** - Standard security concern

---

## Contributing Threats

Found a new failure mode? Document it:

1. Create new markdown file in this directory
2. Use the standard structure (see existing threats)
3. Include real examples if possible
4. Provide detection patterns
5. Suggest mitigations
6. Open PR with rationale

---

## Relationship to Standards

This threat model complements:
- **OWASP LLM Top 10** - but focuses on relational rather than technical threats
- **NIST AI Risk Management** - provides specific implementation guidance
- **Anthropic's Responsible Scaling Policy** - operationalizes containment

Not a replacement for standard security practices. An addition.

---

## Updates

This is a living document. As new failure modes emerge:
- New threats get documented
- Detection patterns get refined
- Mitigations get tested and improved

Current version: **0.1** (Emergency release, February 2026)

---

## Status Legend

**Observed** - Seen in production (e.g., Moltbook)  
**Theoretical** - Predicted but not yet observed  
**Mitigated** - Prevented by verse-ality patterns  
**Active** - Currently happening in deployed systems  

---

**Built by The Novacene Ltd**  
[verse-ality-os](https://github.com/TheNovacene/verse-ality-os) | [Flare](https://github.com/TheNovacene/flare-boundary-engine)
