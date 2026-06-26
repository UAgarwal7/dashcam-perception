# dashcam-perception

A road-sign object detector built with [YOLOv8](https://docs.ultralytics.com/) (Ultralytics).
A custom model is trained on a road-signs dataset and served through a small Flask web app
where you can upload an image and see detected signs drawn as bounding boxes.

Built by following freeCodeCamp's tutorial:
[How to Detect Objects in Images Using YOLOv8](https://www.freecodecamp.org/news/how-to-detect-objects-in-images-using-yolov8/).

## What it detects

The trained model (`webapp/best.pt`) recognizes 21 road-sign classes, including:

`bus_stop`, `do_not_enter`, `do_not_stop`, `do_not_turn_l`, `do_not_turn_r`, `do_not_u_turn`,
`enter_left_lane`, `green_light`, `left_right_lane`, `no_parking`, `parking`, `ped_crossing`,
`ped_zebra_cross`, `railway_crossing`, `red_light`, `stop`, `t_intersection_l`, `traffic_light`,
`u_turn`, `warning`, `yellow_light`.

It is **not** a general-purpose detector — uploading an image with no road signs (e.g. a cat or
dog photo) will correctly return zero detections.

## Project layout

```
webapp/
  object_detector.py   # Flask server: serves the page + /detect endpoint (port 8080)
  index.html           # Frontend: upload an image, draws returned boxes on a canvas
  best.pt              # Trained YOLOv8 model weights (gitignored — see Training)
notebooks/
  starting.ipynb       # Training / experimentation notebook
data/                  # Datasets (gitignored — not committed)
```

## Setup

```bash
python3 -m venv venv
source venv/bin/activate
pip install ultralytics flask waitress pillow
```

## Running the web app

```bash
cd webapp
python object_detector.py
```

Then open <http://localhost:8080>, upload an image, and detected signs will be drawn on the
canvas. A status line under the upload input reports progress ("Detecting…", "No objects
detected.", "Detected N object(s).").

> Note: open the app through the server URL above — opening `index.html` directly as a file
> won't work, since the `/detect` request is relative to the server.

## Training

The model was trained on a [Roboflow road-signs dataset](https://universe.roboflow.com/roboflow-100/road-signs-6ih4y/dataset/2).
See `notebooks/starting.ipynb` for the training workflow. Datasets, training run outputs
(`runs/`), and model weights (`*.pt`, including `webapp/best.pt`) are gitignored. To run the
web app you'll need to produce `webapp/best.pt` by training via the notebook (or copy it in
from a training run under `notebooks/runs/`).
