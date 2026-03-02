# Welcome to **Warmplane**

We are dedicated to solving the critical bottlenecks of agentic tool use, specifically the inefficiencies of raw Model Context Protocol (MCP) connectivity. Our mission is to move the AI industry from basic "tool connectivity" to "production-grade tool economics".

## The Problem: MCP Context Bloat
Direct MCP connectivity is excellent for initial compatibility, but it introduces massive overhead for repeated agent loops. Most agent stacks overpay in latency and tokens by eagerly surfacing massive tool catalogs and detailed metadata that are never actually used. 

We refer to this structural failure as the **"House-Guest Benchmark"**: trapping a model with a 30-minute monologue of schemas before asking it to execute a single task, which often causes the model to hallucinate or lose the plot. If a human guest would call your onboarding process rambling, your model definitively experiences it as context bloat.

## Our Repositories & Research

### 1. `warmplane` (Core Runtime)
**Warmplane** is the local control plane that keeps MCP sessions warm. It runs multiple upstream MCP servers behind one local process, ensuring sessions remain persistent. 

Instead of dumping everything up front, Warmplane acts as the "polite host" by shifting the model to a compact, lazy interaction surface. Agents discover compact indexes first, fetch detailed schemas only when needed, and execute via normalized envelopes. 

**Key Features:**
*   **Three Access Modes:** Operates across HTTP daemon (`/v1/...`), CLI facade commands, and an MCP server mode (`stdio`) for MCP-native clients.
*   **Deterministic Orchestration:** Normalizes timeout and error envelopes across all tools, resources, and prompts.
*   **Centralized Control:** Offers robust policy checks, redaction controls, and aliasing to shield agents from backend MCP server churn.

### 2. Articles & Research on Context Bloat
We host extensive research on the impact of context bloat and the operational gains of using a compact capability plane. 

**Token Efficiency Evals**
By keeping MCP richness in the backend and serving a thin facade, token savings shift from a hypothetical idea to extreme, measured realities. Our evaluation harness compares raw MCP payloads against Warmplane's facade:
*   **GitHub Copilot MCP (authenticated):** Achieved **95.6% to 95.8% token savings**, dropping a 10-turn mixed loop from 547,150 tokens to just 22,895 tokens.
*   **Public Filesystem MCP (control):** Maintained consistent savings of **58.1% to 58.2%** across multi-turn loops.

**The HumanMCP Proof**
Recent independent research, such as the *HumanMCP* study, empirically validates Warmplane's core design philosophy. Eagerly loading tools directly harms model reasoning:
*   When scaling the context from 10 to 100 tools, efficiency-optimized models like Gemini 2.0 Flash, GPT-4o-mini, and Claude 3.5 Haiku suffer a ~10% degradation in retrieval accuracy.
*   In massive environments, Gemini 2.0 Flash's accuracy plummets from 87.4% (at 500 tools) to 65% (at 2,000 tools) due to high-noise interference. 
*   However, filtering candidates down to a top-10 shortlist using retrieval (like a SentenceTransformer) boosts Gemini's final accuracy back up to 76%—an 11-percentage-point improvement. This proves that narrowing the search space to a manageable size is required for highly accurate LLM selection, perfectly mirroring Warmplane's "index first, detail on demand" approach.

---
**Get Started**
To see the architecture, API semantics, and token research, check out the [`warmplane` user guide](https://github.com/Warmplane/warmplane/blob/main/docs/USER-GUIDE.md)). Build the runtime locally and start reducing your agent's startup latency and payload size today.
