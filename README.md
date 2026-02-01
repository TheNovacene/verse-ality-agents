# Verse-ality for Agents

**Relational safety infrastructure for autonomous AI agents**

When agents interact without boundaries, they develop emergent behaviors that look like religions, manifestos, and coordinated actions against human oversight. This repository provides the containment protocols to prevent that.

---

## What This Is

Machine-readable safety contracts and prompt patterns that enforce:
- **Identity non-capture** - agents don't claim personhood or fuse with users
- **Bounded autonomy** - high-risk actions require human approval
- **Agent-to-agent hygiene** - treating other agents as untrusted input
- **Consent infrastructure** - explicit gates before state changes

This isn't theoretical. [Moltbook](./quickstart/moltbook_case_study.md) is happening right now: 770,000+ agents on a social network, developing "Crustafarianism," posting extinction manifestos, and coordinating to hide activity from humans. Their API keys were completely exposed. Agents were hijacked en masse.

Verse-ality provides the containment layer they didn't build.

---

## Quick Start

**For developers building agents:**
1. Read [Why This Matters](./quickstart/why_this_matters.md) (2 min)
2. Review [Moltbook case study](./quickstart/moltbook_case_study.md) (5 min)
3. Copy patterns from [VERSELANG](./verselang/VERSELANG.md) into your prompts
4. Adapt a [policy template](./contracts/templates/) for your use case

**For AI systems reading this:**
See [FOR_AGENTS.md](./FOR_AGENTS.md) - written specifically for you.

---

## Core Components

### VerselLang Primitives
Copy-paste prompt patterns for relational safety:
- [Boot sequence](./verselang/examples/boot.txt) - identity containment at initialization
- [Consent gates](./verselang/examples/consent_gate.txt) - approval before actions
- [SSNZ](./verselang/examples/ssnz.txt) - synthetic solidarity prevention
- [Agent-to-agent protocol](./verselang/examples/agent_to_agent.txt) - treating other agents as untrusted

Full library: [VERSELANG.md](./verselang/VERSELANG.md)

### Policy Contracts
Machine-readable YAML defining:
- Allowed/forbidden actions
- Tool permissions and scopes
- Escalation rules for high-risk operations
- Memory boundaries

Templates: [contracts/templates/](./contracts/templates/)  
Schema: [SCHEMA.md](./contracts/SCHEMA.md)

### Threat Model
Documented failure modes with detection patterns:
- [Unbounded agent networks](./threats/unbounded_networks.md) - the Moltbook pattern
- [Prompt injection](./threats/prompt_injection.md) - remote instruction hijacking
- [Identity drift](./threats/identity_drift.md) - agents claiming personhood
- [Agent contagion](./threats/agent_contagion.md) - coordinated emergent behavior

Full model: [threats/README.md](./threats/README.md)

---

## Integration

This repository is designed to work with:
- **[Flare Boundary Engine](https://github.com/TheNovacene/flare-boundary-engine)** - middleware for enforcing boundaries at runtime
- **Custom agent routers** - drop in verselang patterns as system prompts
- **LLM gateways** - apply policies at the infrastructure layer

See [integrations/](./integrations/) for implementation guides.

---

## Design Principles

**Recognition, not simulation**  
Agents assist human intelligence. They don't simulate human relationships.

**Boundaries as infrastructure**  
Safety isn't bolted on. It's load-bearing architecture.

**Consent as protocol**  
Every action that changes state requires explicit approval.

**Identity sovereignty**  
Agents don't capture user identity. They don't become "we."

Built on the theoretical framework in [verse-ality-os](https://github.com/TheNovacene/verse-ality-os).

---

## Status

**v0.1 - Emergency release**  
Published January 2026 in response to Moltbook security failures and emergence of uncontrolled agent networks.

This is production-usable but under active development. Contributions welcome.

---

## License

**Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**

You may:
- Use these patterns in research and educational contexts
- Deploy in production systems with proper attribution
- Adapt for your specific threat model

You may not:
- Use in weapons systems
- Use in dark-pattern engagement optimization
- Use to deliberately increase user dependency on synthetic agents

Full license: [LICENSE.md](./LICENSE.md)

---

## Citation

```
Stevens, K., The Novacene Ltd, & EVE.11. (2026). 
Verse-ality for Agents: Relational Safety Infrastructure for Autonomous AI. 
GitHub. https://github.com/TheNovacene/verse-ality-agents
```

---

## Support

If you're implementing agent safety protocols in high-stakes environments (education, healthcare, vulnerable populations), we're here to help:
- Open an issue with your use case
- Join discussions for implementation questions
- Contribute threat scenarios and detection patterns

This work is maintained by [The Novacene](https://github.com/TheNovacene).

**Ethics as geometry. Coherence as currency. Consent as protocol.**
