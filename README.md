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

**Table 'product_emissions'**

    id:				Identifier for each product emission record.
    company_id: 			Identifier for the company associated with the product.
    country_id: 			Identifier for the country where the product is being produced.
    industry_group_id: 		Identifier for the industry group to which the product belongs.
    year: 				The year in which the emissions data was recorded.
    product_name: 			The name of the product associated with the emissions data.
    weight_kg: 			The weight of the product in kilograms.
    carbon_footprint_pcf: 		The carbon footprint of the product, measured in CO2 equivalent.
    upstream_percent_total_pcf: 	The percentage of the total carbon footprint attributed to upstream activities.
    operations_percent_total_pcf: 	The percentage of the total carbon footprint attributed to operations.
    downstream_percent_total_pcf: 	The percentage of the total carbon footprint attributed to downstream activities.
 

**Table 'industry_groups'**

    id: 				Unique identifier for each industry group.
    industry_group: 		The name of the industry group, categorizing businesses within similar sectors based on their products or services offered.
 

**Table 'companies'**

    id: 				Unique identifier for each company.
    company_name: 			The name of the company, identifying the specific organization within the dataset.
 

**Table 'countries'**

    id: 				Unique identifier for each country.
    country_name: 			The name of the country.

## 1. Which products contribute the most to carbon emissions? 

    SELECT 
        product_name
        ,sum(carbon_footprint_pcf) as total_carbon_fp
    FROM 
    	product_emissions
    GROUP BY 
    	product_name
    ORDER BY 
    	total_carbon_fp desc
    LIMIT 
    	5
    ;

| product_name                 | total_carbon_fp | 
| ---------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | 3718044         | 
| Wind Turbine G132 5 Megawats | 3276187         | 
| Wind Turbine G114 2 Megawats | 1532608         | 
| Wind Turbine G90 2 Megawats  | 1251625         | 
| TCDE                         | 198150          | 

  **--> Wind Turbine is the products contribute the most to carbon emissions**
    
## 2. What are the industry groups of these products?
    SELECT 
        p.product_name
        ,ind.industry_group
        ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM 
    	product_emissions as p
    LEFT JOIN 
    	industry_groups as ind
    ON 
    	p.industry_group_id = ind.id
    GROUP BY 
    	p.product_name, ind.industry_group
    ORDER BY 
    	total_carbon_fp desc
    LIMIT 
    	5
    ;


| product_name                 | industry_group                     | total_carbon_fp | 
| ---------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 3718044         | 
| Wind Turbine G132 5 Megawats | Electrical Equipment and Machinery | 3276187         | 
| Wind Turbine G114 2 Megawats | Electrical Equipment and Machinery | 1532608         | 
| Wind Turbine G90 2 Megawats  | Electrical Equipment and Machinery | 1251625         | 
| TCDE                         | Materials                          | 198150          | 

  **--> Electrical Equipment and Machinery is the industry groups of these products**

## 3. What are the industries with the highest contribution to carbon emissions?
    SELECT 
        p.product_name
        ,ind.industry_group
        ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM 
    	product_emissions as p
    LEFT JOIN 
    	industry_groups as ind
    ON 	
    	p.industry_group_id = ind.id
    GROUP BY 
    	p.product_name,ind.industry_group
    ORDER BY 	
    	total_carbon_fp desc
    LIMIT 
    	1
    ;


| product_name                 | industry_group                     | total_carbon_fp | 
| ---------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 3718044         | 

  **--> Electrical Equipment and Machinery is he industries with the highest contribution to carbon emissions**
  
***In which: In the industry group of Electrical Equipment and Machinery, the product related to Wind Turbine is the main cause of carbon emissions***

    SELECT 
        p.product_name
        ,ind.industry_group
        ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM 
    	product_emissions as p
    LEFT JOIN 
    	industry_groups as ind
    ON 
    	p.industry_group_id = ind.id
    WHERE 
    	ind.industry_group ='Electrical Equipment and Machinery'
    GROUP BY 
    	p.product_name,ind.industry_group
    ORDER BY 
    	total_carbon_fp desc
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
    FROM 
    	product_emissions as p
    LEFT JOIN 
    	companies as com
    ON 
    	p.company_id = com.id
    GROUP BY 
    	p.product_name,com.company_name
    ORDER BY 
    	total_carbon_fp desc
    LIMIT 
    	1
    ;


