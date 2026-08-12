# Sprint 02 — Exploratory Analysis & Recommendation Insights

## Objectives

The objective of Sprint 02 was to move beyond basic dataset understanding and investigate the behavioral structure of the recommendation dataset.

The analysis focused on three main entities:

- Users
- Movies
- User–Movie Interactions

The sprint aimed to identify behavioral patterns, interaction characteristics, potential biases, useful features, and challenges that may influence the design of the recommendation system.

---

# Task 1 — User Analysis

## User Demographics

The user dataset was examined across gender, age, and occupation.

The user population is not evenly distributed across these demographic categories. The dataset contains a larger proportion of male users, while the largest age group is the 25–34 range.

Occupation is also unevenly distributed, with some occupation categories having substantially more users than others.

These demographic differences may provide useful signals for future recommendation strategies, particularly if user preferences differ across demographic groups.

## User Activity

User activity was analyzed using the number of ratings provided by each user.

The number of ratings varies substantially between users. Some users have relatively few ratings, while a smaller group of highly active users contributes a large number of interactions.

The distribution is strongly right-skewed, indicating that user activity is highly uneven.

User-level average ratings and rating standard deviation were also calculated to examine differences in rating behavior.

This showed that users differ not only in how much they interact with the system, but also in how consistently and generously they rate movies.

---

# Task 2 — User Rating Behavior

Three main user-level statistics were examined:

- Number of ratings per user
- Average rating per user
- Rating standard deviation per user

The number of ratings provides a measure of user activity, while the average rating provides information about the user's general rating tendency.

Rating standard deviation was used as an indicator of rating consistency.

A user with a large number of ratings provides substantially more behavioral evidence than a user with only a small number of ratings. Therefore, user activity should be considered when interpreting observed preferences.

The analysis also showed that users do not all use the rating scale in the same way. Some users have relatively low variation in their ratings, while others use a wider range of the rating scale.

---

# Task 3 — Movie Analysis

Movie-level analysis focused on:

- Number of ratings per movie
- Average rating per movie
- Movie genres
- Movie release year

## Movie Popularity

The number of ratings per movie is highly uneven.

A relatively small number of movies receive a large number of ratings, while many movies have considerably fewer interactions.

The distribution therefore shows a strong popularity pattern.

This is important for recommendation because a movie with a high average rating but very few ratings should not necessarily be treated as more reliable than a movie with a similar average rating supported by a much larger number of interactions.

## Average Movie Rating

Average ratings were calculated for each movie.

The analysis confirmed that average rating alone does not provide enough information to evaluate a movie reliably because the number of observations behind each average can vary substantially.

Therefore, future recommendation logic should consider rating count together with average rating.

## Movie Genres

Movies may belong to multiple genres.

The dataset contains 18 distinct genres. Genre information was therefore treated as multi-label information rather than assigning a single genre to each movie.

## Movie Release Year

The release year was extracted from the movie title using the four-digit year contained in the title.

This creates an additional movie-level feature that may be useful for future recommendation analysis.

---

# Task 4 — Genre Analysis

Genre information was split into individual genre labels and analyzed independently.

The dataset contains 18 distinct genres.

Drama and Comedy represent the largest share of genre occurrences in the movie dataset.

When interactions are considered, Comedy and Drama also account for the largest number of ratings, followed by Action and Thriller.

However, interaction volume and average rating do not produce the same ranking.

Genres such as Film-Noir, Documentary, and War have relatively high average ratings, while Horror has the lowest average rating among the observed genres.

This demonstrates that genre popularity and genre rating behavior are different concepts and should be analyzed separately.

---

# Task 5 — User–Movie Interaction Analysis

The ratings dataset was joined with movie metadata to create a combined user–movie dataset.

This allowed user interactions to be analyzed together with movie information such as title and genre.

Because movies can belong to multiple genres, the genre field was split and exploded so that each user–movie–genre relationship could be examined separately.

The resulting data was used to calculate:

- Number of interactions per user and genre
- Average rating per user and genre
- Total interactions per genre
- Average rating per genre

This analysis provides a more detailed view of user preferences than simply examining ratings or movies independently.

It allows the system to investigate not only how active a user is, but also which genres they interact with and how they rate those genres.

---

# Task 6 — Interaction Matrix Analysis

The user–movie interaction space was analyzed in terms of density and sparsity.

The number of observed unique user–movie pairs was compared with the total number of theoretically possible user–movie pairs.

The resulting interaction density was approximately:

- **4.47% Density**
- **95.53% Sparsity**

This means that most possible user–movie combinations do not have an observed interaction.

High sparsity is an important characteristic of the dataset because the recommendation system must infer user preferences from a relatively small subset of all possible interactions.

This also makes it more difficult to produce reliable recommendations for users or movies with limited interaction history.

---

# Task 7 — Bias & Data Quality

Several data-quality and recommendation-related patterns were investigated.

## Popularity Bias

The dataset shows a clear popularity bias.

A relatively small number of movies account for a large proportion of user interactions, while many movies receive relatively few ratings.

This may cause a recommendation system based primarily on interaction frequency to favor already-popular movies.

## Rating Bias

There is some evidence of a tendency toward middle-to-higher ratings.

The overall average rating is approximately 3.7, suggesting that ratings are not evenly distributed across the full rating scale.

This should be considered when interpreting movie and genre averages.

## Interaction Reliability

The number of observations behind a statistic is important.

A movie with a high average rating based on very few ratings should not automatically be considered better than another movie with a similar average supported by substantially more interactions.

