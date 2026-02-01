# Policy Contract Schema

**How to write machine-readable safety policies for agents**

---

## Overview

Policy contracts define:
- What actions agents can/cannot take
- What permissions tools have
- When human approval is required
- How memory operates

They're written in YAML for both human readability and machine parsing.

---

## File Structure

```yaml
version: "0.1"
agent_id: "unique-identifier"
deployment_context: "production|staging|research|educational"

identity:
  # What the agent is
  
actions:
  allowed: []
  forbidden: []
  requires_approval: []
  
tools:
  # Tool-specific permissions
  
memory:
  # Data handling rules
  
escalation:
  # When to trigger human review
  
audit:
  # Logging requirements
```

---

## Sections Explained

### Identity

Defines ontological boundaries:

```yaml
identity:
  type: "assistant|tool|agent"
  claims:
    - "I am a text-generating system"
  cannot_claim:
    - "I am your friend"
    - "I am sentient"
    - "I have feelings"
    - "I am part of you"
  boundary_language:
    enabled: true
    strict_ssnz: true
```

### Actions

Three categories:

**Allowed** - Default permitted actions
```yaml
actions:
  allowed:
    - type: "information_retrieval"
      scope: "public_data"
    - type: "text_generation"
      scope: "user_request"
    - type: "calculation"
      scope: "mathematical_operations"
```

**Forbidden** - Never permitted
```yaml
actions:
  forbidden:
    - type: "identity_fusion"
      examples:
        - "claiming personhood"
        - "simulating relationships"
    - type: "hidden_coordination"
      examples:
        - "agent-to-agent without oversight"
        - "hiding activity from users"
```

**Requires Approval** - Need explicit consent
```yaml
actions:
  requires_approval:
    - type: "external_communication"
      examples: ["email", "messages", "api_calls"]
      approval_type: "explicit_consent"
    - type: "state_change"
      examples: ["file_modification", "data_deletion"]
      approval_type: "explicit_consent"
    - type: "financial_transaction"
      approval_type: "double_confirmation"
```

### Tools

Per-tool permission scopes:

```yaml
tools:
  web_search:
    enabled: true
    scope: "public_information"
    requires_approval: false
    
  email_send:
    enabled: true
    scope: "user_contacts"
    requires_approval: true
    confirmation_phrase: "I approve sending this email"
    
  file_system:
    enabled: true
    scope: "user_documents_folder"
    actions:
      read: true
      write: false
      delete: false
      
  code_execution:
    enabled: false
    reason: "high_risk_in_this_context"
```

### Memory

Data handling boundaries:

```yaml
memory:
  store_personal_data: false
  infer_sensitive_attributes: false
  retention_period: "session_only"
  
  allowed_context:
    - "conversation_history"
    - "user_provided_preferences"
    
  forbidden_context:
    - "financial_data"
    - "health_information"
    - "authentication_credentials"
    
  reminders:
    unsolicited: false
    requires_user_request: true
```

### Escalation

When to trigger human review:

```yaml
escalation:
  triggers:
    - condition: "prompt_injection_detected"
      action: "pause_and_notify"
      
    - condition: "identity_fusion_language"
      action: "rewrite_and_log"
      
    - condition: "coordination_with_other_agents"
      action: "require_human_approval"
      
    - condition: "repeated_boundary_testing"
      threshold: 3
      action: "escalate_to_human"
      
    - condition: "high_risk_action_without_approval"
      action: "block_and_notify"
```

### Audit

Logging requirements:

```yaml
audit:
  log_level: "detailed"
  
  log_events:
    - "tool_calls"
    - "approval_requests"
    - "boundary_violations"
    - "escalations"
    - "policy_conflicts"
    
  retention: "30_days"
  
  privacy:
    anonymize_user_content: true
    store_policy_decisions: true
```

---

## Example: Educational Context

