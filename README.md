# ml-notebooks

A collection of small machine-learning projects. Each folder is a self-contained project: a training
**notebook** (where applicable) plus a **Flask web app** that serves the trained model for interactive
predictions. Built with Python, TensorFlow/Keras (and OpenCV for the vision-detection project).

> Trained model weights and large datasets are generally **not committed** — retrain from each
> project's notebook to regenerate them. See each project's README for specifics.

## Projects

| Project | Task | Type | Stack |
|---|---|---|---|
| [`mnist-image-classifier`](./mnist-image-classifier) | Handwritten digit recognition (0–9) | Image classification | Keras CNN + Flask |
| [`cifar10-image-classifier`](./cifar10-image-classifier) | 10-class object recognition | Image classification | Keras CNN + Flask |
| [`brain-tumor-classification`](./brain-tumor-classification) | MRI tumor type (4 classes) | Image classification | Keras CNN + Flask |
| [`face-recognition`](./face-recognition) | Real-time face/eye detection (webcam) | Classical CV | OpenCV Haar cascades + Flask |
| [`emotion-analysis`](./emotion-analysis) | Emotion of text (Fear/Anger/Joy) | NLP classification | Keras + NLTK + Flask |
| [`sentimental-analysis`](./sentimental-analysis) | Sentiment of social text (3-class) | NLP classification | Keras + NLTK + Flask |
| [`product-review-analysis`](./product-review-analysis) | Product-review sentiment (3-class) | NLP classification | Keras + NLTK + Flask |
| [`movie-review-analysis`](./movie-review-analysis) | Movie-review sentiment (binary) | NLP classification | Keras + NLTK + Flask |
| [`sentiment-analysis-evaluation`](./sentiment-analysis-evaluation) | Cross-domain sentiment (binary) | NLP classification | Keras + NLTK + Flask |

## Common structure

Most projects follow:

```
<project>/
├── Model/ (or Training Model/)   # Jupyter notebook + dataset (where included)
└── Web App/
    ├── app.py                    # Flask server; loads the trained model, exposes /predict
    ├── requirements.txt
    └── templates/index.html      # Tailwind UI
```

Each web app exposes `GET /` (the form) and `POST /predict` (returns the prediction as an HTML
fragment). Most run under `waitress` on port `8080`; see the individual READMEs to run one.
