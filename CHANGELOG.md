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