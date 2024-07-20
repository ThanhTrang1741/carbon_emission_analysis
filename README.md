# CARBON_EMISSION_ANALYSIS
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

**Data Source: Where Our Data Comes From**
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

**Data Structure**
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![image](https://github.com/user-attachments/assets/dd528559-bfed-4387-b49e-97177a2d4378)

**Tables' columns description**
*Table 'product_emissions'*
id: Identifier for each product emission record.
company_id: Identifier for the company associated with the product.
country_id: Identifier for the country where the product is being produced.
industry_group_id: Identifier for the industry group to which the product belongs.
year: The year in which the emissions data was recorded.
product_name: The name of the product associated with the emissions data.
weight_kg: The weight of the product in kilograms.
carbon_footprint_pcf: The carbon footprint of the product, measured in CO2 equivalent.
upstream_percent_total_pcf: The percentage of the total carbon footprint attributed to upstream activities.
operations_percent_total_pcf: The percentage of the total carbon footprint attributed to operations.
downstream_percent_total_pcf: The percentage of the total carbon footprint attributed to downstream activities.
 

*Table 'industry_groups'*
id: Unique identifier for each industry group.
industry_group: The name of the industry group, categorizing businesses within similar sectors based on their products or services offered.
 

*Table 'companies'*
id: Unique identifier for each company.
company_name: The name of the company, identifying the specific organization within the dataset.
 

*Table 'countries'*
id: Unique identifier for each country.
country_name: The name of the country.

## 1. Which products contribute the most to carbon emissions? 

    SELECT 
            product_name
            ,sum(carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions
    GROUP BY product_name
    ORDER BY total_carbon_fp desc
    LIMIT 5

| product_name                 | total_carbon_fp | 
| ---------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | 3718044         | 
| Wind Turbine G132 5 Megawats | 3276187         | 
| Wind Turbine G114 2 Megawats | 1532608         | 
| Wind Turbine G90 2 Megawats  | 1251625         | 
| TCDE                         | 198150          | 

  **--> Wind Turbine is the products contribute the most to carbon emissions?**
    
## 2. What are the industry groups of these products?
    SELECT 
          p.product_name
          ,ind.industry_group
          ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN industry_groups as ind
    ON p.industry_group_id = ind.id
    GROUP BY p.product_name, ind.industry_group
    ORDER BY total_carbon_fp desc
    LIMIT 5
    ;


| product_name                 | industry_group                     | total_carbon_fp | 
| ---------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 3718044         | 
| Wind Turbine G132 5 Megawats | Electrical Equipment and Machinery | 3276187         | 
| Wind Turbine G114 2 Megawats | Electrical Equipment and Machinery | 1532608         | 
| Wind Turbine G90 2 Megawats  | Electrical Equipment and Machinery | 1251625         | 
| TCDE                         | Materials                          | 198150          | 

  **--> Electrical Equipment and Machinery is the industry groups of these products?**

## 3. What are the industries with the highest contribution to carbon emissions?
    SELECT 
          p.product_name
          ,ind.industry_group
          ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN industry_groups as ind
    ON p.industry_group_id = ind.id
    GROUP BY p.product_name,ind.industry_group
    ORDER BY total_carbon_fp desc
    LIMIT 1
    ;


| product_name                 | industry_group                     | total_carbon_fp | 
| ---------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 3718044         | 

  **--> Electrical Equipment and Machinery is he industries with the highest contribution to carbon emissions?**
  
***In which: In the industry group of Electrical Equipment and Machinery, the product related to Wind Turbine is the main cause of carbon emissions***

    SELECT 
          p.product_name
          ,ind.industry_group
          ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN industry_groups as ind
    ON p.industry_group_id = ind.id
    WHERE ind.industry_group ='Electrical Equipment and Machinery'
    GROUP BY p.product_name,ind.industry_group
    ORDER BY total_carbon_fp desc
    ;


| product_name                                                                  | industry_group                     | total_carbon_fp | 
| ----------------------------------------------------------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats                                                  | Electrical Equipment and Machinery | 3718044         | 
| Wind Turbine G132 5 Megawats                                                  | Electrical Equipment and Machinery | 3276187         | 
| Wind Turbine G114 2 Megawats                                                  | Electrical Equipment and Machinery | 1532608         | 
| Wind Turbine G90 2 Megawats                                                   | Electrical Equipment and Machinery | 1251625         | 
| Electric Motor                                                                | Electrical Equipment and Machinery | 20008           | 
| Vending Machine                                                               | Electrical Equipment and Machinery | 2268            | 
| T300                                                                          | Electrical Equipment and Machinery | 655             | 
| DC988 Type 11 Cordless Power Drill equipment with rechargeable NiCd batteries | Electrical Equipment and Machinery | 88              | 
| D21710 Type 5 Corded Power Drill                                              | Electrical Equipment and Machinery | 56              | 
| ACTI9 IID K 2P 40A 30MA AC-TYPE RESIDUAL CURRENT CIRCUIT BREAKER              | Electrical Equipment and Machinery | 19              | 
| Cable                                                                         | Electrical Equipment and Machinery | 0               | 

## 4. What are the companies with the highest contribution to carbon emissions?

    SELECT 
          p.product_name
          ,com.company_name
          ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN companies as com
    ON p.company_id = com.id
    GROUP BY p.product_name,com.company_name
    ORDER BY total_carbon_fp desc
    LIMIT 1;


| product_name                 | company_name                           | total_carbon_fp | 
| ---------------------------: | -------------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | "Gamesa Corporación Tecnológica, S.A." | 3718044         | 

  **--> Gamesa Corporación Tecnológica, S.A. is the companies with the highest contribution to carbon emissions**

## 5. What are the countries with the highest contribution to carbon emissions?

    SELECT 
          country.country_name
          ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN countries as country
    ON p.country_id = country.id
    GROUP BY country.country_name
    ORDER BY total_carbon_fp desc
    LIMIT 5
    ;

| country_name | total_carbon_fp | 
| -----------: | --------------: | 
| Spain        | 9786130         | 
| Germany      | 2251225         | 
| Japan        | 653237          | 
| USA          | 518381          | 
| South Korea  | 186965          | 

  **--> Spain, Germany, Japan... are the countries with the highest contribution to carbon emissions**

## 6. What is the trend of carbon footprints (PCFs) over the years?

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

## 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?

    SELECT
          ind.industry_group
          ,p.year
          ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM product_emissions as p
    LEFT JOIN industry_groups as ind
    ON p.industry_group_id = ind.id
    GROUP BY ind.industry_group, p.year
    ORDER BY ind.industry_group, p.year
    ;


| industry_group                                                         | year | total_carbon_fp | 
| ---------------------------------------------------------------------: | ---: | --------------: | 
| "Consumer Durables, Household and Personal Products"                   | 2015 | 931             | 
| "Food, Beverage & Tobacco"                                             | 2013 | 4995            | 
| "Food, Beverage & Tobacco"                                             | 2014 | 2685            | 
| "Food, Beverage & Tobacco"                                             | 2015 | 0               | 
| "Food, Beverage & Tobacco"                                             | 2016 | 100289          | 
| "Food, Beverage & Tobacco"                                             | 2017 | 3162            | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 2015 | 8909            | 
| "Mining - Iron, Aluminum, Other Metals"                                | 2015 | 8181            | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2013 | 32271           | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2014 | 40215           | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 2015 | 387             | 
| Automobiles & Components                                               | 2013 | 130189          | 
| Automobiles & Components                                               | 2014 | 230015          | 
| Automobiles & Components                                               | 2015 | 817227          | 
| Automobiles & Components                                               | 2016 | 1404833         | 
| Capital Goods                                                          | 2013 | 60190           | 
| Capital Goods                                                          | 2014 | 93699           | 
| Capital Goods                                                          | 2015 | 3505            | 
| Capital Goods                                                          | 2016 | 6369            | 
| Capital Goods                                                          | 2017 | 94949           | 
| Chemicals                                                              | 2015 | 62369           | 
| Commercial & Professional Services                                     | 2013 | 1157            | 
| Commercial & Professional Services                                     | 2014 | 477             | 
| Commercial & Professional Services                                     | 2016 | 2890            | 
| Commercial & Professional Services                                     | 2017 | 741             | 
| Consumer Durables & Apparel                                            | 2013 | 2867            | 
| Consumer Durables & Apparel                                            | 2014 | 3280            | 
| Consumer Durables & Apparel                                            | 2016 | 1162            | 
| Containers & Packaging                                                 | 2015 | 2988            | 
| Electrical Equipment and Machinery                                     | 2015 | 9801558         | 
| Energy                                                                 | 2013 | 750             | 
| Energy                                                                 | 2016 | 10024           | 
| Food & Beverage Processing                                             | 2015 | 141             | 
| Food & Staples Retailing                                               | 2014 | 773             | 
| Food & Staples Retailing                                               | 2015 | 706             | 
| Food & Staples Retailing                                               | 2016 | 2               | 
| Gas Utilities                                                          | 2015 | 122             | 
| Household & Personal Products                                          | 2013 | 0               | 
| Materials                                                              | 2013 | 200513          | 
| Materials                                                              | 2014 | 75678           | 
| Materials                                                              | 2016 | 88267           | 
| Materials                                                              | 2017 | 213137          | 
| Media                                                                  | 2013 | 9645            | 
| Media                                                                  | 2014 | 9645            | 
| Media                                                                  | 2015 | 1919            | 
| Media                                                                  | 2016 | 1808            | 
| Retailing                                                              | 2014 | 19              | 
| Retailing                                                              | 2015 | 11              | 
| Semiconductors & Semiconductor Equipment                               | 2014 | 50              | 
| Semiconductors & Semiconductor Equipment                               | 2016 | 4               | 
| Semiconductors & Semiconductors Equipment                              | 2015 | 3               | 
| Software & Services                                                    | 2013 | 6               | 
| Software & Services                                                    | 2014 | 146             | 
| Software & Services                                                    | 2015 | 22856           | 
| Software & Services                                                    | 2016 | 22846           | 
| Software & Services                                                    | 2017 | 690             | 
| Technology Hardware & Equipment                                        | 2013 | 61100           | 
| Technology Hardware & Equipment                                        | 2014 | 167361          | 
| Technology Hardware & Equipment                                        | 2015 | 106157          | 
| Technology Hardware & Equipment                                        | 2016 | 1566            | 
| Technology Hardware & Equipment                                        | 2017 | 27592           | 
| Telecommunication Services                                             | 2013 | 52              | 
| Telecommunication Services                                             | 2014 | 183             | 
| Telecommunication Services                                             | 2015 | 183             | 
| Tires                                                                  | 2015 | 2022            | 
| Tobacco                                                                | 2015 | 1               | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 2015 | 239             | 
| Utilities                                                              | 2013 | 122             | 
| Utilities                                                              | 2016 | 122             | 
