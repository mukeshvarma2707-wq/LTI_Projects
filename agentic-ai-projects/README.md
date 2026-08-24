# Agentic AI Project Portfolio

## Project 1: FinTech — Market Manipulation Detection with Adversarial Verification

### 1. Problem Statement

Financial markets can experience abnormal price and volume movements caused by coordinated social-media activity, including potential pump-and-dump behavior. Traditional anomaly detection systems can identify unusual market activity, but they often struggle to distinguish genuine manipulation from legitimate events such as earnings announcements, product launches, or broader market movements.

The proposed system detects suspicious market events by correlating social-media activity with abnormal price and trading-volume behavior. Instead of immediately classifying an event as manipulation, the system uses an agentic adversarial verification architecture where one AI agent investigates evidence supporting manipulation and another actively searches for legitimate explanations. A final adjudication layer combines the evidence and produces an explainable verdict.

### 2. Objective

* Detect unusual combinations of social-media activity and market movements.
* Identify candidate pump-and-dump or coordinated manipulation events.
* Analyze financial sentiment using a finance-specific language model.
* Investigate suspicious events using an autonomous evidence-gathering agent.
* Generate an independent defense against false accusations.
* Compare supporting and opposing evidence before producing a final verdict.
* Provide evidence-backed explanations rather than an unexplained probability score.
* Evaluate the system against documented real-world cases and controlled synthetic cases.

### 3. System Overview

```text
Reddit / Social Media
        +
Market Price & Volume Data
        ↓
Data Collection & Preprocessing
        ↓
Social Mention Spike Detection
        ↓
FinBERT Sentiment Analysis
        ↓
Feature Engineering
        ↓
XGBoost / LightGBM
        ↓
Suspicious Event Detection
        ↓
┌─────────────────────────────────────┐
│        Agentic Investigation        │
│                                     │
│  ┌────────────┐   ┌─────────────┐ │
│  │ Prosecutor │   │   Defense   │ │
│  │   Agent    │   │    Agent    │ │
│  └─────┬──────┘   └──────┬──────┘ │
│        │                  │        │
│        └────────┬─────────┘        │
│                 ↓                  │
│             Evidence               │
└─────────────────┬───────────────────┘
                  ↓
             Judge / Adjudicator
                  ↓
        Final Verdict + Confidence
                  ↓
             Dashboard / API
```

The system follows a two-stage intelligence architecture.

The first stage is deterministic and ML-based. It identifies events that are statistically unusual.

The second stage is agentic. It investigates the event and determines whether the available evidence supports or contradicts the manipulation hypothesis.

### 4. Technical Pipeline

#### Data Collection

Collect:

* Reddit posts and comments
* ticker mentions
* timestamps
* price
* trading volume
* historical market data
* SEC filings
* financial news
* company announcements

#### Feature Engineering

Potential features include:

* social-media mention count
* mention-volume z-score
* sentiment score
* price-return percentage
* volume z-score
* price reversal magnitude
* lagged social/price correlation
* number of unique accounts
* posting-time concentration
* near-duplicate post ratio

#### Candidate Detection

A boosted-tree model such as XGBoost or LightGBM produces a suspicious-event score.

The model is not intended to prove manipulation. It acts as a **candidate generator**.

### 5. Model Explanation

#### FinBERT

FinBERT is used to analyze financial sentiment from Reddit posts and financial documents.

Input:

```text
"XYZ is going to explode after tomorrow's announcement."
```

Output:

```text
Positive: 0.91
Neutral: 0.07
Negative: 0.02
```

FinBERT is preferable to a generic sentiment model because financial language has domain-specific meanings.

#### XGBoost / LightGBM

The boosted-tree model combines structured signals.

Example:

```text
Mention spike       → 5.4σ
Volume spike        → 4.8σ
Price movement      → +18%
Price reversal      → 12%
Social correlation  → 0.76
```

The model produces:

```text
Suspicious event score = 0.89
```

This triggers agentic investigation.

#### Prosecutor Agent

The Prosecutor Agent attempts to establish the manipulation hypothesis.

It can use tools to:

* retrieve relevant posts
* inspect posting timestamps
* identify coordinated activity
* detect similar phrasing
* examine price/volume behavior
* retrieve historical events

#### Defense Agent

The Defense Agent is explicitly instructed to challenge the manipulation hypothesis.

