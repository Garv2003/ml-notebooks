# Product Review Analysis

A three-class sentiment classifier for product reviews (**Negative** / **Neutral** / **Positive**),
with a training notebook and a Flask web app.

## Overview

- **Task:** 3-class sentiment classification of a product-review text.
- **Model:** a Keras/TensorFlow text model (`Product_Review_Analysis.keras`) with a fitted Keras
  tokenizer (`Product_Review_Analysis.pkl`).
- **Web app:** a Flask form that cleans + tokenizes the input, pads it, runs the model, and shows the
  sentiment.

## Dataset

`Model/reviews.csv` — labelled product reviews. Explored and used to train the model in
`Model/ProductRview.ipynb`.

## Preprocessing & model

`Web App/app.py` preprocesses text: strip HTML, remove non-alphanumeric chars, lowercase, remove NLTK
stopwords, Snowball-stem. The text is tokenized with the saved tokenizer and padded to length 100.
The model outputs a 3-way distribution mapped as `{0: Negative, 1: Neutral, 2: Positive}` via `argmax`.

The trained model and tokenizer live under `Web App/model/` (not committed — regenerate from the notebook).

## Web app

Flask, served via `waitress` on `:8080`:
- `GET /` — renders `templates/index.html` (review input form).
- `POST /predict` — takes `user_input`, preprocesses, predicts, returns the sentiment as an HTML `<p>`.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place model/Product_Review_Analysis.keras and model/Product_Review_Analysis.pkl
python app.py            # http://localhost:8080
```

First run downloads NLTK `punkt` and `stopwords`.

## Project structure

```
product-review-analysis/
├── Model/
│   ├── ProductRview.ipynb     # training notebook
│   └── reviews.csv            # labelled dataset
└── Web App/
    ├── app.py                 # Flask inference server (waitress :8080)
    ├── requirements.txt       # tensorflow, Flask, nltk, waitress
    ├── templates/index.html
    ├── Dockerfile
    └── package.json / tailwind.config.js
```
