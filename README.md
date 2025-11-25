# Jetson Orin Nano Kurulum Rehberi


## Jetpack Kurulumu

Bu bölüm, Jetson Orin Nano'yu çalıştırmak için gerekli olan **JetPack 6.2** işletim sisteminin SD Karta nasıl yazılıp boot edileceğini anlatır.

---


### Adım 1: JetPack İmajını İndirme

NVIDIA'nın resmi [JetPack 6.2 SD Card Image (.zip)](https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v4.3/jp62-orin-nano-sd-card-image.zip) SD Kart imajını indirin.

### Adım 2: İmajı SD Karta Yazdırma (Flashing)
İndirme tamamlandıktan sonra [Balena Etcher](https://etcher.balena.io/) programından image'ı sd card'a yazın.

### Adım 3: Boot
Yazma işlemi bittikten sonra SD kartı bilgisayardan çıkarın.

1.  SD Kartı, Jetson Orin Nano modülünün **alt tarafında** bulunan yuvaya takın.
2.  Monitör, Klavye ve Mouse bağlantılarını yapın.
3.  **En son** güç adaptörünü takarak cihazı açın.
---

### Adım ****: CUDA Destekli OpenCV Derlemesi (Otomatik Script ile)

Aşağıdaki komutlar CUDA destekli OpenCV 4.11.0 sürümünü kurar.

> [!IMPORTANT]
> Kuruluma başlamadan önce hafıza durumunuzu kontrol edin. Derleme işlemi için en az **8.5 GB** alana ihtiyacınız vardır.

```bash
# 1. Hafıza kontrolü yapın
free -m

# 2. Kurulum scriptini indirin
wget https://github.com/Qengineering/Install-OpenCV-Jetson-Nano/raw/main/OpenCV-4-11-0.sh

# 3. Çalıştırma izni verin ve kurulumu başlatın
sudo chmod 755 ./OpenCV-4-11-0.sh
./OpenCV-4-11-0.sh

#4. Kurulum bittiğinde bellekte yer açın
rm OpenCV-4-11-0.sh
sudo rm -rf ~/opencv
sudo rm -rf ~/opencv_contrib
