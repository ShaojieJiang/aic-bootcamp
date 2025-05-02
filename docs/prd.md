# Product Requirements Document  
**Product**: **AIC Flow – Visual Workflow Automation Platform**  
**Version**: 0.1 
**Author**: ChatGPT, Shaojie Jiang
**Date**: 1 May 2025  
**Reviewers**: ChatGPT, Shaojie Jiang  

| Rev | Date       | Author        | Notes                       |
|-----|------------|---------------|-----------------------------|
|0.1  |1 May 2025 |ChatGPT   |Initial skeleton             |

---

## 1 · Overview  

### 1.1 Problem Statement  
Technical and non-technical users struggle to connect heterogeneous data sources, AI agents, and third-party services without writing glue code.
Existing tools are either code-first (expensive to build, deploy, and maintain, like LangGraph) or lack of AI-centric features (like n8n).
Especially, there is no tool that is both easy to use and extensible for developers.

### 1.2 Goal  
Provide a **drag-and-drop, browser-based workflow builder** that lets users design, test, and run complex automations (including AI-centric tasks) in minutes, while keeping the platform extensible for developers.
Benefiting from the joint force of LangGraph and React Flow, AIC Flow is designed to be both easy to use for majority of routine tasks (low-code) and capable of tasks with much higher complexity (code-first).
With first-class support for Python, AIC Flow is also designed to be compatible with the large ecosystem of ML/AI libraries.

### 1.3 Scope
* Fundamental node types (see functional requirements for details)
* Credential vault
* Configuration versioning (git-like)

### 1.4 Non-scope for v1
* All nodes not listed above
* JS code node
* JS backend
* AI-assisted node or workflow generation
* Plugin marketplace
* Mobile app
* Enterprise SSO
* Native mobile authoring application
* Multi-tenant admin panel for Enterprise SSO
* HIPAA / FedRAMP compliance
* In-product AI-assisted node suggestion (planned as future enhancement)

---

## 2 · Assumptions & Constraints  

* Higher requirements for developers as frontend uses React and TypeScript, backend uses Python, and both are needed when building custom nodes.
* SPA built on **React 18+, React Flow, TypeScript**.  
* FastAPI backend, execution engine relies on **LangGraph** and **Celery** workers.  

---

## 3 · User Personas & Key Use Cases  

| Persona | Need (User Story) |
|---------|-------------------|
|**Business Analyst (Beth)**|"As a business analyst, I want to use the drag-and-drop interface to create automated reports by connecting to our CRM and email system, so I can send weekly performance summaries without writing code."|
|**Marketing Manager (Maya)**|"As a marketing manager, I want to use pre-built connectors to automatically sync customer data between our email platform and CRM, so I can maintain accurate campaign lists."|
|**Ops Engineer (Olivia)**|"As an ops engineer, I want to use the scheduling and monitoring features to run nightly ETL pipelines, so I can ensure data is ready for BI dashboards by 6 AM."|
|**Data Scientist (Diego)**|"As a data scientist, I want to leverage the AI node library and webhook triggers to build LLM-based enrichment workflows, so I can classify inbound tickets automatically."|
|**Automation Consultant (Arun)**|"As a consultant, I want to use the workflow packaging and marketplace features to create and share reusable sub-workflows with clients."|
|**Backend Developer (Bao)**|"As a developer, I want to use the Python SDK to create custom nodes that integrate with our proprietary services."|
|**ML Engineer (Ming)**|"As an ML engineer, I want to use the advanced workflow features and custom node capabilities to build complex AI pipelines with evaluation loops."|

---

## 4 · Functional Requirements  

### 4.1 Workflow Editor  
* Drag-and-drop canvas (pan, zoom, grid-snap).  
* Real-time validation (type, dangling edges).  
* Undo / redo (≥ 20 steps).  
* Multi-select & inline node search.  
* Mini-map for large flows.
* Directories for organizing workflows

### 4.2 Fundamental Node Types  
* **Data Sources**:
  * REST API endpoints
  * Database queries
  * Webhook triggers
  * Cron schedules
* **Data Sinks**:
  * REST API calls
  * File exports
  * Notifications
* **Processing**:
  * Python code blocks
  * Data transformations
  * Agent nodes
* **Control Flow**:
  * If-else conditions
  * For-each loops
  * While loops
* **Integration**:
  * Sub-workflow calls
  * Chat message handling
* **Custom**: user-packaged plugin nodes (signed)

