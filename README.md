# Summer-2017

This code was developed during the summer of 2017 for Rivet Labs. The goal of this project was to find a way to determine if two tables uploaded to the system were the same or similar. Initially, we attempted to determine similarity numerically – according to the content in each of the columns of the two tables. For each table, we iterated through every column and selected only the values that could be parsed as real numbers. If those numbers constituted at least 90% of the column’s total values, the column was considered “numeric” and was used to determine numerical similarity by looking at column similarity methods. 

This was done first using the column’s summary statistics (mean, max, min, range, standard deviation), then by determining each column’s best fit distribution, and later using Welsh’s t-test. Each of these tests failed to show similarity due to variance between tables, even when the data was drawn from the exact same distribution (ie as part of a time series). Because of these results, we were forced to reject the notion that statistical methods such as the t-test are accurately able to give the probability that the columns were drawn from the same distribution and are therefore similar. 


Best fit distributions for data taken quarterly for two quarters from a time series:

  |Alabama Wage Data|Quarter 3, 2016|Quarter 4, 2016|
  | ------------- | ------------- | -----------|
  |lq_qtrly_estabs	|'cauchy'	|'dgamma'|
  |lq_month1_emplvl	|'dgamma'	 |'johnsonsu'|
  |lq_month2_emplvl	|'dgamma'	|'cauchy'|
  |lq_month3_emplvl	|'dweibull'	|'dgamma'|
  |lq_total_qtrly_wages	|'gennorm'	|'cauchy'|
  |lq_taxable_qtrly_wages	|'alpha'	|'alpha'|
  |lq_qtrly_contributions	|'t'	|'exponnorm'|
  |lq_avg_wkly_wage	|'dgamma'	|'gennorm'|
  |oty_qtrly_estabs_pct_chg	|'gennorm'	|'dweibull'|
  |oty_month1_emplvl_pct_chg	|'t'|	'gennorm'|
  |oty_month2_emplvl_pct_chg	|'nct'	|'nct'|
  |oty_month3_emplvl_pct_chg	|'laplace'	|'laplace'|
  |oty_total_qtrly_wages_pct_chg	|'tukeylambda'	|'hypsecant'|
  |oty_taxable_qtrly_wages_pct_chg	|'gengamma'	|'exponweib'|
  |oty_qtrly_contributions_pct_chg	|'pearson3'	|'foldnorm'|
  |oty_avg_wkly_wage_pct_chg	|'johnsonsu'	|'burr'|


Welch's t-test results for Alabama wage data taken quarterly over four years from a time-series:
(Columns over time were considered to be "similar" if they returned a pvalue greater than 0.05) 

    Columns that appear similar over time:
     > lq_month2_emplvl
     > lq_month3_emplvl
     > lq_qtrly_estabs

    Columns that appear dissimilar over time:
    
    Columns that sometimes appear similar and sometimes appear dissimilar: 
    (The first value in the bracket represents the instances when the columns over time "passed" the t-test;
    the second value represents the "failed" tests)
     > lq_avg_wkly_wage:[6, 9]
     > lq_total_qtrly_wages:[8, 7]
     > oty_month1_emplvl_pct_chg:[10, 5]
     > oty_month2_emplvl_pct_chg:[11, 4]
     > oty_month3_emplvl_pct_chg:[11, 4]
     > oty_taxable_qtrly_wages_pct_chg:[12, 3]
     > oty_total_qtrly_wages_pct_chg:[5, 10]
     > lq_qtrly_contributions:[6, 9]
     > lq_taxable_qtrly_wages:[3, 12]
     > oty_qtrly_contributions_pct_chg:[11, 4]
     > oty_qtrly_estabs_pct_chg:[12, 3]
     > lq_month1_emplvl:[14, 1]
     > oty_avg_wkly_wage_pct_chg:[1, 14]


Because the data used to test the column similarity methods was drawn from the same time-series, the columns of the tables with the same names were the same semantically as well. Therefore, we are essentially able to rely on column names to determine if two tables are similar, with greater numbers of shared columns connoting the higher similarity. This, however, creates an efficiency challenge because in practical situations there are often many tables to be compared, each containing a vast number of columns. In fact, in order to test such large data in a reliable way, we generated our own CSV files so we could control the numbers of files we were comparing and the number of columns per table. Our upper limit during our tests was testing 100,000 tables with up to 100,000 columns each (the columns were randomly selected from a list). 

We decided to increase the efficiency of this process by using bitwise comparisons. This was first attempted with the BitVector package for Python using a universal column vocabulary. This package, however, still requires more operations than mathematically necessary because the bitsets are very sparse but contain runs. Therefore, we converted the same tables to MutableSparseIntSets and SemiSparseIntSets to see if they were more suited to these types of comparisons. Running the tables through each of these packages showed that using SemiSparseIntSets was the superior method for efficiently determining similarity between very large sets of data.