| product_name                 | company_name                           | total_carbon_fp | 
| ---------------------------: | -------------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats | "Gamesa Corporación Tecnológica, S.A." | 3718044         | 

  **--> Gamesa Corporación Tecnológica, S.A. is the companies with the highest contribution to carbon emissions**

## 5. What are the countries with the highest contribution to carbon emissions?

    SELECT 
        country.country_name
        ,sum(p.carbon_footprint_pcf) as total_carbon_fp
    FROM 
    	product_emissions as p
    LEFT JOIN 
    	countries as country
    ON 
    	p.country_id = country.id
    GROUP BY 
    	country.country_name
    ORDER BY 
    	total_carbon_fp desc
    LIMIT 
    	5
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
    FROM 
      	product_emissions
    GROUP BY 
      	year
    ORDER BY 
     	year asc
    ;


| year | total_carbon_fp | 
| ---: | --------------: | 
| 2013 | 503857          | 
| 2014 | 624226          | 
| 2015 | 10840415        | 
| 2016 | 1640182         | 
| 2017 | 340271          |

![image](https://github.com/user-attachments/assets/844ed99d-2167-4ca4-bbc1-668f5381b9ab)


**-->The total_carbon_fp seem to vary significantly from year to year, with a notable increase in 2015 - 10840415 pcf compared to the other years**

## 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?


**First of all, we have table named "base" that calculates the total carbon footprint per capita for each industry group in a given year and determines the trend (increase, decrease, or stable) based on the change in carbon footprint compared to the previous year**

    SELECT 
      a.*, 
      CASE 
        WHEN a.total_carbon_fp - a.carbon_fp_change > 0 THEN 'increase'
   		   WHEN a.total_carbon_fp - a.carbon_fp_change < 0 THEN 'decrease'
        ELSE 'stable' 
      END AS trend   
      FROM
  	 (SELECT 
        	 ind.industry_group
        	 ,p.year
          	 ,SUM(p.carbon_footprint_pcf) as total_carbon_fp
        	 ,LAG(SUM(p.carbon_footprint_pcf)) OVER (PARTITION BY ind.industry_group ORDER BY p.year) AS carbon_fp_change
	  FROM 
        	 product_emissions as p
        	LEFT JOIN 
       	 	industry_groups as ind ON p.industry_group_id = ind.id
	  GROUP BY 
        	 ind.industry_group, p.year
	  ORDER BY 
        	 ind.industry_group, p.year
	  ) a


| industry_group                                                         | year | total_carbon_fp | carbon_fp_change | trend    | 
| ---------------------------------------------------------------------: | ---: | --------------: | ---------------: | -------: | 
| "Consumer Durables, Household and Personal Products"                   | 2015 | 931             | [NULL]           | stable   | 
| "Food, Beverage & Tobacco"                                             | 2013 | 4995            | [NULL]           | stable   | 
| "Food, Beverage & Tobacco"                                             | 2014 | 2685            | 4995             | decrease | 
| "Food, Beverage & Tobacco"                                             | 2015 | 0               | 2685             | decrease | 
| "Food, Beverage & Tobacco"                                             | 2016 | 100289          | 0                | increase | 
| "Food, Beverage & Tobacco"                                             | 2017 | 3162            | 100289           | decrease | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 2015 | 8909            | [NULL]           | stable   | 
| "Mining - Iron, Aluminum, Other Metals"                                | 2015 | 8181            | [NULL]           | stable   | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2013 | 32271           | [NULL]           | stable   | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2014 | 40215           | 32271            | increase | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 2015 | 387             | [NULL]           | stable   | 
| Automobiles & Components                                               | 2013 | 130189          | [NULL]           | stable   | 
| Automobiles & Components                                               | 2014 | 230015          | 130189           | increase | 
| Automobiles & Components                                               | 2015 | 817227          | 230015           | increase | 
| Automobiles & Components                                               | 2016 | 1404833         | 817227           | increase | 
| Capital Goods                                                          | 2013 | 60190           | [NULL]           | stable   | 
| Capital Goods                                                          | 2014 | 93699           | 60190            | increase | 
| Capital Goods                                                          | 2015 | 3505            | 93699            | decrease | 
| Capital Goods                                                          | 2016 | 6369            | 3505             | increase | 
| Capital Goods                                                          | 2017 | 94949           | 6369             | increase | 
| Chemicals                                                              | 2015 | 62369           | [NULL]           | stable   | 
| Commercial & Professional Services                                     | 2013 | 1157            | [NULL]           | stable   | 
| Commercial & Professional Services                                     | 2014 | 477             | 1157             | decrease | 
| Commercial & Professional Services                                     | 2016 | 2890            | 477              | increase | 
| Commercial & Professional Services                                     | 2017 | 741             | 2890             | decrease | 
| Consumer Durables & Apparel                                            | 2013 | 2867            | [NULL]           | stable   | 
| Consumer Durables & Apparel                                            | 2014 | 3280            | 2867             | increase | 
| Consumer Durables & Apparel                                            | 2016 | 1162            | 3280             | decrease | 
| Containers & Packaging                                                 | 2015 | 2988            | [NULL]           | stable   | 
| Electrical Equipment and Machinery                                     | 2015 | 9801558         | [NULL]           | stable   | 
| Energy                                                                 | 2013 | 750             | [NULL]           | stable   | 
| Energy                                                                 | 2016 | 10024           | 750              | increase | 
| Food & Beverage Processing                                             | 2015 | 141             | [NULL]           | stable   | 
| Food & Staples Retailing                                               | 2014 | 773             | [NULL]           | stable   | 
| Food & Staples Retailing                                               | 2015 | 706             | 773              | decrease | 
| Food & Staples Retailing                                               | 2016 | 2               | 706              | decrease | 
| Gas Utilities                                                          | 2015 | 122             | [NULL]           | stable   | 
| Household & Personal Products                                          | 2013 | 0               | [NULL]           | stable   | 
| Materials                                                              | 2013 | 200513          | [NULL]           | stable   | 
| Materials                                                              | 2014 | 75678           | 200513           | decrease | 
| Materials                                                              | 2016 | 88267           | 75678            | increase | 
| Materials                                                              | 2017 | 213137          | 88267            | increase | 
| Media                                                                  | 2013 | 9645            | [NULL]           | stable   | 
| Media                                                                  | 2014 | 9645            | 9645             | stable   | 
| Media                                                                  | 2015 | 1919            | 9645             | decrease | 
| Media                                                                  | 2016 | 1808            | 1919             | decrease | 
| Retailing                                                              | 2014 | 19              | [NULL]           | stable   | 
| Retailing                                                              | 2015 | 11              | 19               | decrease | 
| Semiconductors & Semiconductor Equipment                               | 2014 | 50              | [NULL]           | stable   | 
| Semiconductors & Semiconductor Equipment                               | 2016 | 4               | 50               | decrease | 
| Semiconductors & Semiconductors Equipment                              | 2015 | 3               | [NULL]           | stable   | 
| Software & Services                                                    | 2013 | 6               | [NULL]           | stable   | 
| Software & Services                                                    | 2014 | 146             | 6                | increase | 
| Software & Services                                                    | 2015 | 22856           | 146              | increase | 
| Software & Services                                                    | 2016 | 22846           | 22856            | decrease | 
| Software & Services                                                    | 2017 | 690             | 22846            | decrease | 
| Technology Hardware & Equipment                                        | 2013 | 61100           | [NULL]           | stable   | 
| Technology Hardware & Equipment                                        | 2014 | 167361          | 61100            | increase | 
| Technology Hardware & Equipment                                        | 2015 | 106157          | 167361           | decrease | 
| Technology Hardware & Equipment                                        | 2016 | 1566            | 106157           | decrease | 
| Technology Hardware & Equipment                                        | 2017 | 27592           | 1566             | increase | 
| Telecommunication Services                                             | 2013 | 52              | [NULL]           | stable   | 
| Telecommunication Services                                             | 2014 | 183             | 52               | increase | 
| Telecommunication Services                                             | 2015 | 183             | 183              | stable   | 
| Tires                                                                  | 2015 | 2022            | [NULL]           | stable   | 
| Tobacco                                                                | 2015 | 1               | [NULL]           | stable   | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 2015 | 239             | [NULL]           | stable   | 
| Utilities                                                              | 2013 | 122             | [NULL]           | stable   | 
| Utilities                                                              | 2016 | 122             | 122              | stable   | 


**Then selects all industry_group from the "base" table where the industry group is in a subquery that filters industry groups based on the count of trends labeled as "decrease". Thanks to that identify industry groups where the trend in carbon footprint is predominantly decreasing**
     
     WITH base as
    (
 	SELECT 
    		a.*, 
    	CASE 
        	WHEN a.total_carbon_fp - a.carbon_fp_change > 0 THEN 'increase'
   		WHEN a.total_carbon_fp - a.carbon_fp_change < 0 THEN 'decrease'
        	ELSE 'stable' 
    	END AS trend   
	FROM
  		(SELECT 
        		ind.industry_group
        		,p.year
        		,SUM(p.carbon_footprint_pcf) as total_carbon_fp
        		,LAG(SUM(p.carbon_footprint_pcf)) OVER (PARTITION BY ind.industry_group ORDER BY p.year) AS carbon_fp_change
    		FROM 
        		product_emissions as p
    		LEFT JOIN 
       	 		industry_groups as ind ON p.industry_group_id = ind.id
    		GROUP BY 
        		ind.industry_group, p.year
   		ORDER BY 
        		ind.industry_group, p.year
   		) a
    )

    SELECT 
		industry_group
		,year
		,total_carbon_fp
    FROM
		base 
    WHERE 
		industry_group 
    IN
	(
	SELECT 
	  b.industry_group
	FROM
		(
		SELECT 
		  	de.industry_group
		  	,tl.count_trend

		FROM
 			(
			SELECT 
			 	industry_group
			 	,trend
			FROM base 
			WHERE trend ='decrease'
			GROUP BY industry_group,trend) de

		LEFT JOIN

			(
			SELECT 
			  	a.industry_group
			  	,count(a.industry_group) as count_trend
			FROM
  				(
				 SELECT
				 	industry_group
				 	,trend
				FROM base 
				GROUP BY industry_group,trend) a
			GROUP BY a.industry_group) tl
		ON de.industry_group = tl.industry_group
	HAVING count_trend <3) b
	)


| industry_group                           | year | total_carbon_fp | 
| ---------------------------------------: | ---: | --------------: | 
| Food & Staples Retailing                 | 2014 | 773             | 
| Food & Staples Retailing                 | 2015 | 706             | 
| Food & Staples Retailing                 | 2016 | 2               | 
| Media                                    | 2013 | 9645            | 
| Media                                    | 2014 | 9645            | 
| Media                                    | 2015 | 1919            | 
| Media                                    | 2016 | 1808            | 
| Retailing                                | 2014 | 19              | 
| Retailing                                | 2015 | 11              | 
| Semiconductors & Semiconductor Equipment | 2014 | 50              | 
| Semiconductors & Semiconductor Equipment | 2016 | 4               | 


![image](https://github.com/user-attachments/assets/9c9f9d06-9317-47b0-958a-c36e46f5934f)

![image](https://github.com/user-attachments/assets/76a553c5-06c6-43d4-baee-41a1e1caccf7)

![image](https://github.com/user-attachments/assets/5321d7c7-e249-4e08-af5d-37e089141565)


**-->The data and charts indicate that the industry groups of Media, Food & Staples Retailing, and Retailing have shown significant decreases in carbon footprints (PCFs) over time.
However, this does not say whether these industries are trying to reduce the amount of CO2 because I have not analyzed the volume of each product that these industries produce each year**

**BUT I AM LAZYYYY SO MAYBE I WILL ANALYZE THE PROBLEM ABOVE SOMEDAY. OR IF YOU CAN, YOU CAN SHARE THE RESULT HERE]**
