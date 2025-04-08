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

# Assessment Using Reliability Growth Testing 

## Result of model comparison (selecting top two models)

## Result of range analysis

## Plots for failure rate and reliability of the SUT for the test data provided

## A discussion on decision making given a target failure rate

## A discussion on the advantages and disadvantages of reliability growth analysis

# Assessment Using Reliability Demonstration Chart 

### Reliability Demonstration Chart Setup

The first step involved modifying the original failure dataset to prepare it for input into the Reliability Demonstration Chart (RDC) workbook. The updated failure data can be viewed [here](Part%202/RDC_spreadsheets/failure-dataset-a5-final.xls).

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



## A discussion on the advantages and disadvantages of RDC

# Comparison of Results

# Discussion on Similarity and Differences of the Two Techniques

# How the team work/effort was divided and managed

# Difficulties encountered, challenges overcome, and lessons learned

# Comments/feedback on the lab itself
