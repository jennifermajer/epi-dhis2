# Health Surveillance System

A comprehensive epidemiological surveillance system providing both interactive dashboards and automated reports for analyzing health consultation data, designed for International Medical Corps-supported facilities in Syria.

## 🎯 Dual-Mode Analysis

**Interactive Shiny Dashboard**: Real-time data exploration with dynamic visualizations, filtering, and drill-down capabilities

**Automated R Markdown Reports**: Static, professional reports with comprehensive analysis suitable for sharing with stakeholders and decision-makers

Both modes leverage the same robust data processing engine and can be configured with optional AI-powered insights.

## 📊 Data Requirements and Format

This epidemiological surveillance system was **built using data from DHIS2** and requires **line list data format** (individual consultation records) to function as designed. It currently does **not integrate aggregate data**, but aggregate data support is planned for future releases.

### Required Data Structure

The system expects individual consultation records (line list format) with the following core variables:

#### Core Required Variables
- **`datevisit`** - Date of consultation/visit (accepts multiple date formats)
- **`morbidity`** - Disease/condition diagnosis (primary classification field)  
- **`orgunit`** - Health facility/organization unit name

#### Essential Demographic Variables
- **`sex`** - Patient gender/sex
- **`age_group`** - Age group classification (0-5, 6-17, 18-59, 60+)
- **`resident`** - Resident status (resident, IDP, refugee, etc.)

#### Geographic Variables  
- **`admin1`** - Primary administrative division (governorate/state/province)
- **`admin2`** - Secondary administrative division (district/sub-district)
- **`orgunit_code`** - Unique facility identifier code

#### Classification Variables
- **`type_case`** - Case type classification (Trauma/Non-trauma)
- **`facilitytype`** - Type of health facility (PHC, Hospital, MMU, etc.)
- **`visit_type`** - Type of visit (new, follow-up, emergency)
- **`visit_number`** - Sequential visit number for patient

#### Optional Enhancement Variables
- **`disability`** - Patient disability status (Y/N)
- **`donor`** - Funding/donor organization
- **`project`** - Specific project/program identifier

### Flexible Column Recognition

The system includes intelligent column recognition that automatically detects variations in column naming:

```r
# Examples of recognized column name variations:
datevisit: "datevisit", "Date of visit", "Event date", "eventdate", "visitdate"
orgunit: "Organisation unit name", "orgunit", "ouname", "facilityname"
morbidity: "SY - Morbidity classification", "morbidity", "diagnosis", "condition"
sex: "Gender", "sex"
age_group: "Age group 1 (0-5,6-17,18-59,60+)", "agegroup", "age group"
```

### Data Quality Requirements

**Minimum Data Quality Standards:**
- Core required variables (`datevisit`, `morbidity`, `orgunit`) must be present
- At least 50% of records should have non-missing values for essential variables
- Date fields must be parseable (supports multiple date formats: YYYY-MM-DD, DD/MM/YYYY, MM/DD/YYYY)
- Geographic variables recommended for spatial analysis features

**Data Source Compatibility:**
- **Primary**: DHIS2 line list exports (tracker programs, event programs)
- **Secondary**: CSV/Excel exports with compatible column structure
- **Planned**: Direct DHIS2 API integration for real-time data access

### Current Limitations

- **Aggregate Data**: Currently does not support aggregate/summary data formats
- **Multiple Programs**: Optimized for single program line lists (can handle multiple but may require preprocessing)
- **Historical Data**: Performance optimized for datasets up to 1M+ records

### Future Development

**Planned Enhancements:**
- Direct aggregate data integration from DHIS2 analytics
- Multi-program data harmonization
- Real-time data streaming capabilities
- Enhanced data validation and quality scoring
- Completed taxonomy system

## 🚀 Key Capabilities

### Interactive Dashboard (Shiny App)
- **Real-time Exploration**: Dynamic filtering, drill-down analysis, and interactive visualizations
- **Disease Surveillance**: Advanced outbreak detection with configurable alert thresholds
- **Geographic Analysis**: Interactive maps with facility-level and regional insights
- **EpiBot Integration**: AI-powered chatbot for natural language data queries
- **Export Functions**: Download charts, tables, and filtered datasets

