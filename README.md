# Health Consultations Dashboard Template

This template includes a Shiny app and an R Markdown report to explore and visualize epidemiological data from a DHIS2 instance.

## Features

- Pulls data via DHIS2 Web API
- Caches locally as `.rds`
- Option to toggle between API and local cache
- Includes summary tables and visualizations

## Setup

1. Clone the repo:
   ```bash
   git clone https://github.com/your-org/epi-dashboard-template.git
   ```
2. Rename config/config.yml.example to config.yml and enter your DHIS2 credentials.
    
3. Install required packages:
    install.packages(c("shiny", "httr2", "config", "here", "tidyverse"))

4. Run the Shiny app:
     shiny::runApp("app")

5. Knit the report:
      rmarkdown::render("reports/epi_report.Rmd")
