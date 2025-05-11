# BigQuery-Data-Exploration
-- PROJECT: Homelessness Data Exploration (2018 Focus)
-- DATA SOURCE: merit-america-data-project-ew.Exploration_Project.homelessness

-- --------------------------------------------------------------------------------
-- QUERY 1: Top 3 CoC Areas with Highest Number of Unaccompanied Homeless Youth Under 18 (2018)
-- --------------------------------------------------------------------------------
SELECT 
  CoC_Name,
  State,
  Homeless_Unaccompanied_Youth_Under_18
FROM 
  `merit-america-data-project-ew.Exploration_Project.homelessness`
WHERE 
  Count_Year = 2018
ORDER BY 
  Homeless_Unaccompanied_Youth_Under_18 DESC
LIMIT 3;

-- --------------------------------------------------------------------------------
-- QUERY 2: Unsheltered Homeless Trends in Delaware (2012–2018)
-- --------------------------------------------------------------------------------
SELECT 
  Count_Year,
  SUM(Unsheltered_Homeless) AS total_unsheltered
FROM 
  `merit-america-data-project-ew.Exploration_Project.homelessness`
WHERE 
  State = 'DE'
GROUP BY 
  Count_Year
ORDER BY 
  Count_Year ASC;

-- Observation: Review `total_unsheltered` year over year to confirm increasing trend.

-- --------------------------------------------------------------------------------
-- QUERY 3: Number of Locations with At Least 1 Person in Safe Haven Shelter in 2018
-- --------------------------------------------------------------------------------
SELECT 
  COUNT(DISTINCT CoC_Name) AS locations_with_safe_haven
FROM 
  `merit-america-data-project-ew.Exploration_Project.homelessness`
WHERE 
  Count_Year = 2018
  AND Sheltered_SH_Homeless > 0;

-- --------------------------------------------------------------------------------
-- QUERY 4: Top 3 CoC Areas by Safe Haven (SH) Sheltered Homeless Count (2018)
-- --------------------------------------------------------------------------------
SELECT 
  CoC_Name,
  State,
  Sheltered_SH_Homeless
FROM 
  `merit-america-data-project-ew.Exploration_Project.homelessness`
WHERE 
  Count_Year = 2018
ORDER BY 
  Sheltered_SH_Homeless DESC
LIMIT 3;

-- --------------------------------------------------------------------------------
-- QUERY 5: Top 7 States by Overall Homeless Population (2018)
-- --------------------------------------------------------------------------------
SELECT 
  State,
  SUM(Overall_Homeless) AS total_overall_homeless
FROM 
  `merit-america-data-project-ew.Exploration_Project.homelessness`
WHERE 
  Count_Year = 2018
GROUP BY 
  State
ORDER BY 
  total_overall_homeless DESC
LIMIT 7;

-- Compare these states to the U.S. 2018 population rankings for representation analysis.

-- --------------------------------------------------------------------------------
-- QUERY 6: Locations with >1000 Overall Homeless AND <100 Unsheltered Homeless (2018)
-- --------------------------------------------------------------------------------
SELECT 
  CoC_Name,
  State,
  Overall_Homeless,
  Unsheltered_Homeless
FROM 
  `merit-america-data-project-ew.Exploration_Project.homelessness`
WHERE 
  Count_Year = 2018
  AND Overall_Homeless > 1000
  AND Unsheltered_Homeless < 100
ORDER BY 
  Overall_Homeless DESC;

-- These areas may be effective at providing shelter despite large homeless populations.

-- --------------------------------------------------------------------------------
-- QUERY 7: From the Above List, Filter for Locations Where Unsheltered is < 2% of Overall Homeless
-- --------------------------------------------------------------------------------
SELECT 
  CoC_Name,
  State,
  Overall_Homeless,
  Unsheltered_Homeless,
  ROUND(Unsheltered_Homeless / Overall_Homeless, 4) AS unsheltered_percentage
FROM 
  `merit-america-data-project-ew.Exploration_Project.homelessness`
WHERE 
  Count_Year = 2018
  AND Overall_Homeless > 1000
  AND Unsheltered_Homeless < 100
  AND (Unsheltered_Homeless / Overall_Homeless) < 0.02
ORDER BY 
  unsheltered_percentage ASC;

-- These are exemplary locations where shelter access is nearly universal among the homeless population.
