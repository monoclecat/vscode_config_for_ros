This is the README for the `vscode_config_for_ros` repository.

## Installation

Simply clone this repository into a folder outside of your catkin workspace and copy its contents into your existing catkin/VS-Code workspace. 


## Contents of this repository

### `bash_scripts/concat_compile_commands.sh`

`concat_compile_commands.sh` is especially useful if you are using [catkin-tools](https://catkin-tools.readthedocs.io/en/latest/index.html) instead of `catkin_make`. 
While `catkin_make` is the tool of choice in the official ROS [workspace creation tutorial](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment), there are [compelling reasons](https://robotics.stackexchange.com/questions/16604/ros-catkin-make-vs-catkin-build) to be using `catkin-tools`. 

There is one drawback however. When building your catkin workspace, `catkin-tools` places the `compile_commands.json` files of your built ROS-packages into their respective build folders, like this: 
```
build
├── ros_package1
│   ├──  ...
│   └── compile_commands.json
├── ros_package2
│   ├──  ...
│   └── compile_commands.json
└── ros_package3
    ├──  ...
    └── compile_commands.json
``` 

This is an issue for VS-Code, as it expects a single `compile_commands.json` at the base of the `build` directory. 
The result is that autocomplete doesn't work and squiggles appear beneath `#include` statements with an error like: 

    "include errors detected. Please update your includePath. Squiggles are disabled for this translation unit ..."

This problem is explained in this under-appreciated [SO answer](https://stackoverflow.com/a/62921325/5568461). 
The script `bash_scripts/concat_compile_commands.sh` traverses all folders in the `build` directory and concatenates the contents of the individual compile command files into one big file which is placed at the base of the `build` directory. 
This script is taken from this [issue](https://github.com/catkin/catkin_tools/issues/551), which is also referenced in the previously-mentioned SO answer.

    If you are experiencing weird behaviour, such as that `catkin build` doesn't create compile command files even though the option is set, try running a good ol' `catkin clean` first.

 