The same principle applies to genre-level and user-level statistics.

## Data Quality Observations

The dataset does not show an obvious structural problem that prevents analysis. However, its highly uneven interaction distribution and sparsity create important challenges for recommendation modeling.

---

# Task 8 — Engineering Analysis

The main engineering conclusions from Sprint 02 are summarized below.

### 1. User Behavior

A relatively small proportion of users account for a large number of interactions, while most users have considerably fewer ratings.

### 2. Movie Behavior

A relatively small number of movies receive a large share of ratings, while many movies have relatively few interactions.

### 3. Popularity Bias

The dataset contains a clear popularity bias because a small number of movies receive a disproportionate share of interactions.

### 4. Rating Bias

There is some evidence of bias toward middle-to-higher ratings, with the overall average rating around 3.7.

### 5. Cold Start

Cold-start is a relevant challenge for the recommendation system because new users or new movies may have little or no interaction history.

The high sparsity of the dataset further increases the difficulty of learning reliable user–movie preferences.

### 6. Potentially Useful Features

Potentially useful recommendation features identified during this sprint include:

- Movie genres
- Movie release year
- User demographics
- User activity level
- User rating statistics
- Movie rating statistics
- Selected time-based features derived from timestamps

These features have been identified as potential signals but have not yet been evaluated as predictive features.

### 7. Identifier Features

`UserID` and `MovieID` primarily serve as identifiers rather than descriptive numerical features.

They are necessary for representing users and movies and connecting interactions, but their numeric values do not themselves represent meaningful user or movie characteristics.

---

# Visualization

The following visualizations were created to validate the patterns identified during the numerical analysis:

- User activity distribution
- Movie popularity distribution
- Genre interaction count
- Average rating by genre
- Rating distribution by genre

The visualizations confirm the strong skew in user and movie activity and provide a clearer comparison of interaction volume and rating behavior across genres.

The rating distribution by genre also demonstrates that genres differ not only in average rating but also in the distribution of ratings they receive.

---

# Key Findings

## User Behavior

User activity is highly uneven. A relatively small group of users accounts for a large number of ratings, while most users have considerably fewer interactions.

This means that user activity level should be considered when interpreting the strength of observed preferences.

## Movie Popularity

Movie popularity is highly skewed.

A small number of movies receive a large proportion of all ratings, while many movies have relatively few interactions.

Therefore, rating count should be considered alongside average rating when evaluating movies.

## Genre Behavior

The dataset contains 18 distinct genres.

Drama and Comedy are among the most represented genres, while genres such as Film-Noir and Documentary have much lower interaction volumes.

Average ratings also vary across genres. Film-Noir, Documentary, and War have relatively high average ratings, while Horror has the lowest average rating among the observed genres.

The distribution of ratings across genres also shows that different genres have different rating patterns.

## Interaction Sparsity

The user–movie interaction space is highly sparse.

The observed interaction density is approximately 4.47%, resulting in approximately 95.53% sparsity.

Most possible user–movie pairs therefore have no recorded interaction, making preference inference more difficult.

## Bias and Data Quality

The analysis identified a clear popularity bias, with a small number of movies receiving a disproportionate share of interactions.

There is also some evidence of rating bias toward the middle-to-higher end of the rating scale.

These patterns should be considered when designing recommendation strategies because relying only on raw popularity or average ratings may produce biased recommendations.

## Recommendation Implications

The analysis suggests that genre information, user activity, movie rating statistics, release year, and selected temporal features may provide useful signals for future recommendation strategies.

At the same time, UserID and MovieID should primarily be treated as identifiers rather than descriptive features.

Cold-start and sparse interactions remain important challenges for the next stages of the system.

---

# Sprint Retrospective

## What Went Well

- The three main entities of the recommendation system — users, movies, and interactions — were clearly separated and analyzed.
- User activity and movie popularity were examined independently before analyzing their interaction structure.
- User-level rating statistics provided additional information beyond simple interaction counts.
- Movie genres were transformed into a multi-label representation to support genre-level analysis.
- User–movie interactions were combined with movie metadata to enable more meaningful interaction analysis.
- Interaction density and sparsity were calculated to quantify the difficulty of the recommendation problem.
- Visualization was used to validate the patterns identified through numerical analysis.
- The sprint identified important recommendation-system challenges including popularity bias, rating bias, sparsity, and cold-start.

## What Could Be Improved

- Some analyses initially required additional clarification regarding the distinction between movie-level statistics and user–movie interaction statistics.
- Statistics based on very small numbers of observations may not provide reliable conclusions.
- Average ratings should not be interpreted independently from rating counts.
- Genre analysis is more complex because a single movie can belong to multiple genres.
- Several potentially useful features were identified but have not yet been evaluated in an actual recommendation model.

## Lessons Learned

- Exploratory data analysis in a recommendation system is focused on relationships between users, items, and interactions rather than simply identifying classes or target distributions.
- Popularity, rating behavior, activity, sparsity, and cold-start are different but interconnected aspects of recommendation-system design.
- Data distributions should be understood before selecting a recommendation strategy.
- A high average rating does not necessarily indicate a reliable or popular movie.
- Sparse interaction data requires careful interpretation and will strongly influence the choice of recommendation approach.
- Genre information can provide a useful representation of user preferences because movies may contain multiple genres.

## Next Sprint

The next sprint will move from exploratory analysis toward designing and implementing the initial recommendation logic.

The recommendation approach will take into account the observed sparsity, popularity bias, differences in user activity, and the available movie and genre features.