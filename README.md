# Agentic GCP Development Guide

This guide provides a comprehensive architectural breakdown of the 2026 Google Cloud Platform (GCP) agentic ecosystem. It categorizes the tools into Core Agent Platforms, Governance & Security, Runtime & Knowledge, Ecosystem Protocols, and Supporting Infrastructure. Each entry details the tool's relevance, its position in the ecosystem, and its ideal use cases.

## 1. Core Agent Development & Platforms

These tools serve as the primary environments and frameworks for designing, building, testing, and prototyping AI agents.

### Gemini Enterprise Agent Platform (formerly Vertex AI & Vertex AI Agent Builder)
* **Relevance & Place:** The overarching enterprise platform for agentic development. In April 2026, Google consolidated its agent tools under this umbrella, which now houses the low-code Agent Designer, the open-source ADK, and the managed Agent Runtime.
* **Best Suited For:** End-to-end enterprise AI architecture, providing a unified space to build, deploy, and govern production-grade agents.
* **Not For:** Ungoverned local hobbyist projects.

### Antigravity (App, CLI, SDK, IDE, Extensions)
* **Relevance & Place:** Google's dedicated developer platform tailored for the "agent-first" era. It acts as a comprehensive suite providing a command center for managing multiple local agents in parallel (Antigravity 2.0 App), a terminal-first execution surface (CLI), a rapid prototyping Python SDK, and a fully-featured agentic IDE with deep codebase understanding.
* **Best Suited For:** Developers (frontend, full-stack, enterprise) who want a complete end-to-end local and cloud-connected environment to build autonomous coding agents and streamline development.
* **Not For:** Non-technical business users or simple conversational chatbots.

### Agent Development Kit (ADK)
* **Relevance & Place:** An open-source, code-first agent development framework available in Python, TypeScript, Go, and Java. It is the standard SDK for defining agent trajectories, tool access, and dynamic models.
* **Best Suited For:** Developers building complex, custom autonomous agents that require deep programmatic control and multi-agent orchestration before deploying to GCP.
* **Not For:** Drag-and-drop conversational agent building.

### Agent Designer (formerly Customer Experience Agent Studio)
* **Relevance & Place:** A comprehensive visual, low-code development platform housed within the Gemini Enterprise Agent Platform. 
* **Best Suited For:** Rapid prototyping and allowing non-technical business users or CX teams to build multimodal support agents using visual workflows.
* **Not For:** Deep infrastructure automation or complex backend data-processing loops.

### Agents CLI 
* **Relevance & Place:** The native command-line interface for the Agent Platform. It allows developers to interactively chat with, debug, and trace their ADK agents locally from the terminal before cloud deployment.
* **Best Suited For:** Rapid local testing of tool execution and conversational logic.

### Google AI Studio
* **Relevance & Place:** A lightweight, web-based prototyping environment for developers to experiment with Gemini models and prompt engineering.
* **Best Suited For:** Rapid prompt testing, few-shot prompt crafting, and initial model evaluation before writing code.
* **Not For:** Production deployment, stateful agent hosting, or complex enterprise governance.

---

## 2. Governance, Identity & Security (The 2026 Governance Stack)

As agentic systems scale, controlling what agents can do and who they are becomes critical. This suite manages agent trust, identity, and discovery.

