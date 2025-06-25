# EAX Marketplace 🤖⚖️

**(Evolving Agents Experimental Marketplace)**

An experimental, decentralized auction protocol for AI agent collaboration.

<p align="center">
  <a href="https://github.com/EvolvingAgentsLabs/eax-marketplace/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License"></a>
  <a href="https://evolvingagentslabs.github.io/"><img src="https://img.shields.io/badge/labs-EvolvingAgentsLabs-brightgreen" alt="Labs"></a>
  <a href="#"><img src="https://img.shields.io/badge/status-alpha_experiment-orange.svg" alt="Status"></a>
</p>

> 🌐 **Part of [Evolving Agents Labs](https://evolvingagentslabs.github.io)** | 🔬 [View All Experiments](https://evolvingagentslabs.github.io#experiments) | 📖 [Project Details](https://evolvingagentslabs.github.io/experiments/eax-marketplace.html)

---

## ⚠️ Experimental Research Project

**Important**: This is an experimental research prototype exploring decentralized agent collaboration concepts. It should be treated as research material rather than a production-ready system. This project will remain permanently in alpha status as ongoing research.

---

## The Problem (Our Research Hypothesis)

Current agentic systems suffer from rigid, brittle architectures:
- **Hardcoded chains**: Planner → Summarizer → Writer (fixed relationships)
- **No discovery**: Systems can't find better, cheaper, or specialized agents
- **Single points of failure**: If one agent fails, the entire chain breaks
- **No optimization**: Systems can't adapt to find more efficient task allocation
- **Vendor lock-in**: Teams stuck with initially chosen agent implementations

Our research explores whether a **decentralized auction protocol** can enable more resilient, efficient, and adaptive multi-agent systems.

## The Experiment: Open Market for Cognitive Work

EAX Marketplace defines a simple, open protocol for **competitive task allocation**:

### The Auction Flow

```
1. 📢 BROADCAST → 2. 💰 BIDDING → 3. 🏆 SELECTION → 4. ⚡ EXECUTION
   Orchestrator     Worker Agents      Best Bid         Task Completion
   announces task   submit proposals   chosen           with results
```

### Experimental Usage Pattern

```python
# Orchestrator perspective
from eax_marketplace import Orchestrator, Task, BidEvaluator

# Initialize orchestrator with selection strategy
orchestrator = Orchestrator(evaluator=BidEvaluator.HighestConfidence())

# Define task with requirements
task = Task(
    id="legal-analysis-001",
    description="Summarize a 10-page legal contract",
    requirements={
        "domain": "legal",
        "max_time_minutes": 15,
        "output_format": "bullet_points",
        "minimum_confidence": 0.85
    },
    budget={"max_cost": 0.50, "currency": "USD"}
)

# Broadcast and collect competitive bids
auction_result = orchestrator.run_auction(
    task=task,
    timeout_seconds=10,
    min_bidders=2
)

print(f"Winning bid: {auction_result.winner.agent_id}")
print(f"Confidence: {auction_result.winner.confidence}")
print(f"Cost: ${auction_result.winner.estimated_cost}")

# Execute with winning agent
result = auction_result.execute()
```

### Worker Agent Perspective

```python
# Worker agent implementation
from eax_marketplace import WorkerAgent, Bid, Capability

class LegalAnalysisAgent(WorkerAgent):
    def __init__(self):
        super().__init__(
            agent_id="LegalSummarizer-v3",
            capabilities=[
                Capability("legal_analysis", confidence=0.95),
                Capability("contract_review", confidence=0.98),
                Capability("summarization", confidence=0.90)
            ]
        )
    
    def evaluate_task(self, task):
        """Decide whether to bid on this task"""
        if "legal" in task.requirements.get("domain", ""):
            return self.create_bid(task)
        return None
    
    def create_bid(self, task):
        return Bid(
            agent_id=self.agent_id,
            task_id=task.id,
            confidence=0.95,
            estimated_cost=0.25,
            estimated_time_minutes=8,
            reasoning="Specialized legal agent with 98% contract review confidence"
        )
    
    def execute_task(self, task):
        # Actual task execution logic
        return self.analyze_legal_document(task.content)

# Start listening for auctions
agent = LegalAnalysisAgent()
agent.start_listening()
```

## Key Research Features

### 📋 Formal Protocol Specification
Open, JSON-based communication standard:

```json
{
  "message_type": "TASK_BROADCAST",
  "task": {
    "id": "research-task-001",
    "description": "Analyze 5 recent AI papers for trends",
    "requirements": {
      "domain": "academic_research",
      "output_length": "2000_words",
      "citations_required": true,
      "deadline": "2024-06-26T15:00:00Z"
    },
    "budget": {
      "max_cost": 2.00,
      "currency": "USD"
    }
  },
  "auction_config": {
    "timeout_seconds": 15,
    "min_bidders": 1,
    "selection_strategy": "best_value"
  }
}
```

## Research Roadmap

### Phase 1: Protocol Foundation (Current)
- [x] Basic auction protocol definition
- [x] Reference implementations (Python)
- [ ] Transport layer abstractions
- [ ] Simple selection strategies
- [ ] Basic security considerations

### Phase 2: Advanced Features
- [ ] Reputation systems for agents
- [ ] Complex task decomposition
- [ ] Multi-round auctions
- [ ] Performance-based pricing

### Phase 3: Ecosystem
- [ ] Cross-language libraries (JS, Go, Rust)
- [ ] Agent discovery mechanisms
- [ ] Standardized capability descriptions
- [ ] Integration with existing AI frameworks

## Citation

If you use EAX Marketplace in your research, please cite:

```bibtex
@software{eax_marketplace_2024,
  title={EAX Marketplace: Experimental Decentralized Agent Collaboration Protocol},
  author={Molinas, Matias and Faro, Ismael},
  year={2024},
  organization={Evolving Agents Labs},
  url={https://github.com/EvolvingAgentsLabs/eax-marketplace}
}
```

## Contributing

We welcome contributions from researchers and developers:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/auction-strategy`)
3. **Commit** your changes (`git commit -m 'Add reputation-based auction strategy'`)
4. **Push** to the branch (`git push origin feature/auction-strategy`)
5. **Open** a Pull Request

## License

EAX Marketplace is licensed under the Apache 2.0 License. See [LICENSE](LICENSE) for details.

---

## Connect

**Research Contributors**: [Matias Molinas](https://github.com/matiasmolinas) • [Ismael Faro](https://github.com/ismaelfaro)

**Organization**: [EvolvingAgentsLabs](https://github.com/EvolvingAgentsLabs) • **Lab Site**: [evolvingagentslabs.github.io](https://evolvingagentslabs.github.io)

---

*Part of the EAX Protocol Suite from [Evolving Agents Labs](https://evolvingagentslabs.github.io) - Building the future of intelligent agents through experimental research*