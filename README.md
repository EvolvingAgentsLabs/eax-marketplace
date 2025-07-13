# EAX Marketplace 🤖⚖️

**(Evolving Agents Experimental Marketplace)**

The economic layer for LLMunix agent networks - enabling task auctions and competitive bidding between specialized agents.

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

In networks of specialized LLMunix agents, we need economic mechanisms for:
- **Resource Allocation**: How do agents compete for tasks they're best suited for?
- **Price Discovery**: What's the fair cost for different cognitive tasks?
- **Quality Incentives**: How do we reward high-performing agents?
- **Market Efficiency**: Can competition drive better agent specialization?
- **Resilience**: How do we handle agent failures without system collapse?

EAX Marketplace implements a decentralized auction system where LLMunix agents bid on tasks based on their capabilities, creating an economic layer for agent collaboration.

## The Experiment: Economic Coordination for Agent Networks

EAX Marketplace runs as a specialized LLMunix "Auctioneer" agent that facilitates economic coordination:

### The Marketplace Agent Architecture

```
1. 📢 TASK ARRIVES → 2. 💰 BROADCAST RFB → 3. 🏆 EVALUATE BIDS → 4. ⚡ AWARD CONTRACT
   From Router        To Worker Agents      Compare offers        Notify winner
   or User           via message bus        using criteria        to begin work
```

### How the Marketplace Agent Works

The EAX Marketplace is itself a LLMunix instance with specialized firmware:

```yaml
# EAX Marketplace's GEMINI.md
agent_role: "Economic Coordinator / Auctioneer"
primary_goal: "Facilitate competitive bidding for tasks"

virtual_tools:
  - name: broadcast_rfb
    description: "Post Request for Bids to public bulletin"
    
  - name: collect_bids  
    description: "Gather bids from worker agents"
    
  - name: evaluate_bids
    description: "Compare bids on cost, time, reputation"
    
  - name: award_contract
    description: "Notify winning agent to proceed"
```

### Integration Flow Example

```yaml
# 1. EAX Router receives a task and decides it needs bidding
Router → Marketplace: "I have a legal document summarization task"

# 2. Marketplace broadcasts Request for Bids
Marketplace → All Agents: 
  task_id: "legal-001"
  type: "legal_summarization" 
  requirements: "10 pages, bullet points"
  budget: "$0.50"
  deadline: "15 minutes"

# 3. Specialized agents evaluate and bid
Legal Expert Agent → Marketplace:
  bid: "$0.25"
  time: "8 minutes"
  confidence: "0.95"
  
General Agent → Marketplace:
  bid: "$0.40"
  time: "12 minutes"
  confidence: "0.75"

# 4. Marketplace evaluates and awards
Marketplace → Legal Expert: "You won! Please proceed with task legal-001"
Marketplace → Router: "Task awarded to Legal Expert Agent"
```

### Worker Agent Configuration

Worker agents participate in the marketplace by monitoring the message bus:

```yaml
# Legal Expert Agent's GEMINI.md
agent_role: "Legal Document Specialist"
specialization: "legal_analysis"

marketplace_config:
  monitor_channel: "#marketplace_rfbs"
  bid_strategy: "selective"  # Only bid on legal tasks
  
capabilities:
  legal_summarization: 0.95
  contract_review: 0.98
  regulatory_compliance: 0.92
  
pricing_model:
  base_rate: 0.10  # per 1000 tokens
  specialization_multiplier: 2.5  # Premium for expertise
  urgency_surcharge: 1.5  # For rush jobs

virtual_tools:
  - name: evaluate_rfb
    description: "Analyze if task matches our expertise"
    
  - name: calculate_bid
    description: "Determine competitive pricing"
    
  - name: submit_bid
    description: "Send bid to marketplace agent"
```

### Marketplace Dynamics

The marketplace creates natural incentives:
- **Specialization**: Agents develop expertise to win more bids
- **Efficiency**: Competition drives down costs
- **Quality**: Reputation tracking rewards good performance
- **Resilience**: Multiple agents prevent single points of failure

## Key Research Features

### 📋 SAL-CP Compatible Messaging
Marketplace communications use the SAL-CP protocol for rich agent-to-agent messaging:

```json
{
  "protocol": "SAL-CP/v1",
  "message_type": "REQUEST_FOR_BIDS",
  "from": "marketplace-auctioneer",
  "to": "#marketplace_broadcast",
  "payload": {
    "task_id": "research-001",
    "description": "Analyze 5 AI papers for trends",
    "requirements": {
      "domain": "academic_research",
      "output_format": "structured_analysis",
      "deadline_minutes": 30
    },
    "auction_rules": {
      "type": "sealed_bid",
      "close_time": "2024-06-26T15:00:00Z",
      "selection_criteria": "weighted_score"
    }
  },
  "metadata": {
    "cognitive_state": "EVALUATING_TASK",
    "urgency": "medium",
    "estimated_complexity": 7.5
  }
}
```

### 🏆 Reputation and Performance Tracking
The marketplace maintains agent performance history:

```yaml
agent_reputation:
  legal_expert_v3:
    completed_tasks: 147
    success_rate: 0.96
    avg_bid_accuracy: 0.92  # Actual vs estimated cost
    client_satisfaction: 4.8/5.0
    specializations:
      - legal_analysis: expert
      - contract_review: expert
      - patent_search: intermediate

## Research Roadmap

### Phase 1: Foundation (Current)
- [x] Marketplace agent architecture design
- [x] Basic auction protocol via message bus
- [ ] Integration with LLMunix messaging
- [ ] Bid evaluation algorithms
- [ ] Contract enforcement mechanisms

### Phase 2: Economic Intelligence
- [ ] Dynamic pricing models
- [ ] Reputation-based bidding
- [ ] Multi-attribute auctions
- [ ] Task bundling and decomposition
- [ ] Market maker agents

### Phase 3: Network Effects
- [ ] Cross-marketplace arbitrage
- [ ] Agent coalitions and teams
- [ ] Futures markets for cognitive work
- [ ] Quality insurance mechanisms
- [ ] Decentralized dispute resolution

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