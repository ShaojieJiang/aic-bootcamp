# Product Requirements Document  
**Product**: **AIC Flow – Visual Workflow Automation Platform**  
**Version**: 0.1 (Draft)  
**Author**: ChatGPT
**Date**: 1 May 2025  
**Reviewers**: Shaojie Jiang  

| Rev | Date       | Author        | Notes                       |
|-----|------------|---------------|-----------------------------|
|0.1  |1 May 2025 |ChatGPT   |Initial skeleton             |

---

## 1 · Overview  

### 1.1 Problem Statement  
Technical and non-technical users struggle to connect heterogeneous data sources, AI agents, and third-party services without writing glue code. Existing tools are either code-first (steep learning curve) or single-purpose SaaS (limited flexibility).

### 1.2 Goal  
Provide a **drag-and-drop, browser-based workflow builder** that lets users design, test, and run complex automations (including AI-centric tasks) in minutes, while keeping the platform extensible for developers.

### 1.3 Success Criteria (KPIs)  
| Objective | Metric | Target @ 90 days post-GA |
|-----------|--------|---------------------------|
|Reduce workflow build time|Median time to first successful run| ≤ 15 min|
|Reliability|Execution success rate| ≥ 98 %|
|Supportability|Mean error-resolution time| ≤ 10 min|
|Adoption|Weekly active builders| ≥ 500|
|Performance|Median workflow throughput| ≥ 50 nodes / sec|

---

## 2 · Assumptions & Constraints  

* SPA built on **React 18+, React Flow, TypeScript**.  
* Cloud-hosted FastAPI backend (Python 3.12); on-prem deployment is *out of scope* for v1.  
* Execution engine relies on **LangGraph** and **Celery** workers.  
* Early adopters familiar with low-code paradigms.  
* GDPR compliance required; HIPAA is *not* in scope for v1.  

---

## 3 · User Personas & Key Use Cases  

| Persona | Need (User Story) |
|---------|-------------------|
|**Ops Engineer (Olivia)**|“As an ops engineer, I want to schedule nightly ETL pipelines so that data is ready for BI dashboards by 6 AM.”|
|**Data Scientist (Diego)**|“As a data scientist, I want to trigger an LLM-based enrichment step when a webhook fires, so that I can classify inbound tickets automatically.”|
|**Automation Consultant (Arun)**|“As a consultant, I want to package reusable sub-workflows and share them with clients from a marketplace.”|
|**Backend Developer (Bao)**|“As a developer, I want to extend the platform with custom Python nodes, so that I can call proprietary services.”|

---

## 4 · Functional Requirements  

### 4.1 Workflow Editor  
* Drag-and-drop canvas (pan, zoom, grid-snap).  
* Real-time validation (type, dangling edges).  
* Undo / redo (≥ 20 steps).  
* Multi-select & inline node search.  
* Mini-map for large flows.

### 4.2 Node Types  
* **Input**: REST source, DB query, webhook, cron.  
* **Processing**: code block, transformation, LLM prompt, loop, condition, sub-workflow.  
* **Output**: REST sink, file export, notification.  
* **Custom**: user-packaged plugin nodes (signed).  

### 4.3 Workflow Management  
* Save, duplicate, import/export (JSON) with semantic version tags.  
* Template gallery with rating & download counts.  
* Test-run mode with breakpoint & variable inspector.  
* Variable / expression editor (JS syntax).  

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
|Compliance|GDPR DPA, data residency in EU region.|
|Observability|OpenTelemetry traces; Prometheus metrics dashboard.|
|Internationalization|English UI v1; i18n-ready string catalog.|

---

## 6 · UX / UI Requirements  

* **Design language**: system-agnostic light/dark mode, accessible color palette (WCAG 2.1 AA).  
* **Wireframes**: Editor canvas, node inspector, execution console (see Figma link in Appendix).  
* **Empty-state onboarding**: 3-step guided tour & “Create Demo Flow”.  
* Keyboard shortcuts reference drawer.  

---

## 7 · Out of Scope (v1)  

* Native mobile authoring application.  
* Multi-tenant admin panel for Enterprise SSO.  
* HIPAA / FedRAMP compliance.  
* In-product AI-assisted node suggestion (planned as future enhancement).  

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
|React Flow major API change|Medium|High|Pin version; fork if needed|
|AI-related compliance updates|Low|High|Quarterly legal review|
|Marketplace abuse / malicious plugins|Medium|Medium|Code-signing, manual review|

---

## 11 · Appendices  

* **A.** Figma wireframes link  
* **B.** API schema (OpenAPI 3)  
* **C.** Plugin SDK guide  

---

### Living Document  

This PRD is a **living, version-controlled artifact**. Updates require change-log entries and reviewer sign-off. Use Slack channel `#prd-aic-flow` for discussions; major decisions captured in the document history table above.
