[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Maskar/chicago_traffic_crashes/HEAD)

# Chicago Traffic Crashes Analysis

This study creates models to predict the seriousness of car crashes using 2019 and 2020 crash reports from the publicly accessable database maintained by the Chicago Police Department. A car crash is considered serious if the crash results in an injury or the car is towed due to the crash. Models use categorical features that describe conditions at the time of the crash and crash causes to predict the required target. The current focus is to classify whether a crash results in an injury. All models are trained, validated, and tested on randomly split 2019 crash reports. The best model (along with all others) are then tested using the full set of 2020 crash reports.


## Data Source

The notebooks' sequence starts with the direct raw download of data from the **Chicago data portal** in notebook **01_cleaning**, subsequent notebooks read from the output file created in the cleaning notebook **crash_df.parquet** and might apply further steps after that.


We work with a snapshot downloaded on 2021-02-28, but the whole process is expected to be re-executable without any issues as in later stages we filter down the working data to include the years of 2018, 2019, and 2020, which are all in the past, so we do not expect them to change.

Also, we write out the data into Parquet format to read and write the massive amount of data faster.

We kept the online download code commented to prevent slowing down the process once we have the local copies downloaded.

## Exploratory Data Analysis

The data used for the study is maintained by the City of Chicago at the Chicago Data Portal: https://data.cityofchicago.org/ .  
  
We apply modeling on counts of traffic crashes per location based on a large set of features:  
(https://data.cityofchicago.org/Transportation/Traffic-Crashes-Crashes/85ca-t3if ).  
  
The dataset is updated daily with new crash reports provided by the Chicago Police Department (CPD) electronic crash reporting system (E-Crash). Data is limited to crashes for which the Chicago Police Department is the responding police agency. Thus, reports are not provided for crashes on the interstate highways that go through Chicago. While data starts in 2015 for some police districts, citywide crash data begins in September 2017. We decided to limit data analysis to 2018-2020 reports as these reports are complete and cover both pre-covid and covid traffic crashes.

Modeling is done on 2019 data and tested on 2020 data.

**Total records downloaded (2018~2020):**  
* 328,790 records

**Total records used (2019-2020):**  
* 116,723 + 91,332 = 208,055 records  

**Target Features (2019 ratio):**  
* has_injuries (0.14)

**Features:**  

    Data columns (total 26 columns):
     #   Column                   Non-Null Count   Dtype         
    ---  ------                   --------------   -----         
     0   crash_date               478484 non-null  datetime64[ns]
     1   crash_year               478484 non-null  int16         
     2   crash_month              478484 non-null  int8          
     3   crash_day_of_week        478484 non-null  int8          
     4   crash_hour               478484 non-null  int8          
     5   crash_time_of_day        478484 non-null  category      
     6  *latitude                 478484 non-null  float32       
     7  *longitude                478484 non-null  float32       
     8  *beat_of_occurrence       478484 non-null  int64         
     9  *address                  478484 non-null  string        
     10 *street_no                478484 non-null  category      
     11 *street_direction         478484 non-null  category      
     12 *street_name              478484 non-null  category      
     13  posted_speed_limit       478484 non-null  int64         
     14  traffic_control_device   478484 non-null  category      
     15  device_condition         478484 non-null  category      
     16  weather_condition        478484 non-null  category      
     17  lighting_condition       478484 non-null  category      
     18  trafficway_type          478484 non-null  category      
     19  alignment                478484 non-null  category      
     20  roadway_surface_cond     478484 non-null  category      
     21  road_defect              478484 non-null  category      
     22  first_crash_type         478484 non-null  category      
     23  prim_contributory_cause  478484 non-null  category      
     24  sec_contributory_cause   478484 non-null  category      
     25  num_units                478484 non-null  int8          
    dtypes: category(15), datetime64[ns](1), float32(2), int16(1), int64(2), int8(4), string(1)

(*) starred features were not all used during the analysis, they were experimented with on and off
    
    
After cleaning is performed in **01_cleaning**, EDA is applied in two steps. The first step is to generate an overview of the whole data to undertstand the data distributions per feature. Also, to find obvious relations and patterns using correlations. This part is auto generated using SweetViz after performing the data cleaning step to generate a consolidated overview report. The report can be found in **02_report_sweetviz**.  

The second step of the EDA is more focused and in depth. A set of tabulations, aggregations, and detailed visualizations are created to uncover any possible latent information that is not revealed in the first general step of EDA. The outcomes can be found in **03_analysis_m**.

The most interesting observation was to find that all the target variables of interest are highly correlated, and each of them is highly imbalanced.


## References

City of Chicago, Traffic Crashes - Crashes, Chicago Data Portal, updated daily from https://data.cityofchicago.org/Transportation/Traffic-Crashes-Crashes/85ca-t3if. 


## Reproduction Steps

Each notebook lists the libraries and print out the versions used by each, you can refer to the html export for the actual ones used during our investigations.

**Modules used:**  
> python 3.9.2  
> numpy 1.20.1  
> pandas 1.1.4  
> sklearn 0.24.1  
> imblearn 0.8.0  
> klib 0.1.5  
> sweetviz 2.0.9  

**Machine used:**
* MacBook Pro (16-inch, 2019)
    * MacOS Big Sur 11.2.3
    * 2.3 GHz 8-Core Intel Core i9
    * 32 GB RAM
    
   
