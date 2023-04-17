![score](image/skyline_energy_grades.png "header")

# Problem Statement
Energy usage prediction is important for New York City (NYC) for several reasons:<br>

Grid Planning and Operation: NYC has a complex energy grid that requires careful planning and operation to ensure a reliable and stable energy supply. Accurate energy usage predictions can help grid operators anticipate and manage changes in demand, plan for energy generation and distribution, and optimize the operation of the grid. This helps ensure that there is enough energy available to meet the city's needs while minimizing the risk of blackouts or other grid-related issues.<br>

Energy Policy and Regulation: Energy usage predictions can inform energy policy and regulation in NYC. Accurate forecasts of energy demand can help policymakers and regulators set appropriate targets, standards, and regulations related to energy efficiency, renewable energy, emissions reductions, and other energy-related policies. This can support evidence-based decision-making and effective policy implementation to achieve NYC's energy and climate goals.<br>

Resource Allocation and Investment: Energy usage predictions can guide resource allocation and investment decisions in NYC. By understanding how energy demand is likely to evolve, utilities, businesses, and investors can make informed decisions about where to allocate resources, such as energy infrastructure investments, renewable energy projects, and energy efficiency programs. This can support strategic planning and optimize resource utilization, leading to more cost-effective and sustainable energy solutions.<br>

Building Energy Management: NYC has a large building stock, and accurate energy usage predictions can help building owners and managers optimize energy management strategies. Predictions can inform decisions related to building operations, maintenance, and retrofit investments, enabling better energy management practices, reducing energy waste, and improving energy performance of buildings.<br>

Climate Resilience Planning: Energy usage predictions can also support climate resilience planning in NYC. Understanding future energy demand can help identify vulnerabilities and risks related to energy supply and demand during extreme weather events or other climate-related disruptions. This information can be used to develop strategies to enhance the resilience of NYC's energy infrastructure and systems to climate impacts.<br> [source](https://www.nyc.gov/site/sustainability/codes/energy-benchmarking.page)<br>

This project aims to develop a machine learning model that can predict annual energy usage per square feet in new york city using 2021 NYC buildings data.<br>


# Data Description
The dataset used in this analysis consists of 29842 NYC properties and 243 features (both continuos numeric and discrete and categorical):<br>
*[`main_data`]('./data/nyc-bldg2021.csv'): New York Ciry's energy use 2021.[source](https://www.urbangreencouncil.org/what-we-do/exploring-nyc-building-data/)<br>

After cleaning the datasets, two final datasets were created:<br>
*[`eda_data`]('./data/nyc-energy-usage-2021.csv'): Dataset to perfom Exploratory Data Analysis.
*[`model_data`]('./data/nyc_bldg2021_model_ready.csv'): Dataset to predict energy usage per square feet of properties.


## Data Dictionary

Here is an overview of features, alongside data types and descriptions of the data used in this analysis:<br>

|Feature|Type|Description|
|-------|----|-------|
|property_id|intiger|Property ID|
|postal_code|intiger|Area zipcode|
|borough|category|NYC five boroughs| 
|year_built|intiger|Built year of Property| 
|property_type|category|Property type| 
|longitude|continuous|Geographic coordinates| 
|latitude|continuous|Geographic coordinates| 
|metered_areas_energy|catogery|The areas within the building are covered by energy meters| 
|multifamily_pop_type|category|Type of housing that applies to the majority (more than 50%) of the resident| 
|occupancy100|category|Whether the property is fully occupied or not| 
|number_of_energy_meters_cat|category|Number of energy meters in the porperty| 
|estimated_gas_usage|category|Whether the property calculates estimated gas use in its bill| 
|estimated_electricity_usage|category|Whether the property calculates estimated electricity use in its bill| 
|subsidized_housing|category|Whether the property is a subsidized housing| 
|energy_use|intiger|energy use of property per area (KBtu/SqFt)| 
|green_electricity_percent|intiger|Electricity portion of energy usage in percent| 
|multifamily_cooled_area|intiger|Total percentage of property that can be cooled by mechanical equipment| 
|multifamily_heated_area|intiger|Total percentage of property that can be heated by mechanical equipment| 
|energy_score|catogery|Proximity of how well your property is performing relative to similar properties| 
|multifamily_gfa|intiger|Gross floor area of multifamily housing| 
|property_gfa|intiger|Gross floor area of other property types|