It searches for:

* earnings announcements
* product announcements
* SEC filings
* legitimate financial news
* macroeconomic events
* sector-wide price movements

#### Judge / Adjudicator

The final layer compares both arguments and their supporting evidence.

Example output:

```text
Verdict: Suspicious but Inconclusive

Confidence: 0.68

Supporting Evidence:
- Significant social-media spike
- Coordinated posting pattern
- Abnormal trading volume

Counter Evidence:
- Positive earnings announcement
- Similar movement across the sector
```

### 6. Agentic AI Architecture

LangGraph can manage the workflow:

```text
Event Detected
      ↓
Prosecutor Agent
      ↓
Defense Agent
      ↓
Evidence Verification
      ↓
Judge Agent
      ↓
Final Report
```

The agents use tools rather than relying exclusively on their internal language-model knowledge.

### 7. Tech Stack

**Programming**

* Python
* Pandas
* NumPy

**Machine Learning**

* XGBoost / LightGBM
* FinBERT
* Scikit-learn

**GenAI**

* Qwen2.5-7B or API-based LLM
* LangGraph
* Tool calling
* Agentic workflows

**RAG**

* Sentence Transformers
* ChromaDB / FAISS
* SEC EDGAR
* Financial/news sources

**Backend**

* FastAPI

**Frontend**

* Streamlit
* Plotly

**Deployment**

* Docker
* Azure

**Monitoring / MLOps**

* MLflow
* logging
* model/version tracking

### 8. Evaluation

Evaluate different parts independently.

**Candidate detection**

* Precision
* Recall
* F1-score
* PR-AUC

**Retrieval**

* Recall@K
* MRR
* citation correctness

**Agentic investigation**

* Evidence relevance
* Evidence completeness
* Prosecutor/Defense agreement quality
* Judge consistency

**End-to-end**

* Correct identification of documented manipulation cases
* False-positive rate
* Confidence calibration

Synthetic manipulation scenarios can be generated separately to increase testing coverage, but real-world and synthetic evaluation results should be reported separately.

---

# Project 2: Cybersecurity — Vulnerability Prioritization + Generative Remediation

### 1. Problem Statement

Organizations may have thousands of known vulnerabilities across their infrastructure. Traditional vulnerability management often relies heavily on CVSS severity, but CVSS alone does not indicate how likely a vulnerability is to be exploited or how important the affected asset is to the organization.

The proposed system combines vulnerability severity, exploit probability, asset criticality, and exposure to prioritize vulnerabilities according to practical risk. An agentic AI system then investigates high-priority vulnerabilities using security advisories and hardening documentation and generates evidence-grounded remediation recommendations.

### 2. Objective

* Ingest current vulnerability information from real security databases.
* Combine CVSS and EPSS with organizational asset context.
* Rank vulnerabilities according to practical risk.
* Retrieve authoritative remediation information.
* Generate specific remediation recommendations.
* Provide citations and evidence for generated recommendations.
* Validate remediation recommendations in an isolated environment.
* Reduce reliance on generic LLM-generated security advice.

### 3. System Overview

```text
NVD API
   +
EPSS API
   +
Asset Inventory
   ↓
Vulnerability Normalization
   ↓
Risk Scoring
   ↓
Prioritized Vulnerability Queue
   ↓
Agentic Investigation
   ↓
Vendor Advisory / CIS RAG
   ↓
Remediation Generation
   ↓
Configuration / Patch Recommendation
   ↓
Sandbox Validation
   ↓
Security Verification
   ↓
Final Remediation Report
```

### 4. Technical Pipeline

#### Vulnerability Ingestion

Retrieve:

* CVE identifier
* CVSS score
* vulnerability description
* affected software
* affected versions
* CWE
* vendor references

#### Exploit Probability

Retrieve EPSS probability.

Example:

```text
CVE-A
CVSS = 9.8
EPSS = 0.02

CVE-B
CVSS = 8.5
EPSS = 0.87
```

CVE-B may deserve higher operational priority despite its lower CVSS.

#### Asset Context

Simulated organizational infrastructure can contain:

```text
Asset
Criticality
Internet Exposure
Business Function
Software Version
```

Example:

```text
Payment Server
Criticality = 10
Internet Exposed = Yes
```

### 5. Model Explanation

#### Risk Scoring Model

