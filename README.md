E-commerce Customer Analytics | 2021–2025
Python · Customer Analytics · Predictive CLV · Bayesian Modelling · Business Intelligence
Project overview
This project investigates changes in e-commerce revenue through customer purchasing behaviour, retention, customer value, discounts, returns, and customer experience. The guiding business question is:
Why did annual gross revenue decline despite a relatively stable number of active customers?

Annual gross sales fell from €38.33 million in 2021 to €37.59 million in 2025, a decline of €743,015 (1.94%). The analysis combines descriptive exploration, customer segmentation, probabilistic customer lifetime value (CLV) estimation, and Bayesian regression to examine potential contributors and identify priorities for further investigation.
Dataset
The notebook uses an e-commerce transactions CSV (Ecommerce.csv) covering 2021–2025, with approximately 138,000 orders and 24,900 observed customers. Fields include:
- Orders and revenue: order_id, order_date, quantity, gross_sales, net_sales, profit, discount_amount
- Customers: customer_id, customer segment/type, location, and demographics
- Sales and marketing attribution: sales_channel, marketing_channel, campaign_name
- Post-purchase experience: return_status, return_reason, customer_rating, review_sentiment, delivery fields
The dataset is not included in this repository unless separately provided. Marketing-channel attribution is not the same as actual marketing expenditure.
Objectives and analytical workflow
Analysis	Why it was performed	What it contributes
Sales performance	Examine revenue, order frequency, average order value (AOV), and seasonality	Establishes the size and pattern of the revenue change
RFM and customer lifetime value	Estimate repeat purchasing and future customer value	BG/NBD purchase predictions and Gamma-Gamma spend estimates
Cohort and retention analysis	Compare behaviour across first-observed customer cohorts	Shows retention patterns while accounting for different observation windows
K-means segmentation	Group customers with similar purchasing characteristics	Four RFM-based customer clusters
Customer acquisition	Investigate the decline in first-observed customer counts	Highlights a trend that cannot be equated with verified new acquisition
Discount analysis	Assess relationships between discount exposure, repeat purchasing, CLV, and profitability	Distinguishes spending associations from margin implications
Returns analysis	Evaluate return trends and affected sales channels	Quantifies returned-order exposure and its changes over time
Customer experience	Examine customer ratings and review sentiment	Checks for evidence of deteriorating experience
Churn and inactivity	Distinguish long purchase gaps from permanent customer loss	Operational inactivity measures and BG/NBD probability-alive estimates
Bayesian regression	Quantify uncertainty in spending associations	Compares Gamma and Normal spending models and posterior predictive fit
Revenue decomposition	Separate the contributions of customer count, order frequency, and AOV	Identifies the primary accounting driver of the revenue decline


Key findings
1. Average order value was the main contributor to lower gross revenue
Revenue driver, 2021–2025	Contribution
Average order value	−€803,577
Orders per active customer	−€126,076
Active customers	+€186,638
Total change in gross revenue	−€743,015


