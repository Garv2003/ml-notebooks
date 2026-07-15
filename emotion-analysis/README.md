# Emotion Analysis

A text emotion classifier that labels input text as **Fear**, **Anger**, or **Joy**, with a Flask web
app for interactive predictions.

## Overview

- **Task:** multi-class text classification into 3 emotions (Fear / Anger / Joy).
- **Model:** a Keras/TensorFlow text model (`Emotion_Analysis_Model.keras`) with a fitted Keras
  tokenizer (`Emotion_Analysis_Tokenizer.pkl`).
- **Web app:** a Flask form that cleans + tokenizes the input, pads it, runs the model, and shows the
  predicted emotion.

## Dataset

`Model/Emotion Classification DataSet.csv` — labelled text samples across the three emotion classes.

## Preprocessing & model

`Web App/app.py` cleans text before inference: strip HTML tags, remove non-alphanumeric chars,
lowercase, remove NLTK English stopwords, and Snowball-stem. The cleaned text is converted to a
sequence with the saved tokenizer and padded to length 100 (`pad_sequences`). The model outputs a
3-way distribution mapped as `{0: Fear, 1: Anger, 2: Joy}` via `argmax`.

The trained model and tokenizer live under `Web App/model/` (not committed — regenerate from training).

## Web app

Flask, served via `waitress` on `:8080`:
- `GET /` — renders `templates/index.html` (text input form).
- `POST /predict` — takes `user_input`, preprocesses, predicts, returns the emotion as an HTML `<p>`.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place model/Emotion_Analysis_Model.keras and model/Emotion_Analysis_Tokenizer.pkl
python app.py            # http://localhost:8080
```

First run downloads NLTK `punkt` and `stopwords` data automatically.

## Project structure

```
emotion-analysis/
├── Model/Emotion Classification DataSet.csv
└── Web App/
    ├── app.py                 # Flask inference server (waitress :8080)
    ├── requirements.txt       # tensorflow, Flask, nltk, waitress
    ├── templates/index.html
    ├── Dockerfile
    └── package.json / tailwind.config.js
```
