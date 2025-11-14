# BubbleBusters
Bubble Busters (BB) is a Python-based financial analytics framework designed to detect, measure, and monitor speculative market exuberance across any thematic asset class. It allows for an portfolio-customized assessment to the risk of bubble-like price misallignment. The repository contains the coding portion of Bubble Busters coursework from the Data Science for Asset Management course by Professor Feng Zhou.

![Bubble Busters](c9a55da5-19d0-44d2-b35b-8f7779b52c32.png)
# The Bubble diagnostics revolution

---

## 1. Executive Summary

Bubble Busters (BB) is a Python-based financial analytics framework designed to detect, measure, and monitor speculative market exuberance across any thematic asset class. It provides a quantitative and explainable means to assess the extent to which valuations deviate from their economic and financial fundamentals, and to identify phases of unsustainable price acceleration, hype or systemic fragility .

Although the current period of market euphoria surrounding artificial intelligence has provided a natural use case, Bubble Busters is built as a **general-purpose analytical platform** that can be applied to any thematic domain. Whether the focus is green technology, digital assets, biotechnology, carbon credits, or another emerging sector, the framework offers the same structured approach to detecting asset price misalignments and potential bubbles.

The platform integrates three primary dimensions of analysis: (1) fundamental valuation alignment, (2) explosivity and regime change detection, and (3) market fragility assessment. These dimensions are combined into a unified metric—the Bubble Risk Index (BRI)—which provides a continuous measure of speculative intensity and systemic vulnerability.  

The ultimate objective of Bubble Busters is to deliver **explainable, data-driven bubble diagnostics** to both institutional and private investors, thereby improving risk awareness, asset allocation discipline, and the transparency of investment decisions.

### Core Innovation

Bubble Busters’ key innovation lies in the fusion of econometric explosivity testing, fundamental valuation diagnostics, and narrative sentiment analytics into a coherent and modular framework. The resulting system produces a quantifiable risk measure that captures both the rational and psychological components of market valuation dynamics.

### Target Users

The tool is intended for institutional investors, hedge funds, asset managers, and financial advisors seeking a structured and explainable framework for thematic risk control. It is also adaptable for private investors and family offices, particularly those whose portfolios are exposed to specific high-growth sectors where enthusiasm and valuation excesses often coincide.

### Monetization Model

Bubble Busters can be commercialized through multiple complementary channels:
- Institutional SaaS licenses (£5,000–£50,000 per year)
- Premium subscriptions for private clients (£25–£200 per month)
- API licensing for integration with wealth management platforms and robo-advisors.

---

## 2. Market Context and Problem Definition

Periods of thematic market exuberance have been recurring features of modern finance. From the dot-com era to the cryptocurrency and clean energy booms, each episode demonstrates how collective narratives can temporarily override valuation fundamentals. Traditional risk systems, however, have limited capacity to capture this phenomenon because they focus on volatility, correlations, or credit spreads, while ignoring the **narrative and behavioral mechanisms** that amplify price movements.

Thematic bubbles are typically driven by a combination of technological promise, media amplification, and investor herding. Asset prices detach from their underlying fundamentals, leading to overvaluation followed by sharp corrections when expectations realign. Investors, both institutional and private, often lack access to tools that can systematically monitor these disconnections in real time.


**Unmet needs include:**
- The ability to **quantify narrative intensity** and its divergence from fundamental metrics.
- Portfolio-level visibility into **thematic contagion risk**.
- Explainable and reproducible analytics that can justify exposure management decisions.

Bubble Busters responds to these needs by providing a **data-driven and transparent measurement system** that allows users to monitor market exuberance in any chosen asset group. The system transforms qualitative market sentiment into quantitative diagnostics, enabling more informed and rational decision-making.

---

## 3. Architecture Overview — The Four Analytical Pillars

The analytical design of Bubble Busters is organized into four interlinked modules, each of which produces a normalized score that feeds into the composite Bubble Risk Index (BRI).

### 3.1 Fundamentals Alignment (FA)