## Approach

The approach for this project will involve the following steps:<br>

Data Preprocessing: Cleaning and preprocessing the data to prepare it for machine learning modeling.<br>

Exploratory Data Analysis (EDA): Visualizing the data to understand its patterns and trends, and to identify any outliers, missing values or correlations between variables.<br>

Feature Engineering: Creating new features, standardizing numeric features and encoding categorical features, to improve the performance of the machine learning model.<br>

Model Selection: Selecting the appropriate machine learning algorithm, based on the characteristics of the data and the problem statement.<br>

Model Training: Training the selected model on the training data, using appropriate hyperparameters.<br>

Model Evaluation: Evaluating the performance of the model on the validation data, using appropriate metrics.<br>

Hyperparameter Tuning: Tuning the hyperparameters of the model, to optimize its performance on the validation data.<br>

Final Model: Training the final model on the entire dataset, using the optimal hyperparameters.<br>

Making Predictions: Using the final model to make predictions for the energy usage of NYC buildings in 2021.<br>

Statistical Inferences:To assess whether the observed relationship is statistically significant and not entirely explained by chance events due to random sampling.<br>


## Executive Summary
The first step in this analysis was cleaning the dataset and dropping any unnecessary columns. This included dropping several columns which had alot of missing values. The next step was organizing the features into categorical and continuous variables for upcoming feature engineering in the model. During the data cleaning process it was noticed that some features such as 'occupancy', 'number_of_energy_meters'have too many zeros. Therefore, I decided to redefine these feature into categorical features. 

The second step of the analysis involved visualizing the dataset using histograms, map plot and scatterplots. In this step, I decided which features to keep for modeling and which features could be dropped. After primary cleaning of the data and choosing the features, distribution of the selected features were plotted. The distribution plots show two of the features are extremely skewed. The log value of 'property_gfa' and 'multifamily_gfa' are used to normalize their distribution. (For some insight about data, please refer to Apendix, at the end of this report.)<br>

After preprocessing the features, the numeric features were standardized and the categorical features were encoded. Three different regression models of Multi-Linear, Ridge, and LASSO regression alongside with Random Forest model were used to analyze the features and their interactions. <br>

The outcome of the models come as below:

|       |      OLS |   Ridge CV |   Lasso CV |   Random Forest |
|:------|---------:|-----------:|-----------:|----------------:|
| Train | 0.597031 |   0.58597  |   0.583781 |        0.726539 |
| Test  | 0.581402 |   0.582202 |   0.583621 |        0.680125 |

Random Forest Regression model offers the best $R^2$ on both train and testing sets, while the other regression models have similar scores slightly lower than Random Forest. <br>

The table below represents the first 10 features which cause the most increase in energy consumption. As we can see, the interaction of energy score with geographic coordinations and gross floor area of a property have the most effect on prediction of energy consumption. <br>

|    | feature                           |   coefficient |
|---:|:----------------------------------|--------------:|
| 38 | energy_score Longitude            |     0.53038   |
| 36 | energy_score Latitude             |     0.11887   |
| 12 | property_gfa energy_score         |     0.0394379 |
| 20 | multifamily_gfa energy_score      |     0.0259204 |
| 47 | multifamily_heated_area Longitude |     0.0239079 |
| 41 | building_age Latitude             |     0.0195851 |
| 50 | Latitude Longitude                |     0.0183075 |
| 45 | multifamily_heated_area Latitude  |     0.0155142 |
| 53 | Longitude^2                       |     0.0143418 |
| 43 | building_age Longitude            |     0.0138727 |


# Statistical Inferences of Linear Regression Model
For every one percentage point increase "green_electricity_percent" feature, energy use will increase by 0.004 KBtu/Ft, which is an unexpected result.<br>

Older buildings consume more energy per area and it is expected,on average, when a building is older by one year its energy use per area is 0.8 KBtu.Sqft higher. This effect is nonlinear with deminishing marginal effects, as indicated by the negative sign of "building_age^2" feature.<br>

Also from the interaction effects between "building_age" and geographic coordinates ("latitude" and "longtitude"), we can infere that the effect is location dependent. Such that, building_age has smaller effect as we go toward north and larger effect as we go far east.<br>

