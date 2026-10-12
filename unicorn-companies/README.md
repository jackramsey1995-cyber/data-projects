# Analysing Unicorn Companies

An investment firm wanted to know which industries are producing the most high-value companies, to help shape their portfolio. I used SQL to find the three industries with the most new unicorns (private companies valued at over $1 billion) from 2019 to 2021, and tracked how many joined each year and their average valuation.

## Approach

I broke the analysis into three separate queries so I could check each step before moving on.

```
Query 1 → find the top 3 industries for 2019–2021
Query 2 → count unicorns and average valuation per industry, per year
Query 3 → combine the two, keeping only the top 3 industries
```

The data came from four tables linked by `company_id`. I used `industries`, `dates` and `funding` for this analysis.

The full queries, outputs and step-by-step notes are in the [notebook](unicorn_analysis.ipynb).

## Tools

PostgreSQL, pandas, DataCamp DataLab
