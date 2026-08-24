# Agentic Customer Retention Engine

> **An MVP multi-agent AI system that turns customer churn signals into
> actionable, explainable retention recommendations.**

## 1. Overview

### The Problem

Churn prediction tells a business **which customers are likely to
leave**, but it does not answer the more important operational question:

> **What should we actually do about it?**

A retention team needs to understand the likely customer problem,
compare possible interventions, consider business trade-offs, and decide
what action to take.

### The Solution

I built this MVP to explore how **agentic AI can bridge the gap between
churn prediction and retention action**.

Instead of using one LLM to generate a recommendation, the system breaks
the problem into specialized stages:

-   **Risk Analysis** --- identifies important churn and protective
    signals.
-   **Pain Point Analysis** --- converts those signals into potential
    customer problems while tracking evidence and uncertainty.
-   **Strategy Generation** --- creates multiple possible retention
    interventions.
-   **Strategy Critique** --- evaluates strategies on customer
    relevance, evidence, retention potential, revenue protection, cost
    and feasibility.
-   **Final Decision** --- selects, modifies, or rejects strategies.
-   **Guardrail** --- applies deterministic business rules before the
    recommendation is shown.

The final output is a business-facing recommendation explaining **what
to do, why, customer value, business value and key risks**.

### Product Principle

> **LLMs handle flexible reasoning; deterministic code handles
> non-negotiable business constraints.**

For example, the MVP enforces a maximum **20% discount** through a
code-based guardrail rather than relying on the LLM to follow the rule.

------------------------------------------------------------------------

## 2. How It Works

The workflow is orchestrated in **n8n**, with structured JSON passed
between agents.

The decision process is:

**Customer Data → Risk → Pain Point → Strategies → Critique → Decision →
Guardrail → Recommendation**

A key feature is the **MODIFY** decision. The system does not have to
blindly select the best generated strategy; it can modify it when the
underlying idea is useful but contains an unsupported assumption.

For example, instead of immediately offering a discounted long-term
contract, the system can modify the recommendation to **first confirm
the customer's willingness to commit**.

This makes the system more conservative and business-aware.

------------------------------------------------------------------------

## 3. Validation

I tested the workflow using data from customers who had **actually
churned** to check whether the recommendations were directionally
consistent with known churn reasons.

### Example --- Customer `3668-QPYBK`

**System recommendation:**\
Set up a customized plan creation call.

**Recorded churn reason:**\
The competitor made a better plan offer.

The recommendation is directionally aligned with the customer's eventual
reason for leaving: a customized plan could potentially have addressed
the customer's perceived value or offer gap.

This is a **sanity check, not proof of causal impact**. We cannot know
whether the intervention would actually have prevented churn without
historical intervention data or controlled experimentation.

------------------------------------------------------------------------

## 4. Tech Stack

-   **n8n** --- agent orchestration and workflow automation
-   **LLMs** --- reasoning, strategy generation, critique and
    decision-making
-   **JavaScript** --- deterministic guardrails
-   **JSON** --- structured communication between agents
-   **n8n Forms** --- final recommendation interface

------------------------------------------------------------------------

## 5. Limitations & Future Scope

This is intentionally an **MVP**. The goal was to validate the agentic
decision-making workflow rather than build a production retention
platform.

### The biggest limitation: historical business context

The system currently does not have access to a historical business
intelligence layer containing:

-   Previous retention interventions and their outcomes
-   Intervention acceptance rates
-   Historical churn patterns
-   Customer lifetime value trends
-   Past competitor offers
-   Product/service usage history
-   Segment-level retention performance

We intentionally did **not** add this layer in the MVP.

As a result, the system can reason about an individual customer's
available data, but it cannot yet answer:

> **"What has historically worked for customers like this?"**

A production version could add a retrieval/data layer containing
historical retention outcomes. The system could then ground
recommendations in evidence such as:

> *Customers with similar characteristics who received intervention X
> had a lower churn rate.*

Other future improvements include:

-   Integrating customer conversations, complaints and sentiment.
-   Running controlled A/B tests to measure intervention impact.
-   Connecting recommendations to CRM/customer-service systems.
-   Optimizing interventions using retention probability, customer
    lifetime value and intervention cost.

### Important limitations

The current system also does not:

-   Establish causal relationships between customer attributes and
    churn.
-   Guarantee that a recommendation will prevent churn.
-   Automatically execute the recommended intervention.
-   Eliminate variability or unsupported assumptions from LLM outputs.

------------------------------------------------------------------------

## Conclusion

The core idea behind this project is simple:

> **Move from predicting churn → to deciding what to do about it.**

The MVP demonstrates how multiple specialized AI agents can turn
customer-level churn information into an **explainable, actionable and
business-constrained retention decision**.

The next step is to ground those decisions in historical business
outcomes, turning the MVP from an **AI reasoning workflow into a
data-driven retention decision engine**.