**Objective:** Measure the degree of misalignment between market valuations and underlying financial fundamentals.

Fundamental analysis serves as the baseline for assessing whether market prices can be justified by a company’s or sector’s economic reality. In this context, mispricing is detected by estimating the deviation of current valuations from what would be implied by historical or sectoral relationships between valuation and fundamentals.

**Methods and Implementation:**
- Panel regression of the logarithm of enterprise value (EV) on key financial variables such as revenues, margins, leverage, R&D intensity, and intangibles using `linearmodels.PanelOLS`.
- Computation of residual z-scores as indicators of relative over- or under-valuation.
- Reverse discounted cash flow (DCF) analysis to infer implied growth rates and compare them with sector norms.
- Cross-sectional ranking of valuation multiples (EV/Sales, EV/EBITDA, P/FCF) to identify outliers.

**Output:**  
The resulting FA-Score (0–100) measures the extent of fundamental mispricing, where higher values indicate a stronger disconnect between observed market valuations and their economic justification.


### 3.2 Explosivity and Regime Change (ER)

**Objective:** Detect statistical signs of speculative dynamics and identify transitions between market regimes.

Speculative price behavior is characterized by self-reinforcing feedback loops, where rising prices attract new capital inflows, which in turn sustain further price acceleration. Econometric tools can detect this explosivity by examining deviations from stationarity and by identifying abrupt shifts in the volatility or drift of price series.

**Methods and Implementation:**
- The **GSADF test (Phillips, Shi, Yu)** is used to detect periods of explosive price behavior, implemented using bootstrap critical values.
- **Changepoint detection** through the `ruptures` library (PELT or Binary Segmentation) identifies structural shifts in mean or variance.
- **Critical slowing down indicators**, such as rising autocorrelation and variance, are used to capture pre-crash dynamics.
- **Principal Component Analysis (PCA)** of asset returns is applied to measure herding, with high first-component variance ratios (>40%) signaling crowding behavior.

**Output:**  
The ER-Score (0–100) quantifies the level of explosivity and market concentration. A high ER value indicates that price movements are being driven more by reflexive momentum and investor herding than by fundamentals.

### 3.3 Market Fragility (MF)

**Objective:** Assess the vulnerability of a market or asset basket to sharp drawdowns or contagion effects.

Market fragility arises when high valuations coincide with narrow market breadth, leveraged positioning, and elevated cross-asset correlations. These conditions make the system susceptible to large downward adjustments once sentiment shifts.

**Methods and Implementation:**
- Estimation of downside beta and realized skewness over rolling windows to capture asymmetry in returns.
- Computation of Drawdown-at-Risk (DaR) using a GARCH(1,1) model combined with Filtered Historical Simulation.
- Conditional correlation analysis under stress conditions, specifically measuring correlations conditional on benchmark returns being below their 25th percentile.

**Output:**  
The MF-Score (0–100) represents the degree of systemic fragility and crash proneness within the monitored asset group.

### 3.4 Bubble Risk Index (BRI)

The Bubble Risk Index aggregates the previous three modules into a single, interpretable score:

$$
BRI = 0.4 \cdot FA + 0.4 \cdot ER + 0.2 \cdot MF
$$

This weighting structure reflects the view that valuation and explosivity are equally important drivers of bubble dynamics, while fragility acts as an amplifier of eventual drawdowns.

**Interpretation of Thresholds:**
- BRI ≥ 80 indicates a high-risk regime requiring hedging or exposure reduction.
- 60 ≤ BRI < 80 signals a watch condition warranting portfolio rebalancing.
- BRI < 60 denotes a normal regime with limited exuberance.

A Streamlit or Dash-based interface visualizes the BRI across time and sectors through heatmaps, alert logs, and comparative dashboards.

---

## 4. Personalization Layer — Bubble Busters Private (BB-P)

### 4.1 Purpose and Motivation

While the main analytical framework focuses on systemic or sectoral exuberance, the **Bubble Busters Private** module extends the concept to the individual portfolio level. It translates macro-level bubble indicators into personalized measures of exposure and potential vulnerability, providing retail and high-net-worth investors with the same analytical rigor available to institutions.

