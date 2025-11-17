# Content_based_movie_recommendation_system
# Movie Recommender System

**Simple content-based movie recommender with a Streamlit UI**  
This repository contains:
- `app.py` — Streamlit web app for selecting a movie and showing 5 recommendations with posters.
- `Movie_recommender_system.ipynb` — Jupyter notebook with data exploration, preprocessing and model-building code.
- `datasets/` — CSV or other files used by the notebook and app (movie metadata, links, posters, etc).
- `requirements.txt` — Python dependencies (suggested).

---

## Short description
This project is a content-based movie recommender. Given a selected movie, it finds 5 similar movies using features extracted from the dataset (genres, title, description, cast, crew, keywords, etc.), ranks them by similarity, then displays movie titles and posters in a Streamlit interface.

---

## Features
- Content-based recommendations (no user history required).
- Uses text features (genres, overviews, cast, crew, keywords) to compute similarity.
- Fetches and displays movie posters (TMDB image links recommended).
- Simple, responsive web UI built with Streamlit.

---

## How the system works — step by step

1. **Load dataset**
   - The notebook and `app.py` load movie metadata (CSV files). Typical columns: `movie_id`, `title`, `overview`, `genres`, `cast`, `crew`, `keywords`, `poster_path` (or full poster URLs).

2. **Preprocess and clean data**
   - Handle missing values (fill with empty strings).
   - Normalize and combine features into a single textual representation for each movie (e.g. combine `genres + cast + crew + overview` into one "metadata" string).
   - Optional: basic text cleaning (lowercase, remove punctuation, tokenization).

3. **Feature extraction**
   - Convert combined text metadata into numeric vectors, commonly using:
     - `CountVectorizer` or `TfidfVectorizer` from `scikit-learn`,
     - or other embeddings.
   - These vectors represent each movie in a high-dimensional feature space.

4. **Similarity computation**
   - Compute pairwise similarity (cosine similarity is typical) between movies.
   - Precompute similarity matrix or compute on-the-fly for the selected movie.

5. **Recommendation function**
   - The `recommend(selected_movie)` function:
     - Finds the index for `selected_movie`.
     - Retrieves similarity scores to all other movies.
     - Sorts movies by score and selects the top N (e.g., 5), excluding the chosen movie.
     - Returns two lists: `recommended_movie_names` and `recommended_movie_posters` (poster URLs).

6. **Poster retrieval**
   - If your dataset stores only `poster_path`, construct full TMDB image URLs:
     ```
     base_url = "https://image.tmdb.org/t/p/w500"
     poster_url = base_url + poster_path
     ```
   - If the dataset already contains full `https://...` URLs, pass them directly to Streamlit.

7. **Streamlit UI (`app.py`)**
   - Loads the prepared movie list and the similarity model/matrix.
   - Provides a selectbox for movies and a "Show Recommendation" button.
   - When pressed, calls `recommend()`, receives 5 names + poster URLs, and displays them in columns using `st.columns()`.

---

## Files in the repository (what to include)
- `app.py` — Streamlit app (the file you shared earlier).
- `Movie_recommender_system.ipynb` — notebook with preprocessing, vectorization, similarity, and helper functions.
- `datasets/`:
  - `movies.csv` — main movie metadata,
  - `links.csv` or `credits.csv` — optional supporting files,
  - `posters.csv` or `poster_paths.csv` — if poster paths are separate.
- `requirements.txt` — dependencies list (example below).
- `.gitignore` — e.g. large datasets, environment files.
- `README.md` — (this file).

---

## Recommended `requirements.txt` (example)
