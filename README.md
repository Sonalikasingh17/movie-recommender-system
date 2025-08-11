# Movie Recommendation System using TMDB Dataset
---
##  Overview
This project builds a **Content-Based Movie Recommendation System** using the [TMDB 5000 Movie Dataset](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata).
It processes and merges the movies and credits datasets, cleans the relevant features, and creates a combined `tags` column for recommending movies based on similarity in plot keywords, genres, cast, and crew.

##  Dataset
The dataset consists of two files:
- `tmdb_5000_movies.csv` — Movie details including title, genres, keywords, overview, budget, and more.
- `tmdb_5000_credits.csv` — Cast and crew details for each movie.

These CSVs are loaded from the Kaggle input directory `/kaggle/input/tmdb-movie-metadata/`.

##  Steps Performed in Notebook
1. **Importing Libraries**
   - `numpy`, `pandas`, and `os` for data handling.

2. **Loading Data**
   - Read `tmdb_5000_movies.csv` and `tmdb_5000_credits.csv` into Pandas DataFrames.

3. **Data Inspection**
   - Preview datasets.
   - Identify relevant columns: `movie_id`, `title`, `overview`, `genres`, `keywords`, `cast`, `crew`.

4. **Data Cleaning & Transformation**
   - Convert JSON-like lists in columns (`genres`, `keywords`, `cast`, `crew`) into plain text lists.
   - Extract top actors/actresses from `cast`.
   - Retrieve directors from `crew`.
   - Remove spaces from multi-word entries for uniformity.

5. **Merging Datasets**
   - Merge movies and credits datasets on the `title` column.

6. **Creating the `tags` Column**
   - Combine `overview`, genres, keywords, cast, and crew into a single string for each movie.
   - This will be used for computing similarity scores in a recommendation engine.

7. **Output**
   - Final DataFrame with:
     - `movie_id`
     - `title`
     - `tags`

## Goal
Prepare a clean dataset for a **Content-Based Recommendation System** that suggests movies similar to a given one based on storyline, genre, keywords, and cast/crew.

##  How to Run
1. Download the TMDB dataset from Kaggle.
2. Place the files in `/kaggle/input/tmdb-movie-metadata/` or update the paths in the notebook.
3. Open and run `notebook.ipynb` in Jupyter Notebook or Kaggle.

---

##  Building a Cosine Similarity Recommendation Engine

This section explains how to convert your cleaned `tags` column into vectors using **CountVectorizer**, then compute cosine similarity to recommend similar movies.

### Step 1: Vectorizing the Tags
Use `sklearn`’s `CountVectorizer` to transform the tags into a matrix of token counts—this creates a numeric representation suitable for similarity computation.

- `max_features=5000`: limits the vocabulary size.
- `stop_words='english'`: removes common English stopwords.

### Step 2: Computing Cosine Similarity
Use `sklearn`’s `cosine_similarity` function to measure similarity between movies based on their tag vectors.

- `similarity[i][j]` gives how similar movie `i` is to movie `j` (ranges from 0 to 1).

### Step 3: Recommending Similar Movies
Build a function that takes a movie title and returns the top N most similar movies:

- Replace `df` with the name of your DataFrame.
- Call `recommend("Avatar")` to get 5 recommendations similar to Avatar.

### Step 4: Saving and Loading
Save processed DataFrame and similarity matrix for quick future use.
```yaml
import pickle

pickle.dump(df, open('movies.pkl', 'wb'))
pickle.dump(similarity, open('similarity.pkl', 'wb'))
```

To reload:
```bash
df = pickle.load(open('movies.pkl', 'rb'))
similarity = pickle.load(open('similarity.pkl', 'rb'))
```

### Author
Sonalika Singh

IIT Madras

