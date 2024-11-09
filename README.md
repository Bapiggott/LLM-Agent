# LLM Agent for Drone Control

## Project Overview

This project focuses on developing a natural language-controlled drone system that minimizes human intervention. The system allows users to command a PX4 drone via natural language prompts, integrating various technologies:

- **Large Language Models (LLMs)** for interpreting user commands.
- **NVIDIA ISAAC SIM** for sensor simulation.
- **Computer Vision** for object detection and depth estimation.
- **MAVLink** for drone communication.
- **PX4-Autopilot** for controlling drone actions.

The system translates natural language instructions into mission commands that enable autonomous drone operation. Users can either define complete missions or utilize a real-time control setup for flexibility.

## System Workflow
1. **User Input:** The user provides a natural language prompt (e.g., "survey area" or "capture images of an object").
2. **LLM Processing:** The LLM combines the prompt with pre-defined drone capabilities and context.
3. **Data Integration:** Sensor and vision data (from ISAAC SIM) are merged into the prompt.
4. **Minispec Code Generation:** A streamlined Python-like code format is generated for efficient execution.
5. **Execution:** [Minispec code](https://arxiv.org/pdf/2312.14950) is converted to full Python and commands are sent to PX4-Autopilot via MAVLink.

## Directory Structure
Each mission is organized in a directory with the following structure:
```
project_root/
├── mission_1/
│   ├── ulog/               # Raw sensor data from the mission
│   ├── minispec.json       # Minispec and Python equivalent commands
│   ├── image_data/
│       ├── 20241107_152241/
│           ├── original.jpg      # Original image
│           ├── depth.jpg         # Depth estimation image
│           ├── depth_with_coordinates.npy  # Depth coordinates data
│           ├── image_data.json  # Image analysis data
├── mission_2/
│   ├── ...
```

## Example: Loading Depth Coordinates
You can load the `.npy` file containing depth estimation data with the following code:
```python
import numpy as np

# Load the .npy file
depth_with_coordinates = np.load("depth_with_coordinates.npy")

# Print without scientific notation
np.set_printoptions(suppress=True, precision=5)  # Set precision as needed
print("Loaded data shape:", depth_with_coordinates.shape)
print("Sample data:", depth_with_coordinates)
```

## Data Conversion Tools
- **ULog to CSV Conversion:** The `pyulog` tool can be used to convert `.ulog` files to `.csv` format. [Link to pyulog GitHub repository](https://github.com/PX4/pyulog/tree/main).
- **Visual Representation of ULog Files:** Use [PX4 Log Analysis Tool](https://logs.px4.io/) to visualize `.ulog` files for detailed mission insights.

## Data Collection Details
- **Sensor Data:** Collected from various sensors, including GPS (for positioning), IMU (for stability), and camera sensors (for visual data). Only relevant data is used in LLM prompt processing for mission precision.
- **Image Data Processing:** The system processes depth estimation, object detection, and scene analysis to generate concise natural language descriptions for input into the LLM.

## Real-Time Control (Currently Undone)
In addition to the mission-based approach, the system supports real-time control through a "joystick" setup. This allows the LLM to adaptively respond to updated prompts in complex or unpredictable environments.

### Benefits of Real-Time Control
- Greater flexibility for navigating challenging terrains.
- Enhanced accuracy and responsiveness in tight spaces or dynamic environments.

## Challenges & Key Considerations
- **Depth Estimation & Object Detection:** These models generate dense data that must be quickly processed to generate actionable insights.
- **Sensor Selection:** Mission-specific sensors are chosen based on requirements to optimize the prompt formulation.
- **Communication Efficiency:** Efficient conversion of Minispec to Python and seamless communication via MAVLink is crucial, especially for real-time operations.

## Parameter Reference
For detailed configuration of the PX4 drone, refer to the [PX4 Parameter Reference](https://docs.px4.io/main/en/advanced_config/parameter_reference.html). This section contains parameters available for customization, allowing you to determine which parameters are important to change, based on sensor data and mission requirements. Understanding these parameters helps ensure the drone operates efficiently and accurately for different types of missions.

## Future Work
- **Sensor Optimization:** Further exploration into dynamic sensor selection and real-time data prioritization.
- **Enhanced Real-Time Control:** Improve the responsiveness of the LLM for live control.
- **Minispec Refinement:** Optimize Minispec generation for faster conversion to PX4-compatible commands.

## Data Availability
The project data will be available via a [MEGA link](https://mega.nz/folder/MaN3USwA#-Dl232AGkgtAH7HnNfR2nA).

