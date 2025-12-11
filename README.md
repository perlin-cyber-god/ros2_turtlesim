
🐢 ROS2 Catch-Them-All — Project README

A ROS2 (turtlesim-based) project designed to demonstrate how distributed robotic systems coordinate perception, decision-making, and actuation.
Even though turtlesim looks simple, the project implements a real robotics-style architecture: multi-node communication, async services, custom messages, and closed-loop control.


---

📌 Features

Dynamic turtle spawning using ROS2 services

Real-time tracking of all alive turtles

Automatic target selection (closest-first or FIFO)

Proportional control (P-controller) that drives /turtle1 toward a moving target

Custom ROS2 interfaces (Turtle, TurtleArray, CatchTurtle.srv)

Node-to-node coordination through topics, services, and timers



---

🔧 Project Architecture

Node 1: Turtle Spawner (turtle_spawner)

Handles world state:

Spawns new turtles at random positions

Maintains an internal list of all alive turtles

Publishes this list using TurtleArray

Implements catch_turtle service, which removes a turtle via /kill


Node 2: Turtle Controller (turtle_controller)

Handles robot behavior:

Subscribes to /turtle1/pose

Subscribes to alive_turtles

Selects a target turtle

Runs a 100 Hz control loop to chase the target

When close, calls catch_turtle service


The controller uses proportional control for:

Linear velocity based on distance

Angular velocity based on heading error


This results in smooth pursuit behavior similar to real mobile robots.


---

📁 Custom Interfaces

Turtle.msg

string name
float64 x
float64 y
float64 theta

TurtleArray.msg

Turtle[] turtles

CatchTurtle.srv

string name
---
bool success


---

🚀 How to Run the Project

1. Create a ROS2 workspace

mkdir -p ros2_ws/src
cd ros2_ws/src

2. Clone or add your package

git clone <your-repo-url>

3. Build the workspace

cd ~/ros2_ws
colcon build

4. Source the workspace

source install/setup.bash


---

▶️ Running the Whole System

Terminal 1: Start turtlesim

ros2 run turtlesim turtlesim_node


---

Terminal 2: Start the spawner

source ~/ros2_ws/install/setup.bash
ros2 run my_robot_interfaces turtle_spawner


---

Terminal 3: Start the controller

source ~/ros2_ws/install/setup.bash
ros2 run my_robot_interfaces turtle_controller


---

🧠 How It Works (Short Explanation)

1. Spawner node periodically creates turtles and publishes the complete list of alive turtles.


2. Controller node reads /turtle1/pose and uses the alive list to choose a target.


3. A proportional controller generates velocity commands:

Moves forward faster when farther from target

Rotates sharply toward the target



4. Once within a threshold (0.5 units), controller calls the catch_turtle service.


5. Spawner removes the turtle and publishes an updated list.


6. Controller picks a new target and continues.



This is the same style of decentralized architecture used in real robots:
perception → decision → actuation → feedback → repeat.


---

📓 Notes Included in This Repo

Debugging service callbacks

Handling race conditions

Finalizing node-to-node communication

Why async service calls matter

Control tuning & behavior differences


These notes reflect the process of designing a clean ROS2 architecture, not game logic.


---

📌 Future Extensions

Adding multi-robot chasing

Integrating visualization tools (rqt_graph, rviz markers)

Turning controller into a full PID system

Using Actions instead of services

Implementing predictive target pursuit



---

✔️ Summary

This project is a hands-on demonstration of a real robotics communication architecture inside ROS2, disguised inside a fun turtlesim environment.
It showcases:

multi-node systems,

distributed decision making,

feedback-based motor control,

and custom ROS2 interfaces.


