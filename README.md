# 🎬 Movie Recommender System

A content-based movie recommendation system built on the TMDB 5000 dataset that suggests the 5 most similar movies to any title you pick. It uses text vectorization and cosine similarity, and is served through a simple Streamlit web app.

---

## 📌 Overview

The system recommends movies by comparing their **content** — plot overview, genres, keywords, top 3 cast members, and director. Each movie is converted into a "tags" text, vectorized, and compared with every other movie using cosine similarity. The closest matches are returned as recommendations.

## ✨ Features

- Content-based filtering (no user ratings needed)
- Combines overview, genres, keywords, cast and director into one feature set
- Text preprocessing with stemming (Porter Stemmer) and stop-word removal
- Bag-of-Words vectorization (5000 features) + cosine similarity
- Interactive Streamlit UI: choose a movie, click **Recommend**, get 5 suggestions

## 🧠 How It Works

1. **Load & merge** `tmdb_5000_movies.csv` and `tmdb_5000_credits.csv` on movie title.
2. **Select columns:** `movie_id, title, overview, genres, keywords, cast, crew`.
3. **Clean data:** drop null values, parse JSON-like columns with `ast.literal_eval`, keep top 3 cast and the director.
4. **Build tags:** remove spaces in names (e.g. `Sam Worthington` → `SamWorthington`) and merge all features into a single `tags` column.
5. **Preprocess:** lowercase + Porter stemming (`loving`, `loved` → `love`).
6. **Vectorize:** `CountVectorizer(max_features=5000, stop_words='english')`.
7. **Similarity:** compute a cosine similarity matrix across all movies.
8. **Recommend:** sort similarity scores for the chosen movie and return the top 5.
9. **Save** `movies.pkl` and `similarity.pkl` for the app to load.

## 🛠️ Tech Stack

- **Language:** Python
- **Libraries:** Pandas, NumPy, NLTK, scikit-learn, Streamlit
- **Dataset:** [TMDB 5000 Movies & Credits](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)

## 📁 Project Structure

```
Movies-Recommender-System/
├── app.py                          # Streamlit web app
├── movie-recommender-system.ipynb  # Data processing, model building, experiments
├── tmdb_5000_movies.csv            # Dataset
├── tmdb_5000_credits.csv           # Dataset
├── movies.pkl                      # Processed movies dataframe
├── similarity.pkl                  # Cosine similarity matrix (generated, ~185 MB)
├── requirements.txt
└── README.md
```

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/Movies-Recommender-System.git
cd Movies-Recommender-System
```

### 2. Create a virtual environment and install dependencies
```bash
python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # macOS / Linux

pip install -r requirements.txt
```

### 3. Generate the model files
`similarity.pkl` is too large for a normal GitHub upload, so generate it by running all cells in
`movie-recommender-system.ipynb`. This creates `movies.pkl` and `similarity.pkl`.

### 4. Run the app
```bash
streamlit run app.py
```
Then open the local URL shown in the terminal (usually http://localhost:8501).

## 💡 Example

Select **Avatar** and click **Recommend** → the app returns the five movies whose plot, genre, cast, and director are most similar to Avatar.

## 🔮 Future Improvements

- Show movie posters using the TMDB API
- Add hybrid filtering (content + user ratings)
- Try TF-IDF or sentence embeddings instead of Bag-of-Words
- Deploy online (Streamlit Community Cloud / Hugging Face Spaces)

## 📄 License

This project is for educational purposes. Dataset credit: TMDB / Kaggle.