### Agent Identity
* **Relevance & Place:** Makes agent identity a first-party platform feature. It assigns every agent a cryptographically attested identity aligned to the SPIFFE standard (backed by an auto-provisioned X.509 certificate). Google Cloud access tokens are cryptographically bound to this certificate.
* **Best Suited For:** Making the agent a native IAM principal. Action logs are attributed directly to the agent (rather than the developer's credentials), preventing token theft and enforcing least-privilege policies.

### Agent Registry
* **Relevance & Place:** The central internal source of truth for an enterprise's approved agents, tools, and skills. It stores A2A-compatible Agent Cards (JSON format), endpoints, and policy attachments.
* **Best Suited For:** Internal discovery of agent capabilities and standardizing which agents are approved for deployment within a tenant.

### Agent Gateway
* **Relevance & Place:** The governed runtime entry point. All traffic in and out of registered agents passes through the Gateway, where authentication, scoping, rate limits, and observability are enforced.
* **Best Suited For:** Enforcing strict enterprise security boundaries. The Registry decides *who* is approved; the Gateway decides *what they can do* per invocation.

### Model Armor
* **Relevance & Place:** A security service providing real-time inline protection. It proactively screens LLM prompts and responses to protect against risks like prompt injection, tool poisoning, and data leakage.
* **Best Suited For:** Sitting seamlessly between the agent (Gateway/Runtime) and the foundational model to enforce Responsible AI guardrails.

### Sensitive Data Protection
* **Relevance & Place:** GCP's DLP (Data Loss Prevention) service.
* **Best Suited For:** Masking, redacting, or tokenizing PII (Personally Identifiable Information) before it reaches the LLM.

### Skill Registry
* **Relevance & Place:** A repository that allows developers to dynamically search, discover, and fetch remote, reusable skills to attach to ADK-built agents.

---

## 3. Runtime & Knowledge Services

These tools dictate where agents physically run, how they maintain state, and how they retrieve enterprise knowledge.

### Agent Runtime (formerly Agent Engine)
* **Relevance & Place:** A managed infrastructure layer explicitly designed to run agents at global scale. It handles session management, code execution, and telemetry natively.
* **Best Suited For:** Deploying stateful ADK agents to production without building custom infrastructure.

### Memory Bank
* **Relevance & Place:** A persistent context storage system natively attached to the Agent Runtime.
* **Best Suited For:** Giving agents long-term memory across sessions. It eliminates the need to manually wire up external databases just so an agent can remember a user's preferences or previous conversations from days ago.

### Agent Search (formerly Vertex AI Search) & RAG Engine
* **Relevance & Place:** Managed enterprise search and Retrieval-Augmented Generation pipelines.
* **Best Suited For:** Grounding agents in proprietary data. Use Agent Search to connect unstructured data (PDFs, Intranets, BigQuery), and RAG Engine to automatically handle chunking and embedding.

### Model Garden
* **Relevance & Place:** A curated hub to discover, test, customize, and deploy Google and open-source foundational models.
* **Best Suited For:** Selecting the right base model (Gemini, Llama, Claude, etc.) for a specific agentic task.

---

## 4. Agentic Protocols & External Ecosystem

Protocols and curated hubs are necessary for multi-agent communication and standardizing tool use.

### Agentic Protocols (A2A, MCP)
* **Relevance & Place:** 
  * **A2A (Agent2Agent):** A standardized specification utilizing JSON "Agent Cards" that allows disparate agents to discover and collaborate with each other securely.
  * **MCP (Model Context Protocol):** An open standard enabling secure connections between your data sources (or APIs) and your agents.
* **Best Suited For:** Building multi-agent systems (A2A) or standardized data connectors (MCP) that avoid vendor lock-in.

### Agent Garden & Agent Gallery
* **Relevance & Place:** 
  * **Agent Garden:** Google-curated set of pre-built agent templates to accelerate development.
  * **Agent Gallery:** A discovery surface for third-party, partner-built enterprise agents that can be imported into your tenant's Agent Registry.

---

## 5. Core Supporting GCP Infrastructure

While not exclusively "agentic," these backend tools provide the required orchestration, testing, and persistence for scalable agent systems.

* **Agent Evaluation (Gen AI Evaluation Service):** Applies LLM-as-a-judge rubrics and deterministic metrics to test an agent's execution trajectories, tool-use accuracy, and reasoning before production launch.
* **Cloud Workflows & Cloud Tasks:** Essential for orchestrating long-running, durable agent trajectories (e.g., background data pipeline migrations) and managing queues that require Human-in-the-Loop (HITL) approvals.
* **Cloud Storage:** Acts as "unstructured memory" for multimodal agents to drop generated files, read large PDFs, and store audio/video artifacts.
* **Auth Manager (OAuth 2.0):** Manages the authorization handshakes required for agents to securely act on behalf of users in third-party systems.
* **Cloud Run & GKE:** Standard compute environments for workloads that require custom containers or fine-grained network control outside the managed Agent Runtime.
* **Google Cloud Observability:** Essential for distributed tracing. Cloud Trace maps the execution trajectory (reasoning process and tool selection paths), while Cloud Logging captures the distinct actions for auditing.
