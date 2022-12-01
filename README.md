# EPI_Project-Covid19-Death-and-NPI
This is  our EPI 8803 project. We are investigating how Nonpharmaceutical Interventions (NPIs) affect the number of death in each county by week using a statistical approach.  We aim to evaluate the importance of NPIs and potentially give advice on policy administration. \
Also, we include the t-test analysis on demographic factors at the end of the file, in order to investigate the influence of demographic factor and give an insight on the result from the above study. \
Note that although we used the death count in each county to evaluate the relationship between NPIs and death count, the dataset can be potentially replaced to case count and instead study the relationship between NPIs and case count.

## Data Description
#### filename: 
Mortality of US county by date, retrieved from: 

#### 'Policies.csv': 
contains US COVID-19 policies, retrieved from CDC: https://ephtracking.cdc.gov/DataExplorer/?c=33

This file contains information on masking mandate(0: no public musking mandate; 1: public masking mandate) and stay-at-home policies(SAH, on an increasing scle from 1 to 7 corresponding to policies from no order to mandatoru SAH for all people)
    
#### 'county_adjacency.csv': 
information on name,fips code of all US counties and its neighboring counties, retrieved from US census https://www.census.gov/. 
    
## User Manual
## (1) Data Preprocessing
#### process_policy_border_county.ipynb
This file select for border counties that have experienced difference in policy at some time point with their neighboring counties in another state.

Cases were chosen where for both states all border counties are under the same covid policy(for example: state1: all border counties have masking mandates; state2: all border counties does not have masking mandate). 

Input files: refer to Data Description section

(1) Policies.csv

(2) county_adjacency.csv

Output file: 

Files will be used as input for 'NPI_death.ipynb': 

(1) 'mask.csv': counties to test based on whether they adopted public maskingmandates

(2) 'sah_applied_or_not.csv': counties to test based on whether they adopted any SAH orders

(3) 'sah_mandatory_or_not.csv': counties to test based on whether they adopted any mandatory SAH orders 

## (2) Statistical Testing
The pipeline is called 'NPI_death.ipynb'. To run the pipeline, either open it on google colab or in local jupyter notebook. Upload the corresponding files and the pipeline is ready to run.
### Packages need to be installed:
numpy, pandas, os, scipy, io, copy, urllib, json, plotly, matplotlib. \
If you are using colab: google.colab is also needed.
### Function description
#### df_delay(delay_week,mask_df_raw):
Parse the raw dataframe to two dataframe with same or different policy, adding the delayed average death cases. \
input1: delay num in week (integer) \
input2: dataframe of policy (pandas dataframe). (See data description for format) \
output: 2 dataframe where policy is same/different for each pair with the delayed average death case of each cluster
#### ttest(df):
The actual t test function for studying the differences in death cases under same/diff policies. \
input: dataframe for t test. Including fips for sample1, sample2, and index for sample1, sample2 (pandas dataframe) \
output: A new pandas dataframe adding the t test result, critical t value, and p value
####  t_result_parse(same_df, diff_df,c_p):
From the t test result, parse subsets to be True Positive (non-critical to critical (from same to diff)), True Negative (non-critical to non-critical), False Positive (critical to critical), False Negative (critical to non-critical). \
input1: t test result dataframe under same policy. (pandas dataframe) \
input2: t test result dataframe under diff policy. (pandas dataframe) \
input3: critical p value the user chose. \
output: four subset pandas dataframes
#### optimal delay(delay_week,c_p):
This function tunes the number of delay in weeks and the critical p value. User can loop for selected delay_week and c_p to plot a chart with the ratio of TP, TN, FP, FN. \
input1: number of delay (int) \
input2: critical p value for threshold (float) \
output: the TP, TN, FP, FN subset of panda dataframes used for plotting.
#### plot_df(t_test_same,t_test_diff,index):
The helper function when plotting on the usa map. Create a dataframe for the selected instance. The output dataframe is used for shading counties with different colors in the scope of the US.\
input1: the t test result dataframe under same policy. (pandas dataframe) \
input2: the t test result dataframe under diff policy. (pandas dataframe) \
input3: the index number selected from user input. (int) \
output: a dataset containing index, counties, and the corresponding policy index.
