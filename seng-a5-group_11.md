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
We use C-SFRAT to select a set of models which provide the best fit for the failure data. To do so, we use two approaches, (1) we attempt to fit models to the entire data and assess goodness of fit, (2) we attempt to fit models to the subset of data which we have determined to be reliable using Laplace test.

### **Using full range of data**
![Goodness of Fit](Part%201/output_plots/CSFRAT_goodness_of_fit_no_subset.png)

The above figure was generated using the C-SFRAT tool using the entire range of the provided failure data. We ran the selected hazard functions for all permutations and combinations of covariates. To select the best model, we pick the models which minimize AIC and BIC.

![Goodness of Fit Comparison](Part%201/output_plots/CSFRAT_goodness_of_fit_no_subset_model_compare.png)

We select DW3(F) and GM(F). 

![Best Models](Part%201/output_plots/CSFRAT_no_subset_best_models.png)

We use DW3(F) and GM(F) to predict future failures.

![Best Models Prediction](Part%201/output_plots/CSFRAT_no_subset_prediction.png)

We use the table to obtain the discrete values for the failures.

![Best Models Prediction Table](Part%201/output_plots/CSFRAT_no_subset_prediction_table.png)

There seems to be a bug in our version of the C-SFRAT tool. When "DW3(F)" and "GM(F)" are selected in the model results, the table which shows predictions appears to provide the data for the incorrect models.

### **Using reliable range of data**
![Goodness of Fit](Part%201/output_plots/CSFRAT_goodness_reliable_subset.png)

The above figure was generated using the C-SFRAT tool using the reliable range of data.

![Goodness of Fit Comparison](Part%201/output_plots/CSFRAT_goodness_of_fit_reliable_model_compare.png)

We select NB2(F), DW2(F).

![Best Models](Part%201/output_plots/CSFRAT_no_subset_best_models.png)

We use NB2(F) and DW2(F) to predict future failures. This figure also appears to have an error. 

![Best Models Prediction](Part%201/output_plots/CSFRAT_reliable_subset_model_predictions.png)

We use the table to obtain the discrete values for the failures.

![Best Models Prediction Table](Part%201/output_plots/CSFRAT_reliable_subset_model_prediction_table.png)

Again, we observe the save issue where we select model results for DW2(F) and NB2(F). The resulting table shows data for IFRGSB(E, F, C) and DW2(E, C).

### **Failure Intensity**

We use the C-SFRAT tool to build a failure intensity figure.
![Failure Intensity](Part%201/output_plots/CSFRAT_failure_intensity.png)

### **Software Reliability Testing in R**

An attempt was made to use another tool. This tool was buggy, and we were only able to get it working a few times consistently before the program would fail to generate plots. The goal was to use this tool to be able to compare results with C-SFRAT and the Python Reliability package. The following figures show usage of this tool, but due to our inability to continue working on this tool, we can only provide these figures. 

![SRT](Part%201/output_plots/SRT.png)

![SRT failure intensity](Part%201/output_plots/SRT_failure_intensity.png)

![SRT time between failures](Part%201/output_plots/SRT_tbf.png)

![SRT model predictions](Part%201/output_plots/SRT_model_predictions.png)


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
### Plots and calculations with input failure data provided
The overall Failure Rate, MTTF and Reliability of the system under test were calculated for the 31 time intervals provided in the input failure data. The calculations for these metrics can be found in the file `Laplace_tests_and_graphs.ipynb` [here](Part%201/Laplace_tests_and_graphs.ipynb). 

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

## A discussion on decision making given a target failure rate
The failure rate vs target failure rate ratio can be plotted to see how the failure rate changes with time. Ideally, to meet the target failure rate, the ratio of failure rate vs target failure rate would reach 1. However in practice, this could take a long time to reach the goal so the failure rate vs target failure rate ratio shouldn't exceed 0.5 and this can be acceptable. The reliability growth models can help in making decisions with a target failure rate because the prediction of the model can be used to determine how much more time would be required to achieve the target failure rate or what increased testing effort is required if the system is not expected to reach the target failure rate. If the predicted failure rate does not appear to be close to reaching the target failure rate by the end of the testing, then decisions could be made to add more testing resources, or deferring some of the features of the system to a future release date. There could also be a decision made to adjust balance between the goals for failure rate, time, and costs. 
For example, in our system, the failure rate after 31 time intervals is 2.968. If the goal is to reach a failure rate of 1.5, the reliability growth model can be used to assess at what time the failure rate would decrease to 1.5 based on the predicted number of failures associated with the model. We provide the model with the failure-count data like we did in this assignment, and determine which reliability growth model best fits the data. Then we can use the model to project the future failure intensity and estimate how much more testing time is needed to reach the failure target rate. If the failure rate isn't decreasing, we may need to review the current test coverage and provide modifications to the testing strategy. The use of reliability growth models to compare with a target failure rate can help with project planning and risk management. 

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

### Advantages:
- Easy to implement. The tool for RDC was an Excel spreadsheet, and did not require any other software.
- Works even with sparse or limited data points.
- Its a fairly simple method however very clearly visualizes the reliability of the system. The accept/continue/reject regions make it very clear if the system is acceptable or not.
- By changing MTTF in the plot, you can visualize the system's reliability at different standards of MTTF. This is useful in real-world applications because businesses will often have a specific MTTF in mind in their requirements when developing the application.
- Regulatory & contractual compliance: useful in industries like aerospace or military where proving reliability to a certain confidence level is mandatory.

