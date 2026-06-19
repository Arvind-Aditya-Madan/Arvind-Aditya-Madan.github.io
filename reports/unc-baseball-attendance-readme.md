# DATA 992 SPRING 2026 - UNC Athletics: Predicting Attendance at Carolina Game Days
**A machine learning framework for forecasting game-day attendance at UNC baseball home games to support smarter staffing, marketing, and fan experience planning.**

---

# Project Overview
## Problem Statement
UNC Athletics hosts home events across 28 teams and supports over 800 student-athletes. Game-day attendance is crucial to operational planning, staffing, marketing effectiveness, and overall fan experience; however, attendance varies widely by sport, venue, opponent, scheduling, and other contextual factors, making reliable advance planning difficult. Current attendance planning often relies on historical averages and scheduling heuristics, which fail to fully capture how multiple drivers interact at the game level, resulting in inefficient resource allocation and suboptimal staffing decisions.

This project develops a data-driven framework that integrates historical and contextual data to forecast game-day attendance and identify key drivers of variation. The project combines interpretable predictive models with interactive dashboards that translate results into actionable guidance for marketing, operations, and event management. Stakeholders, including UNC Athletics leadership, marketing and operations teams, student-athletes, and fans, benefit from improved attendance forecasting, more efficient resource allocation, and enhanced Carolina game-day experiences. Ultimately, this work helps transition attendance planning from reactive assessment to proactive, data-informed decision-making.

**Developed for:** UNC Athletics <br>
**Stakeholder Contact:** Andrea Johnston <br>
**Course:** DATA 992 - MADS Capstone <br>
**Institution:** University of North Carolina School of Data Science and Society <br>
**Team:** Adam Cooke, Arvind Madan, Daniel Fulk <br>

---

## Project Scope & Objectives

The primary goal of this project is to develop a data-driven framework for forecasting attendance at UNC baseball home games and translating those forecasts into actionable operational insights. The project focuses on event-level prediction and decision support to improve planning efficiency, resource allocation, and fan experience.

Specifically, the project aims to:

1. **Identify key drivers of attendance for UNC baseball home games:** Conduct exploratory data analysis and feature engineering to quantify how scheduling, opponent characteristics, weather conditions, academic calendar timing, and other contextual factors influence attendance variability.
2. **Build predictive models to forecast game-day attendance at the event level:** Develop, compare, and validate multiple predictive modeling approaches ranging from baseline statistical models to more flexible machine learning methods. Model performance is evaluated using out-of-sample validation to ensure reliability for real-world forecasting.
3. **Create interpretable dashboards that surface projections and key attendance drivers:** Design interactive visual tools that communicate attendance forecasts, model uncertainty, and influential factors in a clear and accessible way for non-technical stakeholders. Dashboards support scenario exploration and operational planning.
4. **Deliver actionable insights to support staffing, promotions, and operations planning:** Translate modeling results into practical recommendations that help UNC Athletics proactively allocate resources, target marketing efforts, and optimize the overall game-day experience.

The scope of the project is intentionally focused on UNC baseball home games in order to develop and validate a robust forecasting framework. However, the methodology is designed to be scalable and transferable to other sports, venues, or institutions in future phases.

---

## Project Outputs
The project produced a set of technical outputs and decision-support tools that allow stakeholders to understand, interpret, and apply attendance forecasts.

- **Attendance forecasting models:** A set of validated predictive models capable of generating event-level attendance projections using historical and contextual data across multiple forecasting horizons.
- **Interactive Streamlit application:** A user-facing tool that allows stakeholders to explore attendance forecasts, and interact with model outputs in an accessible format.
[Visit App](https://unc-baseball-attendance-forecasting.streamlit.app/)

- **Executive presentation and app tutorial:** A concise presentation of the project’s methodology, findings, model performance, and operational recommendations, along with guidance for using the Streamlit application.
- **Reproducible dataset and documented codebase:** A clean, structured modeling dataset along with documented code to support transparency, reproducibility, and future extension of the forecasting framework.
- **Generalization analysis:** An evaluation of how the modeling framework transfers to other sports and settings, including additional applications beyond UNC baseball.

---

## Team Roles / Responsibilities
The team worked collaboratively throughout the project, while each member maintained primary areas of responsibility that supported efficient execution and clear ownership of major components.

- **Adam Cooke:** *Data Acquisition, Project Infrastructure, and Stakeholder Coordination* - Led development of the GoHeels.com scraper that served as the backbone of the game-level dataset, helped establish the modeling pipeline, and created the five feature tiers based on forecast-time data availability. He also gathered supporting datasets, contributed to GitHub and project organization, and supported team coordination and stakeholder communication throughout the semester.

- **Arvind Madan:** *Data Engineering and Modeling Pipeline Refinement* - Led data cleaning, feature engineering, and integration of multiple project files into a unified master dataset for modeling. He also expanded the modeling pipeline by adding backward feature elimination, replacing the earlier test-set approach with weighted cross-validation, adapting the workflow for preseason forecasting, and engineering additional weather- and attendance-related features. 

- **Daniel Fulk:** *Performance Data Scraping, Streamlit Development, and Documentation* - Built the performance-data scraper for UNC and opponent game-level performance metrics and developed the Streamlit application used to present forecasts and results interactively. He also contributed to project documentation and continuously refined both the scraper and app as project needs evolved.

All team members contributed to the final presentation and to broader project development in complementary ways.

---


## Methodology

**Data Collection:** We will assemble a historical, event-level dataset for UNC baseball home games by integrating attendance records, opponent information, scheduling details, academic calendar context, weather conditions, and relevant team performance metrics. Data will be sourced from official UNC Athletics records and supplemental public sources where appropriate. All data will be standardized to ensure consistency across seasons and structured at the individual game level to support downstream modeling.

**Exploration & Feature Engineering:** Following data assembly, we will conduct exploratory data analysis to identify patterns in attendance across time, opponent quality, day-of-week effects, weather conditions, and seasonal context. Based on these findings, we will construct meaningful game-day context variables, including temporal indicators, performance-based metrics, and operationally relevant features. Feature engineering will prioritize interpretability and alignment with real-world decision timelines.

**Predictive Modeling:** We will begin with baseline modeling approaches to establish a transparent benchmark for attendance prediction. From there, we will evaluate more flexible modeling techniques to determine whether they provide meaningful improvements in predictive performance. Model evaluation will rely on out-of-sample validation strategies that reflect realistic forecasting scenarios and avoid information leakage.

**Insights & Delivery:** Final outputs will translate modeling results into clear visualizations and interactive dashboards designed for operational use. Deliverables will highlight key attendance drivers, forecast scenarios, and actionable insights to support staffing, marketing, and game-day resource allocation decisions. Throughout the project, emphasis will be placed on clarity, reproducibility, and stakeholder usability.


---
## Data Collection and Feature Engineering

The modeling dataset was constructed by combining multiple source files into a unified game-level dataframe for UNC baseball. These sources include attendance and schedule data, academic calendar variables, enrollment data, Google Trends metrics, team location data, conference mapping files, and season-level performance data. Additional datasets were also assembled for transfer testing on NC State baseball and UNC basketball.

### Data Sources

- **UNC baseball base dataset (`unc_baseball_final`)**  
  Core game-level dataset for UNC baseball, including attendance outcomes and foundational schedule/context variables.  
  **Source / collection method:** Custom web scraper built to pull game-level attendance, schedule, and result data from GoHeels.com (UNC Athletics official site). Output stored as CSV for downstream use.

- **UNC baseball season performance (`uncbaseball_season_performance_0331`)**  
  Season- and team-performance data used to engineer historical and form-related predictors.  
  **Source / collection method:** Custom performance scraper that takes the UNC baseball attendance scrape output and retrieves per-game performance statistics (runs, hits, home runs, strikeouts, etc.) for UNC and opponents from stats.ncaa.org game logs.

- **Academic calendar data (`academic_calendar_data`)**  
  Calendar-based variables capturing school session timing, breaks, exams, and other academic context relevant to attendance.  
  **Source / collection method:** Manually compiled from UNC Registrar's Office archives (registrar.unc.edu). Includes semester start/end dates, break periods, exam schedules, and session indicators for each academic year.

- **Enrollment data (`enrollment_data`)**  
  Enrollment-based variables used to represent changes in the campus population over time.  
  **Source / collection method:** Manually compiled from UNC Registrar's Office enrollment archives and institutional research reports (oira.unc.edu).

- **Google Trends web data (`gtrends_data_web_uncbaseball`)**  
  Search-interest data used as a proxy for public attention and fan interest in UNC baseball.  
  **Source / collection method:** Downloaded from Google Trends (trends.google.com) using UNC baseball-related search terms. Monthly web search interest values compiled into a structured CSV.

- **Google Trends YouTube data (`gtrends_data_yt_uncbaseball`)**  
  Video-search interest data used as an additional signal of fan engagement and attention.  
  **Source / collection method:** Downloaded from Google Trends (trends.google.com) using the YouTube search filter for UNC baseball-related terms. Monthly YouTube search interest values compiled into a structured CSV.

- **Baseball team home coordinates (`team_home_coords_baseball`)**  
  Team location data used to support distance- or travel-related feature construction.  
  **Source / collection method:** Manually compiled reference table of stadium coordinates for all opponents. Latitude and longitude values sourced from Google Maps and stadium reference pages.

- **Basketball team home coordinates (`team_home_coords_basketball`)**  
  Team location data used for cross-sport transfer and generalization analysis.  
  **Source / collection method:** Manually compiled reference table of arena coordinates for all basketball opponents. Latitude and longitude values sourced from Google Maps and arena reference pages.

- **ACC mapping for baseball (`acc_mapping_baseball`)**  
  Conference membership reference data used to engineer ACC and opponent-group features.  
  **Source / collection method:** Manually compiled reference table mapping opponent names to conference affiliation and opponent groupings, based on historical ACC membership records.

- **ACC team list for basketball (`acc_team_list_basketball`)**  
  Conference reference file used in basketball transfer testing and feature alignment.  
  **Source / collection method:** Manually compiled reference table of ACC basketball membership, used for conference flag and opponent-group feature construction in the basketball transfer context.

- **NC State baseball dataset (`ncstate_baseball_final`)**  
  Game-level dataset assembled for external transfer testing of the modeling pipeline.  
  **Source / collection method:** Custom web scraper built to pull game-level attendance, schedule, and result data from GoPack.com (NC State Athletics official site). Follows the same structure as the UNC baseball scraper.

- **UNC basketball dataset (`unc_basketball_final`)**  
  Game-level dataset assembled for cross-sport generalization analysis.  
  **Source / collection method:** Custom web scraper built to pull game-level attendance, schedule, and result data from GoHeels.com for UNC men's basketball. Follows the same structure as the UNC baseball scraper.
### Data Integration

After collection, all source files were cleaned, standardized, and merged at the game level. This process included aligning team names, dates, season identifiers, and opponent metadata across files, as well as resolving differences in formatting and temporal coverage. The resulting master dataset served as the foundation for feature engineering, tier construction, and downstream modeling.

### Feature Engineering

Feature engineering focused on building variables that reflect the information available at different forecasting horizons while avoiding data leakage. Broadly, engineered features fell into the following groups:

- **Schedule and game context:** day of week, month, game number, series information, and timing-related indicators  
- **Opponent characteristics:** conference affiliation, rivalry indicators, and opponent groupings  
- **Attendance history:** prior attendance levels, lagged attendance measures, and year-over-year trends  
- **Academic context:** school session timing, breaks, exams, and enrollment-related variables  
- **Fan interest signals:** Google Trends web and YouTube measures  
- **Performance features:** previous-season and in-season team performance variables  
- **Geographic/contextual features:** travel- or location-related variables derived from team coordinate files  

Additional details on rolling features, lag construction, forecast-safe preprocessing, and tier-specific variable availability are described in the sections below.

---

## Dataset Tiers

Because each data source begins in a different season, we construct five tiered datasets from a single master dataframe rather than forcing all models to use only the shortest shared time window. Each tier adds richer contextual information while reducing the number of seasons available for training. This design allows us to directly evaluate the tradeoff between **more history** and **more features**, which is a central question of the project.

![](README_ASSETS/tiers_visualization.svg)

The red dashed line marks **2018**, the first season in which all seven source groups are simultaneously available. Tier 5 is therefore the only tier with full feature coverage, while the earlier tiers use fewer source groups but preserve longer historical windows.

### Tier Definitions

- **Tier 1 (2000+): Schedule + attendance history**  
  The baseline preseason tier includes core schedule and game-context variables, opponent information, distance and rivalry indicators, school-night context, previous-season record, weather contexts, and historical attendance measures.

- **Tier 2 (2010+): Tier 1 + Google Trends**  
  Adds search-interest signals derived from Google Trends, including web, YouTube, and combined search volume measures. This tier captures public attention and fan-interest context.

- **Tier 3 (2014+): Tier 2 + team and opponent performance**  
  Adds season performance measures for both UNC and its opponent, including scoring, run prevention, home runs, strikeouts, win percentage, team strength, and differential-based matchup indicators. This tier introduces a fuller competitive-context view.

- **Tier 4 (2016+): Tier 3 + academic calendar detail and enrollment data**  
  Adds more detailed academic-context features, including semester activity, break periods, exam timing, and break type as well as enrollment headcounts. This tier is intended to capture how student availability and campus timing shape turnout.

- **Tier 5 (2019+): Tier 4 + promotions**  
  Adds promotional variables, including promotion count and promotion-type indicators such as giveaways, fireworks, kids events, bark events, discounts, theme nights, trivia, and other promotions. This is the richest tier, but also the smallest historically.

### Tiering Logic

The tier structure is used in both forecasting settings so that model performance can be compared under consistent historical availability constraints. In practice, this means lower tiers benefit from more seasons of training data, while higher tiers benefit from richer contextual signals. The comparison across tiers helps answer whether additional feature groups meaningfully improve forecasts enough to offset the loss of historical sample size.

---
## Forecast Horizons

The project evaluates attendance forecasting under two operational prediction horizons: a **preseason forecast** and a **short-term update forecast made up to two days before game time**. These two horizons are designed to reflect different planning needs within UNC Athletics.

### Preseason Forecast

The preseason forecast is designed to predict the full home schedule before opening day using only information known prior to the start of the season. This setting supports longer-range planning decisions such as seasonal staffing expectations, budget allocation, concession ordering, and identification of games that may need additional promotional support.

Preseason models use feature groups such as:

- opponent and schedule context
- historical attendance patterns
- prior-season team performance
- academic calendar context
- preseason Google Trends signals
- promotions

Because these forecasts are generated before the season begins, they do not use in-season performance, realized attendance, or near-term weather information.

### Two-Day-Ahead Update Forecast

The short-term update forecast is designed to refine attendance expectations shortly before each game. While the original day-before framework was intended for next-day prediction, the deployed workflow was extended to support predictions **up to two days before game time** so the Streamlit app could surface forecasts for the next two days of scheduled games.

This forecast horizon supports more tactical operational decisions such as final staffing adjustments, concessions planning, and short-term game-day preparation. In addition to the preseason feature set, the two-day-ahead model incorporates more time-sensitive variables, including:

- recent attendance trends
- current-season team and opponent performance
- momentum and excitement measures
- near-term weather forecast variables
- in-season Google Trends signals

Because these features depend on information that updates throughout the season, this forecast horizon is only available for games close enough in time to have current contextual data.

### Horizon Comparison

Together, these two forecast horizons allow the project to support both **strategic planning** and **short-term operations**. The preseason forecast prioritizes full-season coverage and early planning value, while the two-day-ahead update prioritizes accuracy by incorporating real-time contextual information closer to game day.


---
## Exploratory Data Analysis (EDA)

Exploratory data analysis was conducted before model development to understand overall attendance behavior, identify relationships between major contextual factors, and guide feature engineering decisions. These visualizations were used to summarize broad attendance patterns across seasons and to assess how scheduling, opponent quality, weather, promotions, and academic timing may influence turnout before fitting predictive models.

### Overall Attendance Patterns

The following plots summarize long-run attendance behavior across UNC baseball home games, including season-level trends, distributional patterns, within-season dynamics, and the evaluation-period split used later in modeling.

![](README_ASSETS/Average%20Home%20Attendance.png)
![](README_ASSETS/Attendance%20Distribution.png)
![](README_ASSETS/Within-Season%20Patterns.png)
![](README_ASSETS/Evaluation%20Period.png)

These plots show that attendance has increased substantially over time, especially in more recent seasons, while still exhibiting meaningful game-to-game variability. Attendance generally rises later in the season, with stronger averages in postseason settings and later calendar months. The evaluation-period visualizations also show that the held-out seasons come from a higher-attendance regime than much of the earlier historical sample, making forecasting more challenging.

### Scheduling Factors

![](README_ASSETS/Scheduling%20Factors.png)

Scheduling variables show clear relationships with attendance. Weekend games outperform weekday games across time buckets, and night games tend to draw the strongest crowds. Attendance also increases substantially in postseason settings such as regionals and super regionals. Academic timing appears relevant as well, with school nights, breaks, and exams showing noticeable differences in turnout.

### Opponent Factors

![](README_ASSETS/Opponent%20Factors.png)

Opponent-related patterns suggest that matchup quality plays an important role in fan turnout. ACC games and rivalry games draw stronger attendance than non-conference or non-rival matchups, and several opponents consistently appear among the strongest draws. The win-percentage and strength-difference views suggest that more competitive or higher-interest matchups tend to attract more fans than games with weaker opponents or more lopsided expectations.

### Weather Factors

![](README_ASSETS/Weather%20Factors.png)

Weather appears to be an important driver of attendance, particularly for shorter-horizon forecasts. Attendance increases steadily as temperature moves from cold to warm conditions, and a similar pattern appears for the comfort index. The interaction plots also suggest that favorable weather becomes especially valuable later in the season, when warm and comfortable conditions align with the highest observed turnout.

### Promotion Factors

![](README_ASSETS/Promotion%20Factors.png)

Promotions are associated with higher attendance overall, with promoted games drawing substantially larger average crowds than non-promoted games. This pattern appears on both weekdays and weekends, although promoted weekend games still produce the highest turnout overall. Promotion-type comparisons suggest that some event types, such as fireworks, kids events, and giveaways, are associated with particularly strong attendance, though some categories have smaller sample sizes and should be interpreted cautiously.

### Summary of EDA Takeaways


Overall, the EDA suggested that attendance is shaped by a combination of historical attendance trends, scheduling context, opponent quality, weather conditions, promotions, and academic calendar timing. These findings supported the inclusion of feature groups related to attendance history, scheduling, opponent context, weather, academics, search interest, performance, and promotions in the downstream modeling pipeline.

---
## Modeling Pipeline

The modeling workflow was designed to evaluate attendance forecasts under both prediction contexts using a consistent, chronological, and leakage-aware process. Each tier was modeled independently so that performance could be compared across different levels of feature availability and historical depth.

![](README_ASSETS/pipeline_flow_viz_balanced.svg)

### Data Split and Validation Strategy

All modeling was performed using chronological splits rather than random sampling. Seasons **2020** and **2021** were excluded because attendance in those years was artificially affected by COVID-era restrictions, and **2008** was excluded because Boshamer Stadium was under reconstruction.

For each tier, the data was split into:

- **Training window:** all eligible seasons prior to the evaluation years
- **Evaluation years:** **2024** and **2025**
- **Forward prediction year:** **2026**, used only for rolling prediction after model selection

Rather than using standard cross-validation, the project used a **weighted rolling cross-validation** framework. For each evaluation year, the model was trained only on prior seasons and then validated on the next chronological season. This better reflects the real forecasting setting, where future games must be predicted using only past information.

To place greater emphasis on recent performance, evaluation errors were weighted by year, with **2025 receiving more weight than 2024**. Within each training fold, older seasons were also down-weighted using a **seasonal decay parameter**, allowing the pipeline to favor more recent historical data without fully discarding earlier seasons.

### Model Families

Five candidate model families were evaluated in both the preseason and short-term forecast settings:

- **Linear Regression**
- **Elastic Net**
- **Random Forest**
- **XGBoost**
- **LightGBM**

This set was chosen to compare simple linear baselines, regularized linear models, and more flexible tree-based ensemble methods. For linear models, the pipeline also tested multiple scaling strategies:

- **No scaling**
- **StandardScaler**
- **RobustScaler**
- **MinMaxScaler**

Tree-based models were fit without scaling.

### Optimization

For each tier and model combination, hyperparameters were tuned using **Optuna** with weighted rolling cross-validation as the objective. The optimization process selected:

- model-specific hyperparameters
- training start year
- seasonal decay strength
- scaler choice, when relevant

This allowed the pipeline to jointly determine not only the best model configuration, but also how much historical data to include and how strongly to prioritize recent seasons.

### Backward Feature Elimination

After the initial optimization stage, the best configuration for each tier was passed through a **backward feature elimination (BFE)** procedure. Starting from the full feature set for that tier, features were removed one at a time based on whether their removal improved weighted cross-validation RMSE.

This step served two purposes:

- reduce unnecessary complexity
- improve generalization by removing weak or redundant predictors

Because the dataset included many correlated schedule, attendance-history, weather, and performance features, this step was especially important for simplifying the final models.

### Re-Optimization on Reduced Feature Sets

Once each tier’s reduced feature set was identified, the winning model family was **re-optimized** on the smaller predictor set. This final tuning step ensured that the selected hyperparameters, decay settings, and preprocessing choices were matched to the reduced feature space rather than inherited from the original full model.

The resulting reduced models were used for final tier comparisons, rolling predictions, and interpretability analysis.

### Evaluation Metrics

Model performance was compared using several complementary metrics:

- **Weighted CV RMSE** as the primary selection metric
- **RMSE**
- **MAE**
- **R²**
- **MAPE**
- **Percent of predictions within 500 attendees**

A simple attendance-history baseline was also computed for each tier so that model improvements could be interpreted relative to a practical benchmark rather than in isolation.

### Final Model Use

After model selection, the best reduced pipeline for each forecasting context was retrained in a rolling fashion and used to generate season-level predictions for **2024**, **2025**, and true forward predictions for **2026**. The same modeling recipe was then transferred to **NC State baseball** and **UNC basketball** to evaluate how well the framework generalized beyond the original UNC baseball setting.

---

## Results

Results are presented separately for the **preseason** and **two-day-ahead update** forecasting contexts, followed by a direct comparison of the best model from each setting.

### Preseason Results

Across all preseason tiers, the best reduced model was **Elastic Net**. Performance varied meaningfully across tiers, showing that adding more contextual variables did not always improve accuracy once the loss of historical training data was taken into account.

| Tier | wCV RMSE | RMSE | MAE | R² | MAPE | Within 500 | vs. Baseline |
|------|---------:|-----:|----:|---:|-----:|-----------:|-------------:|
| Tier 1 | 425 | 418 | 329 | 0.5879 | 11.5% | 82% | +35.9% |
| Tier 2 | 425 | 417 | 338 | 0.5902 | 11.9% | 82% | +35.8% |
| Tier 3 | 489 | 509 | 398 | 0.3890 | 15.2% | 68% | +26.1% |
| **Tier 4** | **406** | **406** | **330** | **0.6128** | **11.6%** | **82%** | **+38.8%** |
| Tier 5 | 409 | 420 | 340 | 0.5855 | 12.5% | 75% | +38.2% |

The strongest preseason model came from **Tier 4**, which balanced a relatively rich feature set with enough historical data to remain stable. Tier 4 outperformed both the leaner early tiers and the promotion-heavy Tier 5, suggesting that **academic calendar detail and broader preseason context added value**, while the shorter historical window in Tier 5 limited the gains from additional promotion variables. In contrast, Tier 3 performed notably worse than the surrounding tiers, indicating that the added previous-season performance variables alone did not justify the reduction in training history under the preseason setup.

Rolling season evaluation for the winning preseason model showed consistent performance in 2024 and 2025, with some degradation in forward prediction on the partially observed 2026 season. The Tier 4 preseason model achieved an RMSE of **402** in 2024 and **409** in 2025, with corresponding R² values of **0.5134** and **0.5312**. On the first 20 scored games of 2026, it produced an RMSE of **447**, MAE of **328**, and R² of **0.0696**. 

Overall, the preseason model improved on the attendance-history baseline by **38.8%**, with **82%** of evaluation predictions falling within 500 attendees. This level of accuracy suggests that preseason forecasts are useful for longer-range planning tasks such as staffing expectations, budget setting, and identifying likely high- and low-demand games before opening day.

### Two-Day-Ahead Update Results

The two-day-ahead update models also favored **Elastic Net** across all tiers, but performance improved substantially once near-term contextual information was incorporated. In this forecasting setting, the best model came from the richest tier.

| Tier | wCV RMSE | RMSE | MAE | R² | MAPE | Within 500 | vs. Baseline |
|------|---------:|-----:|----:|---:|-----:|-----------:|-------------:|
| Tier 1 | 373 | 369 | 287 | 0.7496 | 9.9% | 86% | +40.5% |
| Tier 2 | 370 | 367 | 297 | 0.7520 | 10.2% | 86% | +40.9% |
| Tier 3 | 361 | 357 | 284 | 0.7659 | 9.8% | 88% | +42.4% |
| Tier 4 | 361 | 353 | 275 | 0.7706 | 9.2% | 82% | +42.3% |
| **Tier 5** | **327** | **326** | **260** | **0.8048** | **9.1%** | **88%** | **+48.0%** |

The best update model came from **Tier 5**, indicating that in the shorter-horizon setting, the additional information from promotions and full contextual coverage more than offset the smaller historical training window. This pattern contrasts with the preseason setting and suggests that **richer real-time context becomes more valuable closer to game day**, especially when combined with recent attendance behavior, weather, and current-season conditions.

Rolling evaluation for the winning two-day-ahead model showed strong performance in both held-out seasons and remained more stable than the preseason model in 2026. The Tier 5 model achieved an RMSE of **321** and R² of **0.8168** in 2024, and an RMSE of **330** and R² of **0.7230** in 2025. On the first 20 scored games of 2026, it produced an RMSE of **390**, MAE of **332**, and R² of **0.2897**.

Across the 2024–2025 evaluation years, the two-day-ahead model reduced RMSE to **326**, achieved **9.1% MAPE**, and placed **88%** of predictions within 500 attendees. It improved on the baseline by **48.0%**, making it the strongest overall forecasting configuration in the project. :
### Prediction Context Comparison

A direct comparison of the best reduced model from each forecasting context highlights the value of additional near-term information.

| Context | Tier | Model | Features | wCV RMSE | RMSE | MAE | R² | MAPE | Within 500 | vs. Baseline |
|---------|------|-------|---------:|---------:|-----:|----:|---:|-----:|------------:|-------------:|
| Preseason | Tier 4 | Elastic Net | 39 | 406 | 406 | 330 | 0.6128 | 11.6% | 82% | +38.8% |
| Two-Day-Ahead | Tier 5 | Elastic Net | 31 | 327 | 326 | 260 | 0.8048 | 9.1% | 88% | +48.0% |

The two-day-ahead model improved RMSE by **80 attendees**, or about **19.7%**, relative to the best preseason model. This gain came from incorporating information unavailable before the season, including **weather forecasts, current team form, in-season attendance history, and in-season Google Trends signals**. The year-by-year breakdown shows that the update model outperformed the preseason model in both evaluation seasons:

| Context | 2024 RMSE | 2024 R² | 2025 RMSE | 2025 R² |
|---------|----------:|--------:|----------:|--------:|
| Preseason | 402 | 0.5134 | 409 | 0.5312 |
| Two-Day-Ahead | 321 | 0.8168 | 330 | 0.7230 |

Together, these results show that the preseason model is strong enough for strategic planning before opening day, while the two-day-ahead update provides a more accurate operational forecast once short-term contextual information becomes available. The gap between the two settings also gives a practical estimate of the value added by near-term signals in attendance forecasting. 

### Summary of Findings

Several clear findings emerged from the results:

- **Elastic Net was the best-performing model family** across both forecasting contexts and all winning tiers.
- In the **preseason** setting, the best balance came from **Tier 4**, where richer contextual features improved forecasting without sacrificing too much historical training depth.
- In the **two-day-ahead** setting, the best results came from **Tier 5**, showing that full contextual coverage is most useful when predictions are made close to game time.
- Adding near-term information reduced error by nearly **20%**, confirming that weather, recent attendance, current form, and current fan-interest signals provide meaningful forecasting value beyond preseason information alone.
- Even the preseason model produced substantial improvement over baseline, making it useful for advance planning, while the two-day-ahead model provided the strongest support for tactical game-day operations.

---
## Interpretability / Key Drivers

To better understand what drives attendance predictions, SHAP-based interpretability analysis was applied to the best reduced Elastic Net model in each forecasting context. We summarize both **grouped feature importance** and the **most influential individual predictors** for the winning preseason and two-day-ahead models.

### Preseason Key Drivers
![](README_ASSETS/group%20importance%20pre.png)
![](README_ASSETS/TOP%2015%20FEATURES%20pre.png)

For the winning **preseason Tier 4** model, the most important feature groups were **attendance history** and **Google Trends**, followed by **performance** and **schedule**. This suggests that before the season begins, the model relies most heavily on prior demand signals and preseason fan-interest measures rather than short-term operational context.

At the individual-feature level, the strongest predictors were **`gt_youtube_preseason`**, **`prev_season_attendance`**, and **`prev_3yr_attendance`**, followed by additional preseason search-interest, schedule timing, and previous-season team-strength variables. Taken together, these results suggest that preseason forecasting is driven primarily by a combination of **historical attendance momentum**, **preseason public attention**, and **last-season team quality**.

### Two-Day-Ahead Key Drivers

![](README_ASSETS/group%20importance.png)
![](README_ASSETS/TOP%2015%20FEATURES.png)

For the winning **two-day-ahead Tier 5** model, **schedule** was the most important feature group by a large margin, followed by **weather**, **academics**, **opponent context**, and **performance**. In contrast to the preseason model, attendance-history features played a smaller role once more immediate contextual information became available close to game day.

At the individual-feature level, the most influential predictors included **`season_year`**, **`day_of_week_sin`**, **`time_bucket_night`**, **`playoff`**, and **`att_avg_last5`**, along with academic-semester indicators and multiple weather-condition features. These results suggest that short-term attendance forecasting depends most on **when the game is being played**, **what part of the season it is**, **recent local demand**, and **near-term game-day conditions**.

### Interpretability Takeaways

Across both forecasting contexts, the interpretability results show that attendance is not driven by a single factor. Instead, the most important predictors shift depending on when the prediction is made:

- **Before the season**, the model depends more on historical demand, preseason fan interest, and previous-season strength.
- **Closer to game day**, the model depends more on schedule timing, weather, current context, and recent attendance behavior.

This difference helps explain why the two-day-ahead model outperformed the preseason model: it is able to incorporate several important short-term signals that are unavailable before opening day.

---

## Actionable Insights for UNC Athletics

The model results suggest several practical takeaways for game-day planning and resource allocation.

### 1. Use preseason forecasts for big-picture planning

The preseason model performs well enough to support higher-level planning before opening day. Because it draws heavily on prior attendance history and preseason fan-interest signals, it can help identify which games are likely to be stronger draws months in advance. This makes it useful for:

- seasonal staffing expectations
- budget and concessions planning
- early identification of likely high-demand and low-demand games

### 2. Use two-day-ahead forecasts for final operational decisions

The two-day-ahead model is more accurate because it incorporates immediate contextual information such as weather, current season timing, and recent attendance trends. This makes it better suited for final operational adjustments, including:

- last-mile staffing decisions
- parking and gate planning
- final concessions preparation
- short-term promotional targeting

### 3. Scheduling context matters most close to game day

Because schedule-related variables dominate the winning two-day-ahead model, UNC Athletics should expect the strongest attendance around favorable time slots, later-season games, and postseason settings. Night games, weekend games, and playoff-related settings appear especially important for turnout.

### 4. Weather should meaningfully adjust short-term expectations

Weather-related features became much more important in the short-term model than in the preseason model. This suggests that attendance expectations should be adjusted meaningfully when near-term conditions change, especially for games that are otherwise on the margin between moderate and strong turnout.

### 5. Historical demand remains the foundation

Even though short-term context improves accuracy, historical attendance signals remain important across both models. Prior-season attendance and recent attendance behavior continue to provide a strong baseline expectation for likely demand.

### 6. Fan-interest signals are valuable before the season begins

The importance of preseason Google Trends in the winning preseason model suggests that public attention may offer useful early signal beyond simple schedule-based planning. This can help distinguish games that look similar on paper but enter the season with different levels of audience interest.

---

## Generalization
 
One of the things we wanted to test was whether this pipeline actually works outside of UNC baseball, or if the results were just specific to Boshamer Stadium. To find out, we took the UNC baseball model configs and applied them directly to two other contexts without changing any hyperparameters: **NC State baseball** (same sport, different school) and **UNC men's basketball** (same school, different sport).
 
### How We Set Up the Transfer
 
We used the pre-BFE day-before configurations from UNC baseball rather than the post-BFE ones, since backward feature elimination was tuned specifically to UNC baseball data and wouldn't necessarily make sense for a different dataset. Everything else stayed the same (model type, hyperparameters, scaler, decay, and preprocessing).
 
We tested three transfer pairs:
 
- **UNC Baseball Tier 1 → NC State Baseball Tier 1:** Same sport, different venue (Doak Field), different fan base. Tests whether the core features like schedule, weather, opponent, and attendance history generalize across schools.
- **UNC Baseball Tier 1 → UNC Basketball Tier 1:** Same school, completely different sport. The Dean Dome is a near-capacity indoor venue, so the attendance dynamics are very different from an outdoor baseball stadium.
- **UNC Baseball Tier 4 → UNC Basketball Tier 2:** Same idea as above, but this one adds academic calendar and enrollment features to see if those help for basketball the way they help for baseball.
 
### Building the Transfer Datasets
 
We built the NC State and UNC basketball datasets from scratch using the same general approach as UNC baseball. Each one included game-level attendance, schedule info, opponent data, and weather pulled from Open-Meteo using venue-specific coordinates. For basketball, we also included academic calendar and enrollment data in the Tier 2 version.
 
NC State used Doak Field as the home venue. UNC basketball used the Dean E. Smith Center. Both excluded COVID seasons and followed the same temporal rules as the baseball pipeline.
 
### Results
 
Every transfer context beat its baseline, which was a good sign that the recipe actually generalizes.
 
| Context | Equiv Tier | RMSE | MAE | R² | MAPE | <500 | vs BL |
|---------|-----------|-----:|----:|---:|-----:|-----:|------:|
| UNC Baseball T1 | (native) | 423 | 335 | 0.670 | 11.0% | 80% | +32.4% |
| NC State Baseball | UNC T1 | 263 | 206 | 0.006 | 7.7% | 91% | +21.4% |
| UNC Basketball T1 | UNC T1 | 1,182 | 904 | 0.311 | 4.3% | 30% | +45.9% |
| UNC Baseball T4 | (native) | 446 | 348 | 0.634 | 12.3% | 70% | +28.8% |
| UNC Basketball T2 | UNC T4 | 1,694 | 1,419 | -0.415 | 6.8% | 17% | +22.5% |
 
The first and fourth rows are UNC baseball's own performance as a reference point.
 
**NC State** had the lowest percentage error of any baseball context at 7.7% MAPE, with 91% of predictions within 500 fans. The R² being near zero looks bad at first glance, but it's actually just because Doak Field attendance doesn't vary much game to game, so even when the predictions are accurate, there isn't much variance for R² to "explain." The MAPE and within-500 numbers tell the real story.
 
**UNC Basketball Tier 1** had the lowest MAPE of any context in the entire project at 4.3%, and it beat baseline by nearly 46%. The RMSE of 1,182 sounds high, but the Dean Dome averages around 19,500 per game and is close to full most nights. In percentage terms the model is actually really precise.
 
**UNC Basketball Tier 2** added academic calendar features but actually got worse, with a negative R². This makes sense when you think about it. For basketball, the Dean Dome sells out whether it's exam week or not, so those features just add noise.
 
### What Drives Attendance Across Sports
 
We ran SHAP on each context's Tier 1 model to compare feature importance on an apples-to-apples basis (Tier 1 is the common feature set available everywhere).
 
| Factor | UNC Baseball | UNC Basketball | NC State Baseball |
|--------|:-----------:|:-------------:|:-----------------:|
| Attendance History | 34% | 12% | 43% |
| Schedule & Timing | 33% | 51% | 35% |
| Opponent | 15% | 14% | 10% |
| Weather | 11% | 16% | 9% |
| Performance | 4% | 5% | 4% |
| Academics | 4% | 2% | 0% |
 
A few things stood out:
 
- **Schedule and opponent matter everywhere.** Doesn't matter what sport or venue, when the game is played and who's playing are always important.
- **Attendance history depends on the venue.** It's huge for both baseball contexts (34% and 43%) because attendance varies a lot game to game. For basketball it's only 12% because the Dean Dome is near capacity most nights, so there just isn't much to predict.
- **Weather affects all three, even indoor basketball.** Fans still have to get to the venue, so bad weather can suppress turnout even when the game itself is indoors.
- **Academics only matter at Boshamer.** The 4% for UNC baseball reflects students walking to games during class hours. For NC State and basketball, this effect is basically zero.
 
### What This Means
 
The main takeaway is that the pipeline works beyond UNC baseball. The same Elastic Net setup, with no re-tuning at all, beat baseline in every context we tested. The core features (schedule, opponent, weather, attendance history) transfer well across sports and venues, though context-specific features like academic calendar don't always carry over.
 
This gives us confidence that extending the framework to other UNC sports like softball, soccer, or lacrosse is a reasonable next step. The core methodology should hold up, though feature groups would need to be evaluated fresh for each new sport rather than assumed to work the same way.

---
## Streamlit App

[Visit App](https://unc-baseball-attendance-forecasting.streamlit.app/)

The project includes an interactive Streamlit application that allows stakeholders to explore UNC baseball attendance forecasts in a simple, decision-support format. The app loads prediction files from `APP/data` and supports both **preseason** and **midseason** forecasting views. 

Users can:
- choose a **season**
- switch between **preseason** and **midseason** predictions
- filter the display by **month**
- review season-level summary metrics such as average prediction, highest prediction, lowest prediction, average actual attendance, and MAE when actuals are available
- inspect an individual game in detail
- explore the filtered dataset in table form 

The main app interface includes:
- a **Season calendar** tab for viewing projected attendance across the schedule
- a **Game detail** tab for reviewing one selected game more closely
- a **Data explorer** tab for comparing filtered rows and columns in a table
- a **Help / tutorial** section to guide non-technical users through the app workflow 

This app is intended to support both **strategic planning** and **short-term operational decisions**. Preseason predictions provide a full-schedule planning view before the season begins, while midseason predictions incorporate more current information and are intended to support closer-to-game decision-making. :contentReference[oaicite:3]{index=3}

In deployment, the app is connected to the project’s refresh pipeline. Updated prediction files are written into `APP/data` and pushed to GitHub through the automated refresh workflow, allowing the deployed app to reflect newly refreshed data as files are updated. 

---
## Repository Structure

The repository is organized into folders for the deployed Streamlit app, data scraping and refresh workflows, model scoring, supporting analysis, and project deliverables.

- **`APP/`**  
  Contains the Streamlit application and the app-facing prediction files stored in `APP/data`. The app reads preseason and midseason prediction bundles from this folder and presents them through the interactive dashboard. :contentReference[oaicite:0]{index=0}

- **`SCRAPERS/`**  
  Contains the attendance scraper, performance-data refresh notebooks, and the automation script used to run the daily data refresh pipeline. This folder supports the end-to-end update process that refreshes source data and prepares downstream inputs for scoring. :contentReference[oaicite:1]{index=1}

- **`SCORING/`**  
  Contains the scoring notebook used to generate the updated app prediction outputs. The refresh pipeline runs this notebook after the upstream attendance and performance data are refreshed. :contentReference[oaicite:2]{index=2}

- **`DATA ANALYSIS/`**  
  Contains exploratory analysis notebooks, modeling analysis, and other project development materials used during research and experimentation.

- **`DATAPLANS/`**  
  Stores project planning materials related to data collection, data organization, and supporting documentation such as data plans and dictionaries.

- **`LITREVIEWS/`**  
  Contains literature review materials and related written project research.

- **`PRESENTATION/`**  
  Stores presentation deliverables used to communicate project results and findings.

- **`PROJECT_PLAN_PRESENTATION/`**  
  Contains earlier planning presentation materials developed during the project setup phase.

- **`README_ASSETS/`**  
  Stores images and visual assets used in the main project README, including plots, diagrams, and workflow graphics.

- **`.gitignore`**  
  Excludes unnecessary intermediate files, caches, and other nonessential artifacts from version control.

- **`README.md`**  
  The main project landing page, which documents the project scope, methods, results, app, and reproducibility details.

Overall, the repository is structured so that the app, refresh pipeline, and modeling workflow are separated clearly from supporting analysis and presentation materials. This makes it easier to maintain the deployed application while preserving the broader project documentation and technical work.

---
## Quick Start / Reproducibility

### Prerequisites
 
- Python 3.9+
- Google Colab or a local Jupyter environment
- Required packages: `pandas`, `numpy`, `scikit-learn`, `optuna`, `shap`, `xgboost`, `lightgbm`, `matplotlib`
 
### Running the Modeling Notebook
 
1. Clone the repository and navigate to the project root.
2. Ensure all CSV data files listed in the Data Sources section above are in the working directory (or adjust file paths as needed).
3. Open the main modeling notebook (`Capstone_Unified_Final.ipynb`) in Google Colab or Jupyter.
4. Run all cells sequentially from top to bottom. The notebook is designed to execute end-to-end without manual intervention.
5. Full execution takes approximately 45-60 minutes on Google Colab, with the majority of time spent on Optuna optimization (5,000 trials per context) and backward feature elimination.
 
### Running the Scrapers
 
1. The attendance and schedule scrapers are located in the `SCRAPERS/` directory.
2. Run the base baseball scraper first to produce `unc_baseball_final.csv`.
3. Run the performance scraper next, which takes the base scrape output as input and produces the season performance file.
4. NC State and UNC basketball scrapers can be run independently to produce their respective datasets.
 
### Running the Streamlit App Locally
 
1. Navigate to the `APP/` directory.
2. Install Streamlit: `pip install streamlit`
3. Run: `streamlit run app.py`
4. The app reads prediction CSVs from `APP/data/`. To update predictions, re-run the scoring notebook in `SCORING/` after refreshing upstream data.
 
### Reproducibility Notes
 
- All random operations use `SEED=22` for reproducibility.
- Optuna uses the TPE sampler with the same seed, so optimization results are deterministic given the same input data.
- Weather data is fetched from the Open-Meteo historical API, which returns consistent results for the same date ranges.
- The only variability comes from upstream data changes (new games played, updated attendance figures), which are expected in a live forecasting system.

---
## Limitations

Several limitations should be considered when interpreting the results of this project.

- **Uneven historical coverage across feature groups:** Because different data sources begin in different years, the richest tiers have much shorter training windows. This makes it difficult to fully separate the value of added features from the cost of reduced historical sample size.

- **COVID-era and stadium-adjustment exclusions:** The 2020 and 2021 seasons were excluded because attendance was artificially constrained, and 2008 was excluded due to Boshamer Stadium reconstruction. These exclusions were necessary, but they reduce the amount of usable historical data.

- **Promotion data is historically limited:** Promotion variables were only available in the most recent seasons, which restricted their usefulness in model selection and prevented them from contributing to some of the strongest longer-history tiers.

- **Short-horizon forecasts depend on real-time inputs:** The two-day-ahead model requires current contextual information such as weather forecasts, recent attendance, and in-season team form. As a result, it cannot generate the full-season forecast coverage that the preseason model can.

- **External demand factors are not fully observed:** The current pipeline does not include some potentially valuable real-time signals such as ticket presales, special campus events, local media attention, or late schedule changes. These factors may explain some remaining forecast error.

- **Early forward prediction remains noisy:** The 2026 forward predictions were generated with only part of the season completed. Because early-season distributions can differ from the historical average, forward performance may shift as more games are played and the model is retrained.

- **Interpretability is approximate, not causal:** SHAP analysis is useful for understanding which features most influence model predictions, but it should not be interpreted as proving causal relationships between specific variables and attendance.

Overall, these limitations reflect the realities of forecasting attendance in a changing, partially observed environment. The models provide useful planning guidance, but they should be treated as decision-support tools rather than exact attendance calculators.

---
## Future Work

There are several natural directions for extending this framework.

- **Annual retraining and maintenance:** Retrain the pipeline after each completed season so the models remain calibrated to the most recent attendance environment.

- **Integrate ticket presales:** Adding presale or advance ticketing information would likely improve short-horizon forecasts and provide a more direct signal of near-term demand.

- **Develop an intermediate forecast horizon:** A mid-range model for the next one to two weeks could fill the gap between preseason planning and two-day-ahead operations.

- **Expand to additional UNC sports:** The general framework could be extended to other programs such as softball, soccer, lacrosse, or volleyball, with sport-specific feature adaptations where needed.

- **Refine promotion modeling:** With more historical promotion data, future versions could better estimate which promotion types work best under different schedule, opponent, and performance conditions.

- **Add richer campus and local context:** Additional features such as concurrent campus events, local weather severity alerts, student calendar intensity, and regional competition for attention may improve accuracy.

- **Incorporate uncertainty estimates into deployment:** Future versions of the app could surface prediction intervals or uncertainty bands so stakeholders can plan around attendance ranges rather than single-point estimates alone.

- **Evaluate transfer learning more systematically:** Additional work could test how well the modeling recipe generalizes across venues, sports, and institutions, and identify which feature groups transfer best.

- **Support more operational scenario analysis:** The Streamlit tool could be extended to let users test what-if scenarios for promotions, weather changes, or scheduling changes in a more structured way.

Together, these next steps would strengthen both the predictive performance and the practical usefulness of the attendance-forecasting framework.

---






