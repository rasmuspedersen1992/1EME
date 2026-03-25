# 1EME Contribution, Rasmus Kristoffer Pedersen

Analysis by Rasmus Kristoffer Pedersen.
- Institute of Epidemiology and Social Medicine, University of Münster, Germany
- Interdisciplinary Center for Mathematical Modeling of Infectious Disease Dynamics (IMMIDD), University of Münster, Germany


# Overview
To determine the six quantities of interest, I made use of a methodology previously developed to identify mortality crises in a similar dataset of all-cause mortality. 
The methods are published in Pedersen et al, AJE, 2025, in which daily all-cause mortality for Denmark between 1815 and 1915 is analyzed. (See doi.org/10.1093/aje/kwae187 for details)

A python repository implementing the previously used methods is available on github.com/pandemixcenter/ExcessMortalityFunctions/. 

Only minor modifications of the code (for compability with a newer versions of Python) was necessary to carry out the present analysis. The updated functions are available in the github repository with the present analysis, see link below.

For consistency in the results, all analysis (except for quantity 4) was done on a daily time-resolution. For monthly estimates, sums of the results on daily time-resolution were used. Similar analysis could be done using data on weekly, monthly or annual time-resolution with the same code.

I have chosen to keep the methodology used very comparable to the one used in my previous work, rather than analyze the results thoroughly and calibrate the method to the data. This was done in part due to time-constraints, but also in order to keep the method close to that used previously and to test how accurate the methods implemented in the _ExcessMortalityFunctions_ package can produce useful results for a new dataset without modification. 

Note that this analysis is based solely on all-cause mortality counts, and hence did not take the additional information on cause-specific deaths into account. Similarly, population estimates were not included in the estimation of excess mortality counts, but only used to determine the per-capita mortality after the counts were estimated. This is an active choice to allow for comparison with similar analysis where no population- or cause-specific data are available.

Code and figures are available on github.com/RKPedersen/1EME

Details of the methodology used are given below.

# Establishing a mortality baseline, state-level
An initial baseline of mortality for any given date is first determined by calculating the mean of the same date in the surrounding $x$ years. For this analysis, I use $x = \pm 3$ for both states for consistency, due to the limited period available in the data for rural Maryland. That is, the baseline on September 10th, 1919 is the average of September 10th in the years 1916, 1917, 1918, 1920, 1921, and 1922. 
The empirical standard deviation of the same years is also calculated. 

An iterative process is then carried out: Any date where the observed data lies more than three standard deviations away from the baseline is omitted and the baseline (as well as the standard deviation) is recalculated. This process is repeated until no days exceed the threshold. 
This iterative process ensures that days of high (assumed excess) mortality do not affect the baseline in surrounding years.

# Identifying mortality crises
From the resulting mortality baseline, I identify mortality crises, that is, continuous periods of excess mortality. 
In this analysis, I define a mortality crisis as any period of time when the daily mortality exceeds the baseline with _three_ standard deviations at least once. Periods are connected and counted as a single crisis if the excess mortality does not drop below _two_ standard deviations for more than seven days. 
This allows me to estimate a distinct start- and end-days of any mortality crisis. 
Excess deaths are determined as the difference between the total mortality observed during this period and the sum of expected mortality (i.e. the baseline) during the same period. 
Uncertainty intervals are calculated similarly as the difference between the observed mortality and the baseline plus/minus two standard deviations.
The primary pandemic period was identified from state-level daily data. The identified periods were subsequently used in the analysis for quantities 3, 5, and 6. 

# Additional details related to specific quantities of interest
## Quantity 1
Only excess mortality during the primary mortality crisis is reported. In both states, the primary mortality crisis was the mortality crisis with the highest excess mortality and coincides with the pandemic period.
The period identified is 1918-09-29 to 1919-02-28 for Kentucky, and 1918-09-22 to 1919-01-29 for Maryland.

## Quantity 2
The reported estimates are monthly-aggregated sums of all identified mortality crises in the period given in the reporting sheet. Days without mortality crises do not contribute to estimated pandemic deaths. 
Two alternative methods could have been considered using the _ExcessMortalityFunctions_ code: Taking the monthly sums of excess mortality during the primary mortality crisis of Quantity 1  (rather than of all identified mortality crises), or by calculating excess mortality on a monthly basis directly, reporting excess mortality (and possible under-mortality) for all months. This would have resulted in a lower or higher estimate respectively. In a more thorough analysis, the results of all three methods could be compared and analyzed.

## Quantity 3:
Due to low counts, I assume Poisson distribution of data, and calculate the excess over the full period. Uncertainty estimates are based on the 95% confidence interval of the estimated (Poisson-distribution) baseline. The reported estimates are the sum of the excess during the primary crisis identified for quantity 1.

## Quantity 4:
Due to very low counts in some counties, I carried out a county-specific analysis of the data aggregated by week, and subsequently determined mortality crises on weekly level.
Rather than only report deaths during the deadliest mortality crisis in the county, I took the sum of all identified mortality crisis that occurred between 1918-03-01 and 1921-05-01 (the period considered in the reporting sheet for quantity 2), in effect overestimating pandemic deaths. This was done to ensure consistency between counties where the deadliest mortality crisis was the pandemic and counties where other crises may have occurred.

## Quantity 5 and 6:
Excess mortality was determined for each of the subgroups, and the sum of the excess in the primary pandemic period (as identified for quantity 1) was reported.
