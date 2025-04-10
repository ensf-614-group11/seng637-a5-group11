**SENG 637- Dependability and Reliability of Software Systems***

**Lab. Report \#5 – Software Reliability Assessment**

| Group \#:      |   11  |
| -------------- | --- |
| Student Names: |     |
|Steven Au       |     |
|Laurel Flanagan |     |
|Rhys Wickens    |     |
|Austen Zhang    |     |

# Introduction
This lab includes analysis of the given failure data (failure-dataset-a5.csv) using both reliability growth testing and reliability demonstration chart tools. The lab includes getting familiar with various tools to assist with reliability growth testing and reliability demonstration chart analysis, getting familiar with the given failure data and different formats of failure data, understanding metrics related to failures such as failure rate, MTTF, and reliability, and understanding how to use these analyses to inform decision making. Reliability is one metric that can be used to identify when testing can be considered complete - when the measured reliability of the system achieves its target value. 

# Assessment Using Reliability Growth Testing 

## Result of model comparison (selecting top two models)

## Result of range analysis
In order to determine the useful range of data for the analysis, a trend check was completed using Laplace tests. Reliablity models should only be used on data where the overall reliability is increasing, therefore this test identifies if the reliability is increasing, decreasing, or stable. The failure data provided for the assignment is in the format of failure count data with failures per interval known, therefore the following formula was used to calculate the Laplace factor for each interval: 

$$
u(k) = \frac{\left( \sum_{i=1}^{k} (i - 1) \cdot n(i) \right) - \left( \frac{k - 1}{2} \cdot \sum_{i=1}^{k} n(i) \right)}{\sqrt{k^2 - \frac{1}{12} \cdot \sum_{i=1}^{k} n(i)}}
$$

Where:
- \( n(i) \) is the number of failures observed during the time interval \( i \), 
- \( k \) is the total number of time intervals considered (up to the current time interval \( T \)), 
- The time period is divided into \( k \) equal intervals. 

The tool being used for this assignment (C-SFRAT) does not include functionality for completing trend tests, therefore this was calculated manually using the failure-data-a5.csv file. The calculations are included in the `Laplace_tests_and_graphs.ipynb` file within the Part 1 folder [here](Part%201/Laplace_tests_and_graphs.ipynb). The Laplace factors for each time interval were calculated and then were plotted.     
![Laplace](Part%201/screenshots/laplace_plot.png)

Negative values of the Laplace factor indicate a decreasing failure intensity or reliability growth. Positive values of the Laplace factor indicate an increasing failure intensity, or reliability decrease. Values between -2 and +2 indicate stable reliability. Therefore, Laplace factor values of less than -2 are useful data for the reliability growth model, where there is a decreasing failure intensity and reliability growth. 

As shown in the plot, there is a decrease in reliability in time intervals 1 and 2. Between time intervals 3 and 18, the reliability generally was growing with a few small decreases. Then at time interval 18 there was another decrease in reliability until time interval 23, where the reliability grew again. 
The range of time intervals that make up the useful data are between 6 and 19 (where the Laplace factor is less than -2). 

## Plots for failure rate and reliability of the SUT for the test data provided
### Plots and calculations with raw failure data 
The overall Failure Rate, MTTF and Reliability of the system under test were calculated for the 31 time intervals provided in the raw failure data. The calculations for these metrics can be found in the file `Laplace_tests_and_graphs.ipynb` [here](Part%201/Laplace_tests_and_graphs.ipynb). 

**Failure Rate:** 2.968
**MTTF:** 0.337
**Reliability:** `1.11 × 10⁻⁴⁰`

Plots were created from the failure data to show time between failures, cumulative failures, failure intensity, reliability, and failure rate. In order to calculate the time between failures from the data given, it was assumed that failures occurred uniformly within each time interval.  

**Time Between Failures**    
![Time Between Failures](Part%201/screenshots/time_between_failures.png)  

**Cumulative Failures**  
![Cumulative Failures](Part%201/screenshots/cumulative_failures.png)  

**Failure Intensity**    
![Failure Intensity](Part%201/screenshots/failure_intensity.png) 

**Reliability**   
![Reliability](Part%201/screenshots/reliability.png) 

**Failure Rate**   
![Failure Rate](Part%201/screenshots/failure_rate.png) 

### Plots from reliability growth model tools 



## A discussion on decision making given a target failure rate



