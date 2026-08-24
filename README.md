# Agentic Customer Retention Engine

An MVP multi-agent AI system that turns customer churn signals into actionable, explainable retention recommendations.

## 1. Overview

Churn prediction tells a business which customers are likely to leave, but not what to do about it. This project explores how agentic AI can bridge that gap.

Instead of one LLM generating a recommendation, the system breaks the problem into stages: Risk Analysis, Pain Point Analysis, Strategy Generation, Strategy Critique, Final Decision, and a Guardrail step.

The output is a business-facing recommendation explaining what to do, why, customer value, business value, and key risks.

Product principle: LLMs handle flexible reasoning, deterministic code handles non-negotiable business rules. For example, a maximum 20% discount is enforced through code, not left to the LLM.

## 2. How It Works

The workflow is orchestrated in n8n, with structured JSON passed between agents.

Flow: Customer Data, Risk, Pain Point, Strategies, Critique, Decision, Guardrail, Recommendation.

A key feature is the MODIFY decision. The system does not have to blindly pick the best generated strategy. It can modify one when the idea is useful but contains an unsupported assumption. For example, instead of immediately offering a discounted long-term contract, it may first confirm the customer's willingness to commit.

## 3. Validation

The workflow was tested using data from customers who had actually churned, to check whether recommendations were directionally consistent with known churn reasons.

Example, customer 3668-QPYBK. System recommendation: set up a customized plan creation call. Recorded churn reason: a competitor made a better plan offer. The recommendation aligns with the underlying issue, since a customized plan could address a perceived value or offer gap.

This is a sanity check, not proof of causal impact. We cannot know whether the intervention would have prevented churn without historical intervention data or controlled experimentation.

## 4. Tech Stack

- n8n for agent orchestration and workflow automation. 
- LLMs for reasoning, strategy generation, critique, and decision-making. 
- JavaScript for deterministic guardrails. JSON for structured communication between agents. 
- n8n Forms for the input and final recommendation interface.

## 5. Limitations & Future Scope

This is an MVP. The goal was to validate the agentic decision-making workflow, not build a production retention platform.

The biggest limitation is that the system has no historical business context, such as past interventions and their outcomes, acceptance rates, churn patterns, customer lifetime value trends, competitor offers, or usage history. It can reason about an individual customer, but cannot yet answer what has historically worked for similar customers.

The system also does not establish causal relationships between attributes and churn, guarantee that a recommendation will prevent churn, automatically execute interventions, or eliminate variability in LLM outputs.

Future improvements include a historical outcomes data layer, customer sentiment integration, controlled A/B testing, CRM connections, and optimisation using retention probability, lifetime value, and intervention cost.
