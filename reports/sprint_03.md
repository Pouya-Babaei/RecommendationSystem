# Sprint 03 — Initial Recommendation Logic & Evaluation

## Objectives

The main objective of Sprint 03 was to move from exploratory movie and user analysis toward an initial personalized recommendation pipeline.

The sprint focused on:

* Building user-level genre preference profiles.
* Estimating movie quality using both ratings and rating counts.
* Separating training and test interactions using a temporal split.
* Generating candidate movies while excluding movies already rated by the user.
* Combining personalized genre preferences with overall movie quality.
* Producing Top-N movie recommendations.
* Evaluating the recommendation results using manually implemented Precision@10, Recall@10, and F1@10.

---

## Task 1 — Recommendation Problem Definition

The recommendation problem was defined as a personalized Top-N recommendation task.

For each user, the system should:

1. Learn the user's preferences from historical ratings.
2. Identify movies that the user has not already rated in the training data.
3. Score these candidate movies.
4. Return the highest-scoring movies as recommendations.
5. Evaluate the recommendations against the user's future relevant ratings.

A movie was considered relevant during evaluation when:

```python
Rating >= 4
```

---

## Task 2 — Temporal Train/Test Split

To simulate a realistic recommendation scenario, the ratings were sorted chronologically for each user.

The final 20% of each user's rating history was used as the test set, while the first 80% was used for training.

This creates the following evaluation structure:

```text
Past interactions
       |
       v
     Train
       |
       v
Recommendation generation
       |
       v
Future interactions
       |
       v
     Test
       |
       v
Evaluation
```

This approach prevents future ratings from being used when constructing the user's training-time preferences.

The test set was filtered using a relevance threshold of 4:

```python
relevance_threshold = 4

test_relevant_df = test_df[
    test_df['Rating'] >= relevance_threshold
]
```

Only users with at least one relevant test interaction were considered for recommendation evaluation.

---

## Task 3 — User Genre Preference Modeling

The user's ratings were merged with movie genre information and multi-genre movies were expanded so that each genre could be analyzed separately.

For each `(UserID, Genre)` pair, the following statistics were calculated:

* Mean rating
* Number of ratings

A confidence-weighted preference score was then calculated.

The score combines the user's observed genre mean with the global training-set rating mean:

```python
Preference Score =
    (v / (v + m)) * R
    +
    (m / (v + m)) * C
```

Where:

* `R` = user's mean rating for the genre
* `v` = number of ratings the user gave within the genre
* `C` = global mean rating in the training data
* `m` = median genre rating count

This reduces the influence of genres with very few observations.

The resulting preference scores were then scaled for use in the recommendation scoring stage.

---

## Task 4 — Movie Quality Estimation

Movie quality was estimated independently from user-specific preferences.

For each movie in the training data, the following were calculated:

* Average rating
* Rating count

A weighted movie rating was then calculated using the same general shrinkage principle:

```python
Weighted Rating =
    (v / (v + m)) * Average Rating
    +
    (m / (v + m)) * Global Mean
```

Where:

* `Average Rating` = movie's mean training rating
* `v` = movie's training rating count
* `m` = median movie rating count
* `Global Mean` = overall training-set rating mean

This prevents movies with very small rating counts from receiving an excessively strong quality score based on limited evidence.

Candidate movies were restricted using a minimum rating-count threshold.

---

## Task 5 — Candidate Generation

Candidate movies were generated from movies with sufficient training-data evidence.

For each user, movies already rated by that user in the training set were removed:

```python
user_train_rated_movies = train_df.loc[
    train_df['UserID'] == user_id,
    'MovieID'
].unique()

user_candidates_df = candidate_movies_df[
    ~candidate_movies_df['MovieID'].isin(user_train_rated_movies)
].copy()
```

This ensures that the recommendation list contains movies that the system could actually recommend to the user.

Movies with multiple genres were then exploded into genre-level rows so that the user's genre preferences could be matched to each movie.

---

## Task 6 — Personalized Recommendation Scoring

Each candidate movie was matched with the user's genre preference profile.

Two scoring approaches were considered.

### Simple Score

For multi-genre movies, the scaled preference scores of the movie's genres were averaged:

```python
Simple Score =
    mean(Preference Score Scaled)
```

This provides a direct measure of how well the movie's genres match the user's learned preferences.

### Final Score

The personalized preference signal was combined with the movie's scaled quality score.

A harmonic-mean-based combination was used:

```python
Final Score =
    2 * Preference Score Scaled * Weighted Rating Scaled
    /
    (
        Preference Score Scaled
        +
        Weighted Rating Scaled
    )
```

This approach rewards movies that perform well on both dimensions:

```text
User Preference
       +
Movie Quality
       |
       v
  Final Score
```

A movie with a high preference score but poor overall quality should not automatically dominate the recommendation list, and vice versa.

After calculating the genre-level scores, the results were aggregated back to the movie level because a movie can belong to multiple genres.

---

## Task 7 — Top-N Recommendation Generation

After movie-level aggregation, the candidate movies were sorted by `Final Score`.

The Top-10 movies were returned as the final recommendation list.

An example Top-10 result for User 1 included:

| Movie                                      | Simple Score | Final Score |
| ------------------------------------------ | -----------: | ----------: |
| Shawshank Redemption, The (1994)           |     0.828052 |    0.905939 |
| Citizen Kane (1941)                        |     0.828052 |    0.871159 |
| Seven Samurai (The Magnificent Seven)      |     0.778953 |    0.860913 |
| Godfather, The (1972)                      |     0.748466 |    0.849493 |
| It's a Wonderful Life (1946)               |     0.828052 |    0.848296 |
| Amadeus (1984)                             |     0.828052 |    0.846766 |
| Bridge on the River Kwai, The (1957)       |     0.787608 |    0.846088 |
| 12 Angry Men (1957)                        |     0.828052 |    0.845687 |
| American Beauty (1999)                     |     0.783651 |    0.841078 |
| Life Is Beautiful (La Vita è bella) (1997) |     0.783651 |    0.836643 |

The resulting recommendations were more diverse than the earlier genre-only ranking because overall movie quality was incorporated into the final score.

---

# Task 8 — Recommendation Evaluation

## Evaluation Setup

The final recommendation pipeline was evaluated using the temporally separated test data.

For each evaluation user:

1. The recommendation function generated the user's Top-10 movies using training data.
2. Relevant movies were identified from the user's test interactions using:

```python
Rating >= 4
```

3. Recommended movies were compared with the user's relevant test movies.
4. Precision@10, Recall@10, and F1@10 were calculated manually.

The evaluation was performed across **5,956 users**.

---

## Precision@10

Precision measures how many of the recommended movies were relevant.

```text
Precision@10 =
Relevant Recommended Movies
/
10
```

The mean Precision@10 was:

```text
0.045064
```

This means that approximately 4.5% of the Top-10 recommendations were relevant on average under the defined evaluation setup.

---

## Recall@10

Recall measures how many of the user's relevant future movies were successfully retrieved in the Top-10 recommendation list.

```text
Recall@10 =
Relevant Recommended Movies
/
All Relevant Test Movies
```

The mean Recall@10 was:

```text
0.028039
```

This indicates that the system retrieved approximately 2.8% of the relevant future test items on average.

---

## F1@10

F1 combines Precision and Recall using their harmonic mean:

```text
F1 =
2 * Precision * Recall
/
(Precision + Recall)
```

The mean F1@10 was:

```text
0.028109
```

The harmonic mean was used because a useful recommendation system should ideally balance both precision and recall rather than optimizing only one of them.

---

## Evaluation Results

| Metric       |     Mean |      Std |   Median |      Min |      Max |
| ------------ | -------: | -------: | -------: | -------: | -------: |
| Precision@10 | 0.045064 | 0.095230 | 0.000000 | 0.000000 | 0.900000 |
| Recall@10    | 0.028039 | 0.073229 | 0.000000 | 0.000000 | 1.000000 |
| F1@10        | 0.028109 | 0.058474 | 0.000000 | 0.000000 | 0.461538 |

The distribution of results shows substantial variation between users.

More than half of the evaluated users had zero Precision@10, Recall@10, and F1@10, while some users achieved considerably higher scores.

---

# Key Findings

### 1. The recommendation pipeline is functional

The complete pipeline successfully progressed from training data to:

```text
Train/Test Split
      ↓
User Genre Preferences
      ↓
Movie Quality
      ↓
Candidate Generation
      ↓
Personalized Scoring
      ↓
Top-10 Recommendations
      ↓
Evaluation
```

The recommendation function was successfully applied to multiple users.

### 2. Combining personalization and movie quality improved recommendation diversity

Using only genre preference produced many recommendations with identical scores, particularly for users with a strong preference for one dominant genre.

Adding movie-level quality created a more differentiated ranking and allowed highly rated, well-supported movies to compete with genre preference.

### 3. Evaluation performance was low overall

The mean evaluation metrics were:

```text
Precision@10 = 0.045064
Recall@10    = 0.028039
F1@10        = 0.028109
```

Therefore, the current recommendation approach has limited predictive performance under the current evaluation setup.

### 4. Performance varies substantially between users

The maximum observed scores were:

```text
Precision@10 = 0.90
Recall@10    = 1.00
F1@10        = 0.461538
```

This suggests that the recommendation approach can work considerably better for some users than others.

### 5. The evaluation problem is difficult under sparse interactions

The dataset contains relatively sparse user-item interactions, while the evaluation requires the system to retrieve future relevant movies within only ten recommendations.

Therefore, the low scores should be interpreted in the context of the evaluation setup and data sparsity rather than being treated as definitive evidence that the recommendation strategy is universally ineffective.

### 6. A single user's results are not sufficient

The evaluation was therefore performed across thousands of users rather than relying only on User 1.

This provides a substantially stronger basis for judging the overall behavior of the recommendation pipeline.

---

# Engineering Decisions

## Temporal Evaluation

A chronological split was selected instead of a random split so that the evaluation better represents the real recommendation scenario of predicting future preferences from past behavior.

## Relevance Threshold

A rating of 4 or higher was selected as the relevance threshold.

```python
relevance_threshold = 4
```

## Confidence-Weighted Genre Preference

Genre preferences were shrunk toward the global training mean to reduce instability caused by genres with few observations.

## Movie Quality Weighting

Movie quality incorporated both average rating and rating count so that movies with limited evidence would not receive disproportionately strong scores.

## Candidate Filtering

Movies already rated by a user in the training data were excluded from that user's recommendation candidates.

## Personalization and Quality Combination

The final score combined the user's genre preference with movie quality using a harmonic-mean-based formulation.

This was chosen to reward movies that perform well on both dimensions.

## Manual Metric Implementation

Precision@10, Recall@10, and F1@10 were implemented manually instead of relying on a metric library.

This provided a direct understanding of how each evaluation metric was calculated.

---

# Sprint Retrospective

## What Went Well

* The recommendation pipeline was successfully built end-to-end.
* The workflow separated training information from future test interactions.
* User preference and movie quality were modeled as separate signals.
* Candidate generation successfully excluded movies already rated by users.
* The final scoring approach produced more differentiated recommendations than genre preference alone.
* The evaluation process was implemented manually and applied across 5,956 users.

## What Was Challenging

* Designing a stable user genre preference score when some genres had very few ratings.
* Separating user-specific preference from general movie quality.
* Handling movies with multiple genres during scoring.
* Understanding the distinction between recommendation scores, predictions, and ground-truth test interactions.
* Dealing with very low Precision, Recall, and F1 for many users.

## What We Learned

* Recommendation quality depends on more than simply identifying a user's favorite genres.
* Rating count provides important information about the reliability of an average rating.
* Personalization and overall item quality represent different recommendation signals and can be combined.
* Evaluation must be performed using unseen future interactions to provide a meaningful test.
* Precision and Recall measure different aspects of recommendation quality and should be interpreted together.
* Performance should be evaluated across many users rather than using a single user's results.

## What Could Be Improved

* Incorporate stronger user-item similarity signals.
* Explore collaborative filtering approaches.
* Introduce additional movie and user features.
* Compare the current approach against simple popularity and rating-based baselines.
* Evaluate additional values of K instead of only Top-10.
* Investigate why some users receive substantially better recommendations than others.
* Explore alternative candidate-generation strategies to improve Recall.

## Next Sprint

The next stage should focus on establishing stronger baselines and exploring more advanced recommendation signals.

Potential directions include:

* Popularity-based recommendation baseline.
* Rating-based baseline.
* User-user similarity.
* Item-item similarity.
* Collaborative filtering.
* More systematic comparison of recommendation strategies.
* Evaluation across multiple Top-K values.
