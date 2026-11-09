
**Project:** Omni-market Research & Autonomous Learning Engine  
**Version:** 1.0.0  
**Status:** Research Initiated  
**Date:** 09 November 2026  
**Development Mode:** Research Prototype  
**Architecture:** Local-first · Hybrid AI · Multi-market  
**Trading Mode:** Semi-autonomous

---

## 1. Project Overview

ORACLE is a modular AI-assisted trading research system designed to collect, process, analyze, and interpret financial market data alongside global political and economic information.

The system combines conventional quantitative analysis, local AI models, and cloud-based Large Language Models to generate structured trading proposals.

ORACLE does not aim to predict financial markets with certainty. Its objective is to evaluate available evidence, identify potential opportunities, assess uncertainty, and support disciplined trading decisions.

### 1.1 Core Philosophy

> The AI observes. The system analyzes. The human approves. The engine executes.

### 1.2 Primary Objectives

- Collect financial market data from multiple markets.
    
- Monitor global political and economic developments.
    
- Analyze technical and fundamental market conditions.
    
- Generate explainable trading proposals.
    
- Minimize LLM token consumption and operational costs.
    
- Maintain independent risk-management controls.
    
- Support human-reviewed paper trading.
    

### 1.3 Non-objectives

The initial release will not:

- Guarantee profitable trading.
    
- Execute trades without human approval.
    
- Manage real-money accounts.
    
- Train proprietary foundation models.
    
- Operate as a high-frequency trading system.
    
- Depend on a single financial data provider.
    

---

## 2. Architecture Decisions

|Decision|Selected approach|
|---|---|
|Deployment|Local-first|
|AI architecture|Hybrid AI|
|Market coverage|Multi-market|
|Trading integration|MetaTrader 5 + TradingView|
|Execution authorization|Human approval required|
|Initial trading environment|Paper trading / demo|
|Backend language|Python|
|API framework|FastAPI|
|Primary database|PostgreSQL|
|Research dashboard|Streamlit|
|Testing framework|Pytest|

### 2.1 Local-first Deployment

The primary application runs on a personal computer.

Local processing is responsible for:

- Market data normalization.
    
- Technical indicator calculations.
    
- News filtering and deduplication.
    
- Local inference.
    
- Database operations.
    
- Research and backtesting.
    

External services are accessed only when required.

### 2.2 Hybrid AI

Local models handle relatively inexpensive and repetitive tasks.

Cloud models handle tasks requiring more complex reasoning.

The system must support model-provider abstraction so that providers and models can be replaced without rewriting business logic.

### 2.3 Trading Integration

**MetaTrader 5**

- Primary broker-side market data integration.
    
- Account and symbol information in demo environments.
    
- Paper execution and future execution adapter.
    

**TradingView**

- Technical alert ingestion.
    
- Webhook-based signal delivery.
    
- Supplementary market observations.
    

TradingView alerts are not automatically treated as trade authorization.

### 2.4 Multi-market Support

Initial research scope:

- Forex: EURUSD, GBPUSD, USDJPY.
    
- Commodities: XAUUSD, WTI.
    
- Indices: S&P 500, NASDAQ 100.
    
- Cryptocurrency: BTCUSD, ETHUSD.
    
- Equities: selected large-cap stocks.
    

The initial implementation will prioritize XAUUSD and EURUSD before expanding coverage.

---

## 3. Functional Requirements

|ID|Requirement|Priority|
|---|---|---|
|FR-001|Retrieve market data|Critical|
|FR-002|Normalize multi-market data|Critical|
|FR-003|Collect economic and political news|Critical|
|FR-004|Detect and classify relevant events|High|
|FR-005|Calculate technical indicators|Critical|
|FR-006|Generate fundamental analysis|High|
|FR-007|Generate structured trade proposals|Critical|
|FR-008|Calculate risk parameters|Critical|
|FR-009|Require human approval|Critical|
|FR-010|Support MT5 integration|High|
|FR-011|Support TradingView webhook ingestion|High|
|FR-012|Record historical decisions|Critical|
|FR-013|Track token usage and costs|High|
|FR-014|Support backtesting|Critical|
|FR-015|Provide research dashboard|Medium|

---

## 4. Non-functional Requirements

### Performance

- Local preprocessing should avoid unnecessary network calls.
    
- Webhook ingestion should acknowledge valid requests quickly.
    
- Data processing must be asynchronous where appropriate.
    
- LLM requests must support configurable timeouts and retries.
    

### Reliability

- Every event must have a timestamp and source.
    
- Missing or stale data must be detectable.
    
- Failed requests must be logged.
    
- Duplicate events must be identifiable.
    
- AI failures must not bypass risk controls.
    

### Security

- API credentials must remain outside source control.
    
- Broker credentials must never be exposed to LLMs.
    
- Webhook endpoints must be authenticated.
    
- Human approval must be enforced server-side.
    
- Trade proposals must be immutable once approved, with modifications requiring renewed approval.
    

### Explainability

Every trade proposal must include:

- Supporting evidence.
    
- Contradictory evidence.
    
- Data timestamps.
    
- Relevant market conditions.
    
- Invalidation conditions.
    
- Risk assessment.
    
- Model and prompt version.
    

---

## 5. System Modules

|Module|Responsibility|
|---|---|
|ingestion|External data collection|
|normalization|Standardized financial data|
|intelligence|News and macroeconomic analysis|
|analysis|Technical and quantitative analysis|
|decision|Signal aggregation and trade thesis|
|risk|Independent risk validation|
|execution|Broker and trading-platform adapters|
|storage|Persistent data management|
|monitoring|Logs, metrics and cost tracking|

