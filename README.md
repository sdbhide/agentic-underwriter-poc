
# Agentic Insurance Underwriter (PoC)

Governance for the Autonomous Enterprise: Underwriting reliability in the age of Agentic Hallucination.

## Overview

As enterprise decisions are increasingly delegated to autonomous agents, the primary bottleneck to adoption is reliability. This project is a Proof of Concept (PoC) for an AI Insurance Underwriter Agent. It acts as a Master Orchestrator (Governance Agent) that sits between user requests and execution agents.

The Underwriter evaluates every task through a Risk Gate. If the risk is low, it "underwrites" the decision for a virtual premium. If the risk is high, it pauses execution and routes the task to a human-in-the-loop (HITL) queue.

## The Problem: Hallucinations

Agentic unreliability stems from two categories of hallucinations:

Low Logprob: Uncertain guesswork (detectable via probability thresholds).

High Logprob: Confident wrong outputs (often caused by the "Lost-in-the-Middle" problem in large context windows).

This PoC demonstrates how to solve these problems through insurance-grade orchestration rather than just prompt engineering.

## System Architecture

The system is composed of four primary layers:

The Master Orchestrator (Underwriter): Owns the API keys and execution gates. It calculates risk before allowing worker agents to act.

Worker Agents: Specialized agents (e.g., Invoicing Agent) that perform the tasks.

The Critic (Auditor): A separate model family (e.g., GPT auditing Claude) that verifies reasoning paths to eliminate shared architectural blind spots.

The Actuarial Ledger: A persistent SQL database logging every logprob, decision, and confidence score for compliance and calibration audits.

## The Math: The Risk Gate

The core decision logic is binary and auditable based on the Expected Value (EV) of Risk:

$$EV = P(\text{Hallucination}) \times \text{Risk Multiplier} \times \text{Cost of Error}$$

Decision Logic:

EV < Labor Cost: 🟢 Auto-Approve. The system executes autonomously and charges a virtual premium.

EV > Labor Cost: 🔴 Human Delegation. The risk is shifted to a human employee; premium is zero.

The Net Cost Equation:

$$\text{Net Cost} = \text{Inference Cost} + \text{Insurance Premium} - \text{Labor Cost Avoided}$$

## Repository Structure

├── agents/

│   ├── worker_agent.py       # Action-oriented agents (e.g., Invoicing)
│   ├── critic_agent.py       # LLM-as-a-Judge reasoning audit
│   └── underwriter_agent.py  # Governance & Master Orchestration
├── core/

│   ├── risk_engine.py        # EV Calculation & Decision Logic
│   ├── ledger.py             # SQL Actuarial Ledger interface
│   └── chunker.py            # Agentic Chunking for large context handling
├── data/

│   └── actuarial_ledger.db   # SQLite DB for audit trails
└── config.yaml               # Thresholds, Labor Rates, and Multipliers

## Getting Started

Configure the Environment: Adjust config.yaml to set your human labor rates and model risk multipliers.

Run a Task: Use main.py to submit a task (e.g., a high-value invoice).

Audit the Ledger: Review data/actuarial_ledger.db to see the logprobs and why the Underwriter chose a specific path.

Calibrate: Run the calibration script to update risk multipliers based on historical accuracy.

Disclaimer: This is a Proof of Concept designed for a virtual environment. No real money or insurance contracts are being exchanged.
