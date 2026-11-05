# AegisMesh

### Zero-Trust Control Plane for Autonomous AI Agents

> **Investigate. Challenge. Control.**

🏆 **Winner — Tether “Best Sovereign App” Track, Hack the North 2026**

AegisMesh is a security and governance platform for autonomous AI agents.

The core design principle is simple:

> **AI agents can reason autonomously without automatically receiving authority to modify production systems.**

Instead of giving an AI agent unrestricted credentials, AegisMesh separates:

```text
INTELLIGENCE
What should we do?

        from

AUTHORITY
Are we allowed to do it?
```

Agents investigate incidents, challenge competing hypotheses and propose remediation. A separate zero-trust control plane determines whether the proposed action is authorized before execution.

---

# The Problem

Autonomous agents increasingly have access to:

- cloud infrastructure
- APIs
- databases
- financial systems
- operational tools
- physical devices

That creates a dangerous failure mode.

An agent may:

- hallucinate;
- misinterpret evidence;
- receive poisoned instructions;
- operate with stale state;
- become compromised;
- exceed its intended permissions.

If that agent also holds production authority, an incorrect decision can immediately become a production incident.

AegisMesh addresses this by treating every agent-generated action as **untrusted until explicitly authorized**.

---

# Architecture

```text
                   INCIDENT / EVENT
                          │
                          ▼
              ┌─────────────────────┐
              │ MULTI-AGENT SWARM   │
              │                     │
              │ Investigator Agent  │
              │ Challenger Agent    │
              │ Risk Agent          │
              │ Response Agent      │
              └──────────┬──────────┘
                         │
                    hypotheses
                    + evidence
                         │
                         ▼
              ┌─────────────────────┐
              │   EVIDENCE COURT    │
              │                     │
              │ supporting evidence │
              │ contradictions      │
              │ confidence          │
              │ counterfactual risk │
              └──────────┬──────────┘
                         │
                   proposed action
                         │
                         ▼
              ┌─────────────────────┐
              │     AEGIS GATE      │
              │                     │
              │ identity            │
              │ permissions         │
              │ trust score         │
              │ policy scope        │
              │ incident state      │
              │ human approval      │
              └──────────┬──────────┘
                         │
                   ALLOW / DENY
                         │
                         ▼
              ┌─────────────────────┐
              │ SIGNED CAPABILITY   │
              │                     │
              │ short lived         │
              │ target scoped       │
              │ action scoped       │
              │ replay protected    │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │   EDGE EXECUTION    │
              │                     │
              │ verify capability   │
              │ validate target     │
              │ execute action      │
              └─────────────────────┘
```

---

# Zero-Trust Model

AegisMesh uses four distinct control layers.

## 1. Investigate — Multi-Agent Swarm

Specialized agents investigate different aspects of an incident concurrently.

Agents can:

- gather evidence;
- generate hypotheses;
- challenge conclusions;
- identify contradictions;
- evaluate possible remediation paths.

An agent's reasoning ability does **not** grant it additional permissions.

---

## 2. Reason — Evidence Court

The Evidence Court compares competing explanations instead of immediately accepting the first agent conclusion.

It tracks:

```text
Hypothesis
    │
    ├── supporting evidence
    ├── contradictory evidence
    ├── confidence
    └── counterfactual risk
```

The objective is not simply to identify the most likely explanation.

The system also asks:

> What happens if this hypothesis is wrong?

That allows the platform to prefer safer and more reversible responses when uncertainty remains high.

---

## 3. Govern — Aegis Gate

Every proposed production action passes through a **deny-by-default policy engine**.

The control plane evaluates factors such as:

```text
Agent identity
       +
Permission scope
       +
Trust state
       +
Target resource
       +
Requested action
       +
Incident state
       +
Risk level
       +
Human approval
       ↓
   ALLOW / DENY
```

Examples of blocked actions include:

- agent lacks permission for the target;
- requested action exceeds the agent's role;
- trust score falls below policy;
- agent has been quarantined;
- incident state does not permit remediation;
- high-impact action lacks human dual approval.

The LLM does not make the final authorization decision.

---

# Signed Capabilities

When an action passes policy validation, AegisMesh issues a short-lived execution capability.

The capability is:

- action-specific;
- resource-specific;
- time-limited;
- cryptographically signed;
- verified before execution;
- protected against replay.

Conceptually:

```text
Agent proposes:

restart(service-A)

        ↓

Policy authorizes:

agent-17
may restart
service-A
for next 30 seconds

        ↓

Signed capability

        ↓

Edge verifies signature + scope + expiry

        ↓

Execute
```

This prevents an agent from turning one approved action into unrestricted access.

---

# Agent Passports

