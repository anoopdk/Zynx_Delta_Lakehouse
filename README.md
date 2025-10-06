# Data and Analytics Project

This exercise focuses on building a **Medallion pipeline** that sources data from CSV files and processes it through the **Raw**, **Enriched (Silver)**, and **Curated (Gold)** layers before it is consumed by the business.

The entire process of data extraction and curation is designed to answer specific **investor investment-related queries**.


Exercise Scope:
1. Summarize overall performance of fund 'Fund Two' for Investor 7002.
2. Compare overall return of Investor (7002) with others.
3. Show trend of MoM sinec initial investments.
4. Identify post-hoc adjustments.

Key considerations/Assumptions:
1. Scope of analysis is restricted to fund "Fund Two"
2. Review is limited to the Investor 7002.
3. For comparision across investors, MoM average is considered.
4. Use quaterly data points to show changing MoM.
5. Data in the transaction table is historical.
6. MoM is the only primary metric for comparision and trend analysis. 


**For exercise solution refer**:

Write-up - https://github.com/anoopdk/medallion_project/blob/main/docs/exercise%20write-up.md

Solution notebook - https://github.com/anoopdk/medallion_project/blob/main/scripts/exercise/exercise_solutions.ipynb

Solution screen-shots - https://github.com/anoopdk/medallion_project/blob/main/docs/solution%20screenshots.md

Databricks work - https://github.com/anoopdk/medallion_project/blob/main/docs/databricks%20work%20documentation.md 

