# Sample AprilTag

sample_apriltag is a ROS 2 package providing an AprilTag detection pipeline sample for Qualcomm robotics platforms. It provides:

* End-to-end AprilTag detection pipeline from camera capture to tag pose estimation.
* NV12-to-RGB8 color space conversion using `qrb_ros_color_space_convert`.
* Image rectification via `image_proc` and tag detection via `apriltag_ros`.

The pipeline accepts NV12 images from `qrb_ros_camera`, converts to RGB8, rectifies the image, and publishes detection results and tag poses via `/tf`.

## 🔎 Table of contents
  * [ROS topics](#-ros-topics)
  * [Supported targets](#-supported-targets)
  * [Usage](#-usage)
  * [Build from source](#-build-from-source)
  * [License](#-license)

## ⚓ ROS topics

| Topic | Type | Description |
| ----- | ---- | ----------- |
| /apriltag/image_nv12 | sensor_msgs/msg/Image | NV12 image input from QRB ROS Camera |
| /apriltag/camera_info | sensor_msgs/msg/CameraInfo | Camera metadata |
| /apriltag/image_rgb8 | sensor_msgs/msg/Image | RGB8 image after color space conversion |
| /apriltag/image_rect | sensor_msgs/msg/Image | Rectified RGB8 image |
| /apriltag/detections | apriltag_msgs/msg/AprilTagDetectionArray | AprilTag detection results |
| /tf | tf2_msgs/msg/TFMessage | Detected tag poses |

## 🎯 Supported targets

- Qualcomm Dragonwing™ IQ-10

---

## 🚀 Usage

### Start the AprilTag pipeline

```bash
source /opt/ros/jazzy/setup.bash
ros2 launch sample_apriltag sample_apriltag.launch.py
```

Check available topics with `ros2 topic list`:

```
/apriltag/camera_info
/apriltag/detections
/apriltag/image_nv12
/apriltag/image_rect
/apriltag/image_rgb8
/tf
```

Check detection results with `ros2 topic echo /apriltag/detections`:

```
header:
  stamp:
    sec: 1756288539
    nanosec: 411482784
  frame_id: stream1_2625
detections:
- family: tagStandard41h12
  id: 1
  hamming: 0
  decision_margin: 153.66729736328125
  centre:
    x: 595.2331201546427
    y: 382.94486450729545
```

### Change camera ID

Use the `camera_id` launch parameter:

```bash
ros2 launch sample_apriltag sample_apriltag.launch.py camera_id:=1
```

### Change tag family and configuration

Download your AprilTag family from https://chaitanyantr.github.io/apriltag.html, then create a YAML config file:

```yaml
/**:
    ros__parameters:
        family: Standard41h12   # tag family name
        size: 0.092             # tag edge size in meter
        max_hamming: 12         # maximum allowed hamming distance
```

Pass the config path at launch:

```bash
ros2 launch sample_apriltag sample_apriltag.launch.py apriltag_conf:=/path/to/your_config.yaml
```

---

## 👨‍💻 Build from source

Source is located at `sources/quic-qrb-ros/qrb_ros_samples/sample_apriltag/` in the workspace.

```bash
cd build-utils/ubuntu/
python3 build.py --gen-debians --package ros-jazzy-sample-apriltag
```

Built `.deb` files are output to:

```
<workspace>/debian_packages/oss/ros-jazzy-sample-apriltag/
```

### Build dependencies

- qrb_ros_camera
- qrb_ros_color_space_convert
- image_proc
- apriltag_ros

## 📜 License

Project is licensed under the [BSD-3-Clause-Clear](https://spdx.org/licenses/BSD-3-Clause-Clear.html) License.

