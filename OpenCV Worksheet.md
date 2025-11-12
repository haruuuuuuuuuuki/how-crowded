# Worksheet: Using OpenCV to Track a Moving Object

---

### **Guide: Setting Up and Understanding OpenCV**

OpenCV (Open Source Computer Vision Library) is a powerful toolkit used to process images and videos. It allows us to analyze visual data, detect motion, and even recognize objects. In this lesson, we will install OpenCV, open a video, and track a moving object.

#### **1. Install OpenCV**

To install OpenCV, open your terminal and run:

```
pip install opencv-python
```

If you are using a Jupyter Notebook, you can install it directly in a cell:

```
!pip install opencv-python
```

#### **2. Import OpenCV and Open a Video**

OpenCV’s main module is called `cv2`. You can use it to read videos, display them frame by frame, and analyze them.

In this example we will use the car-detection.mp4 video from the sample repository here: https://github.com/intel-iot-devkit/sample-videos

Example:

```
import cv2

# Load the video file
video = cv2.VideoCapture("car-detection.mp4")

while True:
    success, frame = video.read()
    if not success:
        break
    cv2.imshow("Video", frame)

    # Exit when 'q' is pressed
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

video.release()
cv2.destroyAllWindows()
```

#### **3. Tracking a Moving Object**

A simple way to track motion is to use background subtraction. OpenCV provides ready-made background subtractors for this purpose.

Example:

```
import cv2

video = cv2.VideoCapture("car-detection.mp4")
subtractor = cv2.createBackgroundSubtractorMOG2()

while True:
    success, frame = video.read()
    if not success:
        break

    mask = subtractor.apply(frame)
    cv2.imshow("Mask", mask)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

video.release()
cv2.destroyAllWindows()
```

---

### **Task: Track a Single Object**

1. Download or use a short sample video (you can use a ball rolling, car moving, or person walking).
2. Use the background subtraction method from the example above to highlight the moving object.
3. Draw a rectangle around the moving object using contours:

```
import cv2

video = cv2.VideoCapture("sample.mp4")
subtractor = cv2.createBackgroundSubtractorMOG2()

while True:
    success, frame = video.read()
    if not success:
        break

    mask = subtractor.apply(frame)
    contours, _ = cv2.findContours(mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

    for contour in contours:
        area = cv2.contourArea(contour)
        if area > 500:  # Ignore small movements/noise
            x, y, w, h = cv2.boundingRect(contour)
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

    cv2.imshow("Tracking", frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

video.release()
cv2.destroyAllWindows()
```

4. Adjust the area threshold to improve accuracy.
5. Once the moving object is successfully tracked, take a screenshot of your result.

---

### **Challenge: Track Multiple Objects**

You will notice in the above examples that the tracking we have done is messy. The boxes don't usually fit cleanly to the objects and we lose track of objects if they stop moving, or if there is other movement in the video.

For your challenge, find or record a short video that contains multiple moving objects (e.g., cars on a street, people walking, or toys rolling).
Modify your code so it can track several objects at once using bounding boxes.

**Hints:**

* Tune the area threshold to avoid detecting noise.
* Try using color filters or edge detection to separate overlapping objects.
* Consider using `cv2.putText` to label each tracked object.
* You may use this guide and code to refine your own work: https://pysource.com/2021/10/05/object-tracking-from-scratch-opencv-and-python/

**Optional extension:**
Try saving your tracked video:

```
output = cv2.VideoWriter('output.mp4', cv2.VideoWriter_fourcc(*'mp4v'), 20, (width, height))
```

---

### **Turn In**

* One screenshot or saved video showing your tracking results for the *task*.
* One video of your *challenge* with multiple tracked objects.
* A short reflection (3–5 sentences) describing how you improved your tracking accuracy.