### Static Reports (R Markdown)
- **Comprehensive Analysis**: Professional epidemiological reports with 13+ analytical sections
- **AI-Enhanced Summaries**: Optional LLM-generated insights and recommendations
- **Consistent Formatting**: Standardized report structure with executive summaries
- **Publication Ready**: High-quality outputs suitable for stakeholders and decision-makers
- **Batch Generation**: Automated report creation with configurable parameters

### Core Infrastructure
- **Taxonomy-Driven**: Standardized disease categorization with Syria-specific mappings
- **Flexible Data Sources**: Support for cached data or live DHIS2 API connections
- **Modular Architecture**: Clean, maintainable codebase with reusable components
- **Configuration-Driven**: Easy feature toggling through YAML configuration
- **Responsive Design**: Works across desktop, tablet, and mobile devices

## 📁 Project Structure

```text
epidashboard/
├── app.R                         # Shiny app entry point (detects layout)
├── app/                          # Main Shiny application
│   ├── global.R                  # Global data loading and configuration
│   ├── ui.R                      # Dashboard UI with modular components
│   ├── server.R                  # Dashboard server logic
│   └── www/                      # Static assets (logos, stylesheets)
├── R/                            # Core analysis functions
│   ├── disease_categories_taxaware.R  # Taxonomy-aware disease classification
│   ├── summary_functions.R       # Standardized summary calculations
│   ├── visualization_helpers.R   # Chart and plot generation
│   ├── data_loader.R            # DHIS2 API and data processing
│   ├── enhanced_ai.R            # AI integration layer
│   ├── validation_rules.R       # Data quality checks
│   ├── geographic_helpers.R     # Spatial analysis functions
│   └── preprocessing.R          # Data cleaning and transformation
├── modules/                      # Modular Shiny components
│   ├── mod_overview.R           # Dashboard overview and summary cards
│   ├── mod_disease_surveillance.R  # Disease trend analysis
│   ├── mod_geographic.R         # Geographic and facility analysis
│   ├── mod_time_trends.R        # Temporal pattern analysis
│   ├── mod_epibot.R            # AI chatbot integration
│   ├── mod_settings.R          # Configuration interface
│   ├── ai_config.R             # AI configuration and customization bridge
│   └── chat_assistant.R        # Enhanced chatbot module with context awareness
├── taxonomy/                     # Disease classification system
│   ├── base.yaml               # Global disease taxonomy (WHO-aligned)
│   ├── syria.yaml              # Syria-specific extensions and mappings
│   └── README_Taxonomy.md      # Taxonomy usage documentation
├── reports/                      # Static report generation
│   ├── epi_report.Rmd          # Comprehensive epidemiological report
│   ├── epi_report.html         # Generated report output
│   └── epi_report_cache/       # Report caching for performance
├── config/
│   ├── config.yml              # Feature flags and system configuration
│   └── ai_customization.yml    # AI behavior customization for different deployment contexts
├── data/                         # Data storage and metadata
│   ├── dhis2_cache.rds         # Cached consultation data
│   ├── data_dictionary.csv     # Field definitions and mappings
│   ├── metadata/               # Organizational and geographic metadata
│   │   ├── Org Unit Metadata.xlsx
│   │   └── syr_adminboundaries_tabulardata.xlsx
│   └── shapefiles/            # Geographic boundary files
├── ai_customization/            # AI feature customization
│   ├── ai_customization_guide.md
│   └── ai_customization_template.xlsx
├── deploy.R                     # Deployment automation
├── renv.lock                   # R package dependencies
└── README.md                   # This documentation
```



### Architecture Highlights

**Taxonomy-Driven Design**: Disease classifications follow WHO standards with Syria-specific extensions, enabling consistent analysis across different data sources and time periods.

**Dual-Mode Operation**: Single codebase supports both interactive dashboard exploration and automated report generation, sharing the same analytical engine.

**AI-Optional Integration**: All AI features (EpiBot chatbot, LLM summaries) are completely optional and configurable, allowing deployment with or without AI capabilities.

**Modular Components**: Each analytical section is implemented as a reusable module, enabling easy customization and extension for different use cases.

## 🗂️ Disease Taxonomy System

The system uses a sophisticated, WHO-aligned disease classification system that enables consistent analysis across different data sources and time periods. UNDER DEVELOPMENT.

### Taxonomy Structure

**Base Taxonomy (`taxonomy/base.yaml`)**
- Global disease categorization aligned with WHO ICD-11 standards
- Primary categories: Respiratory, Enteric, Vector-borne, Non-communicable, etc.
- Standardized disease names using British English spelling conventions
- Epidemic disease groupings and surveillance thresholds
- Severity mappings and alert configurations

**Syria-Specific Extensions (`taxonomy/syria.yaml`)**
- Inherits all base classifications automatically
- Syria-specific terminology mappings (Arabic → English, local variants)
- Context-specific disease groupings for Syrian epidemiological priorities
- Customized alert thresholds based on local surveillance patterns

### Key Features

**Disease Classification**: Maps any diagnostic term to standardized WHO categories
**Synonym Management**: Handles variations in how conditions are recorded (abbreviations, spellings, languages)
**Epidemic Detection**: Groups diseases by transmission patterns for outbreak surveillance
**Severity Assessment**: Categorizes conditions by clinical severity for burden analysis

## 🤖 AI Integration (Optional)

All AI features are completely optional and controlled through configuration. The system works fully without AI enabled.

### AI Features in Dashboard (EpiBot)

**EpiBot Chatbot**: Interactive AI assistant that answers natural language questions about your epidemiological data
- Query disease patterns: "What are the top respiratory diseases this month?"
- Geographic analysis: "Which regions have the highest malnutrition rates?"
- Temporal trends: "How has cholera incidence changed over the past year?"
- Data exploration: "Show me trauma cases by age group and location"

**Technical Implementation**: Uses local LLM (Ollama) or cloud-based AI services
**Privacy**: All processing can be done locally without data leaving your infrastructure

### AI Features in Reports

**LLM-Generated Summaries**: AI-powered insights and analysis throughout the static report
- Executive summaries with key findings and implications
- Disease-specific analysis with epidemiological context
- Geographic pattern recognition and hotspot identification
- Recommendations based on surveillance data patterns

**Smart Analysis**: AI identifies anomalies, trends, and patterns that might be missed in manual review

### Advanced AI Customization

The system now supports **context-aware AI responses** that automatically adapt to different deployment environments and organizational needs through the comprehensive customization system.

#### Deployment-Specific AI Behavior

**Refugee Camp Context**: AI responses focus on overcrowding risks, rapid transmission potential, and immediate containment strategies with shortened action timeframes (0-24h emergency response).

**Urban Clinic Context**: Analysis emphasizes population density patterns, mobility-driven transmission, and community health approaches suitable for urban healthcare delivery.

**Rural Health Context**: AI considers geographic isolation, seasonal patterns, resource constraints, and community-based intervention strategies with extended implementation timelines.

**Emergency Response Context**: Prioritizes immediate threat assessment, rapid response protocols, and life-saving interventions with emergency action thresholds.

#### Customizable AI Templates

Create organization-specific AI prompts and analysis approaches through `config/ai_customization.yml`:

```yaml
# Example: Custom AI templates for Syrian refugee camp context
context:
  context_name: "syria_humanitarian"
  setting_description: "Humanitarian health surveillance in conflict-affected Syria"
  population_description: "Internally displaced persons, refugees, conflict-affected populations"
  
diseases:
  endemic_diseases:
    - "Acute watery diarrhea"
    - "Respiratory infections" 
    - "Skin diseases"
    - "Malnutrition"

thresholds:
  outbreak_multiplier: 2.0        # Alert threshold sensitivity
  high_burden_threshold: 50       # Case count for high burden classification
  geographic_spread_threshold: 2  # Areas for spread concern

prompts:
  executive_summary:
    template: "Generate concise executive summary for {setting_description}. Focus on {surveillance_priorities}."
    max_words: 200
```

#### AI Configuration Hierarchy

**Base Configuration** (`config/config.yml`):
```yaml
ai_features:
  enabled: true
  provider: "ollama"          # or "openai", "anthropic"
  
  ollama:
    host: "http://localhost:11434"
    model: "llama3.2:3b"
    temperature: 0.1
    
  chat_assistant:
    enabled: true
    max_history: 50
    include_context: true
    
  security:
    data_anonymization: true
    audit_enabled: true
```

**User Customization Override** (`config/ai_customization.yml`):
- Deployment-specific settings automatically detected
- Custom disease priorities and thresholds  
- Organizational terminology and constraints
- Context-appropriate response templates
- Multi-language support capabilities

