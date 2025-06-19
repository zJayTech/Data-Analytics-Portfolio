I used R to visualize the percentage of high-risk cases based on preexisting conditions, showing that people with hypertension have the highest percentage of high-risk cases.

Code:

# ─────────────────────────────────────────────
# Install necessary packages
# ─────────────────────────────────────────────

install.packages(c("ggplot2", "readr", "dplyr"))

# ─────────────────────────────────────────────
# Load libraries
# ─────────────────────────────────────────────

library(ggplot2)
library(readr)
library(dplyr)

# ─────────────────────────────────────────────
# Load the dataset
# ─────────────────────────────────────────────

risk_data <- read_csv("COVID-RiskCalculation.csv")

# ─────────────────────────────────────────────
# Prepare the data
# ─────────────────────────────────────────────
# Ordering the conditions by risk ratio (descending) for clean plotting

risk_data <- risk_data %>%
  arrange(desc(risk_ratio)) %>%
  mutate(`Preexisting Condition` = factor(`Preexisting Condition`, levels = `Preexisting Condition`))

# ─────────────────────────────────────────────
# Create the bar chart
# ─────────────────────────────────────────────
ggplot(risk_data, aes(x = `Preexisting Condition`, y = risk_ratio)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  geom_text(aes(label = paste0(round(risk_ratio * 100), "%")), vjust = -0.5, size = 3.5) +
  labs(
    title = "COVID-19 High-Risk Rate by Preexisting Condition",
    subtitle = "Based on hospitalization or severe/critical symptoms",
    x = "Preexisting Condition",
    y = "Risk Ratio (Proportion of High-Risk Cases)"
  ) +
  theme_minimal() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    plot.title = element_text(face = "bold", size = 16),
    plot.subtitle = element_text(size = 12)
  )
