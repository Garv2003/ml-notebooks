# Sentiment Analysis Evaluation

A binary sentiment classifier (**Positive** / **Negative**) trained and evaluated across several
topical datasets, with a Flask web app for interactive predictions.

## Overview

- **Task:** binary sentiment classification, evaluated across multiple domains.
- **Model:** a Keras/TensorFlow text model (`Sentimental_Analysis_Evaluation.h5`) with a fitted Keras
  tokenizer (`SN_tokenizer.pkl`).
- **Web app:** a Flask form that cleans + tokenizes input, pads it, runs the model, and shows the sentiment.

## Dataset

`Model/Sentiment Analysis Evaluation DataSet/` holds topical CSVs — **Politics**, **Education**,
**Finance**, **Sports** — used to train/evaluate the model in `Model/Sentiment_Analysis_Evaluation.ipynb`
(evaluating how a sentiment model generalizes across domains).

## Preprocessing & model

`Web App/app.py` preprocesses text: strip HTML, remove non-alphanumeric chars, lowercase, remove NLTK
stopwords, Snowball-stem; tokenize with the saved tokenizer and pad to length 100. The model outputs a
sigmoid score; the app labels **Positive** if `> 0.5`, else **Negative**.

The trained model and tokenizer live under `Web App/model/` (not committed — regenerate from the notebook).

## Web app

Flask, served via `waitress` on `:8080`:
- `GET /` — renders `templates/index.html`.
- `POST /predict` — takes `user_input`, preprocesses, predicts, returns the sentiment as an HTML `<p>`.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place model/Sentimental_Analysis_Evaluation.h5 and model/SN_tokenizer.pkl
python app.py            # http://localhost:8080
```

First run downloads NLTK `punkt` and `stopwords`.

## Project structure

```
sentiment-analysis-evaluation/
├── Model/
│   ├── Sentiment_Analysis_Evaluation.ipynb
│   └── Sentiment Analysis Evaluation DataSet/   # Politics, Education, Finance, Sports CSVs
└── Web App/
    ├── app.py                 # Flask inference server (waitress :8080)
    ├── requirements.txt       # tensorflow, Flask, nltk, waitress
    ├── templates/index.html
    ├── Dockerfile
    └── package.json / tailwind.config.js
```