### AI System Architecture

**Context-Aware Processing**: The AI system automatically detects deployment type based on configuration and data patterns, then adapts:
- **Prompt templates** to use appropriate terminology and focus areas
- **Analysis priorities** to match organizational surveillance goals  
- **Alert thresholds** to deployment-specific risk levels
- **Recommendation timeframes** to operational constraints

**Dynamic Template System**: AI prompts are no longer hard-coded but dynamically generated based on:
- Deployment context (refugee camp, urban clinic, rural health, emergency)
- User-defined priorities and constraints
- Disease burden patterns specific to the operational area
- Custom organizational terminology and approaches

**Validation and Fallbacks**: Robust error handling ensures the system functions even with partial customization or AI service interruptions.

## 🔬 Advanced Analytics Modules

The system includes sophisticated analytical capabilities beyond basic surveillance through specialized modules:

### Core Analytics Modules

**Disease Surveillance (`mod_disease_surveillance.R`)**
- Statistical outbreak detection using control charts and epidemic thresholds
- Weekly surveillance with automated alert generation
- Disease burden analysis with severity classification
- Temporal pattern recognition and seasonality detection

**Geographic Analysis (`mod_geographic.R`)**  
- Interactive mapping with facility-level and regional visualization
- Spatial clustering analysis for hotspot identification
- Geographic risk assessment and accessibility analysis
- Administrative boundary analysis with population weighting

**Time Trends Analysis (`mod_time_trends.R`)**
- Advanced time series analysis with trend decomposition
- Seasonal pattern detection and forecasting capabilities
- Multi-variable correlation analysis across time periods
- Epidemic curve generation with contextual annotations

### Advanced Features (Under Development)

**Predictive Analytics Module**
- Epidemiological forecasting using time series models
- Risk factor analysis and predictive risk scoring
- Early warning systems for outbreak prediction
- Resource planning and capacity forecasting

**Climate Integration Module**
- Weather and climate data integration for environmental health analysis
- Seasonal disease pattern modeling with meteorological variables
- Climate-sensitive disease surveillance (vector-borne, water-related)
- Environmental risk factor quantification

**Multi-Disease Correlation Analysis**
- Cross-disease surveillance for syndrome detection
- Comorbidity analysis and disease interaction patterns
- Population health burden assessment across conditions
- Intervention impact analysis across disease categories

### Module Architecture

**Modular Design**: Each analytical component is built as a self-contained Shiny module with standardized inputs/outputs, enabling easy customization and deployment flexibility.

**Data Pipeline Integration**: Advanced modules seamlessly integrate with the core data processing pipeline, automatically inheriting taxonomy classifications and data quality validations.

**Configuration-Driven**: Module activation and parameters are controlled through the configuration system, allowing deployments to enable only needed capabilities.

**Performance Optimization**: Advanced analytics use efficient algorithms and caching strategies to handle large datasets without compromising dashboard responsiveness.

## 📝 Using and Updating the Taxonomy

### For Syria HIS Focal Points

**Adding New Disease Conditions**
When new diagnostic terms appear in your HMIS data:

1. **Check existing mappings** in `taxonomy/syria.yaml` first
2. **Add to categories section** if it's a genuinely new condition:
   ```yaml
   categories:
     "New Condition Name": "Appropriate Primary Category"
   ```
3. **Map local terminology** in the synonyms section:
   ```yaml
   synonyms:
     "Local Arabic Term": "Standardized English Term"
     "Common Abbreviation": "Full Standardized Name"
   ```

**Updating Surveillance Thresholds**
Adjust outbreak detection sensitivity based on observed patterns:

```yaml
alert_thresholds:
  cholera:
    recent_cases_threshold: 5      # Alert if 5+ cases in recent period
    recent_days: 7                 # Define recent period as 7 days
    ratio_threshold: 2.0           # Alert if 2x historical average
```

### For Other Countries

**Creating Country-Specific Taxonomy**
1. **Copy structure**: Use `taxonomy/syria.yaml` as template
2. **Replace Syria content** with your country's terms and priorities
3. **Maintain inheritance**: Keep `inherits: "base.yaml"` to get global standards
4. **Customize priorities**: Adjust epidemic groupings for your context

