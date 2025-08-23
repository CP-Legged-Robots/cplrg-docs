The  [Github](https://github.com/jack33001/CP-Quadruped-Control) repository contains a number of ROS2 packages, and a Dockerfile. Once the docker container is built and run, you can use the packages. 
# imu_hardware_interface  
===Jeremy===
# py_pubsub  
===Jeremy===
# quadruped_bringup  
Bringup package for the system, which manages starting up the different packages/subsystems in the correct timing. The only important files are in ```launch/```, and can be launched using ```ros2 launch *filename*```. 

| Filename         | Use                                                                       |
| ---------------- | ------------------------------------------------------------------------- |
| gz.launch.py     | Use to simulate MPC in Gazebo                                             |
| real.launch.py   | Use to launch MPC on the real robot on the Jetson                         |
| remote.launch.py | Use to remotely connect to the Jetson (must be run before real.launch.py) |
# quadruped_hardware  
===Jeremy===

# quadruped_mpc  
MPC package for the system. All controllers are implemented in ros2_control, except for quadruped_teleop, which is implemented in ROS2 and is just a placeholder for a trajectory generation process. 

| controller_interface   | Function                                                                                                                                              |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| state_estimator        | Estimate important state variables from sensor inputs. Houses the kinematic model of the system.                                                      |
| gait_pattern_generator | Trigger foot FSM transitions.                                                                                                                         |
| balance_controller     | MPC.                                                                                                                                                  |
| foot_controller        | Translate desired foot forces (from MPC during stance) or foot positions (from gait pattern generator during flight) to joint torques and apply them. |
```controller_interfaces``` are defined in the ```include/``` and ```src/``` folders. Each has one C++ file and two .hpp files (one for core function definitions, one for everything else).```.cpp``` files are stored in the ```src/``` folders, and contain only ROS2 lifecycle node functions. This is for the sake of legibility, to prevent excessively long files. All non-lifecycle functions are run inside the ```update()``` loop. These non-lifecycle functions are stored in ```.hpp``` files at ```include/quadruped_mpc/control_laws```. The other ```.hpp``` information, like function prototypes and variable definitions, exist in ```include/quadruped_mpc/controller_interfaces```.

The other main set of files are those used to generate the MPC using the ACADOS package, which are stored at ```scripts/acados```.

| File                       | Function                                                                                                                                                                                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| quadruped_model.py         | Defines the ACADOS model used in the MPC.                                                                                                                                                       |
| optimal_controller.py      | Defines the optimal control problem (MPC).                                                                                                                                                      |
| test_optimal_controller.py | A quick-and-dirty test to make sure the optimal controller works, given reasonable inputs. Not a great predictor of system stability, but a good sanity check for early controller development. |
| generate_controller.py     | Compiles the optimal controller and model into C code, to be run by the controller_interfaces.                                                                                                  |
There are also a few miscellaneous files.

| Filepath (from package directory)                                                                | Function                                                                                                                                         |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| ```config/quadruped_controllers.yaml```                                                          | ros2_control configuration file for the system. Contains a number of different parameters like number of actuators, step order, and MPC weights. |
| ```inc/quadruped_mpc/utilities/KalmaFilter.hpp```<br>and<br>```src/utilities/KalmanFilter.cpp``` | Kalman filter C++ library to facilitate future state estimator development.                                                                      |
| ```quadruped_teleop.py```                                                                        | Simple ROS2 node giving desired state commands to the MPC. Placeholder for future trajectory generators.                                         |

# quadruped_msgs  
Custom message definitions used in the project. All message definitions are stored in the ```msgs``` folder.

| Filename           | Use                                                                                    |
| ------------------ | -------------------------------------------------------------------------------------- |
| FootForces.msg     | Desired 3D force at each foot, and total force, generated by ```balance_controller```. |
| FootStates.msg     | Current foot states, generated by ```gait_pattern_generator```.                        |
| GaitPattern.msg    | Gait pattern information, generated by ```gait_pattern_generator```.                   |
| QuadrupedState.msg | Robot state information, generated by ```state_estimator```.                           |

# quadruped_sim  
Manages setting up simulations (currently only Gazebo). ```gazebo_sim.launch.py``` launches a simulation in Gazebo.
# quadruped_urdf  
Contains the URDF definition of the robot. The URDF is split up into a few ```.xacro``` files, which can be mixed and matched depending on the purpose. All these files (and the ```.stl``` meshes of the robot) are in the ```.urdf``` folder. The correct ```.xacro``` file for a given situation (for example, sim vs real) are chosen in the launch files. Currently, only ```sim_urdf.launch.py``` and ```real_urdf.py``` exist.

# quadruped_utils
===jeremy===