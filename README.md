# TMDB Movie Data Pipeline

A small ETL pipeline that pulls popular movie data from the TMDB API, enriches each movie with detail data, and loads the results into SQLite.

The notebook is organized around a simple flow:

```text
TMDB API
   ↓
Extract popular movies
   ↓
Enrich runtime + genre data
   ↓
Load into SQLite
   ↓
Query and verify the result
```

## What it does

- Authenticates with a TMDB API access token entered at runtime
- Retrieves five pages of popular movies
- Checks HTTP responses with `raise_for_status()`
- Fetches movie details to collect runtime and full genre names
- Stores the transformed records in `movies.db`
- Verifies the load with a row count and sample query

## Tech stack

- Python
- Requests
- SQLite
- Jupyter Notebook
- TMDB API

## Getting started

### 1. Clone the repository

```bash
git clone https://github.com/junmw/movie-data-pipeline.git
cd movie-data-pipeline
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Open the notebook

```bash
jupyter notebook pipeline.ipynb
```

Run the cells in order. When prompted, enter your TMDB API access token.

The token is captured with `getpass`, so it is not written into the notebook.

## Database schema

The pipeline creates a `movies` table with:

| Column | Description |
| --- | --- |
| `tmdb_id` | TMDB movie ID and primary key |
| `title` | Movie title |
| `runtime_minutes` | Runtime in minutes |
| `release_date` | Release date from TMDB |
| `genres` | Comma-separated genre names |

## Notes

The popular-movies endpoint does not include runtime or full genre objects, so the pipeline follows each movie ID with a detail request before loading the database.

The current load strategy refreshes the table on each run. A future version could replace that with incremental upserts.