### 4.3 Workflow Management  
* Save, duplicate, import/export (JSON) with semantic version tags.  
* Template gallery with rating & download counts.  
* Test-run mode with breakpoint & variable inspector.  
* Variable / expression editor (Python syntax).  
* Run history with logs, metrics, and traces

### 4.4 Execution Engine  
* LangGraph-based graph runner with parallel & conditional branches.  
* At-least-once execution semantics; configurable retries & compensating error-flows.  
* Execution logs streamed via WebSocket.  
* Metrics: node-level latency, throughput, error codes.

### 4.5 Integrations & Plugins  
* Built-in connectors: HTTP, PostgreSQL/MySQL, S3, Google Sheets, Slack, OpenAI.  
* OAuth 2 credential vault with role-based access.  
* Plugin SDK (Python) with marketplace publishing pipeline.  

### 4.6 Community Hub  
* Discover, vote, and comment on community nodes.  
* Contributor leaderboard & moderation workflow.  

---

## 5 · Non-Functional Requirements  

| Category | Requirement |
|----------|-------------|
|Performance|Editor interactions ≤ 200 ms (p95); engine throughput ≥ 50 nodes/s per worker.|
|Scalability|Horizontal autoscaling of Celery workers; support 1 k concurrent executions with < 5 s queuing delay.|
|Availability|99.9 % uptime (monthly) excluding scheduled maintenance.|
|Security|OWASP Top 10 compliance; AES-256 key storage; SOC 2 road-map.|
|Compliance|GDPR DPA.|
|Observability|OpenTelemetry traces; Prometheus metrics dashboard.|
|Internationalization|English UI v1; i18n-ready string catalog.|

---

## 6 · UX / UI Requirements  

* **Design language**: system-agnostic light/dark mode, accessible color palette (WCAG 2.1 AA).  
* **Wireframes**: Editor canvas, node inspector, execution console (Figma).  
* **Empty-state onboarding**: 3-step guided tour & "Create Demo Flow".  
* Keyboard shortcuts reference drawer.  

---

## 7 · Success Metrics / KPIs

### 7.1 Community Growth
* GitHub stars: 1k+ within 6 months
* Active contributors: 50+ monthly
* Community PRs merged: 20% of total PRs
* Discord/Slack members: 2k+ within 6 months

### 7.2 Product Adoption
* Active workflows: 1k+ within 6 months
* Workflow executions: 10k+ monthly
* Node usage distribution: No single node > 40% of total usage
* Template downloads: 500+ monthly

### 7.3 Technical Health
* Test coverage: > 80%
* CI/CD pipeline success rate: > 95%
* Average response time: < 200ms
* Critical bug resolution: < 24 hours

### 7.4 Commercial Indicators
* Enterprise inquiries: 10+ monthly
* Self-hosted deployments: 50+ within 6 months
* Community to paid conversion: > 5%
* Average revenue per user (ARPU): $50/month

### 7.5 User Satisfaction
* NPS score: > 40
* Documentation page views: 10k+ monthly
* Feature request upvotes: 100+ per quarter
* User retention: > 60% after 3 months

---

## 8 · Dependencies  

| Layer | Dependency | Comment |
|-------|------------|---------|
|Frontend|`@xyflow/react`, React 18, Vite, ESLint|Visualization & build tooling|
|Backend|FastAPI, PydanticAI, LangGraph, Celery, Redis, PostgreSQL|Core API & engine|
|DevOps|Docker, Kubernetes, Helm, OpenTelemetry|Deployment & monitoring|
|Auth|OAuth2 (Auth0) |SSO & credential vault|

---

## 9 · Milestones & Timeline  

| Phase (Duration) | Key Deliverables | Gate / Exit Criteria |
|------------------|------------------|----------------------|
|**MVP** (May – Jun 2025)  |Editor core, 6 node types, single-worker engine, manual deploy|Team demo: build/run sample flow in ≤ 15 min|
|**Beta** (Jul – Sep 2025) |Versioned workflows, template gallery, marketplace read-only, 10 built-in integrations|500 external beta users; error rate < 5 %|
|**GA v1.0** (Oct – Dec 2025)|Full feature set, HA engine, plugin publish flow, RBAC|Uptime ≥ 99.9 %, pass security pen-test|
|**Post-GA** (+) |AI-assisted builder, team collaboration, mobile viewer|Road-map updated Q1 2026|

---

## 10 · Risks & Mitigations  

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|


---

## 11 · Appendices  

* **A.** Figma wireframes link  

---

### Living Document  

This PRD is a **living, version-controlled artifact**. Updates require change-log entries and reviewer sign-off. Use Slack channel `#prd-aic-flow` for discussions; major decisions captured in the document history table above.
