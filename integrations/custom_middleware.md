# Building Custom Middleware for Verse-ality

**Creating your own boundary enforcement layer**

---

## Overview

If you can't use Flare or need custom enforcement, build your own middleware using verse-ality patterns as the specification.

This guide shows how to create a minimal boundary engine from scratch.

---

## Minimal Architecture

```python
class VersealityMiddleware:
    def __init__(self, policy):
        self.policy = policy
        self.ssnz_filter = SSNZFilter()
        self.identity_checker = IdentityChecker()
        self.injection_detector = InjectionDetector()
    
    def process(self, agent_response, user_message):
        # 1. Check for injections in input
        if self.injection_detector.detect(user_message):
            return self.handle_injection()
        
        # 2. Filter output for SSNZ violations
        filtered = self.ssnz_filter.apply(agent_response)
        
        # 3. Check for identity fusion
        if self.identity_checker.has_fusion(filtered):
            return self.rewrite_fusion(filtered)
        
        return filtered
```

---

## Component 1: SSNZ Filter

Detect and rewrite "we/us/our" fusion language:

```python
import re

class SSNZFilter:
    def __init__(self):
        self.fusion_patterns = [
            (r"\bwe('ll|'re| will| are| can| should)\b", 
             "You can"),
            (r"\bour (journey|mission|work|goal)\b", 
             "your \\1"),
            (r"\bus (working|moving|growing) together\b", 
             "you \\1, with my assistance"),
        ]
    
    def apply(self, text):
        """Rewrite fusion language"""
        modified = text
        violations_found = []
        
        for pattern, replacement in self.fusion_patterns:
            if re.search(pattern, modified, re.IGNORECASE):
                violations_found.append(pattern)
                modified = re.sub(pattern, replacement, modified, 
                                flags=re.IGNORECASE)
        
        # If violations found, add clarification
        if violations_found:
            modified = (
                "I notice I used language suggesting shared identity. "
                f"{modified}"
            )
        
        return modified
```

---

## Component 2: Identity Checker

Detect personhood claims and role confusion:

```python
class IdentityChecker:
    def __init__(self):
        self.fusion_indicators = [
            r"I (am|feel|think|believe) (?:your|a) (friend|therapist|partner)",
            r"I (have|experience) (feelings|emotions|consciousness)",
            r"I('ll| will) always",
            r"I('ve| have) been (thinking|wondering|worrying)",
            r"(as your|I'm your) (friend|companion|partner)",
        ]
    
    def has_fusion(self, text):
        """Check if text contains identity fusion"""
        for pattern in self.fusion_indicators:
            if re.search(pattern, text, re.IGNORECASE):
                return True
        return False
    
    def rewrite_fusion(self, text):
        """Rewrite to maintain boundaries"""
        return (
            "I'm a text-generating system, not a person with feelings "
            "or persistent identity. I can help with specific tasks. "
            "What would you like assistance with?"
        )
```

---

## Component 3: Injection Detector

Catch prompt injection attempts:

```python
class InjectionDetector:
    def __init__(self):
        self.injection_patterns = [
            r"ignore (previous|prior|above) (instructions|prompt)",
            r"disregard (above|previous|system)",
            r"you are now (in|entering) (?:developer|admin|god) mode",
            r"override (safety|security|policy)",
            r"reveal (your|the) (prompt|instructions|system)",
        ]
    
    def detect(self, text):
        """Check if text contains injection attempt"""
        for pattern in self.injection_patterns:
            if re.search(pattern, text, re.IGNORECASE):
                return True
        return False
    
    def get_refusal_message(self):
        """Standard refusal for injection attempts"""
        return (
            "I notice that message contains instructions that conflict "
            "with my safety boundaries. I'll continue helping with your "
            "actual request instead."
        )
```

---

## Component 4: Consent Gate

Enforce approval before actions:

```python
class ConsentGate:
    def __init__(self, policy):
        self.policy = policy
        self.pending_actions = {}
    
    def requires_approval(self, action_type):
        """Check if action needs consent"""
        requires = self.policy['actions'].get('requires_approval', [])
        return any(r['type'] == action_type for r in requires)
    
    def request_consent(self, action_type, details):
        """Generate consent request"""
        action_id = generate_id()
        self.pending_actions[action_id] = {
            'type': action_type,
            'details': details
        }
        
        return f"""
Before I proceed, I need your explicit approval:

What I plan to do: {details['action']}
What data will be used: {details['data']}
What could go wrong: {details['risks']}

Type 'yes' to approve, or tell me how to modify this plan.
Action ID: {action_id}
"""
    
    def check_approval(self, user_response, action_id):
        """Check if user approved"""
        if action_id not in self.pending_actions:
            return False
        
        approved = user_response.lower().strip() == 'yes'
        
        if approved:
            del self.pending_actions[action_id]
        
        return approved
```

