# Jetson Orin Nano Kurulum Rehberi
---
## Jetpack Kurulumu
Bu bölüm, Jetson Orin Nano'yu çalıştırmak için gerekli olan **JetPack 6.2** işletim sisteminin SD Karta nasıl yazılıp boot edileceğini anlatır.

### Adım 1: JetPack İmajını İndirme

NVIDIA'nın resmi [JetPack 6.2 SD Card Image (.zip)](https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v4.3/jp62-orin-nano-sd-card-image.zip) SD Kart imajını indirin.

### Adım 2: İmajı SD Karta Yazdırma
İndirme tamamlandıktan sonra [Balena Etcher](https://etcher.balena.io/) programından image'ı sd card'a yazın.

### Adım 3: Boot
Yazma işlemi bittikten sonra SD kartı bilgisayardan çıkarın.

1.  SD Kartı, Jetson Orin Nano modülünün **alt tarafında** bulunan yuvaya takın.
2.  Monitör, Klavye ve Mouse bağlantılarını yapın.
3.  **En son** güç adaptörünü takarak cihazı açın.
---

## Gerekli Kurulumlar
Bu bölüm gerekli modüllerin kurulumunu anlatır
### Adım 1: Ultralytics
```bash
sudo apt update
sudo apt install python3-pip -y
pip install -U pip

pip install ultralytics
```
Cihazı yeniden başlatın
```bash
sudo reboot
```

### Adım 2: Numpy
```bash
pip install numpy==2.3.3   
```

### Adım 3: Torch
```bash
cd /Downloads

wget https://pypi.jetson-ai-lab.io/jp6/cu126/+f/62a/1beee9f2f1470/torch-2.8.0-cp310-cp310-linux_aarch64.whl
pip install torch-2.8.0-cp310-cp310-linux_aarch64.whl

wget https://pypi.jetson-ai-lab.io/jp6/cu126/+f/81a/775c8af36ac85/torchaudio-2.8.0-cp310-cp310-linux_aarch64.whl
pip install torchaudio-2.8.0-cp310-cp310-linux_aarch64.whl

wget https://pypi.jetson-ai-lab.io/jp6/cu126/+f/907/c4c1933789645/torchvision-0.23.0-cp310-cp310-linux_aarch64.whl
pip install torchvision-0.23.0-cp310-cp310-linux_aarch64.whl
```

Kurulum sonrasında kontrol edelim
```bash
python3.10 -c "import torch; print(torch.__version__); print('CUDA:', torch.cuda.is_available())"
# CUDA: True olmalı
```

### Adım 4: Gstreamer
```bash
sudo apt install gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav
sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
```
### Adım 5: Deepstream
Gerekli bağımlılıkları indirin
```bash
sudo apt install \libssl1.1 \libgstreamer1.0-0 \gstreamer1.0-tools \gstreamer1.0-plugins-good \gstreamer1.0-plugins-bad \gstreamer1.0-plugins-ugly
\gstreamer1.0-libav \libgstrtspserver-1.0-0 \libjansson4 \libyaml-cpp-dev \libjson-glib-dev \libglew-dev \libegl1-mesa-dev
```

```bash
cd /Downloads
wget https://catalog.ngc.nvidia.com/orgs/nvidia/resources/deepstream/files/deepstream-7.1_7.1.0-1_arm64.deb
sudo apt-get install ./deepstream-7.1_7.1.0-1_arm64.deb
```
### Adım 6: OpenCV Derlemesi 
> [!IMPORTANT]
> Kuruluma başlamadan önce hafıza durumunuzu kontrol edin. Derleme işlemi için en az **8.5 GB** alana ihtiyacınız vardır.

```bash
# 1. Hafıza kontrolü yapın
free -m

# 2. Scripti indirin
wget https://github.com/YigitAvcioglu/Jetson_Orin_Nano_Setup/blob/main/opencv_cuda_gst.sh

# 3. Çalıştırma izni verin ve başlatın
sudo chmod 755 ./opencv_cuda_gst.sh
./opencv_cuda_gst.sh

# 4. Derleme bittiğinde bellekte yer açın
rm opencv_cuda_gst.sh
sudo rm -r /usr/include/opencv4/opencv2
sudo make install
sudo ldconfig
make clean
sudo apt-get update
sudo rm -rf ~/opencv
sudo rm -rf ~/opencv_contrib
```

Derleme sonrasında kontrol edelim
```bash
python3.10 -c "import cv2; print(cv2.getBuildInformation())"
#CUDA: YES ve GStreamer: YES olmalı
```
