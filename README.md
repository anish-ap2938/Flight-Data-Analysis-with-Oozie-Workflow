# Flight Data Analysis with Oozie Workflow

This repository contains the complete project for analyzing a large volume of flight data using an Oozie workflow built on Hadoop. The project processes 22 years (1987–2008) of Airline On-time Performance data from Data Expo 2009 to answer key operational questions in the aviation industry.

---

## Project Overview

The objective of this project is to develop and run an Oozie workflow that orchestrates three MapReduce jobs to perform the following analyses on the flight data:

1. **On-Time Performance of Airlines:**  
   - Determine the three airlines with the highest and lowest probabilities of being on schedule.
   - The MapReduce job calculates the on-schedule probability for each airline by comparing the sum of arrival and departure delays against a defined threshold.

2. **Average Taxi Time at Airports:**  
   - Identify the three airports with the longest and shortest average taxi times (inbound and outbound combined).
   - The MapReduce job computes the average taxi time per airport by aggregating taxi-in and taxi-out times across all flights.

3. **Flight Cancellation Reasons:**  
   - Determine the most common cancellation cause.
   - The MapReduce job counts cancellation occurrences by cancellation code for flights that were canceled.

---

## Oozie Workflow Structure

The Oozie workflow (defined in `workflow.xml`) sequentially executes the three MapReduce jobs:
- **On-Schedule Analysis:**  
  Processes flight records to calculate on-time probabilities per airline.
- **Taxi Time Analysis:**  
  Computes average taxi times for airports.
- **Cancellation Cause Analysis:**  
  Tallies cancellation reasons.

Each action in the workflow is configured to run in a fully distributed mode on a Hadoop cluster. The workflow is designed to be scalable and is executed on multiple VMs with incremental experiments:
- **Scalability Testing:**  
  The workflow is run first on a small cluster (2 VMs) and then gradually scaled up (up to 7 VMs) to measure execution time improvements.
- **Progressive Data Analysis:**  
  The data is processed progressively by increasing the number of years (from 1 year up to 22 years), and corresponding execution times are measured.

---

## Files Included

- **Source Code:**  
  - `Flight Data Analysis.java` – Java source file containing the MapReduce implementations for:
    - On-Schedule Performance
    - Airport Taxi Time
    - Flight Cancellation Cause
  - Compiled JAR file (`Flight data analysis testoozie.jar`) for the MapReduce jobs.

- **Workflow Definition:**  
  - `workflow.xml` – Oozie workflow file that orchestrates the execution of the three MapReduce jobs.

- **Execution Commands:**  
  - `Flight data analysis commands.txt` – A text file listing all the commands used to run the code and produce the results in fully distributed mode.

- **Output:**  
  - `Flight data analysis output.txt` – The final results of the analysis, including:
    - Airlines with the highest and lowest on-schedule probabilities across different time periods.
    - Airports with the shortest and longest average taxi times.
    - Most common flight cancellation reasons.

- **Project Report:**  
  - `Milestone 2 Big Data.pdf` – Detailed project report, including workflow diagrams, algorithm descriptions, and performance measurement plots comparing execution times with varying numbers of VMs and data sizes.

---

## Algorithms and Implementation Details

### On-Schedule Performance Analysis
- **Algorithm:**  
  - A delay threshold (e.g., 10 minutes) is set to define what constitutes an on-time flight.
  - **Mapper:**  
    - Parses each flight record, sums the arrival and departure delays.
    - Emits a key-value pair (airline, 1) if the combined delay is within the threshold; otherwise, emits (airline, 0).
  - **Reducer:**  
    - Aggregates the values for each airline to calculate the proportion of on-time flights.
    - Results are sorted in reverse order to identify airlines with the highest and lowest probabilities.

### Average Taxi Time Analysis
- **Algorithm:**  
  - **Mapper:**  
    - Emits taxi-out times for origin airports and taxi-in times for destination airports.
  - **Reducer:**  
    - Sums taxi times and counts occurrences per airport.
    - Calculates the average taxi time per airport and sorts the results.

### Flight Cancellation Analysis
- **Algorithm:**  
  - **Mapper:**  
    - Filters canceled flights and emits cancellation codes.
  - **Reducer:**  
    - Counts the frequency of each cancellation code to determine the most common reason.

---

## Performance Evaluation

The project includes experiments to evaluate the performance of the Oozie workflow by:
- **Scaling the Cluster:**  
  Running the workflow on an increasing number of VMs (from 2 up to 7) and measuring the execution time reduction.
- **Progressive Data Processing:**  
  Incrementally processing data from 1 year to 22 years and plotting the workflow execution time relative to data size.
- **Result Analysis:**  
  The performance metrics and observations are documented in the project report, with detailed plots and discussions on the observed trends.

---

## Conclusion

This project successfully demonstrates how to design, implement, and execute a distributed Oozie workflow to analyze a large-scale flight dataset. By orchestrating three MapReduce jobs, the workflow provides insights into airline punctuality, airport taxi times, and cancellation causes. The scalability experiments reveal the benefits of increased computational resources and provide a practical evaluation of distributed processing performance in a Hadoop environment.

All code, workflow files, execution commands, output results, and detailed performance analyses are provided in this repository.