**Example for Jordan:**
```yaml
meta:
  name: "Jordan Canonicals"
  version: "2025-01-01"
  inherits: "base.yaml"

categories:
  "Jordan-specific condition": "Primary Category"

synonyms:
  "Local Jordanian term": "Standard English equivalent"
```

### Version Control Best Practices

**Making Changes**
1. **Update version** in meta section when making changes
2. **Test thoroughly** with sample data before deploying
3. **Document rationale** for major taxonomy changes
4. **Backup previous versions** before updates

**Validation Process**
1. **Review with epidemiologists** to ensure clinical accuracy
2. **Test with data processing pipeline** to catch errors
3. **Check downstream impacts** on reports and dashboards
4. **Monitor for unintended classification changes**

### Technical Integration

The taxonomy files are automatically loaded by the system functions:

- **`R/disease_categories_taxaware.R`** handles taxonomy loading and processing
- **Dashboard and reports** use taxonomy-aware classification automatically
- **No code changes needed** when updating taxonomy files
- **System validates** taxonomy structure on startup

For detailed taxonomy documentation, see `taxonomy/README_Taxonomy.md`.

## ⚙️ Prerequisites

### Required Software
- **R (≥ 4.0.0)** - [Download R](https://cran.r-project.org/)
- **RStudio** (recommended) - [Download RStudio](https://www.rstudio.com/products/rstudio/download/)
- **Pandoc** - Bundled with RStudio

### Optional (for AI features)
- **Ollama** - [Download Ollama](https://ollama.ai) for local AI processing

## 📦 Installation

### 1. Clone Repository
```bash
git clone <repository-url>
cd epidashboard
```

### 2. Install R Packages
```r
# Core packages (required)
install.packages(c(
  "shiny", "shinydashboard", "shinydashboardPlus", "shinyWidgets", "shinyjs",
  "DT", "plotly", "leaflet", "dplyr", "tidyr", "ggplot2", "scales",
  "viridis", "RColorBrewer", "readxl", "lubridate", "here",
  "config", "httr", "jsonlite", "logger", "treemapify",
  "gtsummary", "flextable"
))

# Optional packages (for enhanced features)
install.packages(c("openxlsx", "sf", "rmarkdown"))
```

### 3. Configure Application
Edit `config/config.yml`:

```yaml
default:
  # DHIS2 Configuration
  dhis2:
    base_url: "https://your-dhis2-instance.org/api/"
    username: "your_username"
    password: "your_password"
  
  # Data settings
  cache:
    use_cache: true
    cache_expiry_hours: 24
  
  # Feature flags
  features:
    ai_enabled: false           # Set to true for AI features
    chatbot_enabled: false      # Set to true for chatbot
    advanced_surveillance: true
    geographic_analysis: true
```

### 4. Prepare Data
**Option A: Use cached data (recommended)**
- Place your processed data file at `data/dhis2_data.rds`
- Ensure `use_cache: true` in configuration

**Option B: Configure DHIS2 API**
- Set `use_cache: false` in configuration
- Provide valid DHIS2 credentials and parameters
- Configure organizational units and dimensions

### 5. Add Metadata (Optional)
- Place organizational metadata at `data/metadata/Org Unit Metadata.xlsx`
- Add shapefiles for geographic analysis in `data/shapefiles/`

## 🚀 Running the System

### Interactive Dashboard Mode

**Local Development**
```r
# Test the application
source("deploy.R")
test_local()

# Run the interactive dashboard
shiny::runApp("app")
```

The dashboard will be available at `http://localhost:3838` with full interactivity, filtering, and EpiBot integration (if AI enabled).

### Static Report Generation

**Basic Reports**
```r
# Generate comprehensive epidemiological report
rmarkdown::render("reports/epi_report.Rmd")
```

**AI-Enhanced Reports** (requires Ollama or cloud AI setup)
```r
# Report with AI-generated insights and recommendations
rmarkdown::render(
  "reports/epi_report.Rmd", 
  params = list(enable_ai = TRUE)
)
```

**Batch Report Generation**
```r
# Generate reports for different time periods
dates <- seq(as.Date("2024-01-01"), as.Date("2024-12-01"), by = "month")
for(date in dates) {
  rmarkdown::render(
    "reports/epi_report.Rmd",
    params = list(date_end = date),
    output_file = paste0("epi_report_", format(date, "%Y_%m"), ".html")
  )
}
```

## 🌐 Deployment

### Setup Deployment
```r
source("deploy.R")
setup_deployment()  # Configure ShinyApps.io credentials
```

### Deploy Application
```r
# Standard deployment (no AI)
deploy_standard()

# With AI features
deploy_with_ai()

# Custom deployment
deploy_epidashboard(
  app_name = "my-epidashboard",
  include_ai = FALSE,
  force_upload = TRUE
)
```

## 🤖 AI Setup and Configuration

The dashboard includes optional AI-powered features with comprehensive customization capabilities:

### Quick AI Setup

1. **Install Ollama**: Download from [ollama.ai](https://ollama.ai)
2. **Install AI Model**:
   ```bash
   ollama pull llama3.2:3b
   ```
3. **Enable in Configuration**:
   ```yaml
   ai_features:
     enabled: true
     provider: "ollama"
     
     ollama:
       host: "http://localhost:11434"
       model: "llama3.2:3b"
       temperature: 0.1
       
     chat_assistant:
       enabled: true
       max_history: 50
   ```

### Advanced AI Customization

**Context-Aware Deployment Setup**: The AI system automatically adapts to your operational context when properly configured in `config/ai_customization.yml`:

```yaml
# Deployment context detection and AI behavior adaptation
context:
  context_name: "syria_humanitarian"          # Your deployment identifier
  setting_description: "Refugee camp health surveillance"  # AI uses this in prompts
  deployment_type: "refugee_camp"             # Auto-detected: refugee_camp, urban_clinic, rural_health, emergency_response

# AI will automatically adjust its:
# - Alert sensitivity and outbreak thresholds  
# - Response timeframes and urgency levels
# - Analysis focus areas and priorities
# - Terminology and recommendations
```

**Full Customization Example**: Complete AI behavior customization for Syrian refugee camp deployment:

```yaml
meta:
  version: "1.0"
  deployment_date: "2025-01-24"
  description: "Syria humanitarian health surveillance AI customization"

context:
  context_name: "syria_humanitarian"
  setting_description: "Humanitarian health surveillance in conflict-affected Syria"
  population_description: "Internally displaced persons, refugees, conflict-affected populations"
  
  operational_constraints:
    - "Limited healthcare access"
    - "Security challenges"
    - "Resource constraints"
    - "Intermittent supply chains"
  
  surveillance_priorities:
    - "Outbreak detection"
    - "Vulnerable population health"
    - "Cost-effective interventions"
    - "Preventable disease burden"

diseases:
  endemic_diseases:
    - "Acute watery diarrhea"
    - "Respiratory infections"
    - "Skin diseases"
    - "Malnutrition"
    - "Urinary tract infections"
    
  epidemic_diseases:
    - "Cholera"
    - "Suspected Cholera"
    - "Measles"
    - "Suspected Measles"
    - "Meningitis"

populations:
  high_priority:
    - "children under 5"
    - "pregnant women"
    - "lactating women"
    - "elderly (65+)"
    - "malnourished individuals"

thresholds:
  outbreak_multiplier: 2.0        # 2x baseline triggers alerts
  high_burden_threshold: 50       # 50+ cases = high burden
  geographic_spread_threshold: 2  # 2+ areas = geographic spread concern
  
ai_params:
  temperature: 0.1               # Low temperature = more focused responses
  max_tokens: 1000              # Maximum response length
  response_style: "professional" # Professional tone for reports
  include_recommendations: true  # Always include actionable recommendations

prompts:
  executive_summary:
    template: "Generate a concise executive summary of epidemiological patterns for {setting_description}. Focus on {surveillance_priorities}. Consider constraints: {operational_constraints}."
    max_words: 200
    
  outbreak_alert:
    template: "Assess outbreak risk for {epidemic_diseases} in {setting_description}. Alert if patterns exceed {outbreak_multiplier}x baseline. Consider {operational_constraints}."
    max_words: 150
```

### AI System Capabilities

- **Context-Aware Analysis**: AI automatically adapts responses based on deployment type (refugee camp vs urban clinic vs rural health vs emergency response)
- **Dynamic Prompts**: AI templates change based on your configuration - no more generic responses
- **Custom Disease Priorities**: AI focuses on diseases relevant to your operational context
- **Deployment-Specific Recommendations**: Action timeframes and strategies appropriate for your setting
- **Multi-Language Support**: Configurable for different languages and terminology
- **Privacy-First**: All processing can be done locally without data leaving your infrastructure

## 📊 Key Features

### Dashboard Overview
- **Summary Cards**: Key metrics with trend indicators
- **Time Series**: Interactive consultation trends over time
- **Disease Distribution**: Top conditions and burden analysis
- **Demographics**: Age-sex pyramids and population analysis

### Disease Surveillance
- **Outbreak Detection**: Statistical alert system for epidemic diseases
- **Geographic Hotspots**: Spatial analysis of disease patterns
- **Trend Monitoring**: Weekly surveillance of priority conditions
- **Alert Management**: Configurable thresholds and notifications

### Reports & Analytics
- **Standard Reports**: Comprehensive epidemiological analysis
- **Custom Exports**: Data export in CSV and Excel formats
- **Interactive Visualizations**: All charts support zooming and filtering
- **Print-Ready Formats**: Professional report layouts

## 🔧 Configuration Options

### Feature Flags
Control which features are available:

```yaml
features:
  ai_enabled: false              # AI-powered analysis
  chatbot_enabled: false         # Interactive chat assistant
  advanced_surveillance: true    # Enhanced surveillance features
  geographic_analysis: true      # Maps and spatial analysis
  automated_reports: true        # Report generation
```

### Data Sources
Choose your data source:

```yaml
cache:
  use_cache: true               # Use local cache file
  cache_expiry_hours: 24        # Cache refresh interval

# OR for live API access:
dhis2:
  base_url: "your-api-url"
  username: "username"
  password: "password"
```

## 🔍 Troubleshooting

### Common Issues

**1. "No data available"**
- Check that `data/dhis2_data.rds` exists
- Verify DHIS2 API credentials if using live data
- Review configuration in `config/config.yml`

**2. "Package not found" errors**
- Install missing packages: `install.packages("package_name")`
- Check R version compatibility (≥ 4.0.0 required)

**3. "AI features not working"**
- Ensure Ollama is installed and running: `ollama serve`
- Check AI model is available: `ollama list`
- Verify AI is enabled in configuration

**4. Deployment issues**
- Run deployment setup: `setup_deployment()`
- Check file size limits (< 100MB recommended)
- Verify ShinyApps.io account credentials

### Getting Help
1. Check application logs in `logs/` directory
2. Run diagnostic functions in `deploy.R`
3. Review configuration settings
4. Test locally before deployment

## 📝 Customization

### Disease Categories
Update disease classifications in `R/data_processing.R`:

```r
# Modify get_morbidity_categories() function
# Add your program-specific disease groupings
```

### Surveillance Thresholds
Adjust outbreak detection sensitivity:

```r
# In surveillance module, modify threshold multipliers
# Higher values = fewer alerts (more conservative)
# Lower values = more alerts (more sensitive)
```

### Visual Styling
Customize colors and themes in `R/visualizations.R`:

```r
# Modify theme_epi_dashboard() function
# Update APP_COLORS list in global.R
```

## 🤝 Contributing

### Development Workflow
1. **Test Locally**: Always test changes with `test_local()`
2. **Follow Patterns**: Use existing code patterns and naming conventions
3. **Document Changes**: Update documentation and comments
4. **Validate Data**: Ensure changes work with different data scenarios

### Code Style
- Use clear, descriptive function names
- Include error handling and logging
- Follow tidyverse coding conventions
- Add comments for complex logic

## 📊 Performance

### Optimization Tips
- **Use Data Caching**: Enable `use_cache: true` for better performance
- **Filter Large Datasets**: Apply date/geographic filters to reduce data size
- **Monitor Memory**: Large datasets may require server resources
- **Enable Logging**: Monitor performance through application logs

### Scalability
- Supports datasets up to 1M+ records (with adequate server resources)
- Modular architecture allows selective feature loading
- Efficient data processing with optimized queries
- Responsive design adapts to different screen sizes

## 📞 Support

For technical support, feature requests, or questions:
- Contact: Jennifer Majer, Sr M&E and Research Advisor
- Documentation: See inline code comments and function documentation
- Issues: Use the project's issue tracking system

---

*Last Updated: August 2025*
*Version: 2.0.0 - Streamlined Architecture*