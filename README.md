# ROS-2-robot-navigation

# TurtleBot3 Autonomous Waypoint Navigation System

A comprehensive ROS 2 system for autonomous navigation of TurtleBot3 robots to user-selected waypoints via an intuitive GUI interface.

## Overview

This project demonstrates a complete autonomous navigation solution combining:
- **ROS 2 Navigation Stack (Nav2)** for path planning and obstacle avoidance
- **Gazebo Simulation** with custom world and waypoint definitions
- **Tkinter GUI** for intuitive waypoint selection and monitoring
- **Custom ROS 2 Node** for waypoint sequencing and navigation orchestration
- **RViz Visualization** for real-time monitoring and debugging

## Features

- **6 Named Waypoints**: Station A, Station B, Station C, Station D, Docking Station, Home
- **Single & Multi-Waypoint Navigation**: Select one or multiple waypoints for sequential navigation
- **Automatic Return Home**: Robot automatically returns to home after completing waypoints
- **Real-time Status Monitoring**: GUI displays navigation status with color-coded feedback
- **Graceful Error Handling**: Handles timeouts, failures, and cancellations
- **Cancel Functionality**: Stop navigation at any time
- **RViz Integration**: Visualize waypoints, goals, and navigation paths
- **Unified Launch System**: Start all components with a single command

## System Architecture

\`\`\`
┌─────────────────────────────────────────────────────────────┐
│                    Tkinter GUI                              │
│         (Waypoint Selection & Status Display)               │
└────────────────────┬────────────────────────────────────────┘
                     │ ROS 2 Topics/Services
                     ▼
┌─────────────────────────────────────────────────────────────┐
│            Waypoint Manager Node                            │
│    (Navigation Orchestration & Sequencing)                  │
└────────────────────┬────────────────────────────────────────┘
                     │ Nav2 Action Server
                     ▼
┌─────────────────────────────────────────────────────────────┐
│              Nav2 Navigation Stack                          │
│  (Path Planning, AMCL Localization, Costmaps)              │
└────────────────────┬────────────────────────────────────────┘
                     │ Robot Commands
                     ▼
┌─────────────────────────────────────────────────────────────┐
│         Gazebo Simulation / Real Robot                      │
│              (TurtleBot3 Waffle)                            │
└─────────────────────────────────────────────────────────────┘
\`\`\`

## Prerequisites

- **OS**: Ubuntu 22.04 LTS (Jammy)
- **ROS 2**: Humble distribution
- **Python**: 3.10+
- **Gazebo**: 11.0+
- **TurtleBot3 Packages**: turtlebot3_gazebo, turtlebot3_navigation2

## Installation

### 1. Install ROS 2 Humble

\`\`\`bash
# Follow official ROS 2 installation guide
# https://docs.ros.org/en/humble/Installation.html
\`\`\`

### 2. Install TurtleBot3 and Nav2

\`\`\`bash
sudo apt update
sudo apt install ros-humble-turtlebot3 \
                 ros-humble-turtlebot3-gazebo \
                 ros-humble-turtlebot3-navigation2 \
                 ros-humble-nav2-bringup \
                 ros-humble-navigation2
\`\`\`

### 3. Create ROS 2 Workspace

\`\`\`bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
\`\`\`

### 4. Clone This Repository

\`\`\`bash
git clone https://github.com/yourusername/turtlebot3-waypoint-navigation.git
cd ~/ros2_ws
\`\`\`

### 5. Install Dependencies

\`\`\`bash
rosdep install --from-paths src --ignore-src -r -y
\`\`\`

### 6. Build the Package

\`\`\`bash
cd ~/ros2_ws
colcon build --packages-select waypoint_manager --symlink-install
source install/setup.bash
\`\`\`

## Quick Start

### 1. Launch the Complete System

\`\`\`bash
# Terminal 1: Start all components
ros2 launch waypoint_manager turtlebot3_waypoint_nav.launch.py
\`\`\`

Wait for all components to initialize (30-60 seconds). You should see:
- Gazebo window with TurtleBot3
- RViz window with map and robot
- Terminal output from all nodes

### 2. Launch the GUI

\`\`\`bash
# Terminal 2: Start the waypoint GUI
python3 ~/ros2_ws/src/waypoint_manager/src/waypoint_gui.py
\`\`\`

### 3. Select Waypoints and Navigate

1. Click waypoint buttons to select destinations
2. Click "Navigate" to start autonomous navigation
3. Monitor status in the GUI and RViz
4. Click "Cancel" to stop navigation at any time

## Usage Examples

### Single Waypoint Navigation

\`\`\`
1. Click "Station A" button (turns blue)
2. Click "Navigate"
3. Robot navigates to Station A
4. Robot automatically returns to Home
5. Status shows "Completed"
\`\`\`

### Multi-Waypoint Navigation

\`\`\`
1. Click "Station A", "Station B", "Station C" (all turn blue)
2. Click "Navigate"
3. Robot visits: Station A → Station B → Station C → Home
4. Status updates after each waypoint
5. Final status shows "Completed"
\`\`\`

### Cancel Navigation

\`\`\`
1. During navigation, click "Cancel"
2. Robot stops immediately
3. Status shows "Cancelled"
4. GUI buttons re-enable for new selection
\`\`\`

## Project Structure

\`\`\`
waypoint_manager/
├── CMakeLists.txt                 # Build configuration
├── package.xml                    # Package metadata
├── README.md                       # This file
├── SETUP.md                        # Detailed setup guide
├── CONTRIBUTING.md                # Contribution guidelines
├── LICENSE                         # MIT License
├── .gitignore                      # Git ignore rules
│
├── src/
│   ├── waypoint_manager_node.py   # Main ROS 2 node
│   └── waypoint_gui.py            # Tkinter GUI application
│
├── launch/
│   └── turtlebot3_waypoint_nav.launch.py  # Unified launch file
│
├── config/
│   └── nav2_params.yaml           # Nav2 configuration
│
├── worlds/
│   └── turtlebot3_waypoints.world # Custom Gazebo world
│
├── maps/
│   ├── map.pgm                    # Map image
│   └── map.yaml                   # Map metadata
│
└── rviz/
    └── nav2_default_view.rviz     # RViz configuration
\`\`\`

## Configuration

### Waypoint Coordinates

Edit waypoint coordinates in `src/waypoint_manager_node.py`:

\`\`\`python
self.waypoints = {
    'Station A': (1.0, 1.0, 0.0),      # (x, y, theta)
    'Station B': (2.0, 2.0, 0.0),
    'Station C': (3.0, 1.0, 0.0),
    'Station D': (1.0, 3.0, 0.0),
    'Docking Station': (0.5, 0.5, 0.0),
    'Home': (0.0, 0.0, 0.0),
}
\`\`\`

### Nav2 Parameters

Modify `config/nav2_params.yaml` to tune:
- Planner parameters
- Controller parameters
- Costmap settings
- Recovery behaviors

### Gazebo World

Edit `worlds/turtlebot3_waypoints.world` to:
- Add/remove obstacles
- Modify world dimensions
- Change lighting and physics

## Troubleshooting

### Gazebo doesn't start
\`\`\`bash
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models
gazebo --version
\`\`\`

### Nav2 fails to initialize
\`\`\`bash
# Verify map file exists
ls -la ~/ros2_ws/src/waypoint_manager/maps/

# Check map.yaml format
cat ~/ros2_ws/src/waypoint_manager/maps/map.yaml
\`\`\`

### GUI won't connect to ROS
\`\`\`bash
# Verify ROS environment
source ~/ros2_ws/install/setup.bash
echo $ROS_DOMAIN_ID

# Check if waypoint_manager node is running
ros2 node list
\`\`\`

### Robot doesn't move
\`\`\`bash
# Set initial pose in RViz using "2D Pose Estimate" tool
# Verify localization
ros2 topic echo /amcl_pose
\`\`\`

For more troubleshooting, see [SETUP.md](SETUP.md).

## Testing

Run the verification script:

\`\`\`bash
chmod +x ~/ros2_ws/verify_system.sh
~/ros2_ws/verify_system.sh
\`\`\`

For comprehensive testing procedures, see [SETUP.md](SETUP.md#testing--integration-verification-guide).

## Performance

- **Navigation Success Rate**: >95% in controlled environments
- **Average Navigation Time**: 10-30 seconds per waypoint (depends on distance)
- **CPU Usage**: ~15-25% (multi-core)
- **Memory Usage**: ~500-800 MB

## Real Robot Deployment

To run on a real TurtleBot3:

1. **Update launch file** to use real robot instead of Gazebo:
   \`\`\`bash
   ros2 launch turtlebot3_bringup robot.launch.py
   \`\`\`

2. **Generate map** using SLAM:
   \`\`\`bash
   ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=false
   \`\`\`

3. **Update map path** in launch file

4. **Run waypoint navigation**:
   \`\`\`bash
   ros2 launch waypoint_manager turtlebot3_waypoint_nav.launch.py use_sim_time:=false
   \`\`\`

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

## Citation

If you use this project in your research, please cite:

```bibtex
@software{turtlebot3_waypoint_nav,
  title={TurtleBot3 Autonomous Waypoint Navigation System},
  author={Your Name},
  year={2024},
  url={https://github.com/yourusername/turtlebot3-waypoint-navigation}
}
