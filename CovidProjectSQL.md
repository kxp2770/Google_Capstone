
## Covid-19 Data Exploration [from](https://ourworldindata.org/covid-deaths) Feb2020 to May2021
Skills used: Joins, CTE's, Windows Functions, Aggregate Functions, Creating Views, Converting Data Types



```sql

SELECT * 
FROM CovidProject..CovidDeaths
ORDER BY 3, 4

-- Select data to use
SELECT Location, date, Total_Cases, New_Cases, Total_Deaths, Population
FROM CovidProject..CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 1, 2


-- Looking at total cases vs total deaths
-- Percentage of dying to covid in the United States 
SELECT Location, date, Total_Cases, Total_Deaths, (total_deaths/total_cases)*100 AS Death_Rate_Percentage
FROM CovidProject..CovidDeaths
WHERE location LIKE '%states%' AND continent IS NOT NULL
ORDER BY 1, 2


-- Looking at Total Cases vs Population
-- Shows percentages of United States population that contracted covid
SELECT Location, date, Total_Cases, Population, (total_cases/population)*100 AS Infection_Percentage
FROM CovidProject..CovidDeaths
WHERE location LIKE '%states%' AND continent IS NOT NULL
ORDER BY 1, 2


Select Location, date, MAX(total_cases) as Highest_Infection_Count, Population, Max((total_cases/population))*100 as Percent_Population_Infected
From CovidProject..CovidDeaths
Group by Location, Population, date
order by Percent_Population_Infected desc


-- Looking at countries with highest infection rate compared to population 
SELECT Location, MAX(total_cases) AS Highest_Infection_Count, Population, MAX((total_cases/population)*100) AS Infection_Percentage
FROM CovidProject..CovidDeaths
WHERE location IS NOT NULL
GROUP BY population, location
ORDER BY Infection_Percentage DESC

-- Showing countries with highest death count 
SELECT Location, MAX(CONVERT(int, total_deaths)) AS Total_Death_Count
FROM CovidProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY Location
ORDER BY Total_Death_Count DESC



-- DATA EXPLORATION BY CONTINENT

-- Showing total deaths count by continent and entire world
SELECT location, MAX(CONVERT(int, total_deaths)) AS Total_Death_Count
FROM CovidProject..CovidDeaths
WHERE continent IS NULL AND location NOT IN ('World', 'European Union', 'International')
GROUP BY location
ORDER BY Total_Death_Count DESC

-- Looking at total cases vs total deaths per continent
-- Showing percentage of death by continent
SELECT Location, SUM(new_cases) AS Total_Cases, SUM(CAST(new_deaths as int)) AS Total_Deaths, (SUM(CAST(new_deaths as int))/SUM(new_cases))*100 AS Death_Rate_Percentage
FROM CovidProject..CovidDeaths
WHERE continent IS NULL
GROUP BY Location
ORDER BY 1, 2

-- Looking at Total Cases vs Population
-- Looking at continent with highest infection rate compared to population 
SELECT Location, MAX(total_cases) AS Highest_Infection_Count, Population AS Continent_Population, MAX((total_cases/population)*100) AS Infection_Percentage
FROM CovidProject..CovidDeaths
WHERE continent IS NULL
GROUP BY location, population
ORDER BY Infection_Percentage DESC



-- GLOBAL NUMBERS

-- Showing total cases vs total deaths with % death chance
SELECT SUM(new_cases) AS Total_cases, SUM(CAST(new_deaths as int)) AS Total_deaths, SUM(CAST(new_deaths as int))/SUM(new_cases)*100 AS Death_Rate_Percentage
FROM CovidProject..CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 1, 2

-- Looking at total population vs vaccinations
-- Instead of using total_vaccination column, creating a new column indicating same information for knowledge demostration of windows function
SELECT dea.continent, dea.location, dea.date, population, vac.new_vaccinations, SUM(CAST(vac.new_vaccinations as int)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS Continuous_Sum_Vaccination
FROM CovidProject..CovidDeaths dea
JOIN CovidProject..CovidVaccinations vac
	ON dea.location = vac.location AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
ORDER BY 2, 3

-- Use CTE
-- Showing total percent vaccination vs population
WITH PopvsVac (Continent, Location, Date, Population, New_Vaccinations, Continuous_Sum_Vaccination)
as (
SELECT dea.continent, dea.location, dea.date, population, vac.new_vaccinations, SUM(CAST(vac.new_vaccinations as int)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS Continuous_Sum_Vaccination
FROM CovidProject..CovidDeaths dea
JOIN CovidProject..CovidVaccinations vac
	ON dea.location = vac.location AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
	)
SELECT *, (Continuous_Sum_Vaccination/population)*100 AS Total_Vaccination_Percentage
FROM PopvsVac

-- Showing highest count of vaccination vs population for each country
SELECT dea.continent, dea.location, population, MAX(vac.total_vaccinations) AS Total_Vaccinations
FROM CovidProject..CovidDeaths dea
JOIN CovidProject..CovidVaccinations vac
	ON dea.location = vac.location
WHERE dea.continent IS NOT NULL
GROUP BY dea.continent, dea.location, population


-- Showing current total population that is vaccinated by country
WITH CurrentVac (Continenet, Location, Population, Total_Vaccinations)
as 
(
SELECT dea.continent, dea.location, population, MAX(vac.total_vaccinations) AS Total_Vaccinations
FROM CovidProject..CovidDeaths dea
JOIN CovidProject..CovidVaccinations vac
	ON dea.location = vac.location
WHERE dea.continent IS NOT NULL
GROUP BY dea.continent, dea.location, population
)
SELECT *, (Total_Vaccinations/Population)*100 AS Percent_Vaccinated
FROM CurrentVac
ORDER BY Location 


-- Creating view to store for later visualizations

CREATE VIEW PercentPopulationVaccinated AS
SELECT dea.continent, dea.location, dea.date, population, vac.new_vaccinations, SUM(CAST(vac.new_vaccinations as int)) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS Continuous_Sum_Vaccination
FROM CovidProject..CovidDeaths dea
JOIN CovidProject..CovidVaccinations vac
	ON dea.location = vac.location AND dea.date = vac.date
WHERE dea.continent IS NOT NULL


CREATE VIEW CurrentPopulationVaccinated AS
SELECT dea.continent, dea.location, population, MAX(vac.total_vaccinations) AS Total_Vaccinations
FROM CovidProject..CovidDeaths dea
JOIN CovidProject..CovidVaccinations vac
	ON dea.location = vac.location
WHERE dea.continent IS NOT NULL
GROUP BY dea.continent, dea.location, population

```