Start with a transparent formula:

```text
Risk =
Severity
× Exploit Probability
× Asset Criticality
× Exposure
```

The initial version should use an explainable formula.

A later version can replace the formula with a learned ranking model if sufficient historical or simulated training data becomes available.

#### Agentic Remediation Agent

The agent receives:

```text
CVE
Affected software
Current version
Asset criticality
Risk score
```

It then uses tools to retrieve:

* NVD details
* vendor advisories
* patch information
* CIS Benchmarks
* security documentation

The agent determines the appropriate remediation path.

### 6. RAG Architecture

Security documentation is processed as:

```text
Vendor Documentation
CIS Benchmarks
Security Advisories
        ↓
Document Processing
        ↓
Chunking
        ↓
Sentence Transformer
        ↓
Vector Database
        ↓
Semantic Retrieval
        ↓
LLM / Agent
```

The retrieved documents provide the factual basis for the recommendation.

### 7. Generative Remediation

Instead of producing:

> "Patch the vulnerability."

the system should produce a specific recommendation such as:

```text
Affected Package:
OpenSSL

Current Version:
X

Recommended Version:
Y

Temporary Mitigation:
Disable vulnerable functionality X

Source:
Vendor Security Advisory
```

The agent should distinguish between:

* confirmed vendor remediation
* temporary mitigation
* inferred recommendation

### 8. Sandbox Validation

A stronger implementation adds:

```text
Generated Recommendation
        ↓
Isolated VM / Container
        ↓
Apply Configuration
        ↓
Run Security Scanner
        ↓
Compare Before / After
        ↓
Validation Result
```

This reduces the risk of producing technically incorrect remediation instructions.

### 9. Tech Stack

**Programming**

* Python
* Pandas
* NumPy

**Security Data**

* NVD API
* EPSS API
* Vendor advisories
* CIS Benchmarks

**ML / NLP**

* Sentence Transformers
* optional Transformer-based vulnerability ranking model

**GenAI**

* Qwen / Llama / API-based LLM
* LangGraph
* Tool calling
* Agentic workflows

**RAG**

* ChromaDB / FAISS
* Sentence Transformers

**Backend**

* FastAPI

**Frontend**

* Streamlit
* Plotly

**Deployment**

* Docker
* Azure

**Validation**

* Docker/VM sandbox
* vulnerability scanner

### 10. Evaluation

**Prioritization**

Compare against:

* CVSS-only ranking
* EPSS-only ranking
* proposed risk ranking

Metrics:

* ranking quality
* high-risk vulnerability recall
* risk-weighted exposure reduction

**RAG**

* Retrieval Recall@K
* citation correctness
* grounding accuracy

**Remediation**

* technical correctness
* advisory consistency
* successful sandbox validation
* vulnerability reduction after remediation

---

# Project 3: Telecom — Capacity Forecasting with Agentic Diagnosis and Resolution

### 1. Problem Statement

Telecommunication networks contain many geographically distributed cells with constantly changing traffic loads. Predicting future network demand is useful, but forecasting alone does not solve congestion.

The system must determine whether predicted congestion is a normal recurring pattern or an unusual event. For predictable recurring congestion, the system should calculate an optimal capacity reallocation between cells while respecting network constraints.

The proposed system combines time-series forecasting, agentic diagnosis, and mathematical optimization to create an intelligent network-capacity management system.

### 2. Objective

* Forecast short-term network traffic for individual cells.
* Detect potential future congestion.
* Distinguish recurring congestion from abnormal events.
* Analyze neighboring cells to understand the spatial behavior of traffic.
* Automatically formulate capacity-reallocation problems.
* Use a mathematical optimization solver to determine valid allocations.
* Allow agents to explain the diagnosis and optimization result.
* Escalate uncertain or novel events to human operators.

### 3. System Overview

```text
Telecom Italia Milano Dataset
             ↓
       Data Processing
             ↓
      Feature Engineering
             ↓
   ┌───────────────────────┐
   │ Forecasting Model     │
   │ LightGBM / PatchTST   │
   └───────────┬───────────┘
               ↓
        Future Load Forecast
               ↓
       Congestion Detection
               ↓
        Diagnosis Agent
               ↓
       ┌───────┴────────┐
       ↓                ↓
   Recurring          Novel
    Pattern           Event
       ↓                ↓
 OR-Tools Solver    Human Ops
       ↓
Capacity Reallocation
       ↓
Simulation / Evaluation
       ↓
Agent Explanation
```

