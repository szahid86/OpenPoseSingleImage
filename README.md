# OpenPoseSingleImage
This repository presents the code and help to processes single frame/image using OpenPose library.
Before I start, all the credits and licensing rights goes to:
https://github.com/CMU-Perceptual-Computing-Lab/openpose
and
https://github.com/dlunion/OpenPose
Please check above repositories for specific details.


## Contents
0. [Why you are here?](#why-are-you-here?)
1. [Operating Systems](#operating-systems)
2. [Requirements and Dependencies](#requirements-and-dependencies)
3. [Windows Portable Demo ](#windows-portable-demo)
4. [Download OpenPose Repo](#download-openpose-repo)
5. [Installation](#installation)
6. [Run OpenPose you just built](#run-open-pose-you-just-built)
7. [Run OpenPose on single image](#run-open-pose-on-single-image)
8. [Keypoint ordering](#keypoint-ordering)


## Why are you here?
Basically if you are looking for an awesome body parts estimator you must go through OpenPose, which is real-time multi-person system to jointly detect human body, hand, facial and food keypoints from single image. The official version of OpenPose is at https://github.com/CMU-Perceptual-Computing-Lab/openpose, where you can find more about it. The OpenPose demo does provide class to detect keypoints from videos, folder containing images, but it do not come with a class that takes single image as input and just provide the detected keypoints from that single image. This repo provides a code and start-from-scratch guide for this purpose.



## Operating Systems
I have compiled by code on Windows 7 64-bit on Visual Studio 2014, but the official version of OpenPose support:

- **Ubuntu** 14 and 16.
- **Windows** 8 and 10.
- **Mac OSX** Mavericks and above (only CPU version).
- **Nvidia Jetson TX2**, installation instructions in [doc/installation_jetson_tx2.md](./installation_jetson_tx2.md).
- OpenPose has also been used on **Windows 7**, **CentOS**, and **Nvidia Jetson (TK1 and TX1)** embedded systems. However, OpenPose do not officially support them at the moment.


## Requirements and Dependencies
I have tested the code on NVIDIA GTX 1080 with CUDA 8.0 and CUDNN 6.5. You must have the Cmake-gui and visual studio 2014 to get through to the next steps. The official version of the OpenPose support:
- **Requirements** for the default configuration (you might need more resources with a greater `--net_resolution` and/or `scale_number` or less resources by reducing the net resolution and/or using the MPI and MPI_4 models):
    - Nvidia GPU version:
        - NVIDIA graphics card with at least 1.6 GB available (the `nvidia-smi` command checks the available GPU memory in Ubuntu).
        - At least 2.5 GB of free RAM memory for BODY_25 model or 2 GB for COCO model (assuming cuDNN installed).
        - Highly recommended: cuDNN.
     - AMD GPU version:
        - Vega series graphics card
        - At least 2 GB of free RAM memory.
    - CPU version:
        - Around 8GB of free RAM memory.
    - Highly recommended: a CPU with at least 8 cores.
    
I ran the code using OpenCV 3.1 (prebuild binaries for 64-bit system). For Caffe I also use the pre-build binaries that comes with the OpenPose official version.
- **Dependencies**:
    - OpenCV (all 2.X and 3.X versions are compatible).
    - Caffe and all its dependencies.
    - The demo and tutorials additionally use GFlags.
    - These dependencies will automatically installed during configuration of OpenPose using Cmake during installations.
    
    
## Windows Portable Demo
It is always a good practise to run this windows portable demo. If the demo works fine, it gives you a surety that your machine is not alergic to OpenPose. :) 
Windows portable version: Simply download and use the latest version from https://github.com/CMU-Perceptual-Computing-Lab/openpose/releases or more specifically from https://github.com/CMU-Perceptual-Computing-Lab/openpose/releases/download/v1.3.0/openpose-1.3.0-win64-gpu-binaries.zip
**NOTE**: Read the `Instructions.txt` to learn to download the models required by OpenPose (about 500 Mb).

## Download OpenPose Repo
On windows you can go to https://github.com/CMU-Perceptual-Computing-Lab/openpose and click the green button that says "Clone or Download", and then click "Download Zip". Download to your favorite path (make sure that there are no spaces in the path e.g., "/path_to_open_pose/" instead of "/path to open pose/" etc.). After downloading, extract the OpenPose folder and just look whats inside. Espacially, explore the "3rdparty" folder, which you will be looking around all the time.


## Installation
1. Open CMake GUI and select the OpenPose directory as project source directory, and a non-existing or empty sub-directory (e.g., build) where the Visual Studio solution (Windows) will be generated. If build does not exist, it will ask you whether to create it. Press Yes.

2. Press the `Configure` button, keep the generator to `Visual Studio 14 2015 Win64` (Windows), and press `Finish`.

3. If this step is successful, the `Configuring done` text will appear in the bottom box in the last line. Otherwise, some red text will appear in that same bottom box. During this step, the Cmake-gui will also download the required models, weight files, dependencies etc.

4. Press the `Generate` button and proceed to [OpenPose Building](#openpose-building). You can now close CMake.

### OpenPose Building
#### Windows
In order to build the project, open the Visual Studio solution (Windows), called `build/OpenPose.sln`. Then, set the configuration from `Debug` to `Release` and press the green triangle icon (alternatively press <kbd>F5</kbd>).

**VERY IMPORTANT NOTE**: In order to use OpenPose outside Visual Studio, and assuming you have not unchecked the `BUILD_BIN_FOLDER` flag in CMake, copy all DLLs from `{build_directory}/bin` into the folder where the generated `openpose.dll` and `*.exe` demos are, e.g., `{build_directory}x64/Release` for the 64-bit release version.

After successfull building process, openpose.lib file will be generated in the folder somewhere 'build\src\openpose\Release'


## Run OpenPose you just built
In order to run the openpose you just built on your system, go to `build\x64\Release` and run `OpenPoseDemo.exe` using the command prompt. e.g.:
`OpenPoseDemo.exe --video examples\media\video.avi` (for processing single video) or
`OpenPoseDemo.exe -help` (for checking what options do you have) or 
`OpenPoseDemo.exe --video examples\media\folder_with_images` (for processing images saved in a folder)


## Run OpenPose on single image
Now this is the final part, and the part we are here for, i.e., runing open pose on a single image. For this you can make an empty new Visual Studio Project (C++) and then add the OpenPoseSingleImage.cpp into the project. Add the C/C++ include directories:
`3rdparty/include/boost-1_61/` ;
`3rdparty/include/` ;
`3rdparty/include2/` ;
`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\include`

and set the preprocessor flags
`GLOG_NO_ABBREVIATED_SEVERITIES ;
NDEBUG ;
USE_OPENCV ;
USE_CAFFE ;
USE_CUDA ;
BOOST_ALL_DYN_LINK`

add the Additional Dependencies in to the Linker:
`cudart_static.lib ;
caffe.lib ;
caffeproto.lib ;
openpose.lib ;
gflags.lib ;
glog.lib ;
cublas.lib ;
curand.lib ;
cudart.lib ;
pthreadVC2.lib ;
opencv_world320.lib`
(Please note that you have to provide the correct link to these libraries in the Linker>General>Additional Library Directories)


That is all .. now just build your project. Finally, you can run your .exe file using the following command line arguments:

OpenPoseSingleImage.exe imagefile_path gpuid[0] base_width[656] base_height[368] path_to_pose_deploy_linevec.prototxt path_to_pose_iter_440000.caffemodel

The following function in the main() returns the exact keypoints of the joints in 2d point format `std::vector<cv::Point>` :
`vector<Point> finalKeypoints = renderPoseKeypointsCpu(raw_image, keypoints, shape, 0.05, scale);`

## Keypoint ordering

The keypoitns are arranged as below:
`Point[0] : (x , y) // face center 
Point[1] : (x , y) // neck joint 
Point[2] : (x , y) // right shoulder joint 
Point[3] : (x , y) // right elbow joint 
Point[4] : (x , y) // right palm  
Point[5] : (x , y) // left shoulder joint 
Point[6] : (x , y) // left elbow joint 
Point[7] : (x , y) // left palm  
Point[8] : (x , y) // right hip 
Point[9] : (x , y) // right knee 
Point[10] : (x , y) // left hip 
Point[11] : (x , y) // right eye 
Point[12] : (x , y) // left eye
Point[13] : (x , y)  // right ear
Point[14] : (x , y)  // left ear`

More detail on keypoint ordering at (Pose Output Format (COCO)):
https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/output.md


Enjoy!!
