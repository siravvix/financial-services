# Claude for Financial Services

Reference agents, skills, and data connectors for the financial-services workflows we see most — investment banking, equity research, private equity, and wealth management.

Everything here is available **two ways from one source**: install it as a [Claude Cowork](https://claude.com/product/cowork) plugin, or deploy it through the [Claude Managed Agents API](https://docs.claude.com/en/api/managed-agents) behind your own workflow engine. Same system prompt, same skills — you choose where it runs.

> [!IMPORTANT]
> Nothing in this repository constitutes investment, legal, tax, or accounting advice. These agents draft analyst work product — models, memos, research notes, reconciliations — for review by a qualified professional. They do not make investment recommendations, execute transactions, bind risk, post to a ledger, or approve onboarding; every output is staged for human sign-off. You are responsible for verifying outputs and for compliance with the laws and regulations that apply to your firm.

> [!NOTE]
> **Personal fork** — I'm using this primarily to explore the Earnings Reviewer and Model Builder agents for equity research workflows. The upstream repo is at [anthropics/financial-services](https://github.com/anthropics/financial-services).
>
> **My focus areas:** Earnings Reviewer (testing against tech sector transcripts) and Model Builder (3-statement + DCF). I'm not actively using the fund admin / finance ops agents.
>
> **Test tickers I've been using:** MSFT, GOOGL, META, NVDA — mostly large-cap tech where transcript quality is high and there's plenty of historical data to validate model outputs against. Also starting to test AAPL for services segment breakout modeling.
>
> **Notes from testing:** NVDA earnings transcripts tend to trip up the Earnings Reviewer on guidance language — the forward-looking statements are unusually hedged. Worth flagging if you're using this for semis coverage.
>
> **Model Builder defaults I've changed locally:** DCF terminal growth rate default nudged from 3.0% → 2.5% and WACC floor raised to 8% — felt more conservative and appropriate for the current rate environment. See `plugins/vertical-plugins/equity-research/model-builder/config.yaml`.

What's in the repo:

- **[Agents](#agents)** — named, end-to-end workflow agents (Pitch Agent, Market Researcher, GL Reconciler, …). Each ships as a Cowork plugin **and** as a [Claude Managed Agent template](./managed-agent-cookbooks) you deploy via `/v1/agents`.
- **[Vertical plugins](#vertical-plugins)** — the underlying skills, slash commands, and data connectors, bundled by FSI vertical. Install these on their own if you just want `/comps`, `/dcf`, `/earnings` and the connectors without a full agent.

## Agents

Each agent is named for the workflow it runs. They're starting points: install the ones that match your work, then tune the prompts, skills, and connectors to how your firm does it.

Each agent plugin is **self-contained** — it bundles the skills it uses, so installing the agent is all you need.

| Function | Agent | What it does |
|---|---|---|
| **Coverage & advisory** | **[Pitch Agent](./plugins/agent-plugins/pitch-agent)** | Comps, precedents, LBO → branded pitch deck, end to end |
| | **[Meeting Prep Agent](./plugins/agent-plugins/meeting-prep-agent)** | Briefing pac