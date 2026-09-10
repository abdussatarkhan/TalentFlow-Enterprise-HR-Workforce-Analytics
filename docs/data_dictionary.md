# 📚 Data Warehouse Data Dictionary

## 🏢 Platform: TalentFlow: Enterprise People Analytics & Workforce Attrition Intelligence
**Schema**: `talentflow_dw`  
**Architecture**: 3-Tier Enterprise Star Schema (PostgreSQL 16)  
**SCD Strategy**: Slowly Changing Dimension Type 2 (SCD-2) on primary business entities

---

## 🗄️ Dimension Tables

### 1. `dim_date`
Calendar date dimension providing temporal hierarchical drill-down.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `date_key` | INT | NO | PK | Integer key in YYYYMMDD format (e.g. 20241205) |
| `full_date` | DATE | NO | UNIQUE | Standard ISO-8601 calendar date |
| `day_name` | VARCHAR(12) | NO | - | Day of week string (Monday..Sunday) |
| `is_weekend` | BOOLEAN | NO | - | True for Saturday and Sunday |
| `month` | INT | NO | - | Calendar month integer (1..12) |
| `quarter` | INT | NO | - | Calendar quarter (1..4) |
| `year` | INT | NO | - | 4-digit calendar year |

### 2. `dim_employees`
Dimensional attribute master for Employees.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `employees_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `employees_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

### 2. `dim_job_roles`
Dimensional attribute master for Job Roles.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `job_roles_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `job_roles_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

### 2. `dim_departments_org`
Dimensional attribute master for Departments Org.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `departments_org_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `departments_org_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

### 2. `dim_locations`
Dimensional attribute master for Locations.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `locations_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `locations_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

---

## 📊 Fact Tables

### `fact_headcount_snapshots`
Transactional and snapshot grain telemetry table tracking Headcount Snapshots.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

### `fact_attrition_events`
Transactional and snapshot grain telemetry table tracking Attrition Events.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

### `fact_compensation_equity`
Transactional and snapshot grain telemetry table tracking Compensation Equity.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

### `fact_recruiting_pipeline`
Transactional and snapshot grain telemetry table tracking Recruiting Pipeline.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

---

## 📈 Key Metric Calculation Formulas

1. **SLA Compliance Rate (%)**:
   $$\text{SLA Compliance} = \left( \frac{\sum \text{is\_sla\_compliant = TRUE}}{\text{Total Events}} \right) \times 100$$

2. **Rolling 7-Day Moving Average**:
   $$\text{SMA}_{7} = \frac{1}{7} \sum_{i=0}^{6} \text{metric\_value}_{t-i}$$

3. **Z-Score Anomaly Threshold**:
   $$Z = \frac{X - \mu}{\sigma}$$
   *(Alert flagged when $|Z| \ge 2.58$, representing $p < 0.01$ outlier probability)*
