---
title: 'Robot Operating System (ROS) for Beginners'
date: 2023-07-19
permalink: /posts/2023/07/ros_for_beginners
tags:
  - robotics
  - ROS
excerpt_separator: "<!--more-->"
classes: wide

toc: true
toc_sticky: true
h_max: 2
---

The Robot Operating System (ROS), in short, is a communication tool used in robotics to facilitate transfer of messages or data in a multi-device system. In this article, I go through the very basics of ROS and how to get started with it.<!--more-->


# Introduction

ROS is essentially a middleware that enables efficient hardware-software and software-software communication. It enables reusing code for robotics. ROS is not actually an operating system. ROS 1 runs on Linux (debian or ubuntu), and ROS 2 adds support for various other OSes (macOS, Windows, RTOS etc.).
ROS is comprised of 4 parts:

1.  Plumbing: handles process management, inter-proc comms, device drivers (basically like assembly level stuff)    
2.  Tools: for doing simulation, visualisation, GUI, data logging
3.  Capabilities: handles control, perception, planning, mapping and manipulation
4.  Ecosystem: refers to  package management, software distribution, documentation, tutorials and generally the ROS community

ROS is a P2P distributed framework and there are libraries for many different languages (e.g. Python, C++, Matlab, Java etc.). ROS is open source and free to use.

What is ROS used for?

