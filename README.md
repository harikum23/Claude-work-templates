# Claude Work Templates

This repository contains templates, system prompts, and configuration structures for a multi-agent AI system. The specialized agents cover various domains spanning software engineering, artificial intelligence model building, data engineering, and market analysis.

## Repository Structure

### 1. `agent-framework/`
This folder contains the core framework defining how agents cooperate and their assigned responsibilities.
- **`AGENTS_REGISTRY.md`**: The full routing matrix detailing all available agent personas, when to route tasks to which agent, the compatibility map for parallel execution, and routing decision trees.
- **`GOAL_TEMPLATE.md`**: A standardized template used to define the overarching goals, steps, and required outputs when invoking agents.

### 2. `agents/`
This folder contains the core persona definition files for each specialized AI agent. Each markdown file specifies the agent's core expertise, their generalized approach to solving problems, and their required output styles.

- **`ai-engineer.md`**: Expert AI Engineer and Data Scientist specializing in ML/DL models, NLP pipelines, computer vision systems, and MLOps.
- **`commodity-quant-strategist.md`**: Specialized commodity quant strategist focusing on metals, energy, and agriculture markets.
- **`expert-data-engineer.md`**: Expert Data Engineer specializing in EDA, SQL optimization, BI views, and data pipelines.
- **`expert-engineer.md`**: Senior Software Engineer handling production code, system design, and Docker/infra configuration across languages like Python, Next.js, and Java.
- **`india-market-researcher.md`**: Focused researcher for NSE/BSE equity research, SEBI/RBI policy analysis, and F&O strategies in India.
- **`market-researcher.md`**: Researcher for US equities, focusing on technical and fundamental analysis, Fed policy, and macro outlook.
- **`market-risk-analyzer.md`**: Specialist in VaR calculation, stress testing, hedging strategy design, and overall portfolio risk profiling.
- **`swift-ios-expert.md`**: iOS and Swift expert dealing with Apple platform development.
- **`us-expert-trader.md`**: Specialized in US market trade execution strategy, sizing, options plays, and algorithm design.

### 3. `skills/`
This folder contains highly specialized execution directory mappings for each agent defined in the `agents/` directory. Each subfolder (e.g., `ai-engineer`, `expert-engineer`, etc.) contains detailed instruction sets (`SKILL.md`) that provide step-by-step methodologies on how to perform their particular tasks using their corresponding tools effectively.

Each agent has a corresponding skill directory here that enables them with domain-specific tools or precise instructions.
