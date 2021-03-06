# OpenCV Capture Camera Video
Use OpenCV and MediaPipe to detect hands.
https://google.github.io/mediapipe/solutions/hands

## Requirements

1. It'll open the computer's camera and show the video in a window.
2. When hands show in the video, it'll draw the hands landmarks.
3. When you press "q" key it'll quit.

## What will we practice in this project?

- OpenCV: you need to install the `opencv-python` package via `pip install opencv-python` command in this project environment.
- while loop
- MediaPipe hands solution

## A reference code

### handUtils.py
```python
import cv2
import mediapipe as mp

class HandDetector():
    def __init__(self):
        self.hand_detector = mp.solutions.hands.Hands()
        self.drawer = mp.solutions.drawing_utils

    def process(self, img, draw=True):
        img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
        self.hands_data = self.hand_detector.process(img_rgb)
        if draw:
            if self.hands_data.multi_hand_landmarks:
                for handlms in self.hands_data.multi_hand_landmarks:
                    self.drawer.draw_landmarks(img, handlms, mp.solutions.hands.HAND_CONNECTIONS)

    def find_position(self, img):
        h, w, c = img.shape
        position = {'Left': {}, 'Right': {}}
        if self.hands_data.multi_hand_landmarks:
            i = 0
            for point in self.hands_data.multi_handedness:
                score = point.classification[0].score
                if score >= 0.8:
                    label = point.classification[0].label
                    hand_lms = self.hands_data.multi_hand_landmarks[i].landmark
                    for id, lm in enumerate(hand_lms):
                        x, y = int(lm.x * w), int(lm.y * h)
                        position[label][id] = (x, y)
                i = i + 1
        return position

```
### hand.py
```python
import cv2
from handUtils import HandDetector

camera = cv2.VideoCapture(1)
hand_detector = HandDetector()

while True:
    success, img = camera.read()
    if success:
        img = cv2.flip(img, 1)
        hand_detector.process(img, draw=False)
        position = hand_detector.find_position(img)
        left_finger = position['Left'].get(8, None)
        if left_finger:
            cv2.circle(img, (left_finger[0], left_finger[1]),10, (0, 0, 255), cv2.FILLED)
        right_finger = position['Right'].get(8, None)
        if right_finger:
            cv2.circle(img, (right_finger[0], right_finger[1]),10, (0, 255, 0), cv2.FILLED)
        cv2.imshow('Video', img)
    k = cv2.waitKey(1)
    if k == ord('q'):
        break

camera.release()
cv2.destroyAllWindows()

```

## Run the demo
Install the OpenCV & MediaPipe:
```shell
pip install opencv-python mediapipe
```
Please save the 2 Python files and run it???

```
python hand.py
```

----

# ??????OpenCV???MediaPipe??????????????????

## ????????????

- ??????????????????????????????????????????????????????????????????
- ???????????????????????????????????????????????????
- ???`q`??????????????????

## ????????????

- ??????OpenCV???????????????`pip install opencv-python mediapipe`???OpenCV???MediaPipe????????????????????????
- MediaPipe???????????????https://google.github.io/mediapipe/solutions/hands

## ??????????????????

### handUtils.py
```python
import cv2
import mediapipe as mp

class HandDetector():
    def __init__(self):
        self.hand_detector = mp.solutions.hands.Hands()
        self.drawer = mp.solutions.drawing_utils

    def process(self, img, draw=True):
        img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
        self.hands_data = self.hand_detector.process(img_rgb)
        if draw:
            if self.hands_data.multi_hand_landmarks:
                for handlms in self.hands_data.multi_hand_landmarks:
                    self.drawer.draw_landmarks(img, handlms, mp.solutions.hands.HAND_CONNECTIONS)

    def find_position(self, img):
        h, w, c = img.shape
        position = {'Left': {}, 'Right': {}}
        if self.hands_data.multi_hand_landmarks:
            i = 0
            for point in self.hands_data.multi_handedness:
                score = point.classification[0].score
                if score >= 0.8:
                    label = point.classification[0].label
                    hand_lms = self.hands_data.multi_hand_landmarks[i].landmark
                    for id, lm in enumerate(hand_lms):
                        x, y = int(lm.x * w), int(lm.y * h)
                        position[label][id] = (x, y)
                i = i + 1
        return position

```
### hand.py
```python
import cv2
from handUtils import HandDetector

camera = cv2.VideoCapture(1)
hand_detector = HandDetector()

while True:
    success, img = camera.read()
    if success:
        img = cv2.flip(img, 1)
        hand_detector.process(img, draw=False)
        position = hand_detector.find_position(img)
        left_finger = position['Left'].get(8, None)
        if left_finger:
            cv2.circle(img, (left_finger[0], left_finger[1]),10, (0, 0, 255), cv2.FILLED)
        right_finger = position['Right'].get(8, None)
        if right_finger:
            cv2.circle(img, (right_finger[0], right_finger[1]),10, (0, 255, 0), cv2.FILLED)
        cv2.imshow('Video', img)
    k = cv2.waitKey(1)
    if k == ord('q'):
        break

camera.release()
cv2.destroyAllWindows()

```

## ????????????

Install OpenCV and MediaPipe:
```shell
pip install opencv-python mediapipe
```
????????????2?????????????????????

```
python hand.py
```