## A discussion on the advantages and disadvantages of reliability growth analysis
### Advantages   
Reliability growth analysis provides predictions about the reliability of a system over time, which can be used to make decisions regarding priorities for system improvements or allocating resources required to meet a certain reliability in a system. Reliability growth analysis can also be useful to identify issues in the system and understand where the focus needs to be to improve overall reliability. The reliability growth models can also help to understand what tests should be run. It can also help to provide information that can guide decisions about when software is ready for release. Reliability growth analysis helps manage risks associated with the system. 
### Disadvantages  
Some reliability growth models are very complex and therefore require specialized knowledge and expertise to be applied and understood fully. The accuracy of the reliability growth models may depend on the sample size of failure data provided, therefore the usefulness can be limited if there is a limited number of data. The results may also be better if the program has been further developed, so the results may not be as useful for programs in earlier stages of development. Reliability growth models provide predictions, but the results may be different from the predictions if there are other factors that change, since the prediction is made based on the conditions within the failure data available. If there are new types of failures introduced with additional components, or changes that impact the overall system, then the actual results can differ from the model predictions. 


# Assessment Using Reliability Demonstration Chart 

### Reliability Demonstration Chart Setup

The first step involved modifying the original failure dataset to prepare it for input into the Reliability Demonstration Chart (RDC) workbook. The updated failure data can be viewed [here](Part%202/RDC_spreadsheets/failure-dataset-a5-final.xlsb).

The original dataset included the following columns:
- **T**: Time interval  
- **FC**: Failure count  
- **E**: Execution time (in hours)  
- **F**: Failure identification work (in person-hours)  
- **C**: Computer time for failure identification (in hours)

To plot the RDC graph, two additional columns were derived from the original data:
- **Cumulative Failure Count**: The cumulative number of failures from the `FC` column, totaling 92 failures. Each individual failure was assigned an index from 1 to 92.
- **Input Event When Observed**: This represents the time at which each failure occurred, measured in cumulative time intervals. It was assumed that failures occurred uniformly within each time interval.  
  For example, if 2 failures occurred in time interval 1, the first was placed at 0.5 intervals and the second at 1.0 intervals. This method was applied consistently across the dataset to assign a specific input event time to each failure.

The transformed dataset was then copied into the **"Failure Data"** tab of the RDC workbook.  
A detailed explanation of how the minimum Mean Time To Failure (MTTF) was calculated from this data is provided in the following section.

## 3 plots for MTTFmin, twice and half of it for your test data

The following graphs use a standard Risk Profile of the following:
- Discrimination Ratio γ: 2.00
- Developer's Risk α: 0.10
- User's Risk β: 0.10

1) The first plot illustrates the RDC (Reliability Demonstration Chart) for the system under test (SUT) with a minimum MTTF of approximately 0.051, corresponding to 98 failures per 5 intervals. A detailed explanation of how this minimum MTTF was derived is provided in the following section. As shown in the graph, the observed failures initially fall within the "Continue Test" zone. By the 13th failure, the data point nearly reaches the "Reject" zone, but subsequent failures trend back toward the "Continue" zone. By the 23rd failure, the observations enter the "Accept" zone, where all subsequent failures remain. This plot therefore demonstrates the lowest possible MTTF at which all observed failures remain within the "Continue" or "Accept" regions of the RDC graph.

![MTTFmin](Part%202/screenshots/R-demo-chart-MTTFmin.jpg)

Note: You can also see the full excel sheet for MTTFmin [here](Part%202/RDC_spreadsheets/Reliability-Demonstration-Chart-final-MTTFmin.xls).

2) The second plot illustrates the RDC for the SUT with an MTTF that is double the minimum value, yielding an MTTF of approximately 0.102, which corresponds to 98 failures per 10 intervals. As shown in the graph, the observed failures almost immediately enter the "Reject" zone by the 7th failure. Nearly all subsequent failures remain in the "Reject" zone, except for a brief period between failures 39–49, which temporarily re-enter the "Continue Test" zone.
This demonstrates that when the assumed MTTF is increased beyond the actual performance of the system, the observed failures appear relatively earlier than expected. As a result, the test increasingly falls into the "Reject" region, indicating that the system under test does not meet the higher reliability threshold.

![MTTFmin](Part%202/screenshots/R-demo-chart-MTTFtwice.jpg)

Note: You can also see the full excel sheet for MTTFmin [here](Part%202/RDC_spreadsheets/Reliability-Demonstration-Chart-final-MTTFtwice.xls).

3) The third plot illustrates the RDC for the SUT with an MTTF that is half the minimum value, yielding an MTTF of approximately 0.0255, which corresponds to 196 failures per 5 intervals. As shown in the graph, the observed failures begin in the "Accept" zone, and all subsequent failures remain well within this region throughout the test.
This demonstrates that when the assumed MTTF is significantly lower than the actual performance of the system, the observed failure rate appears much better than expected. As a result, the test consistently falls into the "Accept" zone of the RDC, clearly indicating that the system exceeds the reliability requirement at this lower threshold. This can be useful in verifying system robustness when overperforming relative to a conservative MTTF benchmark.

![MTTFmin](Part%202/screenshots/R-demo-chart-MTTFhalf.jpg)

Note: You can also see the full excel sheet for MTTFmin [here](Part%202/RDC_spreadsheets/Reliability-Demonstration-Chart-final-MTTFhalf.xls).

## Explain your evaluation and justification of how you decide the MTTFmin

To determine the minimum Mean Time To Failure (**MTTF<sub>min</sub>**), we first took an exploratory approach using the Reliability Demonstration Chart (RDC) workbook. Specifically, in the **Failure Data** tab, we adjusted two key inputs:

- **Maximum Acceptable Number of Failures**
- **Per Number of Input Events**

Modifying these values would automatically adjust the calculated MTTF and update the **R-Demo-Chart** graph. The goal was to observe how the plotted failure points fell relative to the **Accept**, **Continue Test**, and **Reject** zones.


#### Visual Understanding of MTTF Behavior

We noticed a clear trend:

- **Increasing** the MTTF shifts the observed failure points **to the left**, closer to or into the **Reject zone**.
- **Decreasing** the MTTF shifts the points **to the right**, toward the **Accept zone**.

With this understanding, we iteratively adjusted the MTTF value until the plotted failures were **as close as possible to the Reject zone without entering it** — a visual estimate of the threshold.

#### Numerical Precision: The "MTTF Check" Tab

To enhance precision, we added a new worksheet to the RDC workbook called **MTTF Check**, which evaluates whether each failure lies to the **right of the Reject slope**.

This allowed us to fine-tune the MTTF and identify the **exact point** where a failure crosses into the Reject zone. In the RDC workbook for **MTTF<sub>min</sub>** ([view here](Part%202/RDC_spreadsheets/Reliability-Demonstration-Chart-final-MTTFmin.xls)), we tested the boundary case:

- Setting **FIO** to **97 failures per 5 intervals**, the **13th failure** crosses into the Reject region:
  
  ![MTTF-check-failed](Part%202/screenshots/MTTF-check-failed.jpg)

- Setting it back to **98 failures per 5 intervals**, the 13th failure stays within the **Continue Test** zone:

  ![MTTF-check-passed](Part%202/screenshots/MTTF-check-passed.jpg)

Thus, **98 failures per 5 intervals** was selected as a reasonable and safe **MTTF<sub>min</sub> approximation**, corresponding to:

MTTF<sub>min</sub> ≈ <sup>5</sup>&frasl;<sub>98</sub> ≈ 0.051

#### Exact MTTF<sub>min</sub> Calculation

While the above is a good practical approximation, we also calculated the **exact** MTTF<sub>min</sub>. Since the 13th failure is the first to touch the Reject slope, we solved for the MTTF at which the normalized x-position of this failure **exactly equals** the x-value of the Reject boundary at that point.

Given:

- The x-value of the Reject slope at the 13th failure is: 39.1787104271727
- The observed failure occurs at 2 units of input event time.

Using the formula for normalized input: Normalized X = Input Event / MTTF

We solve for MTTF:  
MTTF_exact = 2 / 39.1787104271727 ≈ 0.0510481324728055

This can also be expressed as the fraction:  
10,209,626,494,561 / 200,000,000,000,000

#### Final Justification

While the exact minimum MTTF is **~0.051048**, using **98 failures per 5 intervals** (i.e., **MTTF ≈ 0.051**) provides a sufficiently close and intuitive approximation for the purposes of this assignment.

## A discussion on the advantages and disadvantages of RDC

# Comparison of Results

# Discussion on Similarity and Differences of the Two Techniques

# How the team work/effort was divided and managed

# Difficulties encountered, challenges overcome, and lessons learned

# Comments/feedback on the lab itself
This lab was confusing as the requirements were pretty unclear, particularly for the reliability growth testing portion. The capabilities of the tool recommended for the lab are quite different from those of CASRE and others shared during the lecture time that are inaccessible, therefore many of the plots and results shown in the lecture slides examples could not be developed from the tool. This resulted in a lot of time spent on manipulating the data and investigating other methods to manually develop the plots and information. It was difficult to understand the models associated with the reliability growth analysis since we didn't cover much detail about the models during the lecture. 
Overall, there is room for improvement in the clarity and instructions for the lab. 
