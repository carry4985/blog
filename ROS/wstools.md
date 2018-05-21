```Sh
catkin init
catkin config --merge-devel
catkin config --extend /opt/ros/kinetic
catkin config --cmake-args -DCMAKE_BUILD_TYPR=Release

cd src
wstool init
wstool merge *.rosinstall
wstool update -j 8
```

怎么写一个rosinstall文件呢：

```
- git:
   local-name: catkin_simple
   uri: https://github.com/catkin/catkin_simple.git
- git:
    local-name: asl_cmake_modules
    uri: git@github.com:ethz-asl/asl_cmake_modules.git
- git:
   local-name: eigen_catkin
   uri: https://github.com/ethz-asl/eigen_catkin
- git:
    local-name: aslam_splines
    uri: git@github.com:ethz-asl/aslam_splines.git
- git:
   local-name: aslam_incremental_calibration
   uri: git@github.com:ethz-asl/aslam_incremental_calibration.git
   version: feature/useGaussNewtonWithLine
- git:
    local-name: aslam_optimizer
    uri: git@github.com:ethz-asl/aslam_optimizer.git
    version: for/calibration_tools
- git:
   local-name: Schweizer-Messer
   uri: https://github.com/ethz-asl/Schweizer-Messer
- git:
   local-name: truncated_svd_solver
   uri: git@github.com:ethz-asl/truncated_svd_solver.git 
- git:
   local-name: glog_catkin
   uri: https://github.com/ethz-asl/glog_catkin
- git:
   local-name: eigen_checks
   uri: https://github.com/ethz-asl/eigen_checks
- git:
   local-name: libnabo
   uri: https://github.com/ethz-asl/libnabo
- git:
   local-name: libpointmatcher
   uri: https://github.com/ethz-asl/libpointmatcher
- git:
    local-name: pcl_catkin
    uri: git@github.com:ethz-asl/pcl_catkin.git
- git:
    local-name: suitesparse
    uri: git@github.com:ethz-asl/suitesparse.git
```