### 4.2 Portfolio Exposure Engine

**Step 1 — Thematic Classification**  
Holdings are classified by their narrative exposure intensity (e.g., “core thematic,” “enabler,” “neutral”) using both *sectoral taxonomies* and *natural language processing* applied to company descriptions and financial reports.

**Step 2 — Narrative Beta Estimation**

Each security’s sensitivity to a thematic narrative index is estimated via **regression**:

```python
from sklearn.linear_model import LinearRegression

beta = LinearRegression().fit(narr_index.values.reshape(-1,1), returns.values).coef_[0]

```

**Step 3 — Mispricing Overlay**

The asset’s **Fundamental Alignment (FA) score** is combined with its **narrative beta** to quantify how fundamental mispricing interacts with the asset’s sensitivity to thematic narratives. This step captures how speculative storytelling and valuation disconnect reinforce one another within a portfolio context. Assets with both high mispricing and strong narrative exposure contribute disproportionately to thematic concentration risk.


**Step 4 — Explosivity Transmission**

A **correlation network** and **Principal Component Analysis (PCA)** decomposition are applied to propagate sector-level explosivity into individual portfolio components. This identifies how shocks or accelerations within the broader theme (for example, a sector-wide AI selloff or clean-tech rally) can transmit through correlated holdings. The approach allows investors to estimate indirect exposure even in assets that are not explicitly thematic but move in tandem with speculative sectors.

**Step 5 — Personalized Stress Testing**

The portfolio is then subjected to a **scenario-based stress test**. A typical simulation involves a **30 percent correction** in the relevant thematic index combined with a **150 basis point increase** in real yields. The resulting changes in portfolio net asset value (ΔNAV) and drawdowns are computed using estimated betas and historical covariances. This process quantifies potential downside risk under plausible but adverse market conditions, helping to assess capital resilience.

---

### 4.3 Personal Bubble Risk Score (PBRS)

The **Personal Bubble Risk Score (PBRS)** integrates all the above effects into a single interpretable metric:

![image.png](attachment:1a1da4f7-2b85-4fa8-8e4e-0a96ad7e7d54.png)

The PBRS is expressed on a scale from **0 to 100**, where higher scores correspond to greater thematic concentration and systemic sensitivity. A score above 80 indicates severe exposure, while a value between 60 and 80 suggests a cautionary regime. Outputs include an exposure breakdown, a historical PBRS timeline, and strategic guidance on potential hedging or diversification measures.

---

## 5. Technical Architecture, Business Model, and Roadmap

### 5.1 Technical Implementation

| Layer | Libraries / Tools | Description |
|-------|-------------------|-------------|
| **Data** | pandas, duckdb, yfinance, fredapi | Collection of daily prices, fundamentals, and macroeconomic indicators |
| **Modelling** | statsmodels, linearmodels, arch, ruptures, scikit-learn | Econometric and machine learning modules for core analytics |
| **Orchestration** | prefect, joblib, pydantic | Task scheduling, caching, and data validation |
| **UI / API** | streamlit, dash, plotly, FastAPI | Visualization layer and REST API delivery |
| **Storage** | Parquet, Arrow, AWS S3 | Lightweight data lake for efficient access and persistence |


### Example Implementation — GSADF Test

```python
from statsmodels.tsa.stattools import adfuller
import numpy as np

def gsadf(series, min_window=30):
    T = len(series)
    stats = []
    for r0 in range(T - min_window):
        for r2 in range(r0 + min_window, T):
            sub = series[r0:r2]
            stat = adfuller(sub, regression='c', autolag=None)[0]
            stats.append(stat)
    return np.max(stats)
```
### 5.2 Business Model and Financial Feasibility

#### Revenue Streams

- **Institutional SaaS subscriptions and data access.**  
  Annual enterprise licenses provide access to the full analytics suite, API endpoints, and dedicated model calibration support.

- **Premium tier for private users.**  
  A subscription-based offering aimed at professional and high-net-worth individuals, delivering simplified dashboards and PBRS monitoring.

