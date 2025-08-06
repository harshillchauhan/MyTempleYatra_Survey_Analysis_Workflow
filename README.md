# MyTempleYatra Survey Analysis Workflow

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Workflow Components](#workflow-components)
- [Database Schema](#database-schema)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [Data Flow](#data-flow)
- [AI Analysis](#ai-analysis)
- [Monitoring & Analytics](#monitoring--analytics)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🌟 Overview

The **MyTempleYatra Survey Analysis Workflow** is a comprehensive N8N automation that processes spiritual tourism survey data, performs intelligent analysis, and generates actionable business insights. This workflow is designed for **MyTempleYatra**, a spiritual tourism company that organizes temple visits and religious experiences.

### Key Capabilities
- 🔄 **Real-time Survey Processing**: Automatically processes incoming survey responses from Zoho Forms
- 📊 **Intelligent Data Structuring**: Organizes raw data into meaningful business collections
- 🤖 **AI-Powered Analysis**: Uses OpenAI GPT-4 for comprehensive survey analysis
- 📈 **Business Intelligence**: Generates actionable insights and strategic recommendations
- 🗄️ **Multi-Collection Storage**: Stores data in structured MongoDB collections for easy querying
- ⏰ **Automated Reports**: Weekly automated analysis generation
- 📱 **Real-time Webhooks**: Instant processing of new survey submissions

## 🏗️ Architecture

```
┌─────────────────┐    ┌──────────────┐    ┌─────────────────┐
│   Zoho Forms    │────│   Webhook    │────│   N8N Workflow  │
│   Survey Data   │    │   Trigger    │    │   Processing    │
└─────────────────┘    └──────────────┘    └─────────────────┘
                                                     │
                       ┌─────────────────────────────┼─────────────────────────────┐
                       │                             │                             │
                       ▼                             ▼                             ▼
              ┌─────────────────┐           ┌─────────────────┐           ┌─────────────────┐
              │   Data          │           │   MongoDB       │           │   AI Analysis   │
              │   Processing    │           │   Storage       │           │   (OpenAI)      │
              │   & Validation  │           │   (5 Collections)│          │   Generation    │
              └─────────────────┘           └─────────────────┘           └─────────────────┘
                       │                             │                             │
                       └─────────────────────────────┼─────────────────────────────┘
                                                     │
                                            ┌─────────────────┐
                                            │   Business      │
                                            │   Intelligence  │
                                            │   Dashboard     │
                                            └─────────────────┘
```

## ✨ Features

### 🔄 **Real-Time Data Processing**
- Webhook-triggered survey processing
- Automatic data validation and cleaning
- Multi-format data handling (arrays, strings, ratings)
- Error handling and logging

### 📊 **Intelligent Data Organization**
- **Raw Data Collection**: Complete survey responses with metadata
- **Respondent Profiles**: Demographic and contact information
- **Experience Analysis**: Past yatra experiences and feedback
- **Motivation Insights**: Decision-making factors and preferences
- **Market Intelligence**: Budget preferences and content consumption

### 🤖 **AI-Powered Analytics**
- Comprehensive survey analysis using GPT-4
- Customer segmentation and persona development
- Service gap analysis and recommendations
- Market opportunity identification
- ROI projections and business impact assessment

### 📈 **Business Intelligence**
- Executive dashboard with key metrics
- Customer satisfaction scoring
- Net Promoter Score (NPS) estimation
- Service performance benchmarking
- Competitive analysis insights

### ⚡ **Automation Features**
- Weekly automated analysis reports
- Real-time webhook processing
- Automatic data backup and archival
- Error monitoring and alerting

## 🔧 Prerequisites

### Software Requirements
- **N8N** (v1.0+) - Workflow automation platform
- **MongoDB** (v5.0+) - Database for data storage
- **Node.js** (v18+) - Runtime environment
- **OpenAI API** - For AI analysis capabilities

### API Keys & Access
- OpenAI API key with GPT-4 access
- MongoDB connection string
- Zoho Forms webhook access
- N8N deployment environment

### Technical Skills
- Basic understanding of N8N workflows
- MongoDB query knowledge (optional)
- JavaScript for customizations (optional)

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/mytempleyatra-survey-workflow.git
cd mytempleyatra-survey-workflow
```

### 2. Environment Setup
Create a `.env` file based on `.env.example`:
```bash
cp .env.example .env
```

### 3. Configure Environment Variables
```env
# MongoDB Configuration
MONGODB_CONNECTION_STRING=mongodb://username:password@host:port/database
MONGODB_DATABASE_NAME=mytempleyatra_survey

# OpenAI Configuration
OPENAI_API_KEY=your_openai_api_key_here
OPENAI_MODEL=gpt-4

# Webhook Configuration
WEBHOOK_BASE_URL=https://your-n8n-instance.com
WEBHOOK_PATH=/webhook/survey-response

# N8N Configuration
N8N_ENCRYPTION_KEY=your_encryption_key
N8N_USER_MANAGEMENT_JWT_SECRET=your_jwt_secret
```

### 4. Import Workflow to N8N
1. Open your N8N instance
2. Go to **Workflows** → **Import from File**
3. Upload the `MyTempleYatra_Survey_Analysis_Workflow.json` file
4. Configure credentials as described below

## ⚙️ Configuration

### 1. MongoDB Credentials Setup
1. In N8N, go to **Credentials** → **Add Credential**
2. Select **MongoDB**
3. Configure connection details:
   ```
   Connection String: mongodb://username:password@host:port/database
   Database: mytempleyatra_survey
   ```

### 2. OpenAI Credentials Setup
1. Add **OpenAI** credential in N8N
2. Enter your OpenAI API key
3. Select model: `gpt-4` (recommended)

### 3. Webhook Configuration
1. Update webhook URL in the trigger node
2. Configure webhook authentication if required
3. Test webhook connectivity

### 4. Database Collections Setup
The workflow automatically creates these collections:
- `raw_data` - Complete survey responses
- `respondent_info` - Demographic information
- `previous_yatra_experiences` - Past travel experiences
- `yatra_motivation` - Motivation and preference data
- `preference_collection` - Travel preferences and criteria
- `survey_analysis_results` - AI-generated analysis results

## 🧩 Workflow Components

### 1. **Zoho Survey Trigger**
- **Type**: Webhook Trigger
- **Function**: Receives survey responses from Zoho Forms
- **Configuration**: POST endpoint with JSON payload
- **Response**: Returns acknowledgment status

### 2. **Data Processing Code Node**
- **Function**: Processes and validates incoming survey data
- **Features**:
  - Cleans bracket notation values `[25-46]` → `25-46`
  - Handles array data joining with commas
  - Generates unique IDs for tracking
  - Creates structured output with 38+ mapped fields

### 3. **Raw Data Storage**
- **Collection**: `raw_data`
- **Purpose**: Complete backup of all survey responses
- **Fields**: All original survey fields plus metadata

### 4. **Data Distribution Nodes**
- **Code1**: Structures previous yatra experience data
- **Code2**: Organizes motivation and service importance data  
- **Code3**: Processes preference and market data
- **No Operation**: Passes respondent info unchanged

### 5. **Specialized Collections**
#### Respondent Info
```javascript
{
  survey_id: "unique_identifier",
  name: "respondent_name",
  age: "age_range",
  current_residence: "location",
  family_structure: "family_type",
  contact_method: "preferred_contact",
  whatsapp: "phone_number",
  email: "email_address"
}
```

#### Previous Yatra Experiences
```javascript
{
  respondent_info: {
    survey_id: "reference_id",
    name: "respondent_name",
    // ... other respondent details
  },
  connection_spiritual_practices: "spiritual_engagement_level",
  past_yatras: "participation_history", 
  type_yatras: "yatra_types_experienced",
  happiest_yatra_moments: "positive_experiences",
  challenges_during_yatras: "pain_points",
  yatra_improvement_suggestion: "feedback_for_improvement"
}
```

#### Yatra Motivation
```javascript
{
  respondent_info: { /* respondent details */ },
  yatra_motivation: "primary_motivation_factors",
  family_yatra_motivation: "family_decision_factors",
  factors_chosing_mty: "selection_criteria",
  // Service importance ratings (1-5 scale)
  imp_spiritual_guide: "5",
  imp_personalised_itenary: "4",
  imp_group_size: "4",
  imp_tech_updates: "3",
  imp_transparent_pricing: "5",
  imp_qualaity_accomadation: "5",
  imp_special_access: "2",
  imp_pre_preperation: "5", 
  imp_post_followup: "3",
  imp_sustainabilty: "4"
}
```

#### Preference Collection
```javascript
{
  respondent_info: { /* respondent details */ },
  reasonable_3day_cost: "budget_preference",
  offers_inc_price: "value_added_services",
  exploring_offbeat_destinations: "interest_level",
  criteria_visiting_offbeat_destination: "selection_criteria",
  source_yatra_opportunities: "information_sources",
  valuable_yatra_content: "content_preferences",
  transformative_yatra_opinion: "spiritual_impact_views"
}
```

### 6. **Automated Analysis Pipeline**

#### Schedule Trigger
- **Frequency**: Weekly (configurable)
- **Time**: 9:00 AM (configurable)
- **Purpose**: Triggers automated analysis generation

#### Data Reading Node
- **Collection**: `raw_data`
- **Limit**: 25 recent responses (configurable)
- **Purpose**: Fetches latest survey data for analysis

#### Raw Data Processing Code
- **Function**: Structures data for AI analysis
- **Output**: Comprehensive analysis object with:
  - Demographics distribution
  - Experience patterns
  - Motivation analysis
  - Service importance scores
  - Market insights
  - Qualitative feedback compilation

#### AI Analysis Agent
- **Model**: OpenAI GPT-4
- **Role**: Senior Business Intelligence Analyst
- **Expertise**: Spiritual tourism, customer experience, market research
- **Output**: 3000-4000 word comprehensive analysis report

#### Analysis Processing Code
- **Function**: Structures AI output for storage
- **Features**:
  - Extracts key findings and insights
  - Calculates business impact scores
  - Creates executive summary
  - Formats for dashboard consumption

#### Analysis Storage
- **Collection**: `survey_analysis_results`
- **Purpose**: Stores structured analysis results
- **Fields**: Analysis metadata, AI report, executive summary

## 📊 Database Schema

### Collection Structure Overview
```
mytempleyatra_survey/
├── raw_data/                    # Complete survey responses (backup)
├── respondent_info/             # Demographic and contact data  
├── previous_yatra_experiences/  # Past travel and spiritual experiences
├── yatra_motivation/            # Decision factors and service ratings
├── preference_collection/       # Market preferences and criteria
└── survey_analysis_results/     # AI-generated business intelligence
```

### Key Relationships
- All collections reference `survey_id` for data linking
- `respondent_info` contains core demographic data
- Specialized collections contain domain-specific insights
- `survey_analysis_results` provides aggregated business intelligence

### Data Types and Validation
- **Strings**: Text responses, selections, feedback
- **Numbers**: Service importance ratings (1-5 scale)
- **Arrays**: Multi-select responses (joined as comma-separated strings)
- **Objects**: Nested respondent information in related collections
- **Dates**: ISO timestamp format for all temporal data

## 🔄 Usage

### Real-Time Survey Processing
1. Survey respondent completes Zoho form
2. Webhook automatically triggers N8N workflow
3. Data is processed and distributed to 5 MongoDB collections
4. Response confirmation sent back to Zoho

### Weekly Analysis Generation
1. Schedule trigger activates every Monday at 9 AM
2. Latest survey data is fetched from `raw_data`
3. Data is structured and sent to AI analysis agent
4. Comprehensive business intelligence report is generated
5. Analysis results stored in `survey_analysis_results` collection

### Manual Workflow Execution
1. Open N8N workflow interface
2. Click "Execute Workflow" button
3. Monitor execution in real-time
4. Check logs for any errors or issues
5. Verify data in MongoDB collections


## 🎯 Data Flow

### Survey Response Processing Flow
```
Survey Submission
      ↓
Webhook Trigger (N8N)
      ↓  
Data Validation & Processing
      ↓
Raw Data Storage (MongoDB)
      ↓
Data Distribution (4 parallel branches)
      ↓
Specialized Collection Storage
      ↓
Process Complete
```

### Analysis Generation Flow
```
Schedule Trigger (Weekly)
      ↓
Fetch Recent Survey Data
      ↓
Structure Data for AI Analysis
      ↓
AI Analysis Generation (OpenAI GPT-4)
      ↓
Process & Structure AI Output
      ↓
Store Analysis Results
      ↓
Analysis Complete
```

## 🤖 AI Analysis

### Analysis Scope
The AI agent performs comprehensive analysis across multiple dimensions:

#### 1. **Executive Summary & Key Findings**
- Overall survey response analysis
- Top 5 critical business insights
- Customer satisfaction overview
- Market positioning assessment
- Urgency indicators for immediate action

#### 2. **Customer Demographics & Segmentation**
- Age group behavioral patterns
- Geographic distribution analysis
- Family structure impact on decisions
- Spiritual engagement correlation with loyalty
- Customer persona development (3-4 segments)

#### 3. **Experience Journey Analysis**
- Past yatra participation patterns
- Spiritual connection business implications
- Customer delight moment identification
- Pain point and challenge analysis
- Experience-based satisfaction scoring

#### 4. **Service Performance & Gap Analysis**
- Service importance ratings breakdown
- Expectation vs performance gaps
- Service improvement priority matrix
- Competitive advantage identification
- Quality benchmarking

#### 5. **Market Intelligence & Opportunities**
- Budget preference and pricing strategy
- Offbeat destination market expansion
- Marketing channel optimization
- Content strategy recommendations
- Referral program potential assessment

### Analysis Output Structure
```javascript
{
  analysis_id: "unique_analysis_id",
  generated_at: "2024-01-15T09:00:00.000Z",
  ai_generated_analysis: {
    full_report: "comprehensive_3000_word_report",
    report_sections: {
      executive_summary: "key_findings_summary",
      customer_intelligence: "segmentation_analysis", 
      service_performance: "gap_analysis",
      strategic_recommendations: "action_plan",
      // ... other sections
    },
    word_count: 3247,
    confidence_level: "High"
  },
  executive_summary: {
    key_findings: [
      "finding_1",
      "finding_2", 
      // ...
    ],
    critical_insights: [
      "insight_1",
      "insight_2",
      // ...
    ],
    immediate_actions: [
      "action_1", 
      "action_2",
      // ...
    ],
    overall_sentiment: "Positive|Negative|Neutral",
    business_impact_score: 78
  }
}
```

## 📈 Monitoring & Analytics

### Key Performance Indicators (KPIs)
- **Response Processing Rate**: Surveys processed per hour
- **Data Quality Score**: Completeness and accuracy metrics
- **Analysis Generation Time**: Time to generate AI insights
- **Error Rate**: Failed processing percentage
- **Customer Satisfaction Index**: Derived from service ratings

### Monitoring Tools
- **N8N Execution Logs**: Real-time workflow monitoring
- **MongoDB Metrics**: Database performance and storage
- **Custom Dashboards**: Business intelligence visualization
- **Alert Systems**: Error and anomaly notifications

### Analytics Dashboards
```javascript
// Sample dashboard metrics
{
  total_responses: 150,
  avg_processing_time: "2.3 seconds",
  data_quality_score: 94,
  customer_satisfaction_index: 4.2,
  recommendation_likelihood: "78% likely to recommend",
  top_pain_points: [
    "Long waiting times",
    "Overcrowding at temples", 
    "Poor accommodation quality"
  ],
  service_priorities: [
    "Spiritual guides (avg: 4.8/5)",
    "Transparent pricing (avg: 4.7/5)",
    "Quality accommodation (avg: 4.5/5)"
  ]
}
```

## 🔧 Troubleshooting

### Common Issues

#### 1. **Webhook Connection Issues**
```
Problem: Survey responses not triggering workflow
Solution: 
- Verify webhook URL accessibility
- Check N8N instance connectivity  
- Validate Zoho webhook configuration
- Review firewall and security settings
```

#### 2. **MongoDB Connection Errors**
```
Problem: Database connection failures
Solution:
- Verify MongoDB credentials in N8N
- Check database server accessibility
- Validate connection string format
- Review network connectivity
```

#### 3. **OpenAI API Errors**
```
Problem: AI analysis generation failures
Solution:
- Verify OpenAI API key validity
- Check API quota and usage limits
- Review model availability (GPT-4)
- Validate request payload format
```

#### 4. **Data Processing Errors**
```
Problem: Survey data processing failures
Solution:
- Check incoming data format
- Validate field mappings
- Review JavaScript code syntax
- Test with sample data
```

### Debug Mode
Enable debug logging in N8N:
```javascript
// Add to code nodes for debugging
console.log("Processing item:", JSON.stringify(item, null, 2));
console.log("Current state:", { 
  itemCount: items.length,
  currentIndex: index,
  processedData: outputData 
});
```

### Error Handling
The workflow includes comprehensive error handling:
- Try-catch blocks in all processing nodes
- Graceful degradation for missing data
- Error logging with context information
- Automatic retry mechanisms for transient failures


