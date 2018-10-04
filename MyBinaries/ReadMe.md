This folder contains the binaries and exe I generated. You can test this on your system (Windows 7 64-bit).
Usage:

open cmd, run the following command:

OpenPose.exe facedetectionpotw_01.png 0

(please note that '0' at the end of above command is the gpuid on my system. You can check your gpuid on your system using nvidia-smi command for Nvidia GPU's)

You need to download the caffemodel and network prototxt file from:
https://github.com/CMU-Perceptual-Computing-Lab/openpose

and also download the following dlls (can't upload these because of big sizes):
cublas64_80.dll
cudnn64_5.dll
curand64_80.dll
libopenblas.dll
