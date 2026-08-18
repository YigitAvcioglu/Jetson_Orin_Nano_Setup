# Jetson Orin Nano Setup Guide
---
## JetPack Installation
This section explains how to flash the JetPack 6.2 operating system to an SD card and boot the Jetson Orin Nano.

### Step 1: Download the JetPack Image

Download the official NVIDIA [JetPack 6.2 SD Card Image (.zip)](https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v4.3/jp62-orin-nano-sd-card-image.zip)

### Step 2: Flash the Image to the SD Card
Once the download is complete, flash the image to the SD card using [Balena Etcher](https://etcher.balena.io/)

### Step 3: Boot
After the flashing process finishes, safely remove the SD card from your computer.

1. Insert the SD card into the slot located on the underside of the Jetson Orin Nano module.
2. Connect your monitor, keyboard, and mouse.
3.  Power on the device by connecting the power adapter last.
---

## Required Installations
This section explains the installation of necessary modules.
### Step 1: Ultralytics
```bash
sudo apt update
sudo apt install python3-pip -y
pip install -U pip

pip install ultralytics
```

### Step 2: PyTorch
```bash
cd Downloads/

wget https://pypi.jetson-ai-lab.io/jp6/cu126/+f/62a/1beee9f2f1470/torch-2.8.0-cp310-cp310-linux_aarch64.whl
pip install torch-2.8.0-cp310-cp310-linux_aarch64.whl

wget https://pypi.jetson-ai-lab.io/jp6/cu126/+f/81a/775c8af36ac85/torchaudio-2.8.0-cp310-cp310-linux_aarch64.whl
pip install torchaudio-2.8.0-cp310-cp310-linux_aarch64.whl

wget https://pypi.jetson-ai-lab.io/jp6/cu126/+f/907/c4c1933789645/torchvision-0.23.0-cp310-cp310-linux_aarch64.whl
pip install torchvision-0.23.0-cp310-cp310-linux_aarch64.whl
```

Verify the installation
```bash
python3.10 -c "import torch; print(torch.__version__); print('CUDA:', torch.cuda.is_available())"
# must be "CUDA: True"
```

### Step 3: GStreamer
```bash
sudo apt install gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav
sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
```
### Step 4: Build OpenCV from Source
```bash
# Download the script
wget https://raw.githubusercontent.com/YigitAvcioglu/Jetson_Orin_Nano_Setup/main/opencv_cuda_gst.sh

# Grant execution permissions and run
sudo chmod 755 ./opencv_cuda_gst.sh
./opencv_cuda_gst.sh
```

Free up disk space after the build finishes
```bash
rm opencv_cuda_gst.sh
sudo rm -r /usr/include/opencv4/opencv2
sudo make install
sudo ldconfig
make clean
sudo apt-get update
sudo rm -rf ~/opencv
sudo rm -rf ~/opencv_contrib
```

Verify the OpenCV build
```bash
python3.10 -c "import cv2; print(cv2.getBuildInformation())"
# must be "CUDA: YES" and "GStreamer: YES"
```