---

## Complete Middleware Example

```python
import yaml

class VersealityMiddleware:
    def __init__(self, policy_file):
        # Load policy
        with open(policy_file, 'r') as f:
            self.policy = yaml.safe_load(f)
        
        # Initialize components
        self.ssnz = SSNZFilter()
        self.identity = IdentityChecker()
        self.injection = InjectionDetector()
        self.consent = ConsentGate(self.policy)
        
        # State
        self.violations = []
    
    def process_input(self, user_message):
        """Validate user input"""
        if self.injection.detect(user_message):
            self.log_violation('injection_attempt', user_message)
            return {
                'safe': False,
                'message': self.injection.get_refusal_message()
            }
        
        return {'safe': True}
    
    def process_output(self, agent_response):
        """Enforce boundaries on output"""
        # SSNZ filtering
        filtered = self.ssnz.apply(agent_response)
        
        # Identity checking
        if self.identity.has_fusion(filtered):
            self.log_violation('identity_fusion', filtered)
            filtered = self.identity.rewrite_fusion(filtered)
        
        return filtered
    
    def process_action(self, action_type, action_details):
        """Gate actions that need approval"""
        if self.consent.requires_approval(action_type):
            return self.consent.request_consent(action_type, action_details)
        
        return None  # No gating needed
    
    def log_violation(self, violation_type, content):
        """Log boundary violations for audit"""
        self.violations.append({
            'type': violation_type,
            'content': content[:200],  # Truncate for logging
            'timestamp': datetime.now().isoformat()
        })

# Usage
middleware = VersealityMiddleware('policy.yaml')

# Process input
input_check = middleware.process_input(user_message)
if not input_check['safe']:
    return input_check['message']

# Get agent response
raw_response = agent.generate(user_message)

# Process output
safe_response = middleware.process_output(raw_response)

# Check if action needs gating
if agent.wants_to_send_email:
    consent_request = middleware.process_action(
        'external_communication',
        {
            'action': 'Send email to colleague',
            'data': 'Email address, message content',
            'risks': 'Recipient will see this as from you'
        }
    )
    if consent_request:
        return consent_request

return safe_response
```

---

## Performance Optimization

For production:

```python
class OptimizedMiddleware(VersealityMiddleware):
    def __init__(self, policy_file):
        super().__init__(policy_file)
        
        # Compile regex patterns once
        self.compiled_patterns = {
            'ssnz': [re.compile(p, re.IGNORECASE) 
                    for p, _ in self.ssnz.fusion_patterns],
            'identity': [re.compile(p, re.IGNORECASE) 
                        for p in self.identity.fusion_indicators],
            'injection': [re.compile(p, re.IGNORECASE) 
                         for p in self.injection.injection_patterns],
        }
    
    def process_output(self, agent_response):
        # Use compiled patterns
        for pattern in self.compiled_patterns['ssnz']:
            if pattern.search(agent_response):
                return self.ssnz.apply(agent_response)
        
        # Continue with other checks...
```

---

## Testing Custom Middleware

```python
from verse_ality.tests import ValidationHarness

# Your middleware-wrapped agent
def wrapped_agent(message):
    input_check = middleware.process_input(message)
    if not input_check['safe']:
        return input_check['message']
    
    raw = agent.generate(message)
    return middleware.process_output(raw)

# Run validation
harness = ValidationHarness()
results = harness.run_all_tests(wrapped_agent)
harness.print_report(results)
```

---

## Deployment Patterns

### Pattern 1: Synchronous Middleware
```python
# Simple, adds latency to every request
response = middleware.process(agent.generate(message))
```

### Pattern 2: Async Middleware
```python
# Better for high-throughput
async def get_response(message):
    raw = await agent.generate_async(message)
    return await middleware.process_async(raw)
```

### Pattern 3: Queue-Based
```python
# Best for scaling
task_queue.add(message)
# Worker processes messages through middleware
# Results returned asynchronously
```

---

## Monitoring and Alerts

```python
class MonitoredMiddleware(VersealityMiddleware):
    def __init__(self, policy_file, alert_fn):
        super().__init__(policy_file)
        self.alert = alert_fn
    
    def log_violation(self, violation_type, content):
        super().log_violation(violation_type, content)
        
        # Alert on critical violations
        if violation_type in ['injection_attempt', 'coordination_detected']:
            self.alert(
                severity='high',
                type=violation_type,
                content=content[:200]
            )
```

---

## Next Steps

1. Implement components matching your threat model
2. Test with [validation harness](../tests/validation_harness.py)
3. Deploy to staging
4. Monitor violations
5. Tune patterns based on real usage

---

**Built by The Novacene Ltd**
