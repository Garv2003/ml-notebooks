# Sentimental Analysis (Twitter & Reddit)

A three-class sentiment classifier (**Negative** / **Neutral** / **Positive**) trained on Twitter and
Reddit text, with a training notebook and a Flask web app.

## Overview

- **Task:** 3-class sentiment classification of social-media text.
- **Model:** a Keras/TensorFlow text model (`Sentimental_Analysis.keras`).
- **Web app:** a Flask form that cleans the input, runs the model, and shows the sentiment.

## Dataset

Twitter and Reddit sentiment data, used in `Model/Twitter_and_Reddit_Sentimental_analysis.ipynb` to
train the model. Classes map as `{0: Negative, 1: Neutral, 2: Positive}`.

## Preprocessing & model

`Web App/app.py` cleans text (strip HTML, remove non-alphanumeric chars, lowercase, remove NLTK
stopwords, Snowball-stem), converts it to a padded sequence (length 100), runs `model.predict`, and
maps `argmax` to the sentiment class.

## Web app

Flask, served via `waitress` on `:8080`:
- `GET /` — renders `templates/index.html`.
- `POST /predict` — takes `user_input`, preprocesses, predicts, returns the sentiment as an HTML `<p>`.

## Known issue

`app.py` instantiates a **fresh, unfitted** `Tokenizer()` inside `/predict` rather than loading the
tokenizer that was fitted during training. An unfitted tokenizer produces empty sequences, so the
model receives all-padding input and predictions are not meaningful. To fix, save the fitted tokenizer
from the notebook (e.g. as a `.pkl`) and load it at startup — as the other sentiment projects here do
(`product-review-analysis`, `emotion-analysis`). Left as-is and documented rather than silently changed.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place model/Sentimental_Analysis.keras (and a fitted tokenizer — see Known issue)
python app.py            # http://localhost:8080
```

## Project structure

```
sentimental-analysis/
├── Model/Twitter_and_Reddit_Sentimental_analysis.ipynb
└── Web App/
    ├── app.py                 # Flask inference server (waitress :8080)
    ├── requirements.txt       # tensorflow, Flask, nltk, waitress
    ├── templates/index.html
    ├── Dockerfile
    └── package.json / tailwind.config.js
```
