# Open-Source-Digital-Health-Informatics-Framework-Patient-Led-Speech-Disfluency-Nutritional-Tracking
An open-source Power BI dashboard for individuals who stutter (developmental speech disfluency). This repository provides a dual-table tracking system and an interactive dashboard to analyze how morning nutritional regimens, baseline anxiety, and sleep duration correlate with speech disfluency severity and physical blockage durations.

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

### 2. `Disfluency_Events_Log` (Dimension Table)

_Logged in real-time or immediately following a notable disfluency episode._

|**Column Name**|**Data Type**|**Permissible Values**|**Operational Definition & Protocol**|
|---|---|---|---|
|**`Date`**|Date|`YYYY-MM-DD`|Foreign Key linking event instances to `Daily_Tracking_Log[Date]`.|
|**`Event_Type`**|Text|`Block`, `Prolongation`, `Repetition`|Classification of disfluency:<br><br>  <br><br>• **Block**: Inability to initiate sound.<br><br>  <br><br>• **Prolongation**: Unnatural lengthening of sounds.<br><br>  <br><br>• **Repetition**: Repeated sounds/syllables.|
|**`Situational_Anxiety`**|Integer|`1` – `10`|Acute anxiety score experienced during the specific speech incident.|
|**`Trigger_Context`**|Text|Categorical|Specific environment or speaking scenario where the event occurred (e.g., _Client Call_, _Ordering Food_).|
|**`Block_Duration_Sec`**|Integer|`1`+|Approximate physical duration of sound blockage in seconds.|
