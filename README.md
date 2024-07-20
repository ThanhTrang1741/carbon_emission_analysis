# carbon_emission_analysis
This report aims to analyze carbon emissions to examine the carbon footprint across various industries

## Which products contribute the most to carbon emissions? **** Wind Turbine G128 5 Megawats
    SELECT 
    product_name
    ,sum(carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions
    GROUP BY product_name
    order by total_carbon_fp desc
    LIMIT 5

| product_name                 | total_carbon_fp | 
| ---------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | 3718044         | 
| Wind Turbine G132 5 Megawats | 3276187         | 
| Wind Turbine G114 2 Megawats | 1532608         | 
| Wind Turbine G90 2 Megawats  | 1251625         | 
| TCDE                         | 198150          | 


    

## What are the industry groups of these products? **** Electrical Equipment and Machinery
    SELECT 
    p.product_name
    ,ind.industry_group
    ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN industry_groups as ind
    ON p.industry_group_id = ind.id
    GROUP BY p.product_name,ind.industry_group
    order by total_carbon_fp desc
    LIMIT 5;


| product_name                 | industry_group                     | total_carbon_fp | 
| ---------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 3718044         | 
| Wind Turbine G132 5 Megawats | Electrical Equipment and Machinery | 3276187         | 
| Wind Turbine G114 2 Megawats | Electrical Equipment and Machinery | 1532608         | 
| Wind Turbine G90 2 Megawats  | Electrical Equipment and Machinery | 1251625         | 
| TCDE                         | Materials                          | 198150          | 

## What are the industries with the highest contribution to carbon emissions? **** Electrical Equipment and Machinery
    SELECT 
    p.product_name
    ,ind.industry_group
    ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN industry_groups as ind
    ON p.industry_group_id = ind.id
    GROUP BY p.product_name,ind.industry_group
    order by total_carbon_fp desc
    LIMIT 1;


| product_name                 | industry_group                     | total_carbon_fp | 
| ---------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 3718044         | 

## What are the companies with the highest contribution to carbon emissions? **** Gamesa Corporación Tecnológica, S.A.

    SELECT 
    p.product_name
    ,com.company_name
    ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN companies as com
    ON p.company_id = com.id
    GROUP BY p.product_name,com.company_name
    order by total_carbon_fp desc
    LIMIT 1;


| product_name                 | company_name                           | total_carbon_fp | 
| ---------------------------: | -------------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | "Gamesa Corporación Tecnológica, S.A." | 3718044         | 

## What are the countries with the highest contribution to carbon emissions? **** Spain  

    SELECT 
    p.product_name
    ,country.country_name
    ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN countries as country
    ON p.country_id = country.id
    GROUP BY p.product_name,country.country_name
    order by total_carbon_fp desc
    LIMIT 1;


| product_name                 | country_name | total_carbon_fp | 
| ---------------------------: | -----------: | --------------: | 
| Wind Turbine G128 5 Megawats | Spain        | 3718044         | 

## What is the trend of carbon footprints (PCFs) over the years?

    SELECT
    year
    ,sum(carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions
    GROUP BY year
    ORDER BY year asc
    ;


| year | total_carbon_fp | 
| ---: | --------------: | 
| 2013 | 503857          | 
| 2014 | 624226          | 
| 2015 | 10840415        | 
| 2016 | 1640182         | 
| 2017 | 340271          |

![image](https://github.com/user-attachments/assets/de08e780-941f-4725-8aa8-8e7b4603d8be)

## Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
