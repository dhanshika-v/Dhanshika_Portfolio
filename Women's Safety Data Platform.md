## Women's Safety Data Platform – SQL & ERD Design


WomenSafe is a non-profit organization dedicated to enhancing the safety and security of women across various communities. Their mission is to empower women by providing them with the necessary tools, resources, and support systems to ensure their safety and well-being in both public and private spaces. This issue is pervasive and affects women from all walks of life, making it imperative to develop a comprehensive approach to address these threats. By developing this database application, WomenSafe aims to create a safer environment for women, where they can live, work, and travel without fear. Our goal is to harness the power of data and technology to drive meaningful change and provide a reliable support system for women everywhere.

This project focused on leveraging data to support meaningful, real-world solutions for women’s safety in public and private spaces. I led the design and development of a structured SQL-based system that transforms raw data into actionable insights for safer communities.


### 🔍 Objective
To build a scalable database solution that could store, organize, and analyze data related to women's safety incidents, enabling data-driven interventions and policy decisions.

### 🛠️ My Role & Contributions

- **Database Design with ERDs:**  
  I designed detailed Entity-Relationship Diagrams (ERDs) to structure the database around incident reports, geographic locations, timestamps, risk factors, and response actions.
  
  ![Enity Relationship Diagram](https://github.com/user-attachments/assets/b2170a0c-d31e-4c78-8132-9f2341b6d3f6)

  - The relationship between the Women and Threat Report entities is a one-to-many relationship, where one woman can have multiple threat reports, but each threat report is associated with only one woman.
  - The relationship between the Threat Report and Specific Threat entities is a many-to-one relationship, where many threat reports can reference a single specific threat, but each threat report is associated with only one specific threat.

- **Data Collection & Integration:**  
  Collected, cleaned, and standardized datasets on women’s safety indicators, combining official sources with community-reported data to ensure a comprehensive view.

- **SQL Development in Azure Data Studio:**  
  Built a relational database application using Microsoft Azure Data Studio. I wrote advanced SQL queries to extract trends, identify hotspots, and analyze the effectiveness of response measures.

  
- **Insights for Safer Environments:**  
  The system supports real-time insights and historical trend analysis, helping urban planners, advocacy groups, and policy makers develop interventions that make environments safer for women to live, work, and travel without fear.

### 📈 Impact
The project provided a structured, scalable framework for analyzing safety data and laid the groundwork for data-informed decisions in public safety strategy and community empowerment.

### My code:

```
-- Identifying Top Threat Locations
/*designed to fetch detailed information about threat incidents reported within a specified period. 
It focuses on gathering comprehensive data such as threat descriptions and specifics about the women who
 reported them. This query aims to provide a holistic view of threat activities over time, allowing 
 managers to analyze trends effectively. By identifying recurring patterns and understanding the nature of threats 
 faced, managers can implement targeted preventative measures to enhance security protocols and ensure a safer environment 
 for employees and stakeholders.This proactive approach is crucial for mitigating risks and maintaining a secure workplace environment.*/
WITH cte_ThreatCount AS (
    SELECT 
        loc.Location_ID,
        loc.Address,
        loc.City,
        loc.State,
        loc.Zip_Code,
        loc.Country,
        COUNT(specT.ThreatID) AS ThreatCount
    FROM 
        Location loc
        INNER JOIN Specific_Threat specT ON loc.Location_ID = specT.Location_ID
    GROUP BY 
        loc.Location_ID, 
        loc.Address, 
        loc.City, 
        loc.State, 
        loc.Zip_Code, 
        loc.Country
)
SELECT 
    TOP 5 *
FROM 
    cte_ThreatCount
ORDER BY 
    ThreatCount desc;

-- /* Threat Analysis by Location and Category */
/*The analysis identifies the top 5 locations with the highest number of threats, breaking down the threat categories and their respective counts for each location. 
This information allows managers to prioritize safety measures and resources effectively. By understanding which types of threats are most prevalent in specific
 high-risk areas, managers can implement targeted interventions and security protocols. This approach enables a more efficient allocation of resources, potentially 
 reducing overall risk and improving safety outcomes across the organization. The data-driven insights can inform strategic decision-making, such as where to 
focus training efforts, deploy security personnel, or invest in protective equipment, ultimately enhancing the organization's threat management capabilities.*/


WITH 
cte_countThreat AS (
    SELECT 
        loc.Location_ID,
        loc.Address,
        loc.City,
        loc.State,
        loc.Zip_Code,
        loc.Country,
        COUNT(specT.ThreatID) AS TotalThreats
    FROM 
        Location loc
    INNER JOIN 
        SpecificThreat specT ON loc.Location_ID = specT.Location_ID
    GROUP BY 
        loc.Location_ID, 
        loc.Address, 
        loc.City, 
        loc.State, 
        loc.Zip_Code, 
        loc.Country
),
cte_topLocations AS (
    SELECT 
        TOP 5 Location_ID, 
        Address, 
        City, 
        State, 
        Zip_Code,
        Country, 
        TotalThreats
    FROM 
        cte_countThreat
    ORDER BY 
        TotalThreats desc
)
SELECT 
    topL.Location_ID,
    topL.Address,
    topL.City,
    topL.State,
    topL.Zip_Code,
    topL.Country,
    threatC.Category_Name,
    COUNT(specT.ThreatID) AS ThreatCountPerCategory
FROM 
    cte_topLocations topL
INNER JOIN 
    SpecificThreat specT ON topL.Location_ID = specT.Location_ID
INNER JOIN 
    ThreatCategory threatC ON specT.Category_ID = threatC.Category_ID
GROUP BY 
    topL.Location_ID, 
    topL.Address, 
    topL.City, 
    topL.State, 
    topL.Zip_Code, 
    topL.Country, 
    threatC.Category_Name
ORDER BY 
    --topL.TotalThreats desc, 
    ThreatCountPerCategory desc;

-- Help Sources and Employment Analysis--
/*stored procedure aims to retrieve detailed threat reports within a specified period. It includes comprehensive information such as threat descriptions, 
details of the women reporting them, the count of threats per location, and categorization by threat types. The query provides a holistic view of threats, 
offering insights into threat distribution across locations and trends over time. Managers can utilize this data to analyze patterns, identify recurring threats,
and proactively implement targeted measures to enhance security and mitigate risks effectively. 
This approach supports strategic decision-making aimed at fostering a safer and more secure environment for all stakeholders involved.*/

CREATE PROCEDURE ComprehensiveThreatReports
    @StartDate DATE,
    @EndDate DATE
AS
BEGIN
    ;WITH threatSummary AS (
        SELECT 
            threatR.ReportID,
            threatR.Report_Date,
            threatR.ReportDescription,
            specT.Threat_Description,
            specT.Threat_Date,
            specT.Threat_Time,
            wom.first_name,
            wom.last_name,
            wom.Marital_Status,
            wom.DOB,
            loc.Address,
            loc.City,
            loc.State,
            loc.Zip_Code,
            loc.Country,
            threatC.Category_Name,
            COUNT(specT.ThreatID) OVER (PARTITION BY loc.Location_ID) AS ThreatCountPerLocation
        FROM 
            ThreatReport threatR
        INNER JOIN 
            SpecificThreat specT ON threatR.ThreatID = specT.ThreatID
        INNER JOIN 
            Women wom ON threatR.id = wom.id
        INNER JOIN 
            Location loc ON specT.Location_ID = loc.Location_ID
        INNER JOIN 
            ThreatCategory threatC ON specT.Category_ID = threatC.Category_ID
        WHERE 
            threatR.Report_Date BETWEEN @StartDate AND @EndDate
    )
    SELECT 
        ReportID,
        Report_Date,
        ReportDescription,
        Threat_Description,
        Threat_Date,
        Threat_Time,
        first_name,
        last_name,
        Marital_Status,
        DOB,
        Address,
        City,
        State,
        Zip_Code,
        Country,
        Category_Name,
        ThreatCountPerLocation,
        (SELECT COUNT(*) FROM threatSummary) AS totalThreatWithInDateRange
    FROM 
        threatSummary
    ORDER BY 
        Report_Date desc;
END;

-- /* Query to identify top threat locations */
-- Purpose: Identify the top 5 locations with the highest number of reported threats.
-- Summarize the results: The query will return the location details and the number of threats reported at each location.
-- Managerial implications: Managers can use this information to allocate more resources or implement safety measures in high-risk areas.

WITH cte_ThreatCount AS (
    SELECT 
        loc.Location_ID,
        loc.Address,
        loc.City,
        loc.State,
        loc.Zip_Code,
        loc.Country,
        COUNT(specT.ThreatID) AS ThreatCount
    FROM 
        Location loc
        INNER JOIN Specific_Threat specT ON loc.Location_ID = specT.Location_ID
    GROUP BY 
        loc.Location_ID, 
        loc.Address, 
        loc.City, 
        loc.State, 
        loc.Zip_Code, 
        loc.Country
)
SELECT 
    TOP 5 *
FROM 
    cte_ThreatCount
ORDER BY 
    ThreatCount desc;
```
