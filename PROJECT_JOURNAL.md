# Project Journal

## Project 4 --- Recommendation System

### Sprint 01 --- Dataset Understanding & Initial Data Profiling

Sprint 01 focused on understanding the dataset before selecting and
implementing a recommendation algorithm.

The first stage was loading the three raw `.dat` files and creating
separate DataFrames for users, ratings/interactions, and movies. The raw
files did not contain headers, so explicit column names were provided
during loading. The movie file required `latin-1` encoding because UTF-8
decoding produced an error caused by characters outside the standard
English character set.

The initial data-quality checks showed no missing values or duplicate
rows in the three loaded DataFrames.

The analysis then focused on user activity, movie popularity, rating
distribution, sparsity, and timestamps. User and movie IDs were treated
as identifiers rather than meaningful numerical features.

The interaction density was approximately 4.26%, showing that the
user-movie interaction matrix is highly sparse.

Rating values were concentrated toward higher ratings, with Rating 4
being the most frequent value. User activity and movie popularity were
strongly right-skewed: most users and movies had relatively few
interactions, while a smaller number accounted for a large number of
interactions.

The Unix timestamps were converted into datetime values and then
aggregated monthly. Monthly aggregation was selected as a balance
between the noisy detail of daily aggregation and the coarse view of
yearly aggregation. The temporal analysis showed a pronounced
interaction peak around late 2000 followed by a substantial decline.

### Main Learning

The main lesson from this sprint was that recommendation modeling should
not begin before understanding the structure and behavior of the
interaction data. Dataset sparsity, user activity imbalance, item
popularity imbalance, and temporal variation may all affect later
modeling and evaluation decisions.

### Status

Sprint 01 completed.

---

## Sprint 02 — Exploratory Analysis & Recommendation Insights

Sprint 02 focused on understanding the behavioral structure of the recommendation dataset.

The analysis moved beyond basic dataset exploration and focused on the relationships between users, movies, and interactions. User activity, movie popularity, rating behavior, genre preferences, and interaction sparsity were examined.

One of the most important findings was the highly skewed interaction distribution. A relatively small number of users and movies account for a large proportion of interactions. The calculated interaction density was approximately 4.47%, highlighting the sparsity of the user–movie interaction space.

Genre-level analysis also showed that movie genres have different interaction volumes and rating patterns. This suggests that genre information may provide useful signals for future recommendation strategies.

The sprint also highlighted several engineering challenges, including popularity bias, rating bias, sparse interactions, and cold-start scenarios.

The main outcome of Sprint 02 was a clearer understanding of the data limitations and the signals that can potentially be used when designing the first recommendation approach in Sprint 03.