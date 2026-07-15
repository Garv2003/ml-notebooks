# Face Recognition (Haar Cascade)

A real-time webcam face- and eye-detection app using OpenCV Haar cascades, streamed to the browser
through a Flask MJPEG endpoint.

## Overview

- **Task:** real-time face and eye **detection** (classical computer vision — not deep learning).
- **Method:** OpenCV `CascadeClassifier` with pre-trained Haar cascades.
- **Web app:** Flask captures frames from the webcam, draws boxes around detected faces (blue) and
  eyes (green), and streams the annotated video to the browser.

## How it works

`app.py`:
- Opens the default webcam via `cv2.VideoCapture(0)`.
- For each frame, runs `haarcascade_frontalface_default.xml` (`detectMultiScale(frame, 1.1, 7)`) to
  find faces, then `haarcascade_eye.xml` within each face region to find eyes, drawing rectangles.
- Encodes each annotated frame as JPEG and yields it as a `multipart/x-mixed-replace` MJPEG stream.

## Routes

- `GET /` — renders `templates/index.html`, which embeds the video stream.
- `GET /video_feed` — the live MJPEG stream (`multipart/x-mixed-replace; boundary=frame`).

Served with Flask's development server (`app.run(debug=True)`).

## Cascades included

`Haarcascades/` and `opencv/` hold the pre-trained XML cascades: frontal face, eye, full body, and a
car cascade.

## Getting started

```bash
pip install -r requirements.txt
python app.py            # http://localhost:5000  (grants webcam access)
```

> Requires a webcam. `cv2.VideoCapture(0)` uses the default camera device.

## Project structure

```
face-recognition/
├── app.py                    # Flask MJPEG webcam detection server
├── requirements.txt          # flask, opencv-python
├── Haarcascades/             # face, eye, fullbody, car cascades
├── opencv/                   # frontal-face cascade
└── templates/
    ├── index.html            # embeds /video_feed
    └── result.html
```
