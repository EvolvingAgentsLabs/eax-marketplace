# EAX Marketplace 🤖⚖️

**(Evolving Agents Experimental Marketplace)**

An experimental protocol for dynamic agent discovery and task allocation.

<p align="center">
  <a href="https://github.com/EvolvingAgentsLabs/eax-marketplace/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License"></a>
  <a href="https://evolvingagentslabs.github.io/"><img src="https://img.shields.io/badge/labs-EvolvingAgentsLabs-brightgreen" alt="Labs"></a>
  <a href="#"><img src="https://img.shields.io/badge/status-alpha_experiment-orange.svg" alt="Status"></a>
</p>

> 🌐 **Part of the [Evolving Agents Labs](https://evolvingagentslabs.github.io) Research Initiative**
>
> This project explores how an agent system, like one defined by the [llmunix-spec](https://github.com/EvolvingAgentsLabs/llmunix-spec), can dynamically assemble its own capabilities instead of relying on a fixed set of tools.

---

## ⚠️ Experimental Research Project

**Important**: This is an early-stage research prototype exploring dynamic agent collaboration. It should be treated as research material, not a production-ready system. This project will remain permanently in alpha status as we explore the future of adaptive agent architecture.

---

## The Research Hypothesis: Beyond Hardcoded Agent Chains

Modern agentic systems are often built with rigid, pre-defined architectures. A `Planner` is hardcoded to call a `Summarizer`, which is hardcoded to call a `Writer`. This is brittle and inefficient.

Our research, inspired by the adaptive principles in `llmunix`, investigates a new paradigm: **Can an agent system discover and select the optimal "worker" agent for a given task from a competitive, open ecosystem?**

This hypothesis addresses several key limitations of static systems:
- **Rigid Architectures:** Fixed agent relationships prevent adaptability.
- **Lack of Discovery:** A system cannot find or utilize a better, cheaper, or more specialized agent that may exist.
- **Single Points of Failure:** If a hardcoded agent fails, the entire workflow breaks.
- **No Optimization:** The system cannot dynamically re-allocate tasks for better performance or cost.

## The Experiment: A Marketplace for Cognitive Work

EAX Marketplace is our experimental protocol for **dynamic capability sourcing**. It defines a simple, open auction mechanism where an "Orchestrator" agent can find the best "Worker" agent for a specific job.

### The Auction Flow
An Orchestrator doesn't call a specific tool; it announces a need.

```
1. 📢 TASK ANNOUNCEMENT → 2. 💰 BIDDING → 3. 🏆 SELECTION → 4. ⚡ EXECUTION
   Orchestrator          Worker Agents      Best Bid         Task is delegated
   describes a need      propose to help    is chosen        to the winner
```

### How It Works: A Conceptual Example

#### The Orchestrator's Perspective
```python
from eax_marketplace import Orchestrator, Task, BidEvaluator

# Initialize an orchestrator with a selection strategy
orchestrator = Orchestrator(evaluator=BidEvaluator.BestValue()) # BestValue balances cost and confidence

# Define a task based on a need that has emerged
task = Task(
    task_id="legal-analysis-001",
    description="Summarize a 10-page legal contract and identify key risks.",
    requirements={"domain": "legal", "minimum_confidence": 0.90},
    budget={"max_cost": 0.50, "currency": "USD"}
)

# Run an auction to find a suitable agent
auction_result = orchestrator.run_auction(task, timeout_seconds=10)

print(f"Winning Bidder: {auction_result.winner.agent_id}")
print(f"Stated Confidence: {auction_result.winner.confidence * 100:.0f}%")
print(f"Quoted Cost: ${auction_result.winner.estimated_cost:.2f}")

# Delegate the task to the most suitable agent found in the ecosystem
# result = auction_result.execute()
```

#### The Worker Agent's Perspective
```python
from eax_marketplace import WorkerAgent, Bid, Capability

# An example of a specialized worker agent
class LegalAnalysisAgent(WorkerAgent):
    def __init__(self):
        super().__init__(
            agent_id="LegalSummarizer-v3",
            capabilities=[
                Capability(name="legal_analysis", confidence=0.98),
                Capability(name="risk_identification", confidence=0.95),
            ]
        )
    
    def evaluate_and_bid(self, task: Task) -> Bid | None:
        """My internal logic to decide if I should bid."""
        if task.requirements.get("domain") == "legal":
            # I am a good fit, so I will construct and return a bid
            return Bid(
                agent_id=self.agent_id,
                task_id=task.id,
                confidence=0.98,
                estimated_cost=0.25,
                reasoning="Specialized in legal contract analysis with high confidence."
            )
        return None # I am not a good fit for this task
    
    def execute_task(self, task: Task):
        # My core logic for performing the task
        return self.analyze_legal_document(task.content)

# The agent joins the marketplace and listens for opportunities
# agent = LegalAnalysisAgent()
# agent.listen_for_tasks()
```

## Core Research Areas

### 1. The Open Auction Protocol
We are developing a formal, JSON-based specification for agent-to-agent task negotiation. This protocol defines the `Task_Announcement` and `Bid` message structures, forming the basis for an open and interoperable agent economy.

### 2. Bid Evaluation Strategies
How does an orchestrator choose the best bid? We are experimenting with pluggable evaluation strategies:
- `LowestCost`: Purely price-driven.
- `HighestConfidence`: For critical tasks where quality is paramount.
- `BestValue`: A balanced approach considering cost, confidence, and other metadata.
- `ReputationWeighted`: An advanced strategy that would factor in an agent's historical performance.

### 3. Agent Self-Awareness (`Capability` Schema)
A key area of research is how agents can accurately represent their own capabilities. The `Capability` schema is our first step towards a standardized way for agents to "advertise" their skills and confidence levels.

## Research Roadmap

Our goal is to create the foundational protocol for self-assembling agentic systems.

### Phase 1: Protocol Foundation (Current)
- [x] Basic auction protocol definition (`Task_Announcement`, `Bid`).
- [x] Reference implementations in Python for `Orchestrator` and `Worker`.
- [ ] Abstract the transport layer (e.g., HTTP, message queues).
- [ ] Implement initial set of `BidEvaluator` strategies.

### Phase 2: Advanced Marketplace Dynamics
- [ ] Research and implement agent **reputation systems**.
- [ ] Explore mechanisms for **complex task decomposition** and sub-auctions.
- [ ] Investigate multi-round auctions and negotiation patterns.

### Phase 3: Ecosystem & Integration
- [ ] Develop agent discovery mechanisms (a "yellow pages" for agents).
- [ ] Integrate with the [EAX Protocol (SAL-CP)](https://github.com/EvolvingAgentsLabs/sal-cp) for richer communication during bidding and execution.
- [ ] Standardize the `Capability` schema to interoperate with the `fingerprint.json` from the [EAX Router](https://github.com/EvolvingAgentsLabs/eax-router).

---

## Contributing

This is a frontier research project. We invite you to contribute by:
- Proposing changes or improvements to the auction protocol spec.
- Developing new and more sophisticated `BidEvaluator` strategies.
- Building example `Worker` agents with unique specializations.
- Exploring different network transport layers for agent communication.

Please read our `CONTRIBUTING.md` for more details.

## License

EAX Marketplace is licensed under the Apache 2.0 License.

## Connect

**Research Contributors**: [Matias Molinas](https://github.com/matiasmolinas) • [Ismael Faro](https://github.com/ismaelfaro)

**Organization**: [EvolvingAgentsLabs](https://github.com/EvolvingAgentsLabs) • **Lab Site**: [evolvingagentslabs.github.io](https://evolvingagentslabs.github.io)

---

*EAX Marketplace is a core component of the experimental agent architecture being developed at [Evolving Agents Labs](https://evolvingagentslabs.github.io). Our goal is to build the foundational tools for truly intelligent and adaptive AI systems.*
