# Engineering Decisions

## Sprint 01

### 1. Monthly Temporal Aggregation

**Decision:**\
Use monthly aggregation for temporal analysis.

**Why:**\
Monthly aggregation provides a balanced level of temporal detail. Daily
aggregation would introduce more noise, while yearly aggregation would
hide important temporal patterns.

**Alternative:**\
Quarterly aggregation.

------------------------------------------------------------------------

### 2. Movie Dataset Encoding

**Decision:**\
Use `latin-1` encoding when loading `movies.dat`.

**Why:**\
The movie titles contain characters outside the standard English
character set. Using `latin-1` allowed the dataset to be loaded
successfully without the Unicode decoding error encountered with UTF-8.

**Alternative:**\
Another compatible character encoding identified through further
investigation.

------------------------------------------------------------------------

### 3. Loading Headerless Raw Files

**Decision:**\
Use `header=None` and explicitly provide column names.

**Why:**\
The raw `.dat` files do not contain column headers. Therefore, pandas
must be informed that no header exists, while the column names are
provided explicitly based on the dataset documentation.

**Alternative:**\
Preprocess the raw files and add the column names before loading them.

------------------------------------------------------------------------

### 4. Interaction Counting with `size()`

**Decision:**\
Use `size()` to count interaction records within groups.

**Why:**\
`size()` counts rows within each group, including rows containing
missing values. At this stage, potentially informative records should
not be implicitly excluded before completing the data-quality
investigation.

**Alternative:**\
Use `count()`, which excludes missing values from the count of the
selected column.

------------------------------------------------------------------------

### 5. Rating Distribution Visualization

**Decision:**\
Use a bar plot for the rating distribution.

**Why:**\
Ratings are discrete values. A bar plot makes it straightforward to
compare the frequency of each rating value.

**Alternative:**\
Pie chart.

------------------------------------------------------------------------

### 6. User and Movie Activity Visualization

**Decision:**\
Use histograms to analyze user activity and movie interaction counts.

**Why:**\
The activity counts contain many distinct numerical values. Histograms
help reveal their range, concentration, and strongly right-skewed
distributions.

**Alternative:**\
Box plot.

------------------------------------------------------------------------

### 7. Temporal Interaction Visualization

**Decision:**\
Use a line plot for monthly interaction volume.

**Why:**\
Time is ordered and the monthly observations form a temporal sequence. A
line plot makes changes, peaks, and declines over time easier to
observe.

**Alternative:**\
Histogram or box plot.

------------------------------------------------------------------------

### 8. Timestamp Transformation

**Decision:**\
Convert Unix timestamps into a datetime representation and derive a
monthly temporal representation.

**Why:**\
Raw Unix timestamps are difficult to interpret directly. Converting them
into a meaningful temporal representation allows interaction activity to
be analyzed over time.

**Alternative:**\
Keep the original Unix timestamp and perform analysis directly on the
numeric values.

------------------------------------------------------------------------

### 9. UserID and MovieID Treatment

**Decision:**\
Treat UserID and MovieID primarily as identifiers rather than meaningful
numerical features.

**Why:**\
The numerical value of an ID does not represent an inherent relationship
between users or movies. For example, UserID 3600 is not inherently more
similar to UserID 10 than to another user because of the numerical
values of their IDs.

The IDs remain essential for identifying users and movies and for
joining the datasets.

**Alternative:**\
Use the IDs only as keys for data integration while excluding them from
numerical feature representations used for similarity or prediction.

---

## Sprint 02 Decisions

### Interaction Sparsity

The user–movie interaction matrix is highly sparse, with approximately 4.47% density and 95.53% sparsity.

This will be considered when selecting and designing recommendation methods.

### Movie Rating Evaluation

Average movie rating should not be interpreted independently from rating count because movies with very few ratings can produce unstable averages.

### Genre Representation

Because movies can belong to multiple genres, genre information will be represented as multi-label data rather than assigning a single genre to each movie.

### User Activity

User activity level will be considered when interpreting observed preferences, since users with substantially different numbers of ratings provide different amounts of behavioral evidence.

### Identifiers

UserID and MovieID will primarily be treated as identifiers rather than descriptive numerical features.

---

## Sprint 03 — Recommendation System Decisions

### ED-03-01 — Temporal Train/Test Split

**Decision:** Use a chronological 80/20 split for each user's rating history.

**Rationale:** Recommendation systems should be evaluated by predicting future interactions from past interactions. A temporal split better represents this scenario than a random split.

---

### ED-03-02 — Relevance Threshold

**Decision:** Define a test rating of 4 or higher as relevant.

**Rationale:** The evaluation requires a binary distinction between relevant and non-relevant future interactions.

---

### ED-03-03 — Confidence-Weighted Genre Preference

**Decision:** Shrink user-genre mean ratings toward the global training mean using the number of observations.

**Rationale:** Genre preferences based on very few ratings are less reliable than preferences supported by many observations.

---

### ED-03-04 — Confidence-Weighted Movie Quality

**Decision:** Combine movie average rating with rating count when estimating movie quality.

**Rationale:** A high average rating supported by very few ratings should not automatically dominate a score supported by substantially more evidence.

---

### ED-03-05 — Candidate Filtering

**Decision:** Exclude movies already rated by the user in the training data.

**Rationale:** Previously rated movies should not appear as new recommendations.

---

### ED-03-06 — Combining Personalization and Quality

**Decision:** Combine scaled user preference and scaled movie quality using a harmonic-mean-based final score.

**Rationale:** The recommendation should perform well on both personalization and overall movie quality rather than relying entirely on either signal.

---

### ED-03-07 — Manual Evaluation Metrics

**Decision:** Implement Precision@10, Recall@10, and F1@10 manually.

**Rationale:** Manual implementation provides direct visibility into the definitions and calculations of the evaluation metrics and avoids treating the metrics as black-box library functions.

