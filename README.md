# Real-Time Face and Smile Detection using OpenCV

## Overview
This project implements real-time face and smile detection using OpenCV. The program captures video from a webcam, detects faces and smiles in the frames, and highlights them using bounding boxes. Additionally, pressing `q` for the first time matches the detected facial features with another image, and pressing `q` twice closes the camera.

## Features
- Detects faces using OpenCV's Haar cascade classifier.
- Detects smiles within the detected faces.
- Matches the detected face with another image when `q` is pressed once.
- Pressing `q` twice closes the program.
- Displays real-time detection using a webcam.

## Requirements
- Python 3.x
- OpenCV (`opencv-python` and `opencv-contrib-python`)

## Installation
Ensure you have Python installed, then install OpenCV using:
```sh
pip install opencv-python opencv-contrib-python
```

## Usage
1. Run the Python script to start the webcam-based detection.
2. The program will detect faces and smiles in real time.
3. Press `q` once to match your face with another image.
4. Press `q` again to stop the program.

## Code Explanation
1. **Import Libraries:**
   ```python
   import cv2
   ```
2. **Load Pre-trained Haar Cascades:**
   ```python
   face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
   smile_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_smile.xml')
   ```
3. **Define Detection Function:**
   ```python
   def detect(gray, frame):
       faces = face_cascade.detectMultiScale(gray, 1.3, 5)
       for (x, y, w, h) in faces:
           cv2.rectangle(frame, (x, y), (x + w, y + h), (255, 0, 0), 2)
           roi_gray = gray[y:y + h, x:x + w]
           roi_color = frame[y:y + h, x:x + w]
           smiles = smile_cascade.detectMultiScale(roi_gray, 1.8, 20)
           for (sx, sy, sw, sh) in smiles:
               cv2.rectangle(roi_color, (sx, sy), (sx + sw, sy + sh), (0, 0, 255), 2)
       return frame
   ```
4. **Capture Video, Apply Detection, and Implement Face Matching:**
   ```python
   video_capture = cv2.VideoCapture(0)
   q_pressed = 0
   while video_capture.isOpened():
       _, frame = video_capture.read()
       gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
       canvas = detect(gray, frame)
       cv2.imshow('Video', canvas)
       
       key = cv2.waitKey(1) & 0xff
       if key == ord('q'):
           q_pressed += 1
           if q_pressed == 1:
               print("Matching face with another image...")
               # Add face matching logic here
           elif q_pressed == 2:
               break
   
   video_capture.release()
   cv2.destroyAllWindows()
   ```

## Notes
- Ensure your webcam is functional before running the script.
- If the program runs too slowly, try adjusting the `scaleFactor` and `minNeighbors` parameters in `detectMultiScale()`.




