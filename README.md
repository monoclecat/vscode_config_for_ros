This is the README for the `vscode_config_for_ros` repository.

# Installation

Simply clone this repository into a folder outside of your catkin workspace and copy its contents into your existing catkin/VS-Code workspace. 


# Contents of this repository

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

    "#include errors detected. Please update your includePath. Squiggles are disabled for this translation unit ..."

This problem is explained in this under-appreciated [SO answer](https://stackoverflow.com/a/62921325/5568461). 

To solve this issue, the script `bash_scripts/concat_compile_commands.sh` traverses all folders in the `build` directory and concatenates the contents of the individual compile command files into one big file which is placed at the base of the `build` directory. 
This shell script is run after each build, as you can see in `.vscode/tasks.json`.

This script is by @ndepal and taken from this [issue](https://github.com/catkin/catkin_tools/issues/551), which is also referenced in the previously-mentioned SO answer. 

_Note: If you are experiencing weird behaviour, such as that `catkin build` doesn't create compile command files even though the option is set, try running a good ol' `catkin clean` first._

# `tasks.json` and `launch.json` in `.vscode`

These three files are created according to the instructions by Tahsincan Kose in his blog post [A Decent Integration of VSCode to ROS](https://medium.com/@tahsincankose/a-decent-integration-of-vscode-to-ros-4c1d951c982a), plus a bit of my own configuration which I found helpful. 

`.vscode/tasks.json` includes tasks which are mapped to the function keys <kbd>F7</kbd> to <kbd>F10</kbd>, covering build, clean, test and release cases. 
Note that the build task can also be triggered with <kbd>ctrl+shift+B</kbd> due to its label being "build". 

Note that in order to get the keybindings working you need to copy the `keybindings.json` file into `~/.config/Code/User` on Ubuntu. 

`launch.json` contains a launch configuration for debugging, which stops at breakpoints defined in the executable. Each executable you wish to debug must be specified in the `options` parameter of the `executable_to_debug` input. 

