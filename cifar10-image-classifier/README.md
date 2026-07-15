# CIFAR-10 Image Classifier

An image classifier for the 10 CIFAR-10 object categories, with a Flask web app that predicts the
class of an uploaded image.

## Overview

- **Task:** image classification into 10 classes: airplane, automobile, bird, cat, deer, dog, frog,
  horse, ship, truck.
- **Model:** a Keras/TensorFlow CNN trained on CIFAR-10, saved as `model.h5`.
- **Web app:** a Flask service that accepts an uploaded image, resizes to 32×32, runs the model, and
  returns the predicted class.

## Dataset

CIFAR-10 (loaded via `keras.datasets.cifar10` in the notebook) — 32×32 RGB images across 10 classes.
Pixel values scaled to `[0,1]`.

## Model

Defined and trained in `Training Model/CIFAR10 DataSet Model.ipynb`. Architecture (from the notebook):

```
Conv2D(256, 3×3, relu, input 32×32×3) → MaxPool2D(2×2)
Conv2D(128, 3×3, relu) → MaxPool2D(2×2)
Conv2D(64,  3×3, relu) → MaxPool2D(2×2)
Dense(32, relu) → Dense(10, softmax)
```

Optimizer `Adam`, loss `sparse_categorical_crossentropy`. Trained weights export to `model.h5`
(not committed — retrain from the notebook to regenerate).

## Web app

`Web App/app.py` (Flask, served via `waitress` on `:8080`):

- `GET /` — renders `templates/index.html` (upload form).
- `POST /predict` — reads the uploaded `image`, resizes to 32×32 and scales to `[0,1]`, runs
  `model.predict`, maps `argmax` to a CIFAR-10 class, and returns an HTML `<p>` fragment.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place the trained model.h5 next to app.py (export it from the notebook)
python app.py            # http://localhost:8080
```

## Project structure

```
cifar10-image-classifier/
├── Training Model/CIFAR10 DataSet Model.ipynb   # training notebook (CNN on CIFAR-10)
└── Web App/
    ├── app.py                                   # Flask inference server (waitress :8080)
    ├── requirements.txt
    ├── templates/index.html                     # upload UI (Tailwind)
    └── package.json / tailwind.config.js
```