1.  Used by many robotics universities/companies for research and prototyping
2.  Industry production robot software
3.  NASA uses ROS in some space robots (e.g. [Astrobee](https://github.com/nasa/astrobee))
4.  BMW uses ROS for automated driving
5.  See [here](https://github.com/vmayoral/ros-robotics-companies) for a list of companies that use ROS in their products
    
An [ROS Node](https://roboticsbackend.com/what-is-a-ros-node/#:~:text=A%20ROS%20node%2C%20according%20to,%2C%20services%2C%20actions%2C%20etc.) is a single-purpose executable program that is individually compiled, executed and managed. Nodes are organized in packages. In ROS 1, the ROS Master manages the communication between nodes. It registers every node at startup. Nodes are organized in a tree structure.

> Note: Nodes cannot have the same name. To run multiple instances of the same node, add a prefix/suffix or declare it as anonymous.

ROS uses the following tools to facilitate software-software communication:

*   Actions: Enables users to give directions that aren't time-sensitive to monitor the server's state
*   Topics: stream of messages relayed between nodes. Nodes can publish or subscribe to a topic. Typically 1 publisher to n subscribers.
*   Message: Data struct defining the type of a topic. Is defined in `.msg` files
*   Services: creates a synchronous channel btw nodes to allow users to request specific actions from robots and change settings
    

## ROS 1 vs ROS 2

[ROS 2](https://www.science.org/doi/10.1126/scirobotics.abm6074) is an updated version of the ROS 1 framework, made in 2018. It is a separate framework from the original ROS 1 due to various breaking changes and improvements made.  The latest ROS 1 update (version [Noetic Ninjemys](https://discourse.ros.org/t/ros-noetic-ninjemys-release/14262)) has an end-of-life date of May 2025, after which it will be replaced by ROS 2.


Changes made by ROS 2 (as of version [Humble Hawksbill](https://docs.ros.org/en/humble/)):

*   Introduced support for various OSes    
*   Enhanced transmission mechanism through removing reliance on master node and offering intra-process API
*   Improved timeliness of robots (to make up for ROS 1's lack of real-time control)
*   Improved network performance and support for multi-robot systems
*   Adds support for embedded microcontroller units (MCU) (e.g. M7, ARM-M4 etc.)

Note: The ROS 2 [wiki](https://docs.ros.org/en/humble/Citations.html) requests that you cite ROS 2 in your work like this:

```
@article{
doi:10.1126/scirobotics.abm6074,
author = {Steven Macenski and Tully Foote and Brian Gerkey and Chris Lalancette and William Woodall },
title = {Robot Operating System 2: Design, architecture, and uses in the wild},
journal = {Science Robotics},
volume = {7},
number = {66},
pages = {eabm6074},
year = {2022},
doi = {10.1126/scirobotics.abm6074},
URL = {https://www.science.org/doi/abs/10.1126/scirobotics.abm6074}
}
```

# Getting Started with ROS 2

The official ROS 2 [docs](https://docs.ros.org/en/humble/Installation.html) provide an excellent installation guide. Select the distro compatible with the OS you are running on. I use Iron Irwini for Ubuntu 22.04 LTS (bc my experience with simply *installing* ROS on windows has been absolute ass).


Ubuntu Installation Tips:

*   If using a fresh linux installation, remember to run `sudo apt update && sudo apt upgrade` before beginning any installation steps.
*   If encountering `unable to locate package` error, run `sudo apt update && sudo apt upgrade` and then redo the installation steps from the beginning.
*   If encountering `unmet dependencies` error, check which package versions have been reported and downgrade if necessary. If problem persists, consider install with the aptitude package manager instead of apt (`sudo apt-get install aptitude` )

Windows Installation Tips:

*   If using Window's Chocolatey, remember to run your shell as administrator.
*   ROS 2 docs also has an installation [debug guide](https://docs.ros.org/en/humble/How-To-Guides/Installation-Troubleshooting.html)

> If you want to install with Anaconda, RoboStack provides a package wrapper. Installation guide can be found [here](https://robostack.github.io/GettingStarted.html), but the ROS2 docs advises against it due to conda's different python interpreter being potentially incompatible with ROS 2  binaries. If you do go ahead with this, you may also want to check out [this](https://www.prouvost.dev/post/2021/c-development-with-conda/) guide for using C++ with Conda.

  
### Configuring the Environment and Workspace

The official docs provide a tutorial on configuring the ROS 2 environment, found [here](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html).
In summary, configuring the environment follows these steps:

1.  Source the environment
    
```
## linux
source /opt/ros/<distro_name>/setup.bash

## macOS
. ~/ros2_install/ros2-osx/setup.bash

## windows
call C:\\dev\\ros2\\local_setup.bat
```

2.  Check environment variables with

```
## linux/macOS
printenv | grep -i ROS  

## windows
set | findstr -i ROS
```
  
3.  Set the `ROS_DOMAIN_ID` variable (choose a number 0 - 101)
    
```
## linux / macOS
export ROS_DOMAIN_ID=<your_domain_id>

## windows
set ROS_DOMAIN_ID=<your_domain_id>
```  

4.  Set the `ROS_LOCALHOST_ONLY` variable
    
```
## linux / macOS
export ROS_DOMAIN_ID=<your_domain_id>

## windows
set ROS_DOMAIN_ID=<your_domain_id>
```

Normally, every time you open a new terminal, you will need to source ROS 2 and prepare the environment. However, step 1, 3 and 4 can be omitted by permanently adding sourcing to your shell startup script. This can be done with

```
## linux
echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bashrc

## macOS
echo "source ~/ros2_install/ros2-osx/setup.bash" >> ~/.bash_profile
echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bash_profile
echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bash_profile

## windows
# First create a folder in ‘My Documents’ called ‘WindowsPowerShell’. Within ‘WindowsPowerShell’, 
# create file ‘Microsoft.PowerShell_profile.ps1’. Inside the file, paste: C:\\dev\\ros2_foxy\\local_setup.ps1

# in an elevated terminal run:
Unblock-File C:\\dev\\ros2_foxy\\local_setup.ps1
setx ROS_DOMAIN_ID <your_domain_id>
setx ROS_LOCALHOST_ONLY 1
```

### Using Turtlesim, ros2 and rqt

| Turtlesim : lightweight simulator for learning ROS 2 (very similar to python's turtle)
| ros2 : pkg allowing users to manage, introspect and interact with a ROS system.
| rqt : GUI for ROS 2 (optional)

Follow the official humble tutorial on Turtlesim, ros2 and rqt [here](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html).  

Before running any ROS program, always run `roscore` to initialize the basic ROS node communication system. Alternatively, you can use `ros launch` with your project as well, which automatically starts `roscore` along with whatever package is specified to be run. More information on the `ros launch` command can be found in the section below.

# Understanding ROS 2 CLI Tools

## The Basics

### ROS Nodes

[Nodes](https://docs.ros.org/en/iron/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html) are small programs meant to control a single task (i.e. controlling wheel motors, publishing data from a laser range-finder etc.). Each node can send and receive data from other nodes via topics, services, actions or parameters. In ROS 2, a single executable can contain one or more nodes.


| Operation | Command | Note |
| ------------ | ---------- | ----------- | 
| Run a node | `ros2 run <package_name> <executable_name>` | |
| List running nodes | `ros2 node list` | |
| Remap nodes | `ros2 run <package> <node> --ros-args --remap __node:=<new_node_name>` | Remapping allows you to reassign default node properties, like node name, topic names, service names, etc., to custom values.|
| Get node info | `ros2 node info <node_name>` | |

  

### [Launching Multiple Nodes](https://docs.ros.org/en/iron/Tutorials/Intermediate/Launch/Launch-Main.html)

For large projects, many nodes will need to be run at once. It becomes cumbersome to open a new terminal and manually run each node with `run` . Hence, launch files can be created, which allows the startup and configuration of multiple executables containing ROS nodes simultaneously.

| Operation | Command | Note |
| ------------ | ---------- | ----------- | 
|Run launch file | `ros2 launch <package_name> <launch_file_name>` or `ros2 launch <path_to_launch_file>` | optional arguments can be passed using the key:=value syntax with the launch command

Launch files can be written in Python, YAML, or XML. Examples of launch files can be found [here](https://docs.ros.org/en/iron/How-To-Guides/Launch-file-different-formats.html). In general, python provides a greater flexibility in writing launch files compared to YAML or XML, since it is a scripting language and you can leverage python libraries to script your launch files. Using python also grants you lower level access to launch features that are inaccessible to YAML or XML formats, as the ros2 implementation of `launch` is written in python. However, launch files written in Python may also be more complex and verbose than XML or YAML files.


### Writing Your Own Launch Files

Launch files can perform the following actions:

*   Setup command line arguments with defaults
*   Include another launch file (even if in another namespace)
*   Start a node and setting its namespace and setting parameters in that node (using the args)
*   Create a node to remap messages from one topic to another

### ROS Topics  

[Topics](https://docs.ros.org/en/iron/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html) act as a bus for nodes to exchange messages, and are based on a publisher-subscriber model. A node can publish data and subscribe to to any other number of topics. Node communication can be one-to-one, one-to-many, many-to-one, and many-to-many. Topics are one of the main ways data is moved between different parts of the system.

  
| Operation | Command | Note |
| ------------ | --------------------- | ----------- | 
| List active Topics | `ros2 topic list` | Use `-t` to see topic types |
| View data  published in Topic | `ros2 topic echo <topic_name>` | |
| View Topic info | `ros2 topic info <topic_name>` | |
| View Message info | `ros2 interface show <msg_type>` <br><br> To find msg_type, use: `ros2 topic list -t` | Shows structure of the data the message expects for a specific message type.
| Publish some data to Topic | `ros2 topic pub <topic_name> <msg_type> '<args>'` <br><br> Use `--once` to publish one message and exit <br> Use `--rate x` to publish data at a steady stream of x Hz | The `<args>`  argument is the actual data you’ll pass to the topic, using the format specified by its type. | 
| View rate of Topic | `ros2 topic hz <topic_name>` | |


### ROS Services
  
ROS [Services](https://docs.ros.org/en/iron/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.html) are another method of communication between nodes, and is based on a request-response model. Services provide data only when called by a client. Service servers operate on a  one-to-one or one-to-many basis, where there can only be one server for a service, but many clients using it.

  
  
| Operation | Command | Note |
| ------------ | ---------- | ----------- | 
| List active services | `ros2 service list` <br><br> Use `-t` to show types | |
| Show specific service type | `ros2 service type <service_name>` | |
| Find all services of type | `ros2 service find <type_name>` | |  
| Show input/output format of service | `ros2 interface show <type_name>.srv` | Returns the input and output format, delimited by `---` |
| Call a service | `ros2 service call <service_name> <service_type> <arguments>` | |  

### ROS Parameters

A [parameter](https://docs.ros.org/en/foxy/Concepts/About-ROS-2-Parameters.html) is a configuration value of a node. They can be floats, ints, bool, strings and lists. Each node in ROS 2 maintains its own parameters. Parameters can be`get` and `set` using the CLI, or

  
| Operation | Command | Note |
| ------------ | ---------- | ----------- | 
| List parameters for each active node | `ros2 param list` | |  
| Show type and current value of param | `ros2 param get <node_name> <parameter_name>` | |
| Change a param value at runtime | `ros2 param set <node_name> <parameter_name> <value>` | This is a temp change |
| View all current param values for node | `ros2 param dump <node_name>` <br><br> To save output to file, use `ros2 param dump <node_name> > <file_name>.yaml` | Useful for reloading nodes with the same params |
| Load param on active node from file | `ros2 param load <node_name> <parameter_file>` | |  
| Start node with param from file | `ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>` | | 

### ROS Actions

[Actions](https://docs.ros.org/en/iron/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.html) are one of the communication types in ROS 2. They are like services that allow you to execute long running tasks, provide regular feedback, and are cancelable. They consist of a goal, feedback and result. Actions use a client-server model. An action client node sends a goal to the action server node. The action server acknowledges the goal and returns a steady stream of feedback, and a result.

  
| Operation | Command | Note |
| ------------ | ---------- | ----------- | 
| List all actions in rqt_graph | `ros2 action list` <br><br> Use `-t` to show types | |
| Show specific action info | `ros2 action info <action>` | |
| Show format of goal, result and feedback | `ros2 interface show <action>` | Delimited by `---` , in the order of goal, result and feedback
| Send goal | `ros2 action send_goal <action_name> <action_type> <values>` <br><br> Use `--feedback` to see the feedback of the goal until completion | `<values>` should be in YAML format |

Actions could be used for robot systems in navigation, where a goal informs the robot of the end destination. While the robot travels to the position, it send feedback updates along the way, and returns a final result message upon goal completion.


## `rqt` Monitoring Tools

### ROS Graph with `rqt_graph`
  

The ROS 2 graph is a network of various ROS 2 elements processing data together at the same time. This includes all executables and connections between them. Changing nodes, topics and their connections can be visualised with the `rqt_graph` tool.

To run, use `rqt_graph` in a terminal.

`rqt_graph` is very useful when examining more complex systems with many nodes and topics connected in different ways.

### Viewing Logs with `rqt_console`

`rqt_console` is a GUI tool used to introspect log messages, which are the messages that typically also appear in the terminal. With `rqt_console` , log messages can be accumulated and viewed closely in a more organized manner with filtering tools, and can be saved and reloaded for later viewing.
  

The content of logs are messages related to events and node status. Logs are ordered by severity:
1.  Fatal: system is going to terminate to protect itself
2.  Error: significant issues preventing proper function, but not damaging to the system
3.  Warn: unexpected activity or non-ideal results, may indicate a deeper issue
4.  Info (default level): event and status updates
5.  Debug: step-by-step process of system execution

The default level can be changed, which would indicate logging of messages of that severity level and higher (more critical).

  
| Operation | Command | Note |
| ------------ | ---------- | ----------- | 
| Start the console | `ros2 run rqt_console rqt_console` | Opens a GUI window |
| Set default severity level | `ros2 run <package> <node> --ros-args --log-level WARN` | Sets default severity level when a node is run by adding `--log-level` to the run command |

Monitoring the process through the`rqt_console` is a good practice for debugging the system.
  
### Recording and Playback Data
  
Data published by topics during it's process can be recorded and played back with the `ros2 bag` command. Saved data is saved as either a `.mcap` or `.db3` file.

As a good practice, create a new directory to store saved recordings:

```
mkdir bag_files
cd bag_files
```
  
To record, first start the desired nodes. The record operation creates a new node that subscribes to the stated topics, and saves their published data to a file.
  
| Operation | Command | Note |
| ------------ | ---------- | ----------- | 
| Record topic(s) | `ros2 bag record <topic_name>` <br><br> Use `-o` to specify file name for the bag file. <br> Useful since default file name looks like `rosbag2_year_month_day-hour_minute_second` | To record multiple topics, simply list all topic names with the record command. Press `ctrl+C` to terminate. |
| See recorded file info | `ros2 bag info <bag_file_name>` | Open the database to view individual messages (MCAP or SQLite3) |
| Play recorded file | `ros2 bag play <bag_file_name>` | |

`ros2 bag` should be installed by default with your ROS installation. However, if not found in your system, you can install following this [README](https://github.com/ros2/rosbag2).

### Creating and Building ROS Packages

A package is like a container for your ROS 2 code. This facilitates ease of installation and sharing, allowing others to build and use your code easily. ROS packages can be created with either CMake or Python, and uses `ament`  as its build system and `colcon`  as its build tool.

> For Windows Users, you need to be in a [Visual Studio environment](https://docs.ros.org/en/iron/Installation/Alternatives/Windows-Development-Setup.html#windows-dev-build-ros2) to build packages

If not already installed, you can install `colcon` with:

```
## linux
sudo apt install python3-colcon-common-extensions

## macOS
python3 -m pip install colcon-common-extensions

## windows
pip install -U colcon-common-extensions
```

ROS 2 packages have their minimum required content, depending on whether CMake or Python is used:

  

|  | CMake | Python |
| ------------- | ------------ | ----------------|
| Command | `ros2 pkg create --build-type ament_cmake <package_name>` <br><br> Use the optional `--node-name <node_name>` param to create an executable in the package <br> Use the optional `--cmake-args -DBUILD_TESTING=0` param to avoid configuring and building tests | `ros2 pkg create --build-type ament_python <package_name>` <br><br> Use the optional `--node-name <node_name>` param to create an executable in the package |
| Requirements | `package.xml`  file containing meta information about the package <br> `CMakeLists.txt file`  that describes how to build the code within the package <br> `package.xml`  file containing meta information about the package | `setup.py`  containing instructions for how to install the package <br> `setup.cfg` is required when a package has executables, so `ros2 run` can find them <br> `/<package_name>`  - a directory with the same name as your package, used by ROS 2 tools to find your package, contains `__init__.py`|
| Sample Directory Structure | my_package/ <br> CMakeLists.txt <br> package.xml <br> my_package/ | setup.py <br> package.xml <br> resource/my_package |

Organizing your work into packages in your workspace is important since it allows you to build many packages at once by running `colcon build` in the workspace root. If there are many packages, `colcon build` may take a long time to run. You can specify specific packages you want to build instead with `colcon build --packages-select <package_name>` .

  
In summary, to create and use your packages, do:

1.  Create package with CMake or Python  
2.  Build package with `colcon build --packages-select <package_name>`
3.  Source setup file if necessary with `. install/setup.bash` on linux or `call install/local_setup.bat` on windows
4.  Run the package with`ros2 run <package_name> <node_name>`

> `colcon` is an iteration on the ROS build tools `catkin_make`, `catkin_make_isolated`, `catkin_tools` and `ament_tools`. For more information on the design of colcon see [this document](https://design.ros2.org/articles/build_tool.html).


### Examining ROS Packages

You can see the autogenerated files and folders created by ROS in the `<workspace>/src/<package_name>` directory.  Custom nodes can be found inside the package directory.
  
By default, the package description, maintainer and license declaration are not  set. These fields are required if the package is released for distribution. These can be modified in the `package.xml` file in your package directory.
  

## ROS Workspaces

A ROS workspace refers to the filesystem structure of your project. A ROS workspace may look something like this:

```
.
├── build
├── install # contains setup files to source the overlay
├── log # ros log files
└── src # main code
```
  
To create a workspace, use the following commands:

```
## linux / macOS
mkdir -p ~/ros2_ws/src # create workspace
cd ~/ros2_ws
git clone https://github.com/ros2/examples src/examples -b iron # clone or add source code
rosdep install -i --from-path src --rosdistro iron -y # always resolve dependencies before building (skip if macOS)
colcon build --symlink-install # build workspace
colcon test # optional: run test for built packages

## windows
md \\dev\\ros2_ws\\src # create workspace
cd \\dev\\ros2_ws
git clone https://github.com/ros2/examples src/examples -b iron # clone or add source code
colcon build --symlink-install --merge-install # build workspace
colcon test # optional: run test for built packages
```

For more detailed explanations of each step, see this [tutorial](https://docs.ros.org/en/iron/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html). To ignore certain packages in your build, you can specify a `COLCON_IGNORE` file in the directory.

> You can also run specific tests for the package with the command: `colcon test --packages-select <package_name> --ctest-args -R <test_name>`

  
### Sourcing the Overlay

First, open a new terminal to avoid complex issues which may arise if you use the same terminal environment to build and source the overlay at the same time.  
In the new environment, source the main ROS 2 environment as the underlay (if not auto sourced). To build the overlay, do:

```
source /opt/ros/iron/setup.bash
cd ~/ros2_ws
source install/local_setup.bash
```
  
To modify the overlay, you can edit the files in your workspace `/src` folder. This will only change the outputs for the environment with the overlay sourced, it will not change your base ROS 2 installation, so new terminals that don't source your additional overlay will not have any changes. After modifying the code, run `colcon build` again.

  
### Changing CWD with `colcon_cd`

`colcon_cd <package_name>`  is a command that allows you to quickly change the working directory of your shell to the directory of a package. This can be set up with:

```
## linux
echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
echo "export _colcon_cd_root=/opt/ros/iron/" >> ~/.bashrc

## macOS
echo "source /usr/local/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
echo "export _colcon_cd_root=~/ros2_install" >> ~/.bashrc
```
> [Usage](https://colcon.readthedocs.io/en/released/user/installation.html#quick-directory-changes) of this command depends on the way `colcon_cd` is installed and where the workspace is. This command is not available on Windows.

  

### Setup `colcon` Tab Completion

The `colcon` command also supports CLI tab completion for bash and bash-like shells. To enable, install the `colcon-argcomplete` package:

```
## linux
echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc

## macOS
echo "source $HOME/.local/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bash_profile
```

## Glossary


| Concept | Explanation |
| ---------------- | --------------------|
| Nodes | An executable program that uses ROS to communicate with other nodes |
| Messages | A ROS data type. Used when subscribing or publishing to a topic |
| Topics | A program that broadcasts data periodically to subscribed nodes |
| Services | Sends some data to a client upon request |
| Actions | Program for long running tasks. Consists of a goal, feedback stream and result report.  Can be cancelled if required. |
| Master | The name service for ROS (helps nodes find each other) |
| Launch file | A file specifying run instructions for a collection of nodes, so they can be run together |
| ROS bag | The CLI tool used to record and playback data |
| `rosout` | ROS equivalent of `stdout/stderr` |
| `roscore` | Initializes the ROS Master, `rosout`  logging mode and parameter server |

## Links and Resources

### Tutorials
*   [http://wiki.ros.org/ROS/Tutorials](http://wiki.ros.org/ROS/Tutorials) (ROS 1) 
*   [https://docs.ros.org/en/iron/Tutorials.html](https://docs.ros.org/en/iron/Tutorials.html) (ROS 2)
*   [https://docs.ros.org/en/iron/Releases.html](https://docs.ros.org/en/iron/Releases.html) (ROS 2 Distributions)
*   [https://www.youtube.com/playlist?list=PLE-BQwvVGf8HOvwXPgtDfWoxd4Cc6ghiP](https://www.youtube.com/playlist?list=PLE-BQwvVGf8HOvwXPgtDfWoxd4Cc6ghiP)
*   [https://robostack.github.io](https://robostack.github.io)
*   [https://husarion.com/tutorials/](https://husarion.com/tutorials/)
  

### Practice Resources
*   [https://www.duckietown.org/instructors/classes/educational-resources](https://www.duckietown.org/instructors/classes/educational-resources)
*   [https://www.theconstructsim.com](https://www.theconstructsim.com)
  

### Notable Articles
*   [https://jfrog.com/connect/post/what-is-ros-and-why-its-needed/](https://jfrog.com/connect/post/what-is-ros-and-why-its-needed/)
*   [https://design.ros2.org/articles/why_ros2.html](https://design.ros2.org/articles/why_ros2.html)
*   [https://www.stereolabs.com/blog/ros-and-nvidia-jetson-nano/](https://www.stereolabs.com/blog/ros-and-nvidia-jetson-nano/)