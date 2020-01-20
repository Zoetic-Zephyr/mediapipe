# Hand Skeleton Tracking using Mediapipe

This repository provides the Hand Skeleton Tracking (pose estimation) functionality and additionally two applications that derive from it.

1. [Hand Skeleton Tracking (core functionality)](#hand-tracking-on-desktop-with-cpu) @ branch:master
2. [Joint Landmark Output](#output-landmarks) @branch: out-landmark
3. [Gesture Recognition](#recognize-gestures) @branch: gesture

To use this repo, simply `clone` it to your local machine, `cd` to the root folder, and execute in your `cmd` tool the corresponding commands provided in the **To build** section in order to start the compliation process. When finished, execute the commands in the **To run** section accordingly.

Note: All three branches are already compiled for MacOS (CPU). If you are working in the same envionment, you can skip the compilation step.

Here is the official [README](./official-readme.md) file provided by Google.

Also, check out the official docs folder where additional documents are provided. Among them, I find [how_to_questions.md](./mediapipe/docs/how_to_questions.md) particularly helpful.

[//]: # "Image References"

[landmarks]: ./readme/landmarks.jpg "landmarks"

## Hand Tracking on Desktop with CPU

**@branch: master**

### To build

```bash
bazel build -c opt --define MEDIAPIPE_DISABLE_GPU=1  \
mediapipe/examples/desktop/hand_tracking:hand_tracking_cpu
```

### To run

```bash
GLOG_logtostderr=1 bazel-bin/mediapipe/examples/desktop/hand_tracking/hand_tracking_cpu  \
--calculator_graph_config_file=mediapipe/graphs/hand_tracking/hand_tracking_desktop_live.pbtxt
```

## Output Landmarks

**@branch: out-landmark**

### Note

Hand position output generated at Mediapipe v0.6.4.

v0.6.7.1 does not work.

### To build

```bash
bazel build -c opt --define MEDIAPIPE_DISABLE_GPU=1 mediapipe/examples/desktop/hand_tracking:hand_tracking_out_cpu
```

### To run

```bash
bazel-bin/mediapipe/examples/desktop/hand_tracking/hand_tracking_out_cpu --calculator_graph_config_file=mediapipe/graphs/hand_tracking/hand_tracking_desktop_live.pbtxt
```

## Recognize Gestures

**@branch: gesture**

The current system can recognize 10 gestures using heuristics based on 21 landmark positions.

`ONE`, `TWO`, `THREE`, `FOUR`, `FIVE`, `SIX`, `YEAH`, `ROCK`, `SPIDERMAN`, and `OK`.

![alt text][landmarks]

It detects the state (open/closed) of each finger and match the states to preset gestures. In the official blog [post](https://ai.googleblog.com/2019/08/on-device-real-time-hand-tracking-with.html), it seems that the developers at Google is also using heuristics for gesture recognition, but presumably in a more sophisticated way.

> We apply a simple algorithm to derive the gestures. First, the state of each finger, e.g. bent or straight, is determined by the accumulated angles of joints. Then we map the set of finger states to a set of pre-defined gestures.

### Note

Tested on Mediapipe v0.6.8 only.

The building & running methods are identical to the standard methods as in [Hand Tracking on Desktop with CPU](#hand-tracking-on-desktop-with-cpu) because the dependant calculators are modified directly.

### To build

```bash
bazel build -c opt --define MEDIAPIPE_DISABLE_GPU=1  \
mediapipe/examples/desktop/hand_tracking:hand_tracking_cpu
```

### To run

```bash
GLOG_logtostderr=1 bazel-bin/mediapipe/examples/desktop/hand_tracking/hand_tracking_cpu  \
--calculator_graph_config_file=mediapipe/graphs/hand_tracking/hand_tracking_desktop_live.pbtxt
```

## Multi Hand Tracking on Desktop with CPU

**@branch: master**

###  To build

```bash
bazel build -c opt --define MEDIAPIPE_DISABLE_GPU=1  \
mediapipe/examples/desktop/multi_hand_tracking:multi_hand_tracking_cpu
```

### To run

```bash
GLOG_logtostderr=1 bazel-bin/mediapipe/examples/desktop/multi_hand_tracking/multi_hand_tracking_cpu  \
--calculator_graph_config_file=mediapipe/graphs/hand_tracking/multi_hand_tracking_desktop_live.pbtxt
```

**To change the number of hands to `x` in this application, change:**

1. `min_size:x` in `CollectionHasMinSizeCalculatorOptions` in `mediapipe/graphs/hand_tracking/multi_hand_tracking_desktop.pbtxt`.
2. `max_vec_size:x` in `ClipVectorSizeCalculatorOptions` in `mediapipe/examples/dekstop/hand_tracking/subgraphs/multi_hand_detection_cpu.pbtxt`.

## Offline Input

**@branch: master**

### To build

```bash
bazel build -c opt mediapipe/examples/desktop/hand_tracking:hand_tracking_tflite --define MEDIAPIPE_DISABLE_GPU=1
```

### To run

```bash
export GLOG_logtostderr=1

bazel-bin/mediapipe/examples/desktop/hand_tracking/hand_tracking_tflite \
  --calculator_graph_config_file=mediapipe/graphs/hand_tracking/hand_detection_desktop.pbtxt \
  --input_side_packets=input_video_path=/path/to/input/file,output_video_path=/path/to/output/file
```

