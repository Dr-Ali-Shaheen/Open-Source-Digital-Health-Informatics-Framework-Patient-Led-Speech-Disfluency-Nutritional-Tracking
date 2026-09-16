<img width="1920" height="1080" alt="Beige and Brown Minimalist Creative Portfolio Presentation" src="https://github.com/user-attachments/assets/51548db9-3355-4df3-a7fd-e278ea9cca01" /># Open-Source-Digital-Health-Informatics-Framework-Patient-Led-Speech-Disfluency-Nutritional-Tracking
An open-source Power BI dashboard for individuals who stutter (developmental speech disfluency). This repository provides a dual-table tracking system and an interactive dashboard to analyze how morning nutritional regimens, baseline anxiety, and sleep duration correlate with speech disfluency severity and physical blockage durations.

<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/96223a5f-4405-4d31-b606-c188321dd36c" />


## 📊 Google Sheets Data Schema Definition

The data model uses a Star Schema design split across two primary tables to prevent data redundancy and support temporal aggregation inside Power BI.

### 1. `Daily_Tracking_Log` (Fact Table)

_Logged nightly before sleeping (except `Sleep_Hours` and `Regimen_Adherence`)._

|**Column Name**|**Data Type**|**Permissible Values**|**Operational Definition & Protocol**|
|---|---|---|---|
|**`Date`**|Date|`YYYY-MM-DD`|Primary Key linking daily entries to the model Date Table.|
|**`Severity_Score`**|Integer|`1` – `10`|Subjective rating of daily overall speech disfluency. (`1` = Minimal blocks; `10` = Severe/frequent blocks). _Logged at night._|
|**`Anxiety_Baseline`**|Integer|`1` – `10`|General emotional tension/stress level felt across the day. (`1` = Calm; `10` = High anxiety). _Logged at night._|
|**`Sleep_Hours`**|Decimal|`0.0` – `24.0`|Hours slept during the previous night. _Logged in the morning/evening._|
|**`Regimen_Adherence`**|Binary|`1` or `0`|Compliance flag for morning supplement protocol (Sulbutiamine, Vitamin B1, Creatine taken at breakfast). (`1` = Full regimen met; `0` = Regimen missed).|
|**`Speech_Context`**|Text|Categorical|Primary or most challenging speaking situation encountered during the day (e.g., _Work Presentation_, _Phone Call_, _Casual Conversation_, _Low Social Interaction_).|
|**`Notes`**|Text|Free text|Qualitative observational context (e.g., _Felt well-rested_, _Rush morning_).|

<img width="1920" height="1080" alt="Beige and Brown Minimalist Creative Portfolio Presentation" src="https://github.com/user-attachments/assets/83ba0d73-2102-4131-a589-4e317e35ca85" />


### 2. `Disfluency_Events_Log` (Dimension Table)

_Logged in real-time or immediately following a notable disfluency episode._

|**Column Name**|**Data Type**|**Permissible Values**|**Operational Definition & Protocol**|
|---|---|---|---|
|**`Date`**|Date|`YYYY-MM-DD`|Foreign Key linking event instances to `Daily_Tracking_Log[Date]`.|
|**`Event_Type`**|Text|`Block`, `Prolongation`, `Repetition`|Classification of disfluency:<br><br>  <br><br>• **Block**: Inability to initiate sound.<br><br>  <br><br>• **Prolongation**: Unnatural lengthening of sounds.<br><br>  <br><br>• **Repetition**: Repeated sounds/syllables.|
|**`Situational_Anxiety`**|Integer|`1` – `10`|Acute anxiety score experienced during the specific speech incident.|
|**`Trigger_Context`**|Text|Categorical|Specific environment or speaking scenario where the event occurred (e.g., _Client Call_, _Ordering Food_).|
|**`Block_Duration_Sec`**|Integer|`1`+|Approximate physical duration of sound blockage in seconds.|

## 📈 Dashboard Architecture & Visual Specifications

The Power BI dashboard is organized across dedicated functional tabs and panels to systematically isolate variables affecting speech control.

### Executive KPI Summary Banner (Top Canvas)

- **Average Severity & Baseline Anxiety (Cards):** Displays baseline metrics to evaluate overall monthly speech trends.
    
- **Adherence Rate (%) Card:** Tracks protocol compliance percentage over selected date ranges.
	
- Average Sleep Hours: Displays the average sleeping hour metrics
    
- **Supplement Efficacy Delta (Formatted Callout Card):**
    
    - **DAX Measure:** `[Avg Severity (Non-Adherent)] - [Avg Severity (Adherent)]`
        
    - **Function:** Quantifies the average reduction in disfluency severity achieved when taking morning supplements.
        
    - **Conditional Formatting:** Green for positive delta values (indicating lower disfluency on supplement days); Red/Gray for negative values.
        

### Rolling Trends & Moving Averages

- **Visual Type:** Line and Clustered Column Chart.
    
- **X-Axis:** `DateTable[Date]`
    
- **Y-Axis (Lines):** `7-Day Rolling Avg Severity` & `7-Day Rolling Avg Anxiety`
    
- **Y-Axis (Columns):** `Sleep_Hours`
    
- **Function:** Smooths out daily volatility to reveal lag indicators—such as how cumulative sleep debt across 48–72 hours correlates with multi-day speech blockage spikes.
    
### Micro-Disfluency Breakdown

- **Visual Type 1:** Donut Chart (`Event_Type` in **Legend**, `Count of Event_Type` in **Values**).
    
    - **Function:** Illustrates the proportional distribution of physical speech disfluency types (_Blocks_ vs. _Prolongation_ vs. _Repetition_).
        
- **Visual Type 2:** Horizontal Bar Chart (`Trigger_Context` on **Y-Axis**, `Avg Block Duration Sec` & `Block Count` on **X-Axis**).
    
    - **Function:** Identifies high-risk communication environments (e.g., _Work Presentations_, _Phone Calls_) that trigger the longest physical blockages.
        

### Contextual Correlation Matrix

- **Visual Type:** Scatter Plot.
    
- **X-Axis:** `Anxiety_Baseline`
    
- **Y-Axis:** `Severity_Score`
    
- **Bubble Size:** `Avg Block Duration Sec`
    
- **Legend:** `Speech_Context`
    
- **Function:** Disaggregates acute speech blockages from generalized anxiety to identify whether high disfluency occurs independently of mood states.