### 5.1 Intelligence Engine

The Intelligence Engine processes political and economic events.

Each event must contain:

- Event identifier.
    
- Publication timestamp.
    
- Source.
    
- Country or region.
    
- Event category.
    
- Affected assets.
    
- Actual, forecast and previous values where applicable.
    
- Reliability assessment.
    
- Relevance score.
    

### 5.2 Decision Engine

The Decision Engine supports four outputs:

- BUY
    
- SELL
    
- HOLD
    
- ABSTAIN
    

ABSTAIN must be available whenever evidence is insufficient, conflicting, outdated, or unreliable.

### 5.3 Risk Engine

The Risk Engine must independently validate:

- Position sizing.
    
- Stop-loss constraints.
    
- Exposure limits.
    
- Maximum drawdown rules.
    
- Available margin.
    
- Spread and slippage conditions.
    

The AI cannot override risk restrictions.

---

## 6. AI and Token Optimization

### 6.1 Processing Strategy

1. Collect raw data.
    
2. Normalize and deduplicate.
    
3. Apply deterministic filtering.
    
4. Calculate quantitative features locally.
    
5. Compress relevant context.
    
6. Invoke local models where appropriate.
    
7. Invoke cloud models for complex reasoning.
    
8. Validate structured outputs.
    
9. Store results and usage metrics.
    

### 6.2 Model Responsibilities

**Local AI**

- Event classification.
    
- Basic sentiment extraction.
    
- News categorization.
    
- Structured information extraction.
    
- Preliminary relevance scoring.
    

**Cloud AI**

- Complex macroeconomic reasoning.
    
- Multi-event relationship analysis.
    
- Scenario evaluation.
    
- Contradictory evidence analysis.
    
- Trade thesis generation.
    

### 6.3 Cost Metrics

The system must record:

- Input tokens.
    
- Output tokens.
    
- Cached input tokens.
    
- Number of model invocations.
    
- Cost per analysis.
    
- Cost per asset.
    
- Cost per day.
    
- Cost per accepted proposal.
    

Optimization must preserve analytical quality rather than simply minimize token usage.

---

## 7. Data Strategy

The project will prioritize official or primary sources.

Initial candidates:

- MetaTrader 5 broker-provided market data.
    
- FRED and ALFRED economic datasets.
    
- U.S. Bureau of Labor Statistics.
    
- CFTC Commitments of Traders.
    
- SEC EDGAR.
    
- TradingView alerts.
    
- Public cryptocurrency exchange APIs.
    

Every provider must undergo evaluation for:

- Data coverage.
    
- Historical availability.
    
- Latency.
    
- Update frequency.
    
- API limits.
    
- Licensing.
    
- Cost.
    
- Reliability.
    

Provider selection remains provisional until validation is complete.

---

## 8. Research Methodology

The project will follow an evidence-driven development process.

### Data validation

Check missing values, duplicates, timezone consistency, stale records, and historical revisions.

### AI validation

Evaluate event extraction accuracy, reasoning consistency, hallucinations, source attribution, and abstention behavior.

### Strategy validation

Use chronological data splits, historical simulation, walk-forward validation, and realistic transaction costs.

### Execution validation

Test order rejection, stale proposals, duplicate requests, network failures, and approval-state enforcement.

### Cost validation

Compare local and cloud model performance against token consumption and actual operating costs.

---

## 9. Development Roadmap

|Phase|Scope|Deliverable|
|---|---|---|
|0|Research and feasibility|Research Specification|
|1|Market data infrastructure|Market Data Engine|
|2|World Intelligence|Intelligence Engine|
|3|Decision and Risk|Decision Engine|
|4|Backtesting and paper trading|Evaluation Report|
|5|Dashboard and integration|Research Prototype v1.0|

Estimated duration: 10–14 weeks, subject to research results and hardware availability.

---

## 10. Acceptance Criteria

ORACLE Research Prototype v1.0 is considered ready for research evaluation when:

- Market data can be retrieved and normalized.
    
- At least two initial asset classes are supported.
    
- Economic and news events can be collected and classified.
    
- Local and cloud AI providers can be switched through configuration.
    
- Token usage and estimated cost are recorded.
    
- Trade proposals follow a validated schema.
    
- BUY, SELL, HOLD and ABSTAIN are supported.
    
- Risk checks operate independently of AI.
    
- Human approval is mandatory.
    
- Paper execution is supported.
    
- Backtesting produces reproducible results.
    
- Decision history and audit logs are available.
    

Passing these criteria does not establish profitability or authorize live trading.

---

## 11. Phase 0 Research Questions

|ID|Research question|Status|
|---|---|---|
|RQ-01|Which market data providers are suitable?|In progress|
|RQ-02|Which news and economic sources are reliable?|In progress|
|RQ-03|How should multi-market data be normalized?|Pending|
|RQ-04|Which LLM providers offer suitable cost/performance?|In progress|
|RQ-05|How can token usage be minimized?|In progress|
|RQ-06|How should AI reliability be evaluated?|Pending|
|RQ-07|Which backtesting methodology is appropriate?|Pending|
|RQ-08|How should human approval and risk controls work?|In progress|

---

## 12. Current Project Status

**Research Specification:** v1.0 completed.

**Phase 0:** Initiated.

**Implementation:** Not started.

**Live trading:** Out of scope.

**Next milestone:** Complete the feasibility study and validate the initial technology and data-provider selections.

---

_End of Research Specification v1.0._