# Azure Kinect - OpenCV KinectFusion Sample

This sample is taken from [this](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/opencv-kinfu-samples) repository and modified so that it can be used with CMake.

## ArchLinux instructions

Just install the dependencies using AUR.

* opencv4
* vtk

## Original Introductions

The Azure Kinect - OpenCV KinectFusion sample shows how to use the Azure Kinect SDK with the KinectFusion from opencv_contrib's rgbd module (https://github.com/opencv/opencv_contrib/tree/master/modules/rgbd). This sample demonstrates how to feed calibration and undistorted depth images from Azure Kinect to the OpenCV's KinectFusion module. We render live KinectFusion results and generate fused point cloud as ply when user exits application.

The sample requires user having OpenCV/OpenCV_Contrib/VTK installed. The following steps are tested on Windows with OpenCV 4.1.0 (user should configure the right path of OpenCV dependencies):
- Download and install opencv-4.1.0 from https://opencv.org/releases/ (e.g. on windows, download the opencv-4.1.0-vc14_vc15.exe, and extract contents), then, copy opencv\build\include\* to the opencv-kinfu-samples\extern\opencv-4.1.0\include\* (please create extern directory and subdirectories accordingly).
- Download opencv_contrib-4.1.0 source from https://github.com/opencv/opencv_contrib/releases and extract contents, then copy opencv_contrib-4.1.0\modules\rgbd\include\* to extern\opencv_contrib-4.1.0\modules\rgbd\include\*, and copy opencv_contrib-4.1.0\modules\viz\include\* to extern\opencv_contrib-4.1\modules\viz\include\*.
- Download VTK-8.2.0 from https://vtk.org/download/ and build the source accordingly.
- Follow the instruction from opencv_contrib (https://github.com/opencv/opencv_contrib) to build opencv with extra modules (we used cmake-gui to generate opencv sln file and built opencv and opencv_contrib modules with Visual Studio 2017, user needs to configure the WITH_VTK, VTK_DIR and OPENCV_ENABLE_NONFREE in the cmake-gui before generating sln).
- We pre-configured kinfu_example.vcxproj with opencv/opencv_contrib include/lib dependencies paths, the following is a list of dependencies we need for this sample:
    ```
    Includes:
        extern\opencv-4.1.0\include
        extern\opencv_contrib-4.1.0\modules\rgbd\include
        extern\opencv_contrib-4.1.0\modules\viz\include
    Libs:
        extern\lib\Debug\opencv_calib3d410d.lib
        extern\lib\Debug\opencv_core410d.lib
        extern\lib\Debug\opencv_highgui410d.lib
        extern\lib\Debug\opencv_imgproc410d.lib
        extern\lib\Debug\opencv_rgbd410d.lib
        extern\lib\Debug\opencv_viz410d.lib
        extern\lib\Release\opencv_calib3d410.lib
        extern\lib\Release\opencv_core410.lib
        extern\lib\Release\opencv_highgui410.lib
        extern\lib\Release\opencv_imgproc410.lib
        extern\lib\Release\opencv_rgbd410.lib
        extern\lib\Release\opencv_viz410.lib
    ```
- Please add opencv lib dependencies in the kinfu_example.vcxproj file. E.g. for release configuration, you can do: 
    ```
    <AdditionalDependencies>%(AdditionalDependencies);opencv_core410.lib;opencv_calib3d410.lib;
    opencv_rgbd410.lib;opencv_highgui410.lib;opencv_viz410.lib;opencv_imgproc410.lib;</AdditionalDependencies>
    ```
- Uncommenting the HAVE_OPENCV pound define in the main.cpp and build the kinfu_example.sln.
- You need to copy the opencv/opencv_contrib dlls as well as VTK dlls to the Visual Studio output bin folder which contains kinfu_example.exe and Azure Kinect binaries before running the application.

## Usage Info

    Usage: kinfu_example.exe [Optional]<Mode>
    Mode: nfov_unbinned(default), wfov_2x2binned, wfov_unbinned, nfov_2x2binned
    Keys:   q - Quit
            r - Reset KinFu
            v - Enable Viz Render Cloud (default is OFF, enable it will slow down frame rate)
            w - Write out the kf_output.ply point cloud file in the running folder
    * Please ensure to uncomment HAVE_OPENCV pound define to enable the opencv code that runs kinfu
    * Please ensure to copy opencv/opencv_contrib/vtk dlls to the running folder

Example:

    Usage: kinfu_example.exe