Each agent has an explicit identity and security state.

Example:

```text
Agent
├── ID
├── Role
├── Allowed capabilities
├── Trust score
├── Current task
└── State
```

Possible states include:

```text
INVESTIGATING
CHALLENGING
APPROVED
DEGRADED
QUARANTINED
```

If behavior violates policy or trust thresholds, the control plane can quarantine the agent without shutting down the entire investigation.

---

# Human Dual Control

High-impact operations require explicit human authorization.

Examples might include:

- production rollback;
- service shutdown;
- infrastructure isolation;
- high-value financial action;
- physical-device command.

The workflow becomes:

```text
Agent recommendation
        ↓
Policy validation
        ↓
Human approval
        ↓
Signed capability
        ↓
Execution
```

This preserves automation while maintaining human authority over consequential operations.

---

# Replay Protection & Scoped Execution

Authorization is intentionally temporary.

A capability is tied to:

```text
WHO
which agent

WHAT
which operation

WHERE
which resource

WHEN
expiration time

WHY
associated incident / authorization
```

A previously approved token cannot simply be reused indefinitely for later actions.

---

# Example Incident Flow

A typical AegisMesh scenario:

```text
1. Monitoring detects anomalous behavior.

2. Investigator agents collect evidence.

3. A challenger agent disputes the initial hypothesis.

4. Evidence Court compares both explanations.

5. Agents recommend the least-destructive remediation.

6. Aegis Gate evaluates:
      identity
      permissions
      trust
      action scope
      risk
      approval requirements

7. High-impact remediation requires human approval.

8. A short-lived signed capability is issued.

9. The edge validates the capability.

10. Execution occurs.

11. The result becomes new evidence for continued monitoring.
```

---

# Security Principle

AegisMesh follows a principle similar to zero-trust infrastructure:

> **Never trust an action simply because it originated from an intelligent system. Verify identity, scope, state and authorization every time.**

This means:

```text
Agent intelligence ≠ production authority
```

Agents can be powerful without being privileged.

---

# Key Engineering Concepts

AegisMesh explores practical systems concepts including:

- multi-agent orchestration
- zero-trust authorization
- deny-by-default policy enforcement
- least privilege
- capability-based security
- cryptographic signing
- replay protection
- trust-state management
- agent quarantine
- human-in-the-loop authorization
- evidence provenance
- competing-hypothesis evaluation
- counterfactual risk analysis
- safe autonomous operations

---

# Demo Scenarios

The project includes guided workflows demonstrating different system states.

### Normal Operations

Agents monitor a healthy environment without unnecessary intervention.

### False Positive

An initial anomaly is investigated but contradictory evidence prevents an unnecessary remediation.

### Active Incident

Multiple agents investigate a genuine failure and propose a response.

### Compromised Agent

An agent attempts an unauthorized action and is blocked or quarantined.

### High-Risk Remediation

An otherwise valid action is withheld until human approval is provided.

### Governed Execution

An authorized action receives a scoped signed capability and executes successfully.

---

# Cross-Domain Applications

Although cybersecurity and infrastructure incidents provide the primary demonstration environment, the control-plane model is domain independent.

### Cloud & DevOps

Agents can investigate outages and recommend infrastructure changes without receiving unrestricted production credentials.

### Financial Systems

Autonomous agents can analyze and propose purchases, transfers or trades while policy and authorization remain external.

### Robotics & Infrastructure

Reasoning systems can select actions while physical execution remains protected by an independent authorization boundary.

### Healthcare Operations

Agents can coordinate workflows and surface recommendations without automatically expanding their access to sensitive or high-impact operations.

---

# Why This Architecture Matters

Many agentic AI systems focus on making agents increasingly capable.

AegisMesh explores the complementary question:

> **As agents become more capable, how do we prevent capability from automatically becoming authority?**

The system intentionally separates:

```text
reasoning
from
authorization

recommendation
from
execution

confidence
from
permission
```

That separation allows autonomous systems to remain useful while reducing the consequences of hallucination, compromise or incorrect reasoning.

---

# Hack the North 2026

🏆 **Tether — Best Sovereign App**

AegisMesh was recognized for its approach to autonomous-agent sovereignty, security and controlled execution.

The project demonstrated how decentralized or autonomous intelligence can remain powerful while authority over consequential actions stays explicit, scoped and verifiable.

---

# Engineering Takeaway

The most important lesson from building AegisMesh was:

> **The safest autonomous system is not necessarily the one with the smartest agent. It is the one that assumes agents can be wrong and designs the authorization boundary accordingly.**

AI can investigate.

AI can challenge.

AI can recommend.

**But authority should still be earned, verified and scoped.**
