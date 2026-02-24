# Essentials of a URDF File

A **URDF (Unified Robot Description Format)** file is an XML-based format used in ROS to describe the physical structure of a robot. It defines the robot’s kinematic structure, geometry, and inertial properties so that simulation, visualization, and control tools can correctly interpret the system.

It is inevitable that the robot’s URDF will require updates whenever hardware is added or modified. In a simulation environment, keeping the URDF current is essential to ensure accurate visualization, kinematics, collision behavior, and dynamic response.

For physical hardware operation, URDF updates are primarily necessary when changes affect the robot’s kinematic structure — such as adding or removing an actuator, modifying joint types, or altering link geometry. Minor hardware changes that do not affect the robot’s structure (e.g., electronics, wiring, or embedded components) typically do not require URDF modifications.

A URDF model is built from two fundamental elements: **links** and **joints**.

## Links

A **link** represents a rigid body in the robot (e.g., base, thigh, shank, foot). Each link can include:

- **Visual geometry** – What the robot looks like in visualization tools (RViz, Gazebo).
- **Collision geometry** – Simplified shapes used for physics and collision detection.
- **Inertial properties** – Mass, center of mass, and inertia tensor, which are required for dynamic simulation.

Without accurate inertial properties, physics simulations will not behave realistically.

## Joints

A **joint** connects two links and defines how they move relative to each other. Each joint specifies:

- **Parent link**
- **Child link**
- **Joint type** (`revolute`, `continuous`, `prismatic`, `fixed`, etc.)
- **Axis of motion**
- **Origin transform** (position and orientation between links)
- **Limits** (position, velocity, effort)

Together, the joints define the robot’s kinematic chain.

## Kinematic Tree Structure

A URDF describes a **tree structure**, meaning each link has only one parent (no closed loops). This is important for legged robots, where each leg branches from a central base link.

## Coordinate Frames

Each link and joint introduces coordinate frames. Proper frame placement is essential for:

- Forward and inverse kinematics  
- Jacobian calculations  
- State estimation  
- Controller implementation  

Incorrect frame definitions often cause unexpected motion in simulation.

## Supporting Files

URDF files often reference external mesh files (e.g., `.stl`, `.dae`) for detailed geometry. They are commonly written using **xacro**, a macro-based extension that allows parameterization and reuse (helpful for symmetric legs in quadrupeds).

## Resources

**URDF information:** https://www.youtube.com/watch?v=CwdbsvcpOHM&t=817s

**Making a URDF in Fusion 360:** https://www.youtube.com/watch?v=cQh0gNfb6ro