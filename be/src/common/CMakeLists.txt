# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.


message (STATUS "Common ${BUILD_OUTPUT_ROOT_DIRECTORY}")
# where to put generated libraries
set(LIBRARY_OUTPUT_PATH "${BUILD_OUTPUT_ROOT_DIRECTORY}/common")

# where to put generated binaries
set(EXECUTABLE_OUTPUT_PATH "${BUILD_OUTPUT_ROOT_DIRECTORY}/common")

set(VERSION_CC_GEN_OUTPUT
  ${CMAKE_SOURCE_DIR}/be/src/common/version.cc
)

add_library(Common
  init.cc
  logging.cc
  status.cc
  kudu_version.cc
  ${VERSION_CC_GEN_OUTPUT}
)

add_custom_command(
  OUTPUT ${VERSION_CC_GEN_OUTPUT}
  COMMAND $ENV{IMPALA_HOME}/bin/gen_build_version.py
  COMMENT "Generating the version.cc file"
  VERBATIM
)
add_dependencies(Common gen-deps)

add_library(GlobalFlags
  global-flags.cc
)
add_dependencies(GlobalFlags gen-deps)

ADD_BE_TEST(atomic-test)

# Generate config.h from config.h.in, filling in variables from CMake
CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/config.h)
