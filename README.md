# Staff Scheduling Optimization - Final Report

A comprehensive Jupyter notebook (`174_Final_Report.ipynb`) that demonstrates optimal staff scheduling using Monte Carlo Poisson simulations, queueing theory, and data visualization.

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Notebook Structure](#notebook-structure)
- [Key Features](#key-features)
- [Methodology](#methodology)
- [Visualizations](#visualizations)
- [Output Files](#output-files)

## Overview

The `174_Final_Report.ipynb` notebook provides a complete analysis of staff scheduling optimization for a service operation. It:

1. **Analyzes Historical Demand**: Uses 3-week customer arrival data from `binge_data.xlsx`
2. **Simulates Service Requirements**: Employs Monte Carlo Poisson simulations to determine optimal staff allocation per hour
3. **Performs Queueing Analysis**: Models multi-server queues with heterogeneous employee service rates
4. **Optimizes Schedule**: Determines optimal employee assignments to minimize costs while meeting service level agreements (SLAs)
5. **Visualizes Results**: Creates comprehensive visualizations including heatmaps, cost analysis, and fragility analysis

### Key Features

- **Stochastic Demand Modeling**: Monte Carlo Poisson simulation with piece-wise homogeneous modeling
- **Multi-Server Queue Simulation**: Models heterogeneous employee service rates (drinks per minute)
- **SLA-Based Staffing**: Ensures 90% of customers wait ≤ 2 minutes
- **Cost Optimization**: Tier-based hourly rates ($18/hr for below-median speed, $20/hr for above-median)
- **Fragility Analysis**: Identifies hours most vulnerable to demand variations
- **Comprehensive Visualizations**: Heatmaps, bar charts, line plots, and cost comparisons

## Installation

### Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or JupyterLab

### Setup

1. **Install Python dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

2. **Ensure data file is present**:
   - Place `binge_data.xlsx` in the project directory
   - This file should contain 3 weeks of hourly customer arrival data

3. **Open the notebook**:
   ```bash
   jupyter notebook 174_Final_Report.ipynb
   ```

## Quick Start

1. **Open the notebook**: `174_Final_Report.ipynb`

2. **Run all cells** (Cell → Run All):
   - The notebook will execute all analysis steps
   - Generate all visualizations
   - Display comprehensive results

3. **View results**:
   - All plots are displayed inline
   - Summary statistics are printed in the notebook
   - Generated images are saved to the project directory

## Notebook Structure

The notebook is organized into the following sections:

### 1. Preliminary Modeling - Monte Carlo Poisson Simulations
- Loads and processes `binge_data.xlsx`
- Computes average demand (λ) per day per hour
- Sets up piece-wise homogeneous modeling

### 2. Employee Data and Service Rates
- Defines employee drink-making speeds (drinks per minute)
- Calculates tier-based hourly rates based on median speed
- Sets up employee service rate parameters

### 3. Queue Simulation Functions
- `simulate_arrivals_one_hour()`: Generates Poisson arrivals
- `simulate_hour_queue()`: Simulates multi-server queue with heterogeneous employees
- `evaluate_staffing_for_hour()`: Evaluates different employee combinations
- `plan_week_schedule()`: Plans optimal schedule for entire week

### 4. Schedule Execution
- Runs the week-long scheduling optimization
- Generates `schedule_df_output` with optimal employee assignments

### 5. Visualizations

#### Demand Simulation Visualization
- Heatmap of expected customers (λ) per day per hour
- Line plot showing demand patterns across hours

#### Employee Schedule Heatmap
- Hours scheduled by employee and day of week
- Visual representation of optimal schedule

#### Cost Analysis
- Daily labor costs: Optimal vs Naive scheduling
- Cost savings per day (as percentage)
- Tier-based cost system visualization

#### Schedule and Cost Summary
- Weekly hours scheduled comparison
- Monte Carlo simulation statistics
- Total cost savings analysis

#### Fragility Analysis
- Heatmap of fragility scores by day and hour
- Top 20 most fragile hours identification
- Fragility metrics based on demand-to-staff ratio, wait times, and system load

## Methodology

### 1. Demand Modeling

**Data Source**: `binge_data.xlsx`
- 3 weeks of hourly customer arrival data
- Averaged by day of week (Mon-Sun)
- Operating hours: 10:00 AM - 8:00 PM (11 hours/day)

**Mathematical Model**:
- **Arrival Rate**: `λ(t)` customers per hour (piece-wise constant)
- **Number of Arrivals**: `N(T) ~ Poisson(λT)`
- **Inter-arrival Times**: Exponential with mean `1/λ`

### 2. Queueing Theory

**Model Type**: M/M/c queue (Poisson arrivals, Exponential service, c servers)

**Service Rates**:
- Each employee has service rate `μ_i` (drinks per minute)
- Service time: `S_i ~ Exponential(1/μ_i)` seconds
- Queue discipline: First-Come-First-Served (FCFS)

**SLA Requirements**:
- 90% of customers wait ≤ 2 minutes (120 seconds)
- Mean wait time: ≤ 120 seconds
- 90th percentile wait time: ≤ 180 seconds

### 3. Staffing Optimization

**Algorithm**:
1. For each hour, test employee combinations (2-4 employees)
2. Run Monte Carlo simulations (250 replications)
3. Select combination that minimizes mean wait time while meeting SLA
4. Track daily and weekly hours per employee
5. Enforce constraints:
   - Maximum daily hours per employee
   - Maximum weekly hours per employee

**Employee Selection**:
- Greedy selection: Fastest employees first
- Considers current daily/weekly hours worked
- Respects maximum hour constraints

### 4. Cost System

**Tier-Based Hourly Rates**:
- **Below Median Speed**: $18/hour
- **Above or At Median Speed**: $20/hour

**Median Calculation**: Based on `Drinks_Per_Min` values

### 5. Fragility Analysis

**Fragility Score Calculation**:
- **Demand per Employee**: `λ / num_employees` (higher = more fragile)
- **Wait Time Factor**: Normalized mean wait time (0-1 scale)
- **System Load Factor**: Average customers in system (0-1 scale)
- **Combined Score**: Weighted average (50% demand, 30% wait, 20% load)

**Fragility Classification**:
- **High-Risk Hours**: Fragility score > 0.7
- **Moderate Hours**: Fragility score 0.3-0.7
- **Robust Hours**: Fragility score < 0.3

## Visualizations

The notebook generates the following visualizations:

1. **Demand Simulation Heatmap**: Expected customers per day per hour
2. **Demand Patterns Line Plot**: Hourly demand trends by day
3. **Employee Schedule Heatmap**: Hours scheduled per employee per day
4. **Daily Cost Comparison**: Optimal vs Naive scheduling costs
5. **Cost Savings Percentage**: Daily savings as percentage
6. **Schedule and Cost Summary**: Weekly hours and cost comparison
7. **Monte Carlo Statistics**: Simulation parameters and total runs
8. **Fragility Analysis Heatmap**: Fragility scores by day and hour
9. **Top Fragile Hours**: Bar chart of most vulnerable hours

All visualizations are saved as high-resolution PNG files (300 DPI).

## Output Files

### Generated Images

- `demand_simulation_visualization.png`: Demand heatmap and patterns
- `employee_schedule_heatmap.png`: Employee schedule heatmap
- `cost_savings_analysis.png`: Daily cost comparison and savings
- `schedule_cost_summary.png`: Weekly summary and Monte Carlo stats
- `fragility_analysis.png`: Fragility heatmap and top fragile hours

### Data Structures

- `schedule_df_output`: DataFrame containing:
  - Day, Hour
  - Lambda_per_hour (expected customers)
  - Chosen_employees
  - Mean_wait_min
  - Avg_in_system_at_hour_end
  - Num_Employees

- `schedule_by_day`: DataFrame (employees × days) with hours scheduled

- `df_fragility`: DataFrame with fragility scores for each hour

## Key Metrics

### Cost Savings
- **Naive Schedule**: 4 employees × 11 hours/day × 7 days = 308 hours/week
  - Cost: (3 × $20 + 1 × $18) × 11 hours × 7 days = $6,160/week
- **Optimal Schedule**: Variable staffing based on demand
  - Typically achieves 5-10% cost savings
  - Maintains SLA requirements

### Fragility Metrics
- Average fragility score across all hours
- Maximum fragility score
- Number of high-risk hours (fragility > 0.7)
- Most fragile hour identification

## Usage Tips

1. **Run cells sequentially**: The notebook builds on previous results
2. **Check for errors**: Ensure `binge_data.xlsx` is in the correct location
3. **Review visualizations**: All plots are interactive in Jupyter
4. **Export results**: Use File → Download As to save the notebook
5. **Modify parameters**: Adjust employee data, demand thresholds, or SLA requirements as needed

## Dependencies

See `requirements.txt` for complete list. Key packages:
- `pandas`: Data manipulation
- `numpy`: Numerical computations
- `matplotlib`: Plotting
- `seaborn`: Statistical visualizations
- `scikit-learn`: Additional utilities (if needed)

## Notes

- The notebook uses Monte Carlo simulation with 250 replications per hour evaluation
- Employee service rates are based on drinks per minute (converted to services per second)
- Operating hours are 10:00 AM to 8:00 PM (11 hours per day)
- Schedule optimization considers both daily and weekly hour constraints
- Fragility analysis helps identify hours that may need buffer staffing