Average order value fell from approximately €1,391 to €1,362, while the active-customer count increased slightly. This is an accounting decomposition, not a causal explanation of why transaction values fell.
2. CLV models were useful at the aggregate level, with customer-level limitations
The notebook fits a BG/NBD model for repeat purchasing and a Gamma-Gamma model for monetary value. In the 2025 validation, aggregate predicted value was approximately 2.84% below observed revenue, although customer- and segment-level prediction errors were less uniform. The notebook also estimates 12- and 18-month CLV and expected transactions.
3. Inactivity should not be treated as confirmed churn
Approximately 9.4% of historically observed customers exceeded the selected 754-day prolonged-inactivity threshold at the end of 2025. BG/NBD estimated very low permanent dropout probabilities. Neither measure establishes that those customers are permanently lost.
4. Discounts were associated with profitability trade-offs
The annual discount rate remained around 17.2%. Customer-level discount exposure had a negligible relationship with repeat purchase frequency (Spearman ≈ +0.062) and a strong negative relationship with profit margin (≈ −0.78). These are associations, not causal discount effects; the margin relationship also partly reflects how margin is calculated.
5. Returns increased modestly; customer experience was broadly stable
The order return rate increased from approximately 6.62% in 2021 to 7.15% in 2025. The gross sales value associated with returned orders also rose, but this should not automatically be interpreted as an equivalent realized financial loss. Average customer ratings and review sentiment showed no clear deterioration over the period.
6. Bayesian models highlighted spending relationships and model trade-offs
The notebook uses a Gamma regression with a log link to model positive customer spending as a function of standardized discount rate, purchase frequency, and customer observation tenure (T). Estimated associations for a one-standard-deviation increase were approximately +9.77%, +49.69%, and +1.61%, respectively. Frequency is mechanically related to cumulative spending, so these results are not causal. A comparison Normal model reproduced some distribution summaries more closely but generated negative spending predictions, which are not meaningful for this outcome.
Business implications
1. Investigate falling AOV first. Examine product mix, quantities per order, pricing, and the distribution of order values before attributing the decline to customer loss.
2. Evaluate discounts using profitability, not revenue alone. Higher customer spending does not necessarily imply stronger contribution margins.
3. Use CLV and RFM segments as prioritization tools. Validate customer-level forecasts before making high-stakes targeting decisions.
4. Track returns and customer experience together. Monitor return rates by sales channel and product once reliable product-level information is available.
5. Distinguish inactivity, retention, and acquisition. First-observed purchases are not necessarily verified new customers, and prolonged inactivity is not confirmed churn.
Tools and libraries
- Data preparation: Python, pandas, NumPy
- Visualization: Matplotlib, Seaborn
- Segmentation and evaluation: scikit-learn (KMeans, StandardScaler, silhouette score, MAE, RMSE)
- Probabilistic CLV: PyMC-Marketing (BG/NBD, Gamma-Gamma)
- Bayesian inference and diagnostics: PyMC, ArviZ, ArviZ Plots, xarray
- Environment: Jupyter Notebook
How to run
1. Clone or download the repository and open EcommerceProject.ipynb in Jupyter.
2. Create a Python environment and install the packages used by the notebook:
   pip install jupyter pandas numpy matplotlib seaborn scikit-learn pymc pymc-marketing arviz arviz-plots xarray
3. Obtain the underlying Ecommerce.csv dataset and place it in an appropriate local directory.
4. Update the data-loading path in the notebook. The original notebook uses a machine-specific absolute path. For a CSV placed beside the notebook, use:
   data = pd.read_csv("Ecommerce.csv", sep=";")
5. Run the notebook cells in order. Bayesian model fitting may take time; package API differences may require adjustment depending on installed versions.
Reproducibility note: The notebook is exploratory and includes intermediate variables and model-fitting steps. The README describes the analysis present in the notebook; it is not a guarantee that every cell runs unchanged in a fresh environment. Pin package versions and perform a clean-kernel run before publishing a reproducible release.

Limitations
- No marketing spend data: Marketing mix modelling, channel-level return on advertising spend, and defensible budget optimization were not possible.
- No verified historical acquisition records: Cohorts represent first observed transactions in the available dataset; the first year is left-censored.
- Incomplete product-level explanatory detail: The project cannot determine whether AOV declined because of product mix, pricing, or units per order.
- Observational analysis: Relationships between discounts, purchase frequency, and spending should not be interpreted causally.
- CLV model assumptions: Aggregate accuracy does not imply equally accurate customer-level estimates.
- Return values: Gross sales associated with returned orders are not necessarily the realized refund amount or net financial loss.
Conclusion
The strongest supported finding is that lower average order value—not a substantial reduction in active customers—was the principal accounting contributor to the 2021–2025 gross revenue decline. The next business question is why customers spent less per order. Answering it requires better visibility into product mix, pricing, quantities, and potentially customer-level purchase motivations.# Ecommerce
