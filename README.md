# Bike Sharing Demand Analysis

[![R](https://img.shields.io/badge/R-276DC3?style=flat&logo=r&logoColor=white)](https://www.r-project.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A comprehensive statistical analysis of bike-sharing demand using Linear Models (LM) and Generalized Linear Models (GLM). This project explores how weather conditions, temporal factors, and their interactions influence bike rental patterns.

## Project Overview

This project analyzes historical data from a bike-sharing system to predict rental demand. We implement and compare two statistical modeling approaches:

- **Linear Model (LM)**: Uses square root transformation to meet normality assumptions
- **Generalized Linear Model (GLM)**: Directly models count data without transformation

## Objectives

- Identify key factors influencing bike rental demand
- Build predictive models using rigorous statistical methodology
- Compare different modeling approaches (LM vs GLM)
- Provide actionable insights for bike-sharing operators

## Project Structure

```
.
├── Bike_Sharing_Analysis.Rmd    # Main analysis file (merged LM + GLM)
├── projet.csv                    # Dataset
├── README.md                     # This file
└── outputs/                      # Generated reports and visualizations
```

## Requirements

### R Version
- R >= 4.0.0

### Required Libraries

```r
# Data manipulation
library(dplyr)
library(rsample)

# Visualization
library(ggplot2)
library(GGally)
library(plotly)
library(corrplot)
library(gridExtra)
library(cowplot)

# Statistical modeling
library(car)           # ANOVA Type 2, VIF
library(MASS)          # Stepwise selection
library(lmtest)        # Likelihood ratio tests
library(vcdExtra)      # Model comparison
library(ggfortify)     # Model diagnostics

# Reporting
library(knitr)
library(rmarkdown)
library(kableExtra)
```

### Installation

```r
# Install all required packages
install.packages(c("dplyr", "ggplot2", "GGally", "plotly", "corrplot", 
                   "car", "MASS", "lmtest", "vcdExtra", "ggfortify",
                   "rsample", "knitr", "rmarkdown", "kableExtra",
                   "gridExtra", "cowplot"))
```

## Usage

### Running the Analysis

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/bike-sharing-analysis.git
cd bike-sharing-analysis
```

2. **Ensure the dataset is in the project directory**
```bash
# Make sure projet.csv is present
ls projet.csv
```

3. **Open and run the R Markdown file**
```r
# In R or RStudio
rmarkdown::render("Bike_Sharing_Analysis.Rmd")
```

### Quick Start

```r
# Load the required libraries
source("load_libraries.R")  # If you create a separate script

# Or run the entire analysis
rmarkdown::render("Bike_Sharing_Analysis.Rmd", 
                  output_format = "all")
```

## Dataset Description

The dataset contains hourly bike rental records with the following variables:

### Temporal Variables
- **saison**: Season (1: Winter, 2: Spring, 3: Summer, 4: Fall)
- **mois**: Month (1-12)
- **jour_mois**: Day of month (1-31)
- **jour_semaine**: Day of week (1: Sunday - 7: Saturday)
- **horaire**: Time slot (5 categories)
- **jour_travail**: Working day indicator
- **vacances**: Holiday indicator

### Weather Variables
- **meteo**: Weather condition (1: Clear, 2: Cloudy/Foggy, 3: Rain/Snow)
- **temperature1**: Actual temperature (°C)
- **temperature2**: Feels-like temperature (°C)
- **humidite**: Humidity (%)
- **vent**: Wind speed (km/h)

### Target Variable
- **velos**: Number of bikes rented (count data)

## Methodology

### 1. Data Preprocessing
- Conversion of categorical variables to factors
- Data splitting: 80% training, 20% testing
- Exploratory data analysis and visualization

### 2. Linear Model (LM) Approach

**Steps:**
1. Square root transformation of target variable
2. Correlation analysis
3. Stepwise selection (backward, forward, both)
4. Interaction testing using Type 2 ANOVA
5. Outlier detection (Cook's distance)
6. Model diagnostics (residual plots, Durbin-Watson test)
7. Final model validation

**Key Features:**
- Meets normality assumptions through transformation
- Comprehensive interaction terms
- Rigorous assumption checking

### 3. Generalized Linear Model (GLM) Approach

**Steps:**
1. Direct modeling of count data
2. Studentized residuals for outlier detection
3. Likelihood ratio tests for variable selection
4. Polynomial feature engineering (temperature^4)
5. Extensive interaction modeling
6. Model comparison using AIC/BIC

**Key Features:**
- No transformation required
- Natural handling of count data
- Flexible interaction structure

## Key Findings

### Main Effects

| Variable | Impact | Significance |
|----------|--------|--------------|
| **Temperature** | Strong positive | High |
| **Season** | Summer highest | High |
| **Time of Day** | Peak: 11h-19h | High |
| **Weather** | Rain reduces demand | High |
| **Humidity** | Slight negative | Medium |
| **Wind** | Minimal direct effect | Low |

### Important Interactions

1. **Temperature × Season**: Effect varies by season
2. **Temperature × Month**: Monthly modulation of temperature effect
3. **Time of Day × Weather**: Weather impact depends on time
4. **Time of Day × Month**: Seasonal usage patterns
5. **Temperature × Humidity**: Combined atmospheric conditions

### Model Performance

| Model | MSE | Adjusted R² | Notes |
|-------|-----|-------------|-------|
| **LM (cleaned)** | Low | High | After outlier removal |
| **GLM (final)** | Low | N/A | Best AIC/BIC |

Both models demonstrate excellent predictive performance with low Mean Squared Error on the test set.

## Visualizations

The analysis includes comprehensive visualizations:

- **Distribution plots**: Boxplots for categorical variables
- **Scatter plots**: Continuous variable relationships
- **Heatmaps**: Interaction effects
- **Diagnostic plots**: Residual analysis, QQ-plots
- **Correlation matrices**: Variable relationships

### Sample Visualizations

```r
# Temperature vs. Rentals by Season
ggplot(train, aes(x = temperature1, y = velos, color = saison)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "glm")

# Monthly rental patterns
ggplot(train, aes(x = mois, y = velos, fill = mois)) +
  geom_boxplot()
```

## Practical Applications

This analysis can help bike-sharing operators:

1. **Demand Forecasting**: Predict future rental patterns
2. **Resource Allocation**: Optimize bike distribution across stations
3. **Maintenance Planning**: Schedule based on usage patterns
4. **Capacity Planning**: Anticipate peak demand periods
5. **Marketing Strategies**: Target campaigns based on weather/season
6. **Operational Efficiency**: Data-driven decision making

## Model Selection Criteria

### When to use Linear Model (LM):
- Need interpretable coefficients
- Want familiar regression diagnostics
- Comfortable with transformations
- Focus on R² and traditional metrics

### When to use GLM:
- Count data without transformation
- Need probabilistic predictions
- Want flexibility in link functions
- Focus on AIC/BIC for comparison

## Statistical Tests Used

- **Type 1 ANOVA**: Sequential variable testing
- **Type 2 ANOVA**: Marginal effect testing (controlling for others)
- **Likelihood Ratio Test (LRT)**: Model comparison
- **Durbin-Watson Test**: Autocorrelation detection
- **Cook's Distance**: Influential observation detection
- **F-tests**: Overall model significance


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Data source: Bike-sharing system historical records
- Course: Linear Models - University Paris Dauphine-PSL

## Contact

For questions or collaboration:
- Open an issue on GitHub
- Email: sacha.assouly@dauphine.eu
- LinkedIn: www.linkedin.com/in/sacha-assouly