### Disadvantages:
- It has nothing on analyzing trends in the reliability of the system.
- Not a very powerful tool. It provides a snapshot of the system's performance but does not predict its future performance.
- It is sensitive to input accuracy, as the results are only as good as the estimates for reliability targets, failure rates, etc.
- High reliability goals with high confidence levels can require larger sample sizes or long test durations.
- Limited to pass/fail outcomes, as it doesn’t consider modes of failure, severity, or degradation over time.


# Comparison of Results

Results from Reliability Growth:
**MTTF:** 0.337
**Failure Rate:** 2.968 failures/unit time
**Reliability Estimate:** `1.11 × 10⁻⁴⁰`

Results from RDC:
**MTTF:** 0.051 (Any MTTF greater than this causes the system to be rejected)
**Failure Rate:** 19.589 failures/unit time (Calculated by using Failure Rate = 1/MTTF)
**Reliability Estimate:** `1.84 × 10⁻²⁶⁴` (Calculated with t = 31 intervals like in the Reliability Growth section)

As can be seen, the two methods showed very different results, with RDC showing the system to have a much lower MTTF than Reliability Growth. In turn, failure rate was much higher from RDC and Reliability was much lower. This makes sense since in the original data, there were very high failure counts in the early time intervals, making it easier for the system to cross into the reject portion of the RDC plot. RDC assumes a failure uniform distribution of failures so when the failures are concentrated very highly in the beginning, the resulting MTTF is very low.

However, both methods show that the system is very unreliable, with both Reliability Estimates being very close to 0. 



# Discussion on Similarity and Differences of the Two Techniques

### Similarities:
- Both use the metric MTTF as a metric for the general reliability of the system.
- Both are data driven, they use data in order to measure the reliability of the system.
- Both assess the reliability of the SUT. They both are able to provide a measure of whether a software system is becoming more reliable or less reliable over time.

### Differences:
- Reliability Growth requires specialized software whereas RDC only requires Excel.
- Reliability Growth generally requires much more data than RDC to work properly.
- Reliability Growth is able to predict future reliability while RDC is unable to.
- Reliability Growth uses statistical modelling while RDC is based on graphing.


# How the team work/effort was divided and managed

From the four group members, two pairs were formed, with one taking on Reliability Growth in Part 1, and the other doing RDC in Part 2. After performing the analysis on their respective parts, each pair wrote the parts of the report relevant to the section and then the remaining sections that didn't belong to either part of the assignment (such as 'How the team work/effort was divided and managed) was divided evenly the group members.

# Difficulties encountered, challenges overcome, and lessons learned
During the Reliability Growth Testing phase, there were several difficulties encountered. One of the main challenges was actually getting the software tools to work properly and trying to find a tool that could provide the plots and metrics that were requested for the assignment. Most of the tools seem to only be available for Windows or Linux platforms, so some team members were not able to get the tool working on Mac systems. Some of the tools shared during the lecture session that could provide more detailed plots including reliability and reliability predictions were not accessible. There appeared to be several bugs in the C-SFRAT tool, and we were not able to enter a target failure intensity in the software to compare predictions to a target because this functionality was not working for us. We also were not able to plot the reliability from the tool because the C-SFRAT tool does not produce this type of plot. We ended up working with the failure count data manually to calculate the laplace factors and determine the useful range of data, since the C-SFRAT tool also did not provide this functionality. We also attempted using the Software Reliability Tool which runs in R. We had troubles with running the tool. The tool struggled to successfully assign references to its DataTools methods during environment setup. We were only able to successfully run and operate the SRT tool once. This limited our ability to use the tool for C-SFRAT. We learned a lot about the Laplace test and making decisions based on the outcome of this test since we had to perform the calculation manually. We also manually manipulated the given failure data to plot time between failures, cumulative failures, failure intensity, and reliability. This also resulted in learning a lot more about how these calculations are performed. There was also some difficulty in working with the given failure count data since we were only provided the time intervals, failure counts per interval and then the other covariate variables for the reliability growth model. We didn't have the information about how long the time intervals were, or whether the failures were evenly spaced within the intervals, so we had to make assumptions in order to develop the plots. 



When working on the Reliability Demonstration Chart, there were several difficulties encountered. First off, in the spreadsheet provided all of the tabs were frozen initially which made it difficult to understand the formulas and how the spreadsheet worked. Once we unfroze all the tabs in the spreadsheet, we then needed to update many of different tabs as the failure data only had room for 16 oberservations. Once we added all the additional observation slots (92 oberservations total), we then had to update the Plot Data tab so that you could visualize all these oberservations on the R-demo-chart. This involved adding many additional lines in the Reject, Continue, and Accept lines, as well as the event counts for unit tick marks. Further, once the MTTF was changed in the Failure Data tab, we had to manually change the scale of the R-Demo-Chart in order to easily visualize the Oberservations on the chart. Overall, the Reliability Demonstration Chart spreadsheet was a bit difficult to work with given the manual changes needed in order to suport the additional oberservations. A more flexible chart with greater observation slots and automatic chart scaling would have helped to make this section for straightforward. We learned a lot about the intricacies of spreadsheet given the manual effort to alter it to support the additional observations.

# Comments/feedback on the lab itself
This lab was confusing as the requirements were pretty unclear, particularly for the reliability growth testing portion. The capabilities of the tool recommended for the lab are quite different from those of CASRE and others shared during the lecture time that are inaccessible, therefore many of the plots and results shown in the lecture slides examples could not be developed from the tool. This resulted in a lot of time spent on manipulating the data and investigating other methods to manually develop the plots and information. It was difficult to understand the models associated with the reliability growth analysis since we didn't cover much detail about the models during the lecture. 
Overall, there is room for improvement in the clarity and instructions for the lab. 