Being located in Staten Island, there is a strong indicator of high energy use per area. This may be due to the fact that a lot of energy intesive buildings (such as power plants) are located in Staten Island.<br>

Buildings categorized as subsidized housing have higher energy use on average by 6.32 KBtu/SqFt. One possible explanation for this observation could be that there is less investment on energy efficiency (for instance upgraded technology, insulation and so on) which are normally expensive and less affordable.

# Conclusion
In summary, energy usage prediction is important for NYC as it can support grid planning and operation, energy policy and regulation, resource allocation and investment decisions, building energy management, and climate resilience planning. Accurate predictions of energy usage for the next year can facilitate effective energy management, resource planning, renewable energy integration, energy efficiency planning, and climate change mitigation efforts, benefiting various stakeholders including businesses, households, utility companies, and policymakers.<br>

The energy usage of buildings in NYC varies widely depending on factors such as age, size, and energy score of a building. Older buildings tend to use more energy than newer buildings, and larger buildings tend to use more energy than smaller ones. Additionally, buildings that are used for commercial purposes, such as offices and retail spaces, tend to use more energy than residential buildings.<br>




# Recommendations
Energy usage of a building depends on alot of factors.
Predicting energy usage of a building using the current dataset is challenging because this dataset contains few explanatory variables.  
Also to have a better prediction model of energy usage, gathering data of buildings for several years could be beneficial. Prior years usage of energy can predict how much a biuilding consumes in following years.<br>

One of the recommendations that we can offer is based on the findings on the effect of building age and energy consumption. Given the facts that more than 75 percent of the buildings in NYC age over 50 years, and that energy consumption increases with building age, one practical policy could be to upgrade older buildings to make them in par with newer ones. We can estimate the reduction in energy consumption coming from such upgrades based on the result of our model.<br>

Based on the result of our model it seems paradoxically greener buildings consume more energy. There are at least two explanation for it; first is that the causality may go the other way. That is buildings with high energy consumption like industrial buildings also tend to have a higher percentage of their energy  from green sources. Another explanation is that some explanatory variables are missing from our model and as a result the marginal effect of green electricity percent is not estimated accurately and appears with the wrong sign. So we can not confidently offer any recommendation when it comes focusing on increasing the percentage of green energy in buildings energy consumption.<br>

Ultimately requiring all the buildings to annually disclose their energy consumption and emissions data will generate many more data points which will allow researchers to build more accurate models and make better predictions than what we currently can. This information also helps the city make more informed decision .<br>

## Apendix
1. Distribution of buildings in five boroughs:
<div class='tableauPlaceholder' id='viz1681494430186' style='position: relative'><noscript><a href='#'><img alt='dash1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;da&#47;dash11_16814856149470&#47;dash1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='dash11_16814856149470&#47;dash1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;da&#47;dash11_16814856149470&#47;dash1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='filter' value='publish=yes' /></object></div>            

<br>
2. Energy use average per square feet by borough:
<div class='tableauPlaceholder' id='viz1681440863645' style='position: relative'><noscript><a href='#'><img alt='dash3 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;da&#47;dash3_16814408466140&#47;dash3&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='dash3_16814408466140&#47;dash3' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;da&#47;dash3_16814408466140&#47;dash3&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='filter' value='publish=yes' /></object></div>    

<br>
3. Total carbon emission by borough:
<div class='tableauPlaceholder' id='viz1681607660194' style='position: relative'><noscript><a href='#'><img alt='Dashboard 20 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;da&#47;dash12_16816076397960&#47;Dashboard20&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='dash12_16816076397960&#47;Dashboard20' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;da&#47;dash12_16816076397960&#47;Dashboard20&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='filter' value='publish=yes' /></object></div>                

<br>
4. 
<div class='tableauPlaceholder' id='viz1681485915861' style='position: relative'><noscript><a href='#'><img alt='Energy Use v. Electricity, Gas ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;en&#47;energyusev_elec_gas&#47;EnergyUsev_ElectricityGas&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='energyusev_elec_gas&#47;EnergyUsev_ElectricityGas' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;en&#47;energyusev_elec_gas&#47;EnergyUsev_ElectricityGas&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /></object></div>                