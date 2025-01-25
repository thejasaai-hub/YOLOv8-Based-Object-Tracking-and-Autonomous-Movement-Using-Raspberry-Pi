# YOLOv8-Based Object Tracking and Autonomous Movement Using Raspberry Pi

This project integrates YOLOv8 object detection and segmentation with a Raspberry Pi-controlled robot. The system processes video input, detects objects of interest, and controls the robot's motors for autonomous navigation. It ensures precise movements based on object positioning and distance calculations.

## How It Works

### **1. Motor Control with GPIO**
- **Motor Pins:**
  - `in1`, `in2`: Motor A direction control pins.
  - `in3`, `in4`: Motor B direction control pins.
  - `en1`, `en2`: Motor A and B enable pins, controlled via PWM for speed adjustment.
  
- **Movement Functions:**
  - **`move_forward()`**: Moves the robot forward.
  - **`move_left()`**: Turns the robot left.
  - **`move_right()`**: Turns the robot right.
  - Motors are controlled by setting GPIO pins HIGH or LOW accordingly.

### **2. YOLOv8 Object Detection**
- **Model Initialization:**
  - A pre-trained YOLOv8 model (`best.pt`) is used for object detection and segmentation.

- **Video Input:**
  - Video is read from an external file (`video3.mp4`) using OpenCV.

- **Segmentation Masks:**
  - YOLO generates segmentation masks for detected objects, which are processed to determine the lowest point on each object.

### **3. Object Tracking and Decision-Making**
- **Finding the Lowest Point:**
  - The lowest non-zero point in the segmentation mask is identified, representing the object's position.

- **Intersection with Lane Center:**
  - An intersection point is calculated between the object's lowest point and a defined "lane center" on the frame.

- **Distance Measurements:**
  - The system calculates distances:
    - From the lowest point to the intersection.
    - From the intersection to the lane center.

- **Movement Decision:**
  - The robot's movement is adjusted based on the distance between the intersection point and the lane center:
    - **Within Target Range:** The robot moves forward.
    - **Too Close:** The robot turns left.
    - **Too Far:** The robot turns right.

### **4. Video Output**
- **Real-Time Visualization:**
  - The system overlays segmentation masks, distance lines, and intersection points on the video frames.
  - Frame details include:
    - Green line: Lowest point to intersection.
    - Blue line: Intersection to lane center.
    - Distance values in pixels.

- **Saving Processed Video:**
  - The processed video with annotations is saved as `output.mp4`.

### **5. GPIO and System Cleanup**
- **Interrupt Handling:**
  - Motors are stopped if no masks are detected or the program is interrupted (`'q'` key press).
- **Resource Cleanup:**
  - Releases video resources and shuts down GPIO safely after execution.

## Features
1. **YOLOv8 Object Detection:**
   - Detects objects and generates segmentation masks.
2. **Distance-Based Movement Control:**
   - Calculates object positions relative to the robot's lane and adjusts movements accordingly.
3. **Video Annotation:**
   - Annotates frames with distance measurements, lines, and intersection points for debugging and visualization.
4. **Real-Time Decision-Making:**
   - Autonomous movement based on object positions in the frame.

## Components Required
1. **Raspberry Pi:**
   - Controls GPIO and executes the YOLOv8 model.
2. **Motors and Motor Driver (e.g., L298N):**
   - Drives the robot’s wheels for movement.
3. **Camera:**
   - Captures video for YOLOv8 processing.
4. **Pre-trained YOLOv8 Model:**
   - A custom-trained YOLOv8 model (`best.pt`).
     
## Installation

### **Install Dependencies**

sudo apt-get update
sudo apt-get install python3-opencv
pip install ultralytics numpy RPi.GPIO


## Usage

- The robot processes the video and detects objects using YOLOv8.
- Based on object positioning in the frame, the robot adjusts its movement:
  - Moves forward, left, or right.
- Processed video with annotations is saved as output.mp4.

## Applications

1. **Autonomous Robotics**:

Enables robots to navigate environments based on detected objects.

2. **Object Tracking:**

Tracks and visualizes object positions in real-time.

3. **Industrial Automation**:

Can be adapted for automated material handling or sorting.

## Limitations

1.**Lighting Conditions**:

Performance depends on the quality of video input.

2. **Model Accuracy**:

Detection is as good as the training of the YOLO model.

3. **Hardware Requirement**s:
   
Requires sufficient processing power for YOLOv8 inference.

## Future Enhancements

- Add real-time camera input instead of video file processing.
  
- Integrate additional sensors for obstacle avoidance.
  
- Improve decision-making algorithms for smoother navigation.
