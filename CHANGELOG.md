# Changelog

## \[Unreleased\]

### Added

-   Sprint 01 dataset loading workflow for Users, Ratings, and Movies.
-   Initial dataset profiling and data-quality checks.
-   Rating distribution analysis.
-   User activity analysis.
-   Movie popularity analysis.
-   Interaction sparsity analysis.
-   Timestamp conversion and monthly temporal aggregation.
-   Initial visualization set for rating, activity, popularity, and
    temporal interaction patterns.
-   Sprint 01 engineering decisions and retrospective documentation.

### Engineering Notes

-   Raw `.dat` files were loaded without headers using `header=None` and
    explicit column names.
-   `latin-1` encoding was used for `movies.dat` after UTF-8 decoding
    failed.
-   Monthly aggregation was selected for temporal analysis.
-   UserID and MovieID were treated as identifiers rather than
    meaningful numerical features.

---

## [Sprint 02] — Exploratory Analysis & Recommendation Insights

### Added
- User activity analysis
- User rating statistics and rating consistency analysis
- Movie popularity and average rating analysis
- Movie release year extraction
- Genre-level interaction analysis
- User–movie–genre interaction analysis
- Interaction density and sparsity analysis
- Visualization of user activity and movie popularity distributions
- Genre interaction and average rating visualizations
- Rating distribution by genre visualization
- Bias and data quality analysis
- Engineering analysis of popularity bias, rating bias, sparsity, and cold-start

### Key Findings
- User and movie activity distributions are highly skewed.
- The dataset contains significant popularity bias.
- Interaction density is approximately 4.47%, with approximately 95.53% sparsity.
- Genre-level rating behavior varies across genres.
- Genre, user activity, movie statistics, and selected temporal features may be useful for recommendation.

### Next
- Begin recommendation logic and candidate recommendation strategies in Sprint 03.

---

## [Sprint 03] — Initial Recommendation Logic & Evaluation

### Added

* Temporal 80/20 train-test evaluation split.
* User-genre preference scoring with confidence weighting.
* Movie quality scoring using rating averages and rating counts.
* Candidate movie generation with previously-rated movie exclusion.
* Personalized movie scoring combining genre preference and movie quality.
* Top-10 recommendation generation.
* Manual Precision@10, Recall@10, and F1@10 evaluation.

### Evaluation

* Evaluated recommendations across 5,956 users.
* Mean Precision@10: 0.045064
* Mean Recall@10: 0.028039
* Mean F1@10: 0.028109

### Findings

* Genre preference alone produced less differentiated rankings.
* Adding movie-level quality produced more diverse Top-10 recommendations.
* Overall recommendation performance remained low, with substantial variation across users.

### Next

* Establish stronger recommendation baselines.
* Investigate similarity-based and collaborative filtering approaches.
* Compare performance across multiple Top-K values.
