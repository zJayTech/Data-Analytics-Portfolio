I used SQL to find out which preexisting conditions are most associated with high-risk COVID outcomes.

What Was Done:
Grouped the data per conditions and counted the total entries for each condition.
Filtered the rows to only those with severe symptoms or hospitalization, grouped, and counted how many of those were linked to each preexisting condition.

Code:
WITH total_cases AS (
  SELECT
    `Preexisting Condition`,
    COUNT(*) AS total
  FROM
    `secret-tempest-455720-u3.COVID.severity`
  GROUP BY
    `Preexisting Condition`
),

high_risk_cases AS (
  SELECT
    `Preexisting Condition`,
    COUNT(*) AS high_risk
  FROM
    `secret-tempest-455720-u3.COVID.severity`
  WHERE
    `Severity` IN ('High', 'Critical')
    OR `Hospitalized` = TRUE
  GROUP BY
    `Preexisting Condition`
)

SELECT
  t.`Preexisting Condition`,
  h.high_risk,
  t.total,
  ROUND(h.high_risk / t.total, 2) AS risk_ratio
FROM
  total_cases t
LEFT JOIN
  high_risk_cases h
ON
  t.`Preexisting Condition` = h.`Preexisting Condition`
ORDER BY
  risk_ratio DESC;