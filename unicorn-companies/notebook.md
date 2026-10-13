# Unicorn Companies: Industry Trends Analysis

![Hand with calculator](calculator.jpg)

Did you know that the average return from investing in stocks is [10% per year](https://www.nerdwallet.com/article/investing/average-stock-market-return) (not accounting for inflation)? But who wants to be average?! 

You have been asked to support an investment firm by analyzing trends in high-growth companies. They are interested in understanding which industries are producing the highest valuations and the rate at which new high-value companies are emerging. Providing them with this information gives them a competitive insight as to industry trends and how they should structure their portfolio looking forward.

You have been given access to their `unicorns` database, which contains the following tables:

## dates
| Column       | Description                                  |
|------------- |--------------------------------------------- |
| `company_id`   | A unique ID for the company.                 |
| `date_joined` | The date that the company became a unicorn.  |
| `year_founded` | The year that the company was founded.       |

## funding
| Column           | Description                                  |
|----------------- |--------------------------------------------- |
| `company_id`       | A unique ID for the company.                 |
| `valuation`        | Company value in US dollars.                 |
| `funding`          | The amount of funding raised in US dollars.  |
| `select_investors` | A list of key investors in the company.      |

## industries
| Column       | Description                                  |
|------------- |--------------------------------------------- |
| `company_id`   | A unique ID for the company.                 |
| `industry`     | The industry that the company operates in.   |

## companies
| Column       | Description                                       |
|------------- |-------------------------------------------------- |
| `company_id`   | A unique ID for the company.                      |
| `company`      | The name of the company.                          |
| `city`         | The city where the company is headquartered.      |
| `country`      | The country where the company is headquartered.   |
| `continent`    | The continent where the company is headquartered. |

## The output

Your query should return a table in the following format:
| industry  | year | num\_unicorns       | average\_valuation\_billions |
| --------- | ---- | ------------------- | ---------------------------- |
| industry1 | 2021 |        ---          |             ---              |
| industry2 | 2020 |        ---          |             ---              |
| industry3 | 2019 |        ---          |             ---              |
| industry1 | 2021 |        ---          |             ---              |
| industry2 | 2020 |        ---          |             ---              |
| industry3 | 2019 |        ---          |             ---              |
| industry1 | 2021 |        ---          |             ---              |
| industry2 | 2020 |        ---          |             ---              |
| industry3 | 2019 |        ---          |             ---              |

Where `industry1`, `industry2`, and `industry3` are the three top-performing industries.

## Step 1: Find the top 3 industries (2019–2021)

```sql
SELECT i.industry,
       COUNT(*) AS total_unicorns
FROM industries AS i
INNER JOIN dates AS d
    ON i.company_id = d.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) IN (2019, 2020, 2021)
GROUP BY i.industry
ORDER BY total_unicorns DESC
LIMIT 3;
```

**Output:**

| industry | total_unicorns |
| --- | ---: |
| Fintech | 173 |
| Internet software & services | 152 |
| E-commerce & direct-to-consumer | 75 |

## Step 2: Unicorns and average valuation by industry and year

```sql
SELECT i.industry,
       EXTRACT(YEAR FROM d.date_joined)::INT AS year,
       COUNT(*) AS num_unicorns,
       AVG(f.valuation)::FLOAT AS average_valuation
FROM industries AS i
INNER JOIN dates AS d
    ON i.company_id = d.company_id
INNER JOIN funding AS f
    ON i.company_id = f.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) IN (2019, 2020, 2021)
GROUP BY i.industry, year
ORDER BY year DESC, num_unicorns DESC;
```

**Output:**

| industry | year | num_unicorns | average_valuation |
| --- | ---: | ---: | ---: |
| Fintech | 2021 | 138 | 2,753,623,188.41 |
| Internet software & services | 2021 | 119 | 2,151,260,504.20 |
| E-commerce & direct-to-consumer | 2021 | 47 | 2,468,085,106.38 |
| Health | 2021 | 40 | 1,950,000,000.00 |
| Artificial intelligence | 2021 | 36 | 1,416,666,666.67 |
| Cybersecurity | 2021 | 27 | 2,518,518,518.52 |
| Supply chain, logistics, & delivery | 2021 | 25 | 2,200,000,000.00 |
| Data management & analytics | 2021 | 21 | 2,142,857,142.86 |
| Other | 2021 | 21 | 1,714,285,714.29 |
| Hardware | 2021 | 14 | 2,000,000,000.00 |
| Edtech | 2021 | 12 | 2,000,000,000.00 |
| Consumer & retail | 2021 | 7 | 2,571,428,571.43 |
| Mobile & telecommunications | 2021 | 6 | 1,833,333,333.33 |
| Auto & transportation | 2021 | 4 | 3,750,000,000.00 |
| Travel | 2021 | 3 | 2,666,666,666.67 |
| Internet software & services | 2020 | 20 | 4,350,000,000.00 |
| E-commerce & direct-to-consumer | 2020 | 16 | 4,000,000,000.00 |
| Fintech | 2020 | 15 | 4,333,333,333.33 |
| Other | 2020 | 11 | 3,000,000,000.00 |
| Health | 2020 | 9 | 3,111,111,111.11 |
| Mobile & telecommunications | 2020 | 8 | 3,125,000,000.00 |
| Cybersecurity | 2020 | 7 | 3,285,714,285.71 |
| Data management & analytics | 2020 | 6 | 3,000,000,000.00 |
| Auto & transportation | 2020 | 5 | 3,000,000,000.00 |
| Edtech | 2020 | 4 | 2,750,000,000.00 |
| Artificial intelligence | 2020 | 3 | 4,000,000,000.00 |
| Supply chain, logistics, & delivery | 2020 | 2 | 2,000,000,000.00 |
| Consumer & retail | 2020 | 1 | 15,000,000,000.00 |
| Hardware | 2020 | 1 | 2,000,000,000.00 |
| Fintech | 2019 | 20 | 6,800,000,000.00 |
| Artificial intelligence | 2019 | 14 | 4,500,000,000.00 |
| Internet software & services | 2019 | 13 | 4,230,769,230.77 |
| E-commerce & direct-to-consumer | 2019 | 12 | 2,583,333,333.33 |
| Other | 2019 | 9 | 2,888,888,888.89 |
| Supply chain, logistics, & delivery | 2019 | 8 | 3,000,000,000.00 |
| Auto & transportation | 2019 | 6 | 4,166,666,666.67 |
| Data management & analytics | 2019 | 4 | 11,500,000,000.00 |
| Cybersecurity | 2019 | 4 | 2,250,000,000.00 |
| Mobile & telecommunications | 2019 | 4 | 2,000,000,000.00 |
| Travel | 2019 | 3 | 4,000,000,000.00 |
| Consumer & retail | 2019 | 3 | 3,666,666,666.67 |
| Health | 2019 | 3 | 3,333,333,333.33 |
| Edtech | 2019 | 1 | 1,000,000,000.00 |

## Step 3: Final result for the top 3 industries

```sql
SELECT industry,
       year,
       num_unicorns,
       ROUND(average_valuation / 1000000000, 2) AS average_valuation_billions
FROM (
    -- yearly rankings (same as cell 2)
    SELECT i.industry,
           EXTRACT(YEAR FROM d.date_joined) AS year,
           COUNT(*) AS num_unicorns,
           AVG(f.valuation) AS average_valuation
    FROM industries AS i
    INNER JOIN dates AS d
        ON i.company_id = d.company_id
    INNER JOIN funding AS f
        ON i.company_id = f.company_id
    GROUP BY i.industry, year
) AS yearly_rankings
WHERE industry IN (
    -- top industries (same as cell 1)
    SELECT i.industry
    FROM industries AS i
    INNER JOIN dates AS d
        ON i.company_id = d.company_id
    WHERE EXTRACT(YEAR FROM d.date_joined) IN (2019, 2020, 2021)
    GROUP BY i.industry
    ORDER BY COUNT(*) DESC
    LIMIT 3
)
  AND year IN (2019, 2020, 2021)
ORDER BY year DESC, num_unicorns DESC;
```

**Output:**

| industry | year | num_unicorns | average_valuation_billions |
| --- | ---: | ---: | ---: |
| Fintech | 2021 | 138 | 2.75 |
| Internet software & services | 2021 | 119 | 2.15 |
| E-commerce & direct-to-consumer | 2021 | 47 | 2.47 |
| Internet software & services | 2020 | 20 | 4.35 |
| E-commerce & direct-to-consumer | 2020 | 16 | 4.00 |
| Fintech | 2020 | 15 | 4.33 |
| Fintech | 2019 | 20 | 6.80 |
| Internet software & services | 2019 | 13 | 4.23 |
| E-commerce & direct-to-consumer | 2019 | 12 | 2.58 |
