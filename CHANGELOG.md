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
