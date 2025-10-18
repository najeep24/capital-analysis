## Dataset Overview

The system processes four interconnected datasets containing borrower information and financial performance data across multiple years:

- **Firm Information Table** (34,500 records): Core borrower details including sector, region, size classification
- **Income Statement Table** (154,901 records): Profitability metrics including revenue, expenses, and earnings
- **Balance Sheet Table** (154,901 records): Assets, liabilities, and equity composition with working capital components
- **Cash Flow Statement Table** (154,901 records): Operating, investing, and financing activities

---

## Data Dictionary

### Firm Information Table

|Column|Type|Example Value|Description|
|---|---|---|---|
|firm_id|String|"FRM_0001"|Unique identifier for each borrower/company|
|sector|String|"Manufacturing"|Industry sector classification|
|region|String|"West Java"|Geographic region where firm operates|
|start_year|Integer|2015|Year when the firm started operations|
|size_class|String|"Medium"|Company size classification (Small, Medium, Large)|
|seed|Integer|42|Random seed for data generation/simulation|

### Income Statement Table

|Column|Type|Example Value|Description|
|---|---|---|---|
|firm_id|String|"FRM_0001"|Unique identifier linking to firm information|
|year|Integer|2020|Financial reporting year|
|revenue|Float|5000000.00|Total sales revenue for the period|
|cogs|Float|3000000.00|Cost of goods sold|
|gross_profit|Float|2000000.00|Revenue minus cost of goods sold|
|opex|Float|800000.00|Operating expenses|
|ebitda|Float|1200000.00|Earnings before interest, tax, depreciation, amortization|
|depreciation|Float|150000.00|Depreciation expense for the period|
|ebit|Float|1050000.00|Earnings before interest and tax (operating profit)|
|interest_expense|Float|50000.00|Interest paid on debt obligations|
|ebt|Float|1000000.00|Earnings before tax|
|tax|Float|200000.00|Income tax expense|
|net_income|Float|800000.00|Bottom-line profit after all expenses and taxes|

### Balance Sheet Table

|Column|Type|Example Value|Description|
|---|---|---|---|
|firm_id|String|"FRM_0001"|Unique identifier linking to firm information|
|year|Integer|2020|Financial reporting year|
|cash|Float|500000.00|Cash and cash equivalents|
|receivables|Float|800000.00|Accounts receivable from customers|
|inventory|Float|400000.00|Raw materials and finished goods inventory|
|other_current_assets|Float|100000.00|Other short-term assets|
|total_current_assets|Float|1800000.00|All assets convertible to cash within one year|
|ppe_gross|Float|2000000.00|Gross value of property, plant, and equipment|
|accum_depreciation|Float|300000.00|Accumulated depreciation on fixed assets|
|ppe_net|Float|1700000.00|Net value of property, plant, and equipment|
|other_noncurrent_assets|Float|200000.00|Long-term assets excluding PPE|
|total_assets|Float|3700000.00|Sum of all current and non-current assets|
|payables|Float|600000.00|Accounts payable to suppliers|
|other_current_liabilities|Float|150000.00|Other short-term liabilities|
|current_debt|Float|200000.00|Short-term debt obligations due within one year|
|total_current_liabilities|Float|950000.00|All liabilities due within one year|
|long_term_debt|Float|1000000.00|Long-term debt obligations|
|total_liabilities|Float|1950000.00|Sum of all current and long-term liabilities|
|equity_begin|Float|1600000.00|Shareholders' equity at beginning of period|
|dividends|Float|100000.00|Dividends paid to shareholders|
|equity_injection|Float|200000.00|Additional capital invested by shareholders|
|equity_end|Float|1750000.00|Shareholders' equity at end of period|
|total_liabilities_and_equity|Float|3700000.00|Sum of liabilities and equity (must equal total assets)|
|days_receivable|Float|58.40|Average days to collect payments from customers|
|days_inventory|Float|48.67|Average days inventory held before sale|
|days_payable|Float|73.00|Average days taken to pay suppliers|
|capex|Float|250000.00|Capital expenditures on fixed assets|
|useful_life|Float|10.00|Estimated useful life of fixed assets in years|
|payout_ratio|Float|0.125|Percentage of net income paid out as dividends|

### Cash Flow Statement Table

|Column|Type|Example Value|Description|
|---|---|---|---|
|firm_id|String|"FRM_0001"|Unique identifier linking to firm information|
|year|Integer|2020|Financial reporting year|
|net_income|Float|800000.00|Starting point for operating cash flow calculation|
|depreciation|Float|150000.00|Add-back of non-cash depreciation expense|
|change_receivables|Float|-50000.00|Decrease in receivables (positive for cash inflow)|
|change_inventory|Float|30000.00|Decrease in inventory (positive for cash inflow)|
|change_payables|Float|80000.00|Increase in payables (positive for cash inflow)|
|cash_flow_operations|Float|1010000.00|Net cash generated from operating activities|
|capex|Float|-250000.00|Capital expenditure (cash outflow)|
|asset_disposal_proceeds|Float|50000.00|Cash received from sale of assets|
|cash_flow_investing|Float|-200000.00|Net cash used in investing activities|
|change_long_term_debt|Float|100000.00|Increase in long-term debt (cash inflow)|
|change_current_debt|Float|50000.00|Increase in current debt (cash inflow)|
|equity_injection|Float|200000.00|Cash invested by shareholders|
|dividends_paid|Float|-100000.00|Cash paid out as dividends|
|cash_flow_financing|Float|250000.00|Net cash generated from financing activities|
|net_cash_flow|Float|1060000.00|Total change in cash position (operations + investing + financing)|
|cash_beginning|Float|500000.00|Cash balance at beginning of period|
|cash_ending|Float|1560000.00|Cash balance at end of period|
