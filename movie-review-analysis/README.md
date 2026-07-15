# Movie Review Analysis

A binary sentiment classifier for movie reviews (**Positive** / **Negative**), with a Flask web app
for interactive predictions.

## Overview

- **Task:** binary sentiment classification of a movie-review text.
- **Model:** a Keras/TensorFlow text model (`Movies_Review_Analysis_Model.keras`) with a fitted Keras
  tokenizer (`Movies_Review_Analysis.pkl`).
- **Web app:** a Flask form that cleans + tokenizes the input, pads it, runs the model, and shows
  Positive/Negative.

## Preprocessing & model

`Web App/app.py` preprocesses text: strip HTML, remove non-alphanumeric chars, lowercase, remove NLTK
stopwords, Snowball-stem. The text is tokenized with the saved tokenizer and padded to length 100.
The model outputs a single sigmoid score; the app labels it **Positive** if `> 0.5`, else **Negative**.

The trained model and tokenizer live under `Web App/model/` (not committed).

## Web app

Flask, served via `waitress` on `:8080`:
- `GET /` — renders `templates/index.html` (review input form).
- `POST /predict` — takes `user_input`, preprocesses, predicts, returns the sentiment as an HTML `<p>`.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place model/Movies_Review_Analysis_Model.keras and model/Movies_Review_Analysis.pkl
python app.py            # http://localhost:8080
```

First run downloads NLTK `punkt` and `stopwords`.

## Project structure

```
movie-review-analysis/
└── Web App/
    ├── app.py                 # Flask inference server (waitress :8080)
    ├── requirements.txt       # tensorflow, Flask, nltk, waitress
    ├── templates/index.html
    ├── Dockerfile
    └── package.json / tailwind.config.js
```
