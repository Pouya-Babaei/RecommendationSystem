# Recommendation System

## Project Overview

This project aims to build a portfolio-quality movie recommendation
system using user-item interaction data.

The project follows a sprint-based, engineering-focused workflow
designed to resemble a real-world Machine Learning project.

## Project Goal

Build and evaluate a recommendation engine capable of generating
relevant movie recommendations from user-item interaction data.

## Current Status

-   Sprint 01 --- Dataset Understanding & Initial Data Profiling:
    **Completed**
-   Current phase: Dataset analysis and preparation

## Dataset Overview

  Entity                          Rows   Columns
  ------------------------ ----------- ---------
  Users                          6,040         5
  Ratings / Interactions     1,000,209         4
  Movies                         3,883         3

The dataset contains user information, movie metadata, and user-movie
rating interactions.

---

## Dataset Setup

This project uses the **MovieLens 1M** dataset for building and evaluating the recommendation system.

### 1. Download the Dataset

Download the MovieLens 1M dataset from Kaggle website:

https://www.kaggle.com/datasets/odedgolden/movielens-1m-dataset/data

The dataset is provided as a ZIP archive.

### 2. Extract the Dataset

After downloading the archive, extract its contents.

The extracted directory should contain the following files:

```text
movies.dat
ratings.dat
users.dat
```

### 3. Place the Dataset in the Project

Place the extracted dataset directory inside the project's data directory.

The expected structure is:

```text
project-root/
├── data/
│   └── ml-1m/
│       ├── movies.dat
│       ├── ratings.dat
│       └── users.dat
├── notebooks/
├── reports/
└── README.md
```

> **Note:** The dataset itself is not included in this repository. You must download it separately before running the notebooks.

### 4. Dataset Format

The MovieLens 1M dataset contains:

* **Users:** user demographic information
* **Movies:** movie titles and genres
* **Ratings:** user ratings for movies

The rating data contains user–movie interactions with ratings on a **1–5 scale** and timestamps.

### 5. Verify the Setup

Before running the notebooks, make sure the three `.dat` files are available under:

```text
data/ml-1m/
```

The notebooks can then be executed according to the project workflow described below.


---

## Sprint 01 Findings

-   The dataset contains 6,040 users, 3,883 movies, and 1,000,209 rating
    interactions.
-   The observed interaction density is approximately 4.26%, indicating
    a sparse interaction space.
-   Ratings are concentrated toward higher values, with Rating 4 being
    the most frequent.
-   User activity is strongly uneven, with most users having relatively
    few interactions and a smaller number having many interactions.
-   Movie popularity is strongly uneven, with a small number of movies
    receiving a large number of ratings.
-   Interaction volume varies substantially over time, with a pronounced
    peak around late 2000 followed by a considerable decline.
-   UserID and MovieID are treated primarily as identifiers rather than
    meaningful numerical features.

---

### Sprint 03 — Recommendation Logic & Evaluation

**Status:** Completed

Sprint 03 established the first end-to-end personalized recommendation pipeline.

The pipeline now includes:

* User genre preference modeling
* Movie quality estimation
* Candidate generation
* Personalized Top-10 recommendation scoring
* Temporal train/test evaluation
* Precision@10, Recall@10, and F1@10

The system was evaluated across 5,956 users. The mean evaluation results were:

| Metric       |     Mean |
| ------------ | -------: |
| Precision@10 | 0.045064 |
| Recall@10    | 0.028039 |
| F1@10        | 0.028109 |

The results indicate that the initial recommendation approach is functional but has limited overall retrieval performance. Sprint 03 therefore provides a baseline for future improvements using stronger recommendation and collaborative filtering methods.

---

## Repository Structure

``` text
RecommendationSystem/
│
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
├── figures/
├── models/
├── reports/
├── src/
├── configs/
├── README.md
├── CHANGELOG.md
├── PROJECT_JOURNAL.md
├── ENGINEERING_DECISIONS.md
├── TEAM_GUIDE.md
├── requirements.txt
└── Dockerfile
```

## Engineering Workflow

1.  Dataset Understanding
2.  Exploratory Data Analysis
3.  Data Preparation
4.  Recommendation Baselines
5.  Collaborative Filtering
6.  Similarity-Based Recommendation
7.  Matrix Factorization
8.  Evaluation
9.  Error / Recommendation Analysis
10. Model Persistence
11. Deployment Preparation
12. Documentation

The exact workflow may evolve as engineering constraints and findings
emerge during the project.

## Tools

-   Python
-   Pandas
-   NumPy
-   Matplotlib
-   Scikit-learn
-   Git / GitHub
-   Docker
