# Brain Tumor Classification

An MRI brain-scan classifier that predicts the tumor type, with a Flask web app for uploading a scan
and getting the prediction.

## Overview

- **Task:** image classification into 4 classes: **Glioma Tumor**, **Meningioma Tumor**, **No Tumor**,
  **Pituitary Tumor**.
- **Model:** a Keras/TensorFlow CNN, loaded from `./model/Brain_Tumor_Classification.keras`.
- **Web app:** a Flask service that accepts an uploaded MRI image, resizes to 150×150, runs the
  model, and returns the predicted tumor class.

## Dataset

A brain-MRI dataset of the four classes above (Training/Testing split). The image dataset and the
trained model file are **not included** in this checkout (large binary assets); the web app expects
the trained `.keras` model under `Web App/model/`.

## Model

A CNN taking 150×150 RGB input and producing a 4-way softmax over the tumor classes. Inference in
`Web App/app.py`: images are loaded at 150×150, scaled to `[0,1]`, and passed to `model.predict`;
`argmax` selects the class.

## Web app

`Web App/app.py` (Flask, `app.run` on `:5000`):

- `GET /` — renders `templates/index.html` (upload form).
- `POST /predict` — reads the uploaded `image`, preprocesses to 150×150 scaled to `[0,1]`, runs the
  model, and returns the predicted class as an HTML `<p>` fragment.

## Getting started

```bash
cd "Web App"
pip install -r requirements.txt
# place the trained model at Web App/model/Brain_Tumor_Classification.keras
python app.py            # http://localhost:5000
```

## Project structure

```
brain-tumor-classification/
├── Model/                 # training data/notebook + trained model (large; not in this checkout)
└── Web App/
    ├── app.py             # Flask inference server (:5000)
    ├── requirements.txt   # flask, tensorflow, keras, pillow
    ├── templates/index.html
    └── package.json / tailwind.config.js
```
