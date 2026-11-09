# 🛡️ AI-Powered Surveillance: Real-Time Anomaly Detection

<!-- Add a screenshot of the dashboard or an alert as images/dashboard.png
     (avoid frames that show real people's faces without their consent) -->
![Surveillance dashboard](images/dashboard.png)

Final year project (team), BSc Computer Systems Engineering, NED University of Engineering and Technology.
A system that watches live camera feeds and raises real-time alerts for **falls, fire, weapons and fights**, logging each event so it can be reviewed later.

**Portfolio:** [syedahafsa135.github.io](https://syedahafsa135.github.io)

---

## Problem

CCTV is usually reviewed after something has gone wrong. **Can a camera feed raise an alert the moment someone falls, a fire starts, a weapon appears or a fight breaks out?**

## My role

This was a team project. **My contributions:**

- **Trained the fall and jump detection model:** a YOLO11n pose model on a 15,000+ image, 7-class dataset (`fall`, `jump`, `s_wall`, `sitting`, `standing`, `walking`, `not_fall`). The app raises alerts only for falls and jumps; the other classes help the model tell normal activity apart.
- **Annotated video data in CVAT** for training.
- **Connected Firebase** for live event logging.

The fire, weapon and fight models, the frontend and the rest of the backend were built by my teammates.

## Results: fall and jump model

These are validation metrics stored inside the trained model file `backendforrtspcam/weights/fall_jump_combined_1_1_25_yolo11n_best.pt` (best epoch of 100, bounding boxes, all 7 classes):

| Precision | Recall | mAP50 | mAP50-95 |
|---|---|---|---|
| 0.537 | 0.716 | 0.583 | 0.428 |

The model favours recall, which suits a safety alert: it is better to flag a possible fall for review than to miss one. The next step is to improve precision so there are fewer false alarms, for example with more varied "not fall" footage.

## How it works

```
Camera feed ──► YOLO models (fall/jump, fire, weapon, fight) ──► alert + saved frame
                                                    │
                                                    └──► Firebase event log ──► React dashboard
```

- **Detection:** Ultralytics YOLO models run on each frame with OpenCV (`backendforrtspcam/utils/`).
- **Backend:** Flask receives and serves frames (`backendforrtspcam/app.py`).
- **Logging:** detected events are written to Firebase.
- **Frontend:** React dashboard with charts of logged events.

## Tech stack

Python · Ultralytics YOLO · OpenCV · Flask · Firebase · React · CVAT (annotation)

## How to run

**Backend**

```bash
cd backendforrtspcam
pip install -r requirements.txt ultralytics
python app.py
```

The Firebase connection needs a service-account key file. Keep it **outside the repo** and never commit it.

**Frontend**

```bash
npm install
npm run dev
```
