# AGENTS.md

## Cursor Cloud specific instructions

This repo is a Python data-science project: a set of Jupyter notebooks that perform
sentiment analysis and item-based collaborative filtering on the Amazon Review dataset.
There is no long-running server "application" — the deliverable is the notebooks in
`Logistic Regression /`, `LSTM/`, `Naive Bayes/`, and `Recommender System/`.

### Environment / how to run
- Python dependencies live in a virtualenv at `.venv` (created by the update script) and
  are pinned in `requirements.txt`. Use `.venv/bin/python`, `.venv/bin/pip`,
  `.venv/bin/jupyter`, etc. (or `source .venv/bin/activate`).
- Start the dev app (JupyterLab) with:
  `.venv/bin/jupyter lab --ip=127.0.0.1 --port=8888 --no-browser` (add
  `--ServerApp.token="" --ServerApp.password=""` for token-less local/browser access).
- Execute a notebook headless:
  `.venv/bin/jupyter nbconvert --to notebook --execute "<path>.ipynb"`.

### Important gotchas
- The notebooks require external datasets that are NOT in the repo (e.g.
  `reviewsWithHeader.csv`, `reviews_merged.json`, `amazonReviews.db`), sourced from the
  Amazon Review dataset (http://snap.stanford.edu/data/web-Amazon.html). Data-loading cells
  (`pd.read_csv(...)` / `pd.read_json(...)`) will fail unless you supply these files.
  Committed cell outputs are the original authors' runs, not fresh executions. The import
  cells run fine without data.
- `LSTM/CNN_final.ipynb` targets old library APIs and will NOT import cleanly on the modern
  stack (e.g. `from sklearn.cross_validation import train_test_split` — removed in
  scikit-learn 0.20+; that module is now `sklearn.model_selection`). Treat it as legacy.
  `Logistic Regression /`, `Naive Bayes/`, and `Recommender System/` notebooks import cleanly.
- NLTK corpora (`stopwords`, `wordnet`, `vader_lexicon`, `punkt`, `punkt_tab`,
  `averaged_perceptron_tagger`) are downloaded by the update script into the user's
  `~/nltk_data`. If NLTK raises a `LookupError`, re-download with
  `.venv/bin/python -m nltk.downloader <corpus>`.
- TensorFlow prints benign CPU/oneDNN/"no GPU" info messages on import; these are not errors.
- There are no automated tests, linters, or build steps configured in this repo.
