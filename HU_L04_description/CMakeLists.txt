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

if(DEFINED ENV{ROS_VERSION})
  if($ENV{ROS_VERSION} EQUAL 1)
    # In ROS1 compilation environment
    message("Currently in ROS1 compilation environment")

    find_package(catkin REQUIRED COMPONENTS roscpp)

    catkin_package(CATKIN_DEPENDS)

    # Install 'meshes', 'urdf', 'xml' and 'world' to the package share directory
    install(DIRECTORY meshes urdf xml world DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
  elseif($ENV{ROS_VERSION} EQUAL 2)
    # In ROS2 compilation environment
    message("Currently in ROS2 compilation environment")

    find_package(ament_cmake REQUIRED)

    install(DIRECTORY meshes urdf xml world DESTINATION share/${PROJECT_NAME})

    ament_package()
  else()
    message(WARNING "Unknown ROS version: ${ROS_VERSION}")
  endif()
else()
  if(DEFINED ENV{AMENT_PREFIX_PATH})
    message("Currently is not in ROS compilation environment")
    find_package(ament_cmake REQUIRED)
    install(DIRECTORY meshes urdf xml world DESTINATION etc/${PROJECT_NAME})
    ament_package()
  endif()
endif()
