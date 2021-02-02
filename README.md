# MediaPipe OSC
[MediaPipe](https://google.github.io/mediapipe/) examples which stream their detections over OSC to be used in other applications.

### Install & Run

Currently this is only tested on Windows and MacOS. It's recommended to use Python3 and a virtual environment.

```bash
python install -r requirements.txt
```

To run an example use the basic python command to start up the script.

```bash
# start pose detection with webcam 0
python pose.py --input 0

# start pose detection with video
python pose.py --input yoga.mp4
```

Other parameters are documented in the following list or algorithm specific.

- **input** - The video input path or video camera id (default `0`)
- **min-detection-confidence** - Minimum confidence value ([0.0, 1.0]) for the detection to be considered successful. (default `0.5`)
- **min-tracking-confidence** - Minimum confidence value ([0.0, 1.0]) to be considered tracked successfully. (default `0.5`)
- **ip** - OSC ip address to send to (default `127.0.0.1`)
- **port** - OSC port to send to (default `7500`)

### Full-Body Pose Landmark Model (BlazePose Tracker)
The landmark model currently included in MediaPipe Pose predicts the location of 33 full-body landmarks (see figure below), each with (`x, y, z, visibility`). Note that the z value should be discarded as the model is currently not fully trained to predict depth, but this is something we have on the roadmap.

![Pose Description](readme/pose_tracking_full_body_landmarks.png)

*[Reference: mediapipe/solutions/pose](https://google.github.io/mediapipe/solutions/pose#pose-landmark-model-blazepose-tracker)*

#### Format

- `count` - Indicates how many poses are detected (currently only `0` or `1`)
- list of landmarks (`33` per pose) (if pose has been detected)
    - `x` - X-Position of the landmark
    - `y` - Y-Position of the landmark
    - `z` - Z-Position of the landmark
    - `visibility` - Visibility of the landmark

```
/mediapipe/pose [count, x, y, z, visibility, x, y, z, visibility ...]
```

### Upper-Body Pose Landmark Model (BlazePose Tracker)
The landmark model currently included in MediaPipe Pose predicts the location of 25 upper-body landmarks (see figure below), each with (`x, y, z, visibility`). Note that the z value should be discarded as the model is currently not fully trained to predict depth, but this is something we have on the roadmap. The model shares the same architecture as the full-body version that predicts 33 landmarks, described in more detail in the [BlazePose Google AI Blog](https://ai.googleblog.com/2020/08/on-device-real-time-body-pose-tracking.html) and in this [paper](https://arxiv.org/abs/2006.10204).
To switch to the upper-body detection mode, use the following argument:

```
python pose.py --upper-body-only True
```

![Pose Description](readme/pose_tracking_upper_body_landmarks.png)

*[Reference: mediapipe/solutions/pose](https://google.github.io/mediapipe/solutions/pose#pose-landmark-model-blazepose-tracker)*

#### Format

- `count` - Indicates how many poses are detected (currently only `0` or `1`)
- list of landmarks (`25` per pose) (if pose has been detected)
    - `x` - X-Position of the landmark
    - `y` - Y-Position of the landmark
    - `z` - Z-Position of the landmark
    - `visibility` - Visibility of the landmark

```
/mediapipe/pose [count, x, y, z, visibility, x, y, z, visibility ...]
```

### Hand Detection
The [hand detection model](https://google.github.io/mediapipe/solutions/hands.html) is able to detect and track 21 3D landmarks.

#### Format

- `count` - Indicates how many hands are detected
- list of landmarks (`21` per hand) (if hands has been detected)
    - `x` - X-Position of the landmark
    - `y` - Y-Position of the landmark
    - `z` - Z-Position of the landmark
    - `visibility` - Visibility of the landmark

```
/mediapipe/hands [count, x, y, z, visibility, x, y, z, visibility ...]
```

### Face Mesh
tbd

### Examples

Currently there are very basic receiver examples for processing. Check out the [examples](examples) folder.

### About
* Example code and documentation adapted from [google/mediapipe](https://google.github.io/mediapipe/solutions/)
* OSC sending and examples implemented by [cansik](https://github.com/cansik)