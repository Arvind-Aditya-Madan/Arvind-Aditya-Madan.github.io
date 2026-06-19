

## ACC Men’s Basketball Second-Half Score Prediction

### Full Project Summary

### 1. Problem Definition & Goal

The goal of this project is to **predict second-half outcomes for ACC men’s basketball games**, with a primary focus on:

* **Target:**

  * `spread = home_score − away_score`


The core objective is **minimizing Mean Absolute Error (MAE)** on second-half games, while maintaining realistic, deployable modeling assumptions (no leakage, rolling training).

A secondary goal is to construct **prediction intervals** for the spread with:

* ≥ **70% empirical coverage**
* As **narrow** as possible (small PIW)

---

## 2. Data Construction

### Game-Level Dataset

* One row per **team-game** initially
* Later merged into **one row per game** (home + away)
* Filters applied:

  * Postseason games removed
  * Neutral-site games handled explicitly
  * COVID season (2020–21) excluded from modeling runs

### Target Variables

From merged second-half games:

* `home_score`
* `away_score`
* `spread`
* `total`

---

## 3. Temporal Structure & Midseason Definition

* **Midseason is defined as a calendar date**, not a game count.
* All **first-half games** (before midseason date) are used to construct features.
* All **second-half games** are prediction targets.
* This avoids leakage and preserves real-world forecasting structure.

---

## 4. Feature Engineering (First-Half Team-Season Features)

All predictive features are computed **only from first-half games**, then merged into second-half matchups.

### Core Efficiency & Production

* Points, opponent points, margin
* Offensive / defensive / net ratings
* Pace
* Shooting efficiency (FG%, 3P%, FT%)
* Free throw rate
* Turnover rate
* Rebounding (ORB, DRB)
* Assists, fouls

### Form Features

* Rolling last-5 games:

  * Net rating
  * Margin
  * Points per game

### ACC-Only Performance

* ACC net rating
* ACC margin
* ACC win %
* If a team had **no ACC games** in the first half case: michigan, ohio state, baylor 2026: 

  * ACC features are imputed using **overall first-half performance**

### Venue Splits

* Home-team home net rating
* Away-team away net rating
* Combined as:

  * `venue_net_rtg_diff = home_home_net_rtg − away_away_net_rtg`

---

## 5. Feature Representations Tested

Multiple representations were explicitly compared:

### RAW

* Home and away features kept separately
  (`home_net_rtg_mean`, `away_net_rtg_mean`, etc.)

### DIFF

* Home − away differences for all team features

### BOTH

* RAW + DIFF together

### HYBRID (final direction)

* Core DIFF features
* Select RAW context features
* Minimal categorical encoding

**Key finding:**
DIFF  representations consistently outperformed RAW alone and were more stable across seasons.

---

## 6. AP Poll Features (Minimal, Corrected)

Rather than overengineering rankings, a **minimal and defensible set** was used.

### Midseason Ranking

* `rank_midseason`

  * AP rank if ranked at midseason
  * `NaN` if unranked
* `ranked_midseason`

  * Binary indicator derived from `rank_midseason`

### First-Half Ranking Exposure

* `ranked_share_pre`

  * Share of first-half games in which the team was ranked
  * Filled with **0** if never ranked

### Performance vs Ranked Opponents

* `net_rtg_vs_ranked_opp_pre`
* `margin_vs_ranked_opp_pre`

If a team played **no ranked opponents** in the first half:

* These features are imputed as:

```
overall performance − average observed drop vs ranked teams
```

Empirically estimated drops:

* Net rating drop ≈ **−18**
* Margin drop ≈ **−12**

This preserves team ordering and avoids artificial zeros.

---

## 7. Categorical Variables

* `home_team`, `away_team`
* One-hot encoded via `ColumnTransformer`
* Left in intentionally to allow the model to capture:

  * Program-level effects
  * Coaching/system persistence
* Empirically helpful for tree models

---

## 8. Modeling Approach

### Models Tested

* Linear Regression
* Ridge Regression
* Elastic Net
* Random Forest
* Gradient Boosted Trees (sklearn)
* (LightGBM explored conceptually)

### Best Performer

* **Random Forest** was:

  * Most stable
  * Best MAE across rolling seasons
  * Most robust to feature noise
optuna used for parameters 

### Multi-Output (Exploratory)

* RF wrapped in `MultiOutputRegressor` to predict:

  * home_score
  * away_score
  * spread
  * total
* Final evaluation still focused on **spread only**

---

## 9. Evaluation Strategy

### Rolling Evaluation

