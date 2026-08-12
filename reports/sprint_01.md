# Sprint 01 --- Dataset Understanding & Initial Data Profiling

## Sprint Goal

Understand the structure, quality, sparsity, temporal characteristics,
and basic interaction patterns of the recommendation-system dataset
before entering the modeling stage.

## Business Problem

The project aims to build a recommendation system that can provide
relevant movie recommendations based on user-item interactions. Before
selecting a recommendation approach, the available users, movies,
interactions, and their characteristics must be understood.

## Dataset Overview

  Entity                          Rows   Columns
  ------------------------ ----------- ---------
  Users                          6,040         5
  Ratings / Interactions     1,000,209         4
  Movies                         3,883         3

The dataset contains separate user, movie, and interaction files. UserID
and MovieID are primarily treated as identifiers rather than meaningful
numerical features.

## Key Findings

### Dataset Structure

-   The dataset consists of three main entities: Users, Movies, and
    Ratings/Interactions.
-   The User dataset contains 6,040 users and 5 features.
-   The Ratings dataset contains more than one million interaction
    records.
-   The Movie dataset contains 3,883 movies and 3 features.
-   UserID and MovieID were treated primarily as identifiers rather than
    meaningful predictive features.
-   User-related attributes such as Age, Gender, and Occupation provide
    additional information about users, while Genres provide information
    about movies.

### Rating Distribution

-   Ratings are concentrated toward higher values.
-   Rating 4 has the highest frequency, followed by Ratings 3 and 5.
-   Lower ratings occur less frequently.
-   The rating distribution is therefore not uniform and is shifted
    toward higher ratings.

### User Activity Distribution

-   User activity is highly uneven.
-   Most users have a relatively small number of interactions.
-   A smaller number of users have substantially more interactions.
-   The distribution is strongly right-skewed.

### Movie Popularity Distribution

-   Movie interactions are highly uneven.
-   Most movies receive relatively few ratings, while a small number
    receive a very large number of ratings.
-   This indicates strong item-popularity imbalance and a long-tail
    pattern.

### Sparsity

The observed interaction density is approximately 4.26%, meaning that
most possible user-movie pairs are not observed in the interaction data.

### Timestamp Analysis

-   Interaction timestamps are represented as Unix timestamps in
    seconds.
-   Timestamps were converted into a datetime representation for
    temporal analysis.
-   The dataset covers approximately three years.
-   Monthly aggregation was selected because it provides a balanced
    level of temporal detail: daily aggregation would be noisier, while
    yearly aggregation would hide important temporal patterns.

### Temporal Interaction Patterns

-   Interaction volume varies substantially over time.
-   A pronounced peak occurs around late 2000.
-   Interaction volume then decreases considerably and remains much
    lower during the later period.
-   This temporal pattern should be considered when designing the
    evaluation strategy.

## Sprint Retrospective

This sprint improved my understanding of how to inspect a recommendation
dataset before moving toward modeling.

I learned how to inspect dataset structure and data types and determine
which columns provide meaningful information for the recommendation
problem.

I also learned how visualization choices depend on the type of
information being analyzed. A bar plot was appropriate for discrete
rating values, histograms were useful for user activity and movie
interaction counts, and a line plot was appropriate for interaction
volume over time.

Another important part of the sprint was timestamp processing. I
initially had difficulty converting Unix timestamps into a monthly
representation for temporal analysis. After researching the relevant
pandas functionality, I was able to perform the transformation and
aggregate interactions by month.

The sprint also improved my ability to identify patterns such as skewed
distributions, uneven user activity, uneven movie popularity, and
temporal changes in interaction volume.

For future work, I want to improve my ability to identify useful
patterns from dataset features and make visualization and engineering
decisions more independently.

## Engineering Analysis

The initial analysis revealed several characteristics that may influence
the recommendation system:

-   User activity is highly uneven.
-   Movie popularity is highly uneven.
-   Ratings are concentrated toward higher values.
-   Interaction volume changes considerably over time.
-   The dataset is sparse, with only a small fraction of possible
    user-movie interactions being observed.

These characteristics should be considered during later stages,
particularly when designing the recommendation approach, evaluation
strategy, and validation procedure.

## Sprint Status

**Completed**
