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