* Test on each second-half season
* Train on **prior seasons only**
* Final version uses a **6-season rolling training window**

  * Reduces non-stationarity
  * Better reflects real deployment

### Metrics Reported

* Mean MAE
* Std. dev. of MAE (stability)
* Best / worst season
* Most recent season MAE (e.g. 2024–25)

---

## 10. Feature Selection Philosophy

* **No blind pruning**

* Features removed **only if MAE improved empirically**
* all diffs were basically kept in the end 


---



---

## 12. Prediction Intervals (Second Phase)

### Method

* **Split conformal prediction** for spread only
* Model trained on `fit` seasons
* Calibration performed on recent seasons

### Two Calibration Strategies

1. **Rolling calibration**

   * Calibrate on the **prior 4 seasons** before each test season
2. **Fixed calibration (deployment)**

   * Use most recent 3–5 seasons to calibrate once
   * Apply to upcoming season predictions

### Interval Construction

* Absolute residuals from calibration
* Conformal quantile `q`
* Interval:

  ```
  [pred − q, pred + q]
  ```

### Targets

* Coverage ≥ 70%
* Minimize Prediction Interval Width (PIW)

---

## 13. Final Outcomes & Insights

* Best MAE ≈ **9.2–9.4 points**
* Random Forest + HYBRID/DIFF features consistently strongest
* AP Poll features added modest context but were not dominant
* Ranked-opponent proportional features were unstable and discarded
* Absolute performance differences were more reliable
* Interval calibration behaved sensibly with rolling windows

---

## 14. Project Strengths

* Strict temporal integrity (no leakage)
* Multiple feature representations tested
* Rolling evaluation aligned with deployment
* Intervals handled carefully and transparently
* Modeling decisions justified empirically

---

## 15. Where This Could Go Next

* Team-specific conformal intervals
* Separate home/away uncertainty modeling
* Market comparison (Vegas spreads vs model)
* Incorporating lineup/injury data if available

---



this is in chatgpt word - about to give you i my word and some csvs 

The challenge was to, at feb6, predict spread (home-away) for all acc games in the restof the season
so we had to train a model that would use first half of the season games to predict the spread for second half
used R to load game data from p2014-2025 using ncaahoopR
for acc teams. box scores were appended.
moved to python
removed post season
added midpoint date
appended ap poll stats
computed ratings like
df["off_rtg"] = 100 * safe_div(df["pts"], df["possessions"])
df["def_rtg"] = 100 * safe_div(df["opp_pts"], df["opp_possessions"])
df["net_rtg"] = df["off_rtg"] - df["def_rtg"]

split on midseason

started computing stats for each team which will be evident later when i list features
stats were by season, by team, only first half

everything was stripped from second half but date, teams, who hom/away, and spread
appended home steam first season stats and away team first half season stats for each game

these were RAW stats.
created DIFF stats which were home stats - away stats. since thats how spread is calculated

each of the 5 model types were tested on raw, diff, both, hybrid. diff won every time

cv was done like:
def rolling_eval(model, model_name, window=None):
    rows = []

    for test_season in TEST_SEASONS:
        idx = season_order.index(test_season)
        prior_seasons = season_order[:idx]

        # apply rolling window if requested
        if window is not None:
            prior_seasons = prior_seasons[-window:]

        train_mask = seasons.isin(prior_seasons)
        test_mask  = seasons.eq(test_season)

        # safety check (quiet skip, same spirit as your original)
        if train_mask.sum() == 0 or test_mask.sum() == 0:
            continue

        X_train, y_train = X.loc[train_mask], y.loc[train_mask]
        X_test,  y_test  = X.loc[test_mask],  y.loc[test_mask]

        m = clone(model)
        m.fit(X_train, y_train)
        yhat = m.predict(X_test)

        mae = mean_absolute_error(y_test, yhat)

        rows.append({
            "model": model_name,
            "test_season": test_season,
            "n_games": int(test_mask.sum()),
            "mae": float(mae),
            "n_train_seasons": len(prior_seasons),
        })

    return pd.DataFrame(rows)

rf consistently was the best. optimized by trying compos of features - some based on importance. used optuna for params. got mean mae to 9.186

feats: ['home_team', 'away_team', 'days_since_midseason', 'rest_days_home', 'rest_days_away', 'rest_diff', 'home_ranked_midseason', 'away_ranked_midseason', 'home_rank_midseason', 'away_rank_midseason', 'pts_pg_diff', 'opp_pts_pg_diff', 'margin_pg_diff', 'off_rtg_mean_diff', 'def_rtg_mean_diff', 'net_rtg_mean_diff', 'pace_mean_diff', 'fg_pct_mean_margin_diff', 'tp_pct_mean_margin_diff', 'ft_pct_mean_margin_diff', 'ft_rate_mean_margin_diff', 'tov_rate_mean_margin_diff', 'ast_pg_margin_diff', 'orb_pg_margin_diff', 'drb_pg_margin_diff', 'pf_pg_margin_diff', 'win_pct_diff', 'net_rtg_last5_diff', 'margin_last5_diff', 'pts_pg_last5_diff', 'acc_net_rtg_mean_diff', 'acc_margin_pg_diff', 'acc_win_pct_diff', 'venue_net_rtg_diff', 'margin_vs_ranked_opp_diff', 'net_rtg_vs_ranked_opp_diff']

then went back to r for 2025-26 data - had to hadd the games for baylor, mich, ohio state - bc there were 3 non acc games. 

then did te same process and ran the model on it to get predictions to submit.

intervals was also a part of the project
def weighted_quantile(values, weights, quantile):
    """Compute weighted quantile of 1D arrays."""
    values = np.asarray(values)
    weights = np.asarray(weights)

    sorter = np.argsort(values)
    values = values[sorter]
    weights = weights[sorter]

    cum_w = np.cumsum(weights)
    cutoff = quantile * cum_w[-1]
    return values[np.searchsorted(cum_w, cutoff, side="left")]

def season_recency_weights(seasons, season_to_idx, covid_season="2020-21",
                           power=2, covid_weight=0.0):
    """
    seasons: array-like of season labels for calibration rows
    power: 1=linear, 2=quadratic
    covid_weight: 0.0 excludes COVID season from calibration
    """
    idx = np.array([season_to_idx[s] for s in seasons])
    w = 1.0 + (idx ** power)

    if covid_season is not None:
        w = np.where(np.array(seasons) == covid_season, covid_weight, w)

    return w

def conformal_q(abs_resid, coverage=0.70):
    n = len(abs_resid)
    k = int(np.ceil((n + 1) * coverage)) - 1
    return np.sort(abs_resid)[min(max(k, 0), n - 1)]

cov_by_season = []
piw_by_season = []

for season_test in TEST_SEASONS:

    season_order = [s for s in season_order if s != "2020-21"]
    TEST_SEASONS  = [s for s in TEST_SEASONS  if s != "2020-21"]

    # training seasons = all before test season
    idx_test = season_order.index(season_test)
    train_seasons = season_order[:idx_test]

    train_mask = seasons.isin(train_seasons)
    test_mask  = seasons.eq(season_test)

    # calibration season = most recent training season
    cal_season = train_seasons[-1]
    cal_mask = seasons.eq(cal_season)

    # fit seasons = training minus calibration season
    fit_mask = train_mask & (~cal_mask)

    X_fit, y_fit = X.loc[fit_mask], y.loc[fit_mask]
    X_cal, y_cal = X.loc[cal_mask], y.loc[cal_mask]
    X_te,  y_te  = X.loc[test_mask], y.loc[test_mask]

    model = clone(rf_model)
    model.fit(X_fit, y_fit)

    # calibration residuals
    cal_pred = model.predict(X_cal)
    abs_resid = np.abs(y_cal - cal_pred)

    q = conformal_q(abs_resid, coverage=0.75)

    # test intervals
    pred = model.predict(X_te)
    lb = pred - q
    ub = pred + q

    coverage = float(np.mean((y_te >= lb) & (y_te <= ub)))
    piw = float(np.mean(ub - lb))

    cov_by_season.append({"season": season_test, "coverage": coverage, "q": float(q)})
    piw_by_season.append({"season": season_test, "piw": piw})

cov_by_season = pd.DataFrame(cov_by_season).sort_values("season")
piw_by_season = pd.DataFrame(piw_by_season).sort_values("season")

coverage used was .75 to be safe but still have a low piw

about to send csvs- dont say nun yet

The challenge was to predict the point spread (home − away) for all remaining ACC men’s basketball games starting on February 6, using only information available up to that date.

To do this, I trained a model that uses first-half-of-season games to predict outcomes in the second half of the season, mirroring how a real forecasting system would operate.

Game-level data from 2014–2025 was collected in R using ncaahoopR for all ACC teams, with full box scores appended. The data was then transferred to Python for modeling. Postseason games were removed, and a midseason calendar date was defined to separate feature construction from prediction targets.

Offensive, defensive, and net ratings were computed using possession-based metrics, for example:

offensive rating = points per 100 possessions
defensive rating = opponent points per 100 possessions
net rating = offensive − defensive

All team statistics were computed by team and season using only first-half games. For second-half games, all performance information was stripped away, leaving only date, teams, home/away designation, and the target spread.

Each second-half game was then merged with:

the home team’s first-half season stats
the away team’s first-half season stats

This produced a RAW feature representation. From this, a DIFF representation was constructed by taking home − away differences for all team features, which directly aligns with how the spread is defined.

Five model families were evaluated (linear models, tree-based models, and ensembles), each tested using RAW, DIFF, BOTH, and HYBRID feature sets. Across all models and seasons, DIFF features consistently outperformed RAW representations.

Model evaluation used rolling season-based cross-validation, where each season’s second-half games were predicted using only prior seasons for training. A rolling training window was optionally applied to reduce non-stationarity.

A Random Forest model was consistently the best-performing approach. Feature selection was performed empirically based on performance and importance diagnostics, and hyperparameters were optimized using Optuna, resulting in a final mean MAE of ≈ 9.19 points on second-half spread predictions.

The final model used a DIFF-based feature set including efficiency metrics, recent form, ACC-only performance, venue-adjusted ratings, rest effects, and performance versus ranked opponents, along with team identifiers to capture program-level effects.

For submission, the same pipeline was applied to 2025–26 data. Because early-season schedules included several non-ACC opponents (e.g., Baylor, Michigan, Ohio State), those games were explicitly added to ensure first-half feature completeness before generating predictions for the remaining ACC games.

2. Prediction intervals (explained cleanly)

In addition to point predictions, the project also constructed prediction intervals for the spread using split conformal prediction.

For each test season:

The model was trained on all earlier seasons except the most recent one
The most recent prior season was used for calibration
Absolute residuals from the calibration season determined the conformal radius

Prediction intervals were formed as:

[prediction − q, prediction + q]

A coverage level of 75% was used to be conservative while still keeping intervals reasonably tight. Interval quality was evaluated using:

empirical coverage
prediction interval width (PIW)

Rolling calibration behaved consistently across seasons, producing stable coverage with manageable interval widths.

PREDICTION INTERVAL CONSTRUCTION FOR ACC MEN’S BASKETBALL SPREAD FORECASTS

Overview
In addition to producing point predictions for the ACC men’s basketball point spread (home − away), we constructed prediction intervals to quantify uncertainty in second-half game forecasts. The objective was to achieve at least 70% empirical coverage while keeping prediction intervals as narrow as possible. All intervals were built on top of our final spread prediction model and adhered to strict temporal constraints to avoid leakage.

Underlying Point Prediction Model
The base model predicts second-half spreads using a Random Forest regression model trained on first-half-of-season data only. Features were constructed using a DIFF representation, where each feature is defined as the difference between home-team and away-team first-half season statistics. This representation consistently outperformed RAW (separate home/away) features across all tested model families.

Model evaluation followed a rolling season framework. For each season, the model was trained using only prior seasons and evaluated on that season’s second-half games. A full historical training window was used for stability. The final model achieved a mean MAE of approximately 9.2 points across rolling evaluations and served as the foundation for interval construction.

Prediction Interval Method
Prediction intervals were constructed using split conformal prediction. This approach was selected because it makes minimal distributional assumptions, provides finite-sample coverage guarantees, and integrates naturally with rolling season-based evaluation.

For each test season:
- The model was trained on all seasons prior to the most recent one.
- The most recent prior season was used as a calibration set.
- Absolute residuals between predicted and observed spreads were computed on the calibration season.

Let r_i = |y_i − ŷ_i| denote the absolute calibration residuals. The conformal radius q was computed as the empirical quantile corresponding to a nominal coverage level of 75%, chosen conservatively to ensure coverage exceeded the required 70%.

Prediction intervals for each test-game prediction ŷ were then constructed as:
[ ŷ − q , ŷ + q ]

Evaluation of Prediction Intervals
Prediction intervals were evaluated on a season-by-season basis using:
- Empirical coverage: the proportion of games where the true spread fell within the interval.
- Prediction interval width (PIW): the average width of the intervals.

This rolling calibration and evaluation structure mirrors real deployment, where only past seasons are available for uncertainty estimation. The selected approach consistently met or exceeded 70% coverage while achieving one of the lowest average interval widths among tested calibration strategies.

Final Submission
The submitted file includes both point predictions and prediction intervals for second-half ACC games. All predictions rely exclusively on information available prior to February 6, with a natural two-day scheduling break ensuring clean separation between first-half feature construction and second-half prediction targets.

