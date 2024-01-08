cmake_minimum_required(VERSION 2.8.3)
project(vins_estimator)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++14")
#-DEIGEN_USE_MKL_ALL")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(catkin REQUIRED COMPONENTS
    roscpp
    std_msgs
    geometry_msgs
    nav_msgs
    tf
    cv_bridge
    )

find_package(OpenCV REQUIRED)

# message(WARNING "OpenCV_VERSION: ${OpenCV_VERSION}")

set(Eigen3_DIR "~/Desktop/thirdparties/eigen-3.4.0/share/eigen3/cmake")
find_package(Eigen3 REQUIRED PATHS ${Eigen3_DIR})
message(STATUS "Building ${PROJECT_NAME}: Eigen Version - ${EIGEN3_VERSION_STRING} ...")
set(EIGEN_INCLUDE_DIRS "~/Desktop/thirdparties/eigen-3.4.0/include/eigen3")
include_directories(${EIGEN_INCLUDE_DIRS})

# find_package(Ceres REQUIRED)
set(Ceres_DIR "~/Desktop/thirdparties/ceres-2.1.0/lib/cmake/Ceres")
find_package(Ceres REQUIRED PATHS ${Ceres_DIR})
message(STATUS "Building ${PROJECT_NAME}: Ceres Version - ${CERES_VERSION} ...")
set(CERES_INCLUDE_DIRS "~/Desktop/thirdparties/ceres-2.1.0/include")
include_directories(${CERES_INCLUDE_DIRS})

include_directories(${catkin_INCLUDE_DIRS} ${CERES_INCLUDE_DIRS})

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
find_package(Eigen3)
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
)

catkin_package()

add_executable(vins_estimator
    src/estimator_node.cpp
    src/parameters.cpp
    src/estimator.cpp
    src/feature_manager.cpp
    src/factor/pose_local_parameterization.cpp
    src/factor/projection_factor.cpp
    src/factor/projection_td_factor.cpp
    src/factor/marginalization_factor.cpp
    src/utility/utility.cpp
    src/utility/visualization.cpp
    src/utility/CameraPoseVisualization.cpp
    src/initial/solve_5pts.cpp
    src/initial/initial_aligment.cpp
    src/initial/initial_sfm.cpp
    src/initial/initial_ex_rotation.cpp
    )


target_link_libraries(vins_estimator ${catkin_LIBRARIES} ${OpenCV_LIBS} ${CERES_LIBRARIES}) 


