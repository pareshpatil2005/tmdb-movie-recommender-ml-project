# Movie Recommender System

A content-based movie recommender built with Python and Streamlit. Choose a movie from the list to see five recommendations based on its overview, genres, keywords, cast, and director.

## Project workflow

1. The notebook reads the TMDB movies and credits CSV files from `Dataset/`.
2. It processes the movie metadata, creates tags, and calculates cosine similarity.
3. The last notebook cell saves `movie_list.pkl` and `similarity.pkl` in the project root.
4. The Streamlit app loads those files from the `model/` folder and displays similar movies and their posters.

## Project structure

```text
.
├── Dataset/
│   ├── tmdb_5000_credits.csv
│   └── tmdb_5000_movies.csv
├── model/                 # generated locally; pickle files are not pushed to GitHub
├── app.py
├── notebook.ipynb
├── requirements.txt
└── README.md
```

The pickle files are generated model artifacts. They are excluded from Git because `similarity.pkl` is larger than GitHub's 100 MB per-file limit.

## Run the project

### 1. Clone the repository and install dependencies

```powershell
git clone https://github.com/<YOUR-USERNAME>/<YOUR-REPOSITORY>.git
cd <YOUR-REPOSITORY>
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 2. Generate the model files

Open `notebook.ipynb` from the project root in Jupyter Notebook:

```powershell
jupyter notebook
```

Run the notebook cells in order. The notebook reads:

- `Dataset/tmdb_5000_movies.csv`
- `Dataset/tmdb_5000_credits.csv`

The final cell saves `movie_list.pkl` and `similarity.pkl` in the project root. The app expects them in `model/`, so move them after running the notebook:

```powershell
New-Item -ItemType Directory -Force .\model
Move-Item -Force .\movie_list.pkl .\model\movie_list.pkl
Move-Item -Force .\similarity.pkl .\model\similarity.pkl
```

If both files are already available in `model/`, you can skip this step. Creating the similarity matrix requires several hundred MB of working memory.


``

### 3. Start the Streamlit app

```powershell
python -m streamlit run app.py
```

## Recommendation method

- Movie overviews, genres, keywords, the first three cast members, and directors are combined into text tags.
- `CountVectorizer` represents those tags using up to 5,000 features and removes English stop words.
- Cosine similarity ranks movies by how similar their tags are.
- The app returns the five closest titles, excluding the selected movie.

This is a content-based recommender. It uses movie metadata and does not personalize recommendations based on viewer history or ratings.

## Data and attribution

The project uses the TMDB 5000 movies and credits CSV files. Poster images and movie details are requested from [The Movie Database (TMDB)](https://www.themoviedb.org/). This project is not endorsed or certified by TMDB. Check the original dataset source and its terms before redistributing the data.

## Limitations

- Recommendations depend on the quality and coverage of the available movie metadata.
- Generating the dense similarity matrix requires substantial memory.
- Recommendations are not personalized to individual users.
