# Copyright 2025 LimX Dynamics Technology Co., Ltd.
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Declare the minimum required CMake version
cmake_minimum_required(VERSION 3.10)

# Declare the project
project(HU_L04_description)

# usd/ and usd_parallel/ hold the Isaac Sim / Isaac Lab assets. They are about
# 200 MB together, because usd/configuration/HU_L04_01_base.usd alone is 97 MB
# and its usd_parallel counterpart is 103 MB. ROS, Gazebo and MuJoCo never read
# them, and copying them on every build roughly doubles the install space, so
# they are opt-in. Enable them with one of:
#
#   catkin_make -DINSTALL_USD=ON
#   catkin build --cmake-args -DINSTALL_USD=ON
#   colcon build --cmake-args -DINSTALL_USD=ON
#
option(INSTALL_USD "Install usd/ and usd_parallel/ (~200 MB) into the package share directory" OFF)

set(HU_L04_DESCRIPTION_INSTALL_DIRS meshes urdf xml world)
if(INSTALL_USD)
  list(APPEND HU_L04_DESCRIPTION_INSTALL_DIRS usd usd_parallel)
  message(STATUS "HU_L04_description: installing usd/ and usd_parallel/ (~200 MB)")
else()
  message(STATUS "HU_L04_description: skipping usd/ and usd_parallel/, configure with -DINSTALL_USD=ON to install them")
endif()

if(DEFINED ENV{ROS_VERSION})
  if($ENV{ROS_VERSION} EQUAL 1)
    # In ROS1 compilation environment
    message("Currently in ROS1 compilation environment")

    find_package(catkin REQUIRED COMPONENTS roscpp)

    catkin_package(CATKIN_DEPENDS)

    install(DIRECTORY ${HU_L04_DESCRIPTION_INSTALL_DIRS} DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
  elseif($ENV{ROS_VERSION} EQUAL 2)
    # In ROS2 compilation environment
    message("Currently in ROS2 compilation environment")

    find_package(ament_cmake REQUIRED)

    install(DIRECTORY ${HU_L04_DESCRIPTION_INSTALL_DIRS} DESTINATION share/${PROJECT_NAME})

    ament_package()
  else()
    message(WARNING "Unknown ROS version: ${ROS_VERSION}")
  endif()
else()
  if(DEFINED ENV{AMENT_PREFIX_PATH})
    message("Currently is not in ROS compilation environment")
    find_package(ament_cmake REQUIRED)
    install(DIRECTORY ${HU_L04_DESCRIPTION_INSTALL_DIRS} DESTINATION etc/${PROJECT_NAME})
    ament_package()
  endif()
endif()
