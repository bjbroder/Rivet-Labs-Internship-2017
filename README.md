# Summer-2017

This code was developed during the summer of 2017 for Rivet Labs. The goal of this project was to find a way to determine if two tables uploaded to the system were the same or similar. Initially, we attempted to determine similarity numerically – according to the content in each of the columns of the two tables. For each table, we iterated through every column and selected only the values that could be parsed as real numbers. If those numbers constituted at least 90% of the column’s total values, the column was considered “numeric” and was used to determine numerical similarity by looking at column similarity methods. 

This was done first using the column’s summary statistics (max, min, mean, median, mode, standard deviation), then by determining each column’s best fit distribution, and later using Welsh’s t-test. Each of these tests failed to show similarity due to variance between tables, even when the data was drawn from the exact same distribution (ie as part of a time series). Because of these results, we were forced to reject the notion that statistical methods such as the t-test are accurately able to give the probability that the columns were drawn from the same distribution and are therefore similar. 

Summary statistics for for two quarters from a time series:

(Max, Min, Mean, Median, Mode, Standard Deviation)

    Alabama Wage Data, Quarter 1, 2016: 
     > lq_qtrly_estabs - (44.530000000000001, 0.0, 2.1738356378001873, 1.0, 2.3658741444158093, 5.5973604672152373)
     > lq_month1_emplvl - (30.719999999999999, 0.0, 1.071302110406487, 1.0, 1.0288827600708506, 1.0585997339710116)
     > lq_month2_emplvl - (30.66, 0.0, 1.0685965277055829, 1.0, 1.0245988594877031, 1.0498028228635019)
     > lq_month3_emplvl - (30.760000000000002, 0.0, 1.0688543507641126, 1.0, 1.0236544458296093, 1.0478684244667245)
     > lq_total_qtrly_wages - (26.850000000000001, 0.0, 1.1525756315625324, 1.0, 1.2291188920220222, 1.5107332507254434)
     > lq_taxable_qtrly_wages - (106.70999999999999, 0.0, 1.215167377066223, 0.88, 3.9302066546454588, 15.44652434821945)
     > lq_qtrly_contributions - (471.13999999999999, 0.0, 1.5803363135461066, 0.95999999999999996, 7.826846299257805, 61.259522992205603)
     > lq_avg_wkly_wage - (3.3700000000000001, 0.0, 1.0010422081297434, 1.0, 0.29981829092794815, 0.089891007574955745)
     > oty_qtrly_estabs_pct_chg - (6150.0, -100.0, 0.85199604948539331, 0.0, 45.470661613757883, 2067.5810675928747)
     > oty_month1_emplvl_pct_chg - (928.60000000000002, -100.0, 0.68383407838652666, 0.40000000000000002, 12.933956755899063, 167.287237363467)
     > oty_month2_emplvl_pct_chg - (813.89999999999998, -100.0, 0.83368333506601522, 0.5, 11.23101401976681, 126.13567591219864)
     > oty_month3_emplvl_pct_chg - (917.10000000000002, -100.0, 1.0873271649859655, 0.69999999999999996, 12.128697136470883, 147.105294228237)
     > oty_total_qtrly_wages_pct_chg - (701.60000000000002, -100.0, -0.1514086703399522, 0.20000000000000001, 11.827112290618157, 139.88058513489105)
     > oty_taxable_qtrly_wages_pct_chg - (27797.400000000001, -100.0, 6.9538049693315305, 0.0, 242.08760639315918, 58606.409169169157)

    Alabama Wage Data, Quarter 2, 2016: 
     > lq_qtrly_estabs - (44.960000000000001, 0.0, 2.1814892400457429, 1.0, 2.3802185441195727, 5.6654403177706971)
     > lq_month1_emplvl - (30.800000000000001, 0.0, 1.0715329036282359, 1.0, 1.0242332206755345, 1.049053690335378)
     > lq_month2_emplvl - (30.780000000000001, 0.0, 1.0727263748830438, 1.0, 1.0244269581879926, 1.049450592662303)
     > lq_month3_emplvl - (30.699999999999999, 0.0, 1.077089614305021, 1.0, 1.0363448574971172, 1.0740106636607203)
     > lq_total_qtrly_wages - (27.129999999999999, 0.0, 1.1100478220189207, 1.0, 1.1403267405283157, 1.3003450751639329)
     > lq_taxable_qtrly_wages - (64.109999999999999, 0.0, 1.0015048341823474, 0.70999999999999996, 3.076618292752249, 9.4655801192977638)
     > lq_qtrly_contributions - (158.53, 0.0, 1.3771431541740304, 0.93999999999999995, 5.563226281680314, 30.949486661178575)
     > lq_avg_wkly_wage - (3.5299999999999998, 0.0, 0.96833610562428518, 1.0, 0.27446596245484495, 0.075331564546264343)
     > oty_qtrly_estabs_pct_chg - (1000.0, -100.0, 0.6061960702775756, 0.0, 12.409800775511657, 154.00315528788974)
     > oty_month1_emplvl_pct_chg - (891.39999999999998, -100.0, 0.70900301486641015, 0.5, 12.000187779393805, 144.0045067407124)
     > oty_month2_emplvl_pct_chg - (958.79999999999995, -100.0, 0.73239421977336516, 0.29999999999999999, 12.027979587520266, 144.67229295780419)
     > oty_month3_emplvl_pct_chg - (1084.8, -100.0, 0.93455660671587482, 0.5, 13.181562901193988, 173.75360051813365)
     > oty_total_qtrly_wages_pct_chg - (778.5, -100.0, 2.0831271441937829, 2.1000000000000001, 12.216579794143302, 149.24482186667038)
     > oty_taxable_qtrly_wages_pct_chg - (23796.299999999999, -100.0, 5.0945940326437258, 0.0, 183.83975086814277, 33797.053999260803)
     > oty_qtrly_contributions_pct_chg - (690400.0, -100.0, 78.281645701216334, 0.0, 5665.3227393727757, 32095881.741254251)
     > oty_avg_wkly_wage_pct_chg - (262.5, -100.0, 1.4304501507433205, 1.5, 6.5978802049571428, 43.532023198965312)

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


Because the data used to test the column similarity methods was drawn from the same time-series, the columns of the tables with the same names were the same semantically as well. Therefore, we are essentially able to rely on column names to determine if two tables are similar, with greater numbers of shared columns connoting the higher similarity. This, however, creates an efficiency challenge because in practical situations there are often many tables to be compared, each containing a vast number of columns. In fact, in order to test such large data in a reliable way, we generated our own CSV files so we could control the numbers of files we were comparing and the number of columns per table. Our upper limit during our tests was testing 100,000 tables with up to 100,000 columns each (the columns were randomly selected from a list) but we generally only tested tables with up to 10,000 columns due to time contraints. 

We decided to increase the efficiency of this process by using bitwise comparisons. This was first attempted with the BitVector package for Python using a universal column vocabulary. This package, however, still requires more operations than mathematically necessary because the bitsets are very sparse but contain runs. Therefore, we converted the same tables to MutableSparseIntSets and SemiSparseIntSets to see if they were more suited to these types of comparisons. Running the tables through each of these packages showed that using SemiSparseIntSets was the superior method for efficiently determining similarity between very large sets of data.

Testing runtime (seconds) of BitVector, MutableSparseIntSet, and SemiSparseMutableIntSet packages with varying numbers of tables and columnsper table:

|	 |100 tables	|1000 tables	|10000 tables	|25000 tables	|50000 tables	|75000 tables	|100000 tables|
|---|---|---|---|---|---|---|---|
|100 columns|  |  |  |  |  |  |  |							
|BitVectors	|0.117342861	|1.330632427	|7.833533564	|18.79500713	|160.138788	|211.4978684	|255.0115816|
|MutableSparse	|0.27530164	|0.562335432	|75.65324984	|128.526734	|193.9695324	|100.6847252	|131.9256592|
|SemiSparse	|0.026061443	|0.172061698	|1.420522832	|3.501075161	|7.527625295	|10.84277683	|14.7325597|	
|500 columns|  |  |  |  |  |  |  |							
|BitVectors	|0.503639508	|11.06635143	|80.94988798	|171.869837	|292.4827158	|309.1632609	|460.2269579|
|MutableSparse	|0.428041255	|3.91960056	|70.23124473	|154.1113304	|158.9049406	|177.84524	|198.3105901|
|SemiSparse	|0.030555271	|0.284721495	|0.284721495	|6.176032069	|12.4457266	|18.40387019	|24.7249516|
|1000 columns|  |  |  |  |  |  |  |							
|BitVectors	|0.755797632	|11.13807729	|108.7040595	|176.1630223	|363.6930041	|498.522717	|486.6650112|
|MutableSparse	|0.53404347	|4.066030248	|39.91804323	|105.8073022	|129.4894868	|181.4601254	|180.1262393|
|SemiSparse	|0.04621434	|0.475913274	|4.101160731	|9.258089584	|19.65058712	|28.60314256	|42.41706525|
|5000 columns|  |  |  |  |  |  |  |							
|BitVectors	|2.36950059	|19.68828472	|173.5585634	|422.4184266	|836.6417741	|1246.242342	|1664.412147|
|MutableSparse	|1.744588146	|16.92050881	|113.2346881	|201.5243091	|355.5350035	|467.9677709	|840.2451703|
|SemiSparse	|0.592129024	|2.963505763	|29.94089598	|73.13437793	|138.9306445	|209.4756217	|280.8542945|
|10000 columns|  |  |  |  |  |  |  |							
|BitVectors	|5.075053067	|45.88543995	|431.6960821	|976.0051405	|1926.867358	|2799.844562	|3885.38318|
|MutableSparse	|2.243663899	|18.34730918	|169.0528138	|385.4876877	|729.5381877	|1268.196914	|2779.936492|
|SemiSparse	|2.562875087	|19.99145442	|180.815371	|359.69897	|596.4114277	|490.1489115	|482.0160861|

![BitVectors vs MutableSparseIntSets vs SemiSparseMutableIntSets](https://github.com/bjbroder/Summer-2017/blob/master/bv%20vs%20msis.jpg "BitVectors vs MutableSparseIntSets vs SemiSparseMutableIntSets") 
