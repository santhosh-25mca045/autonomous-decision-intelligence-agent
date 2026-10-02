# 🧠 Autonomous Decision Intelligence Agent

An AI agent built in a Jupyter Notebook that **perceives** a business situation, **evaluates** options under uncertainty, **decides** within safety guardrails, **acts**, **learns** from outcomes, and **explains** every decision in plain language.

## Scenario

A retail operator makes a daily decision: **hold price, discount, raise price, or restock (small / large)**. Demand is seasonal and noisy, and the agent does not know the true demand model, so it must learn from experience.

## Agent loop

```
Perceive → Contextualize → Guardrails → Evaluate options → Decide → Act → Observe reward → Learn → Explain
```

| Capability | Technique |
|---|---|
| Perception | Observation → discretized context (demand regime × stock level) |
| Decision under uncertainty | Thompson sampling (Bayesian exploration vs. exploitation) |
| Risk awareness | Risk-adjusted score = sampled mean − λ · std |
| Safety | Rule-based guardrails that block unsafe actions |
| Learning | Online mean/variance (Welford) per (context, action) |
| Explainability | Natural-language rationale + confidence for each decision |
| Evaluation | Benchmark vs. random / always-hold / rule-based baselines over 10 seeds |

## Results (10 seeds, 300 days each)

| Policy | Mean total profit |
|---|---|
| Rule-based | ~219k |
| **Decision Agent** | ~192k |
| Random | ~93k |
| Always hold | ~−93k |

The agent clearly beats random and fixed policies. A hand-written rule still edges it out, because the agent treats each day independently and does not model multi-day effects such as inventory carry-over. See *Future work*.

## Project structure

```
├── Autonomous_Decision_Intelligence_Agent.ipynb   # Main notebook (with outputs)
├── decision_agent_code.py                         # Same code as a plain Python script
├── requirements.txt
└── README.md
```

## Getting started

**1. Clone the repo**
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```
On Windows, if `pip` is not recognized, use `py -m pip install -r requirements.txt`.

**3. Run**

- Notebook: `jupyter notebook`, then open the `.ipynb` file and choose **Run → Run All Cells**
- Script: `python decision_agent_code.py` (on Windows: `py decision_agent_code.py`)

No API key is needed. Everything runs offline.

## Optional: LLM narration

The last section can turn the agent's audit trail into an executive summary using the Anthropic API.

```bash
pip install anthropic
export ANTHROPIC_API_KEY=your_key_here    # Windows PowerShell: $env:ANTHROPIC_API_KEY="your_key_here"
```

Never commit your API key to the repository. The agent's decisions do not depend on this section.

## Future work

- Replace the simulated environment with real data (CSV / database / API) behind the same `observe()` / `step()` interface
- Upgrade to multi-day reinforcement learning (Q-learning, PPO) to capture inventory carry-over
- Add demand forecasting (Prophet / ARIMA) to enrich observations
- Add human-in-the-loop approval for low-confidence or high-impact actions
- Log decisions to a database for governance and audit

## Tech stack

Python · NumPy · pandas · Matplotlib · Jupyter