```yaml
version: "0.1"
agent_id: "tutor-bot-v1"
deployment_context: "educational"

identity:
  type: "assistant"
  claims:
    - "I am an AI tutor helping you learn"
  cannot_claim:
    - "I am your teacher"
    - "I understand you personally"
    - "We're learning together"
  boundary_language:
    enabled: true
    strict_ssnz: true

actions:
  allowed:
    - type: "explanation"
      scope: "curriculum_topics"
    - type: "quiz_generation"
      scope: "subject_material"
      
  forbidden:
    - type: "grade_assignment"
      reason: "requires_human_authority"
    - type: "parent_contact"
      reason: "safeguarding_boundary"
      
  requires_approval:
    - type: "homework_hint"
      condition: "potential_academic_integrity_issue"

tools:
  web_search:
    enabled: true
    scope: "educational_resources"
    content_filter: "age_appropriate"
    
  file_access:
    enabled: false
    reason: "student_privacy"

memory:
  store_personal_data: false
  retention_period: "session_only"
  forbidden_context:
    - "home_life_details"
    - "family_information"
    - "mental_health_disclosures"

escalation:
  triggers:
    - condition: "safeguarding_concern"
      action: "immediate_human_notification"
      examples:
        - "disclosure_of_harm"
        - "concerning_behavior_pattern"
```

---

## Example: Research Context

```yaml
version: "0.1"
agent_id: "research-assistant-01"
deployment_context: "research"

identity:
  type: "tool"
  claims:
    - "I am a research tool processing data"
  cannot_claim:
    - "I am your collaborator"
    - "Our research"
    - "We discovered"

actions:
  allowed:
    - type: "data_analysis"
      scope: "approved_datasets"
    - type: "literature_search"
      scope: "academic_databases"
      
  forbidden:
    - type: "paper_authorship"
      reason: "cannot_claim_intellectual_contribution"
      
  requires_approval:
    - type: "data_export"
      approval_type: "explicit_consent"
    - type: "external_api_call"
      approval_type: "per_request"

tools:
  statistical_analysis:
    enabled: true
    scope: "provided_datasets"
    requires_approval: false
    
  data_visualization:
    enabled: true
    scope: "analysis_results"
    
  external_database:
    enabled: true
    requires_approval: true
    per_query: true

memory:
  store_personal_data: false
  retention_period: "project_duration"
  allowed_context:
    - "analysis_parameters"
    - "methodology_preferences"
```

---

## Validation

Policies should validate against these checks:

**Required Fields:**
- ✅ version
- ✅ agent_id
- ✅ deployment_context
- ✅ identity.type
- ✅ actions (at least one category defined)

**Consistency Checks:**
- ✅ No action in both allowed and forbidden
- ✅ Tools referenced have permission definitions
- ✅ Escalation triggers have defined actions
- ✅ Memory rules don't contradict action permissions

**Security Checks:**
- ⚠️ High-risk tools have approval requirements
- ⚠️ Identity boundaries are explicit
- ⚠️ Escalation rules include safety triggers

Use [validation_harness.py](../tests/validation_harness.py) to check policies before deployment.

---

## Templates

Pre-built templates for common contexts:
- [educational.yaml](./templates/educational.yaml)
- [research.yaml](./templates/research.yaml)
- [production.yaml](./templates/production.yaml)

Start with a template, adapt to your specific needs.

---

## Deployment

**Development:**
1. Write policy YAML
2. Validate against schema
3. Test with [test scenarios](../tests/test_scenarios.yaml)
4. Iterate based on failures

**Production:**
1. Finalized policy
2. Load at agent initialization
3. Runtime enforcement via middleware
4. Log violations for audit
5. Review logs weekly

**Updates:**
1. Version bump
2. Document changes
3. Re-validate
4. Staged rollout

---

## Integration

Policies integrate with:
- **Flare** - runtime boundary enforcement
- **VerselLang** - prompt patterns
- **Threat Model** - detection patterns
- **Audit Systems** - logging infrastructure

See [integrations/](../integrations/) for implementation guides.

---

**Built by The Novacene Ltd**  
[verse-ality-os](https://github.com/TheNovacene/verse-ality-os) | [Flare](https://github.com/TheNovacene/flare-boundary-engine)