### 4. Dataset

Use the **Telecom Italia Milano Grid CDR dataset** containing mobile activity across geographic grid cells.

Relevant signals include:

* call activity
* SMS activity
* internet activity
* timestamp
* geographical grid location

The data can be converted into time-series sequences for individual cells.

### 5. Technical Pipeline

#### Step 1 — Preprocessing

For each cell:

```text
Timestamp
Cell ID
Traffic
```

Convert the data into ordered time-series sequences.

Handle:

* missing observations
* aggregation
* outliers
* temporal alignment

#### Step 2 — Feature Engineering

Potential features:

```text
lag_1
lag_2
lag_3
lag_24
lag_168
rolling_mean
rolling_std
hour
day_of_week
holiday indicator
neighbor traffic
```

### 6. Model Explanation

#### Baseline — Seasonal Naive

A simple baseline predicts:

> "The traffic at this time will be similar to the previous comparable period."

This provides a reference point.

#### LightGBM

LightGBM receives engineered temporal features.

Example:

```text
Hour = 18
Day = Monday
Previous hour traffic = 82
Previous day traffic = 76
Rolling average = 79
Neighbor average = 81
```

Output:

```text
Predicted traffic = 91
```

LightGBM provides an efficient and strong baseline for structured time-series forecasting.

#### PatchTST

PatchTST is a Transformer-based time-series model.

Instead of processing every time point independently:

```text
Time series
↓
Patches
↓
Transformer
↓
Future forecast
```

For example:

```text
Past 96 time steps
        ↓
Patch creation
        ↓
Transformer attention
        ↓
Next 4 hours
```

PatchTST provides the deep-learning component of the project.

A good experimental progression is:

```text
Seasonal Naive
      ↓
LightGBM
      ↓
LSTM / GRU
      ↓
PatchTST
```

This allows the project to demonstrate model selection rather than simply choosing a complex architecture from the beginning.

### 7. Congestion Detection

After forecasting:

```text
Predicted Load = 92%
Historical 95th percentile = 85%
```

The system identifies:

```text
Potential congestion = TRUE
```

The threshold can be cell-specific rather than globally fixed.

### 8. Agentic Diagnosis

The diagnosis agent investigates:

* historical behavior of the cell
* neighboring cells
* time-of-day patterns
* day-of-week patterns
* known holidays
* known events
* city-wide traffic patterns

The agent classifies the event conceptually as:

```text
Recurring Pattern
Localized Anomaly
City-Wide Surge
Unknown / Novel Event
```

### 9. Spatial Reasoning

Neighboring cells provide useful context.

Example:

```text
Cell A ↑↑↑
Cell B →
Cell C →
Cell D →
```

This suggests a localized anomaly.

But:

```text
Cell A ↑
Cell B ↑
Cell C ↑
Cell D ↑
```

suggests a broader city-wide traffic increase.

A future version can represent the network as a graph:

```text
Cell A ─ Cell B
  │        │
Cell C ─ Cell D
```

and use a Graph Neural Network for spatial-temporal forecasting.

### 10. Optimization with OR-Tools

For recurring congestion, the agent constructs an optimization problem.

Example:

```text
Cell A requires +20 capacity
Cell B has 15 units of spare capacity
Cell C has 10 units of spare capacity
```

OR-Tools determines the optimal allocation while respecting constraints.

Possible objective:

```text
Minimize:
Congestion Cost
+
Reallocation Cost
```

Subject to:

```text
Total Available Capacity = Constant

Minimum Capacity per Cell ≥ Required Minimum

Maximum Reallocation ≤ Operational Limit
```

Possible output:

```text
Cell B → -15
Cell C → -5
Cell A → +20
```

The LLM does not invent these numbers.

The solver produces the mathematically valid solution.

### 11. Agentic AI Architecture

The agent acts as the orchestration and reasoning layer.

```text
Forecast Model
      ↓
Congestion Alert
      ↓
Diagnosis Agent
      ↓
Tool Calls
 ┌────┼───────────┐
 ↓    ↓           ↓
History  Neighbor  Event Data
      ↓
Diagnosis
      ↓
If recurring:
      ↓
OR-Tools
      ↓
Optimization Result
      ↓
LLM Explanation
```

For uncertain cases:

```text
Novel Event
    ↓
Human Operations Team
```

This provides a human-in-the-loop safety mechanism.

### 12. Tech Stack

**Programming**

* Python
* Pandas
* NumPy

**Machine Learning**

* LightGBM
* Scikit-learn

**Deep Learning**

* PyTorch
* PatchTST
* NeuralForecast / Darts

**Optimization**

* Google OR-Tools

**Agentic AI**

* LangGraph
* LLM
* tool calling

**Backend**

* FastAPI

**Visualization**

* Streamlit
* Plotly
* Geographical/grid visualization

**Deployment**

* Docker
* Azure

**MLOps**

* MLflow
* model versioning
* experiment tracking

**Potential Real-Time Extensions**

* Kafka
* Redis
* streaming inference

### 13. Evaluation

#### Forecasting

Compare:

```text
Seasonal Naive
vs
LightGBM
vs
PatchTST
```

Metrics:

* MAE
* RMSE
* WAPE
* sMAPE

#### Congestion Detection

Evaluate:

* precision
* recall
* F1-score
* false-alert rate

#### Diagnosis

Evaluate:

* recurring vs anomalous classification accuracy
* localized vs city-wide classification accuracy

#### Optimization

Compare:

```text
Do Nothing
vs
Optimization-based Reallocation
```

Measure:

* congestion reduction
* capacity utilization
* number of overloaded cells
* reallocation cost

#### End-to-End

Measure:

```text
Forecast
 ↓
Correct congestion prediction
 ↓
Correct diagnosis
 ↓
Valid optimization
 ↓
Reduced simulated congestion
```

---

# Cross-Project Architecture

Although the three projects operate in different industries, they demonstrate the same modern AI engineering pattern.

```text
                    Data Sources
                         ↓
                 Data Processing
                         ↓
              ML / DL Prediction
                         ↓
                 Event Detection
                         ↓
                 Agentic Layer
                         ↓
             ┌───────────┼───────────┐
             ↓           ↓           ↓
           RAG         Tools       Memory
             ↓           ↓           ↓
             └───────────┼───────────┘
                         ↓
                     Reasoning
                         ↓
                  Decision / Action
                         ↓
                    Validation
                         ↓
                 Human / Automation
                         ↓
                   Monitoring
                         ↓
                Feedback / Retraining
```

## Core Agentic AI Concepts Demonstrated

### 1. Tool Use

Agents can interact with external systems instead of only generating text.

### 2. Retrieval-Augmented Generation

Agents retrieve relevant external evidence before making conclusions.

### 3. Multi-Agent Collaboration

Different agents can have different responsibilities and objectives.

### 4. Structured Decision Making

The LLM does not need to perform every calculation itself.

Specialized systems perform specialized tasks:

```text
ML Model      → Prediction
Vector DB     → Retrieval
LLM Agent     → Reasoning
OR-Tools      → Optimization
Security Tool → Validation
```

### 5. Human-in-the-Loop

Uncertain or high-risk situations are escalated instead of being automatically acted upon.

### 6. Evaluation

Each project evaluates individual components as well as the complete system.

### 7. MLOps and Deployment

All three projects can evolve into production systems with:

```text
GitHub
 ↓
CI/CD
 ↓
Testing
 ↓
Docker
 ↓
Model Registry
 ↓
Deployment
 ↓
Monitoring
 ↓
Model/Data Drift
 ↓
Retraining
```

## Overall Portfolio Value

Together, these projects demonstrate three different forms of Agentic AI:

**FinTech — Adversarial Investigation**

> Agents investigate competing hypotheses and verify evidence before reaching a conclusion.

**Cybersecurity — Autonomous Investigation and Remediation**

> An agent investigates vulnerabilities, retrieves authoritative information, generates a remediation strategy, and can validate the recommendation.

**Telecom — Agentic Diagnosis and Optimization**

> An agent interprets model predictions, diagnoses the cause of an event, invokes a mathematical optimizer, and explains the resulting operational decision.

This makes the portfolio demonstrate not only LLM usage, but the broader architecture of modern AI systems:

**Deep Learning + Machine Learning + RAG + Agentic AI + Tool Calling + Optimization + Evaluation + MLOps + Deployment.**
