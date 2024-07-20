# carbon_emission_analysis
This report aims to analyze carbon emissions to examine the carbon footprint across various industries
## Which products contribute the most to carbon emissions
    SELECT product_name
    ,sum(carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions
    GROUP BY product_name
    order by total_carbon_fp desc
    LIMIT 5

product_name
total_carbon_fp
Wind Turbine G128 5 Megawats
3718044
Wind Turbine G132 5 Megawats
3276187
Wind Turbine G114 2 Megawats
1532608
Wind Turbine G90 2 Megawats
1251625
TCDE
198150

    

## What are the industry groups of these products?


## What are the industries with the highest contribution to carbon emissions?


## What are the companies with the highest contribution to carbon emissions?


## What are the countries with the highest contribution to carbon emissions?


## What is the trend of carbon footprints (PCFs) over the years?


## Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