- **Integration APIs for private banks and wealth management platforms.**  
  Allowing direct embedding of Bubble Busters indicators into client portfolio systems.

- **Data licensing of indices and metrics to analytics providers.**  
  Sale of proprietary indices (e.g., Bubble Risk Index, Narrative Intensity Index) for integration into third-party financial platforms.

---

#### Cost Estimates

- **Year 1 development cost:** approximately £120,000, including infrastructure setup, software development, and data acquisition.  
- **Operating expenses:** approximately £40,000 per year for maintenance, cloud resources, and data subscriptions.  
- **Break-even point:** expected with around **15 institutional clients** and **1,000 premium users**.

---

#### Scalability

The platform is built on a **cloud-native architecture** leveraging Docker and AWS Lambda for parallel computation and distributed data handling.  
Its **modular design** ensures that new thematic sectors can be added with minimal code refactoring. The same analytical framework can therefore be extended to monitor diverse themes such as clean energy, digital health, or financial technology without major redevelopment.

---

### 5.3 Competitive Advantage

| Competitor | Limitation | Bubble Busters Advantage |
|-------------|-------------|--------------------------|
| **Bloomberg PORT** | Lacks narrative-driven analysis | Incorporates narrative and behavioral metrics alongside valuation models |
| **Aladdin (BlackRock)** | Enterprise-only access and closed architecture | Python open-core infrastructure with a modular retail and institutional interface |
| **Sentifi** | Focused primarily on social sentiment | Integrates fundamental misalignment, explosivity detection, and market fragility in one unified system |

The platform’s **principal differentiator** lies in its proprietary **Narrative Intensity Index (NII)** and the integration of **GSADF-based bubble detection** within a multi-factor valuation and risk analytics environment.  
This unique combination allows Bubble Busters to deliver quantitative signals that capture both the structural and psychological dimensions of market exuberance.

---

### 5.4 Governance, Validation, and Roadmap

#### Validation and Backtesting

All analytical modules are validated against **historical episodes of speculative exuberance**, including:
- The **dot-com bubble** (1998–2001)
- The **2007 commodity and housing cycle**
- The **2020 technology rally**
- The **2023–2025 artificial intelligence investment surge**

Validation focuses on **hit rates, false-positive ratios**, and **drawdown mitigation effectiveness**.  
Each module’s output is benchmarked against historical market turning points to ensure robustness and interpretability.

---

#### Governance

The model governance process follows a **Champion/Challenger framework** in which competing model specifications are evaluated on a quarterly basis.  
Weights within the Bubble Risk Index and PBRS components are periodically recalibrated to account for evolving market dynamics.  
All datasets are version-controlled using Parquet formats with checksum validation to ensure reproducibility. Each model and dataset is accompanied by documentation, model cards, and metadata to facilitate transparency and auditability.

---

#### Roadmap

- **Q1–Q2 2025:** Prototype development of the Narrative Intensity Index and GSADF modules.  
- **Q3 2025:** Release of the institutional Bubble Busters dashboard.  
- **Q4 2025:** Launch of **Bubble Busters Private (BB-P)** with portfolio upload, exposure tracking, and PBRS analytics.  
- **2026 and beyond:** Expansion into options-based hedging tools, multi-theme integration, and enhanced NLP pipelines for real-time narrative monitoring.

---

## 6. Conclusion

Bubble Busters provides a **comprehensive, transparent, and technically rigorous framework** for identifying and managing speculative excess across thematic markets.  
By uniting quantitative economics, econometric modeling, and narrative analysis, it transforms the assessment of market exuberance from a subjective interpretation into an **empirical and reproducible process**.

The platform’s versatility enables continuous monitoring of new and evolving themes, ensuring that both institutional and private investors can **anticipate systemic risks** rather than merely reacting to them.  
As financial markets increasingly revolve around powerful and rapidly evolving narratives, a **multi-dimensional diagnostic framework** such as Bubble Busters becomes essential to the practice of modern financial analysis and prudent portfolio management.

