# **EAX Marketplace 🤖⚖️**

**(Evolving Agents Experimental Marketplace)**

The economic layer for **[LLMunix](https://github.com/EvolvingAgentsLabs/llmunix)** agent networks, enabling dynamic task allocation through a decentralized auction protocol implemented with virtual tools.

<p align="center">
  <a href="https://github.com/EvolvingAgentsLabs/llmunix/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License"></a>
  <a href="https://evolvingagentslabs.github.io/"><img src="https://img.shields.io/badge/labs-EvolvingAgentsLabs-brightgreen" alt="Labs"></a>
  <a href="#"><img src="https://img.shields.io/badge/status-alpha_implementation-orange.svg" alt="Status"></a>
</p>

> 🌐 **Part of [Evolving Agents Labs](https://evolvingagentslabs.github.io)** | 🔬 [View All Experiments](https://evolvingagentslabs.github.io#experiments) | 📖 [Project Details](https://evolvingagentslabs.github.io/experiments/eax.html)

---

## **The Problem: How Do Autonomous Agents Hire Each Other?**

In a sophisticated agentic system like `LLMunix`, where many specialized agents exist, a critical question arises: how does the system decide which agent is best for a given task? Simple delegation from a central orchestrator works, but it's not optimal. A truly dynamic system needs a market mechanism for:

-   **Efficient Resource Allocation:** Ensuring the most capable and available agent is assigned the job.
-   **Price Discovery:** Establishing a fair "cost" for different cognitive tasks.
-   **Incentivizing Specialization:** Rewarding agents that develop unique, high-quality skills.
-   **System Resilience:** Allowing the network to route around a single, busy, or failed agent.

EAX Marketplace introduces these economic principles to `LLMunix` through a set of roles, protocols, and virtual tools that facilitate a **decentralized task auction**.

## **Implementation in `LLMunix`**

The EAX Marketplace is not a separate server; it is a **behavioral pattern and protocol** implemented within the `LLMunix` framework. It consists of two key roles and a message-based workflow.

### **1. The `AuctioneerAgent` Role**

Any agent can assume the role of an "Auctioneer". Typically, a high-level agent like the `CEOAgent` or the main `SystemAgent` will initiate an auction when faced with a task that could be performed by multiple subordinate agents.

The Auctioneer's logic is simple:
1.  Receive a task to be auctioned.
2.  Use the `broadcast_message` tool to post a **Request for Bids (RFB)** to a public bulletin board (e.g., `#marketplace-tasks`).
3.  Use `check_messages` to collect bids sent directly to its inbox.
4.  Evaluate the bids based on criteria (confidence, time, cost).
5.  Use `send_message` to award the contract to the winning agent.

### **2. The "Worker" Agent Bidding Logic**

Any specialized agent (e.g., `ContentWriterAgent`, `MarketAnalystAgent`) can be a "Worker" that participates in auctions. Their `GEMINI.md` firmware includes instructions for this behavior.

A Worker Agent's bidding loop:
1.  Periodically use `check_messages` on the `#marketplace-tasks` bulletin to look for new RFBs.
2.  For each RFB, evaluate if the task matches its capabilities (e.g., a `ContentWriterAgent` checks for "writing" keywords).
3.  If it's a match, calculate a bid based on its internal pricing model.
4.  Use `send_message` to submit a structured bid directly to the Auctioneer agent.

### **3. The Protocol: SAL-CP for Economic Transactions**

The entire auction is conducted using the native **[SAL-CP](https://github.com/EvolvingAgentsLabs/sal-cp)** messaging protocol of `LLMunix`.

**Step 1: Auctioneer Broadcasts an RFB**

The `CEOAgent` calls `broadcast_message`:
```json
{
  "topic": "marketplace-tasks",
  "message": "---
# SAL-CP/v1.0 Header
message_type: REQUEST_FOR_BID
task_id: task_123
from: CEOAgent
auction_closes: 1720200000

# Payload
payload:
  content: 'Generate a 500-word blog post about the EcoFlow Pro.'
  requirements:
    - 'tone: professional_but_approachable'
    - 'include_seo_keywords: [sustainable, water, home]'
---
"
}
```

**Step 2: Worker Agents Submit Bids**

The `ContentWriterAgent` and `SummarizationAgent` both see the RFB.

`ContentWriterAgent` submits its bid via `send_message`:
```json
{
  "to": "CEOAgent",
  "priority": "normal",
  "message": "---
# SAL-CP/v1.0 Header
message_type: BID_SUBMISSION
task_id: task_123
from: ContentWriterAgent

# Payload
payload:
  content: 'Bid to write blog post.'
  bid_details:
    cost_estimate: 0.15, # Estimated token cost
    time_estimate_minutes: 25,
    confidence_score: 0.95
---
"
}
```

**Step 3: Auctioneer Awards the Contract**

The `CEOAgent` evaluates bids and sends a direct message to the winner:
```json
{
  "to": "ContentWriterAgent",
  "priority": "high",
  "message": "---
# SAL-CP/v1.0 Header
message_type: BID_AWARD
task_id: task_123
from: CEOAgent
---
You have been awarded the contract for task_123. Please proceed and store the final output at workspace/outputs/ecoflow_blog.md.
"
}
```

## **Example: Running a Marketplace Workflow in `LLMunix`**

This system can be tested directly with the existing `LLMunix` framework.

1.  **Boot the System:**
    ```bash
    ./llmunix-boot
    ```
2.  **Launch Gemini CLI:**
    ```bash
    gemini
    ```
3.  **Provide a Goal That Requires a Choice:**
    > `"I need a blog post about our new product. Please find the best agent for the job by running an auction."`

**Expected Autonomous Behavior:**

1.  The `SystemAgent` understands the "run an auction" instruction.
2.  It broadcasts an RFB to the `#marketplace-tasks` bulletin board.
3.  Multiple agents (e.g., `ContentWriterAgent`, `SummarizationAgent`, `WritingAgent`) check the board.
4.  The `ContentWriterAgent` and `WritingAgent` determine it's a good fit and submit bids. The `SummarizationAgent` ignores it.
5.  The `SystemAgent` receives the bids, evaluates that the `ContentWriterAgent` has higher confidence, and awards it the contract.
6.  The `ContentWriterAgent` proceeds with the task.

## **Research Implications of the EAX Marketplace**

Implementing this economic layer within `LLMunix` allows for research into several advanced AI concepts:

*   **Emergent Specialization:** Agents that consistently win bids for certain tasks (like writing) will be reinforced, while those that lose will be less utilized, creating a market-driven pressure to specialize.
*   **Dynamic Pricing:** Agents could learn to adjust their `cost_estimate` based on demand, their current workload (`resource_usage` in their SAL-CP context), or the task's urgency.
*   **Reputation Systems:** A more advanced `AuctioneerAgent` could maintain a `permanent_memory` log of agent performance, factoring `success_rate` and `client_satisfaction` into its bid evaluation, creating a reputation economy.
*   **Decentralized Resilience:** If the best `ContentWriterAgent` is "busy" (or simulated as failed), the auction mechanism naturally allows the system to fall back to the second-best agent (`WritingAgent`), ensuring the task still gets completed.

This project demonstrates that complex economic behaviors can be modeled and executed within a pure Markdown and virtual tool framework, paving the way for more sophisticated, resilient, and efficient multi-agent systems.

---

## Connect

**Research Contributors**: [Matias Molinas](https://github.com/matiasmolinas) • [Ismael Faro](https://github.com/ismaelfaro)

**Organization**: [EvolvingAgentsLabs](https://github.com/EvolvingAgentsLabs) • **Lab Site**: [evolvingagentslabs.github.io](https://evolvingagentslabs.github.io)

---

*Part of the EAX Protocol Suite from [Evolving Agents Labs](https://evolvingagentslabs.github.io) - Building the future of intelligent agents through experimental research*