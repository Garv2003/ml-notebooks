# MNIST Image Classifier

A handwritten-digit classifier (0–9) trained on the MNIST dataset, with a Flask web app that lets you
upload an image of a digit and get the predicted number back.

## Overview

- **Task:** image classification — recognize a single handwritten digit (0–9).
- **Model:** a Keras/TensorFlow CNN trained on MNIST, saved as `mnist_cnn.keras`.
- **Web app:** a Flask service that accepts an uploaded image, preprocesses it to 28×28 grayscale,
  runs the model, and returns the predicted digit as an HTML fragment.

## Dataset

MNIST (loaded via `keras.datasets` in the notebook) — 28×28 grayscale images of handwritten digits,
10 classes (0–9). Labels one-hot encoded (`to_categorical`).

## Model

Defined and trained in `Model/Mnist DataSet Model.ipynb`. Architecture (from the notebook):

```
Conv2D(256, 3×3, relu) → MaxPool2D(2×2)
Conv2D(128, 3×3, relu) → MaxPool2D(2×2)
Conv2D(64,  3×3, relu) → MaxPool2D(2×2)
Dense(32, relu) → Dense(10, softmax)
```

Optimizer `Adam`, loss `CategoricalCrossentropy`. Trained weights export to `mnist_cnn.keras`
(not committed — retrain from the notebook to regenerate).

## Web app

`Web App/app.py` (Flask, served via `waitress` on `:8080`):

- `GET /` — renders `templates/index.html` (upload form).
- `POST /predict` — reads the uploaded `image`, preprocesses to 28×28 grayscale scaled to `[0,1]`,
  runs `model.predict`, maps `argmax` to a digit name, and returns an HTML `<p>` fragment.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place the trained mnist_cnn.keras next to app.py (export it from the notebook)
python app.py            # http://localhost:8080
```

## Project structure

```
mnist-image-classifier/
├── Model/Mnist DataSet Model.ipynb   # training notebook (CNN on MNIST)
└── Web App/
    ├── app.py                        # Flask inference server (waitress :8080)
    ├── requirements.txt
    ├── templates/index.html          # upload UI (Tailwind)
    └── package.json / tailwind.config.js / vercel.json
```
