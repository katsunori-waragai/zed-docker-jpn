# zed-docker
docker example for zed sdk (stereolabs) on Jetson


## Direct install of ZED SDK
- 直接 ZED SDKをインストール

JetPack のバージョンに合わせたインストーラーをダウンロードする。
`ZED_SDK_Tegra_L4T35.2_v4.0.8.zstd.run`
インストーラーを実行する。

## ZED SDK /samples

```
git clone https://github.com/stereolabs/zed-sdk
```

それぞれのsample application の説明は、上記のgithug のリポジトリの各README.md に記載がある。

### 実行結果例
python　スクリプトは、容易に実行できる。

#### body tracking 人のポーズ推定でのtracking
zed-sdk/body tracking/body tracking/python$ python3 body_tracking.py


![](fig/body_tracking_python.png)

#### object detection での検出結果の表示
object detection/image viewer/python$ python3 object_detection_image_viewer.py

![](fig/object_detection_image_viewer.png)

#### object detectionでのbird_viewでの表示
object detection/birds eye viewer/python$ python3 object_detection_birds_view.py
![](fig/object_detection_birds_view.png)
図の右側に、検出された人の位置を表示している。

#### positinal tracking
positional tracking/positional tracking/python$ python3 positional_tracking.py
![](fig/positional_tracking.png)
図に、ZED2のカメラ自体の位置の変化が表示される。

### pytorch_yolov8
object detection/custom detector/python/pytorch_yolov8$ python detector.py
![](fig/pytorch_yolov8.png)
yolov8 を用いているので検出対象物の種類が増えている。
検出した対象物の種類はMS COCO データセットのカテゴリの番号


## install ZED SDK using Docker


### 以下のdocker hub から該当するtagを見つけること
https://hub.docker.com/r/stereolabs/zed/

### JetPack5.1
- JetPack の場合はUbuntuPCとはtagのルールが異なる。

export TAGS=4.0-py-devel-jetson-jp5.1.0
sudo docker pull stereolabs/zed:${TAGS}


sudo docker run -it --privileged  stereolabs/zed:${TAGS}


docker pull stereolabs/zed:3.7-gl-devel-cuda11.4-ubuntu20.04  # pull ZED SDK v3.7.x devel release with OpenGL support under Ubuntu 20.04
xhost +si:localuser:root  # allow containers to communicate with X server
  docker run -it --runtime nvidia --privileged -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix stereolabs/zed:3.7-gl-devel-cuda11.4-ubuntu20.04


Q: g++ が見つからない
```
sudo apt update
sudo apt install build-essential
```

Q: /usr/local/zed/samplesはどこ？

```
  apt update && apt install cmake -y
  cp -r /usr/local/zed/samples/depth\ sensing/ /tmp/depth-sensing
  cd /tmp/depth-sensing/cpp ; mkdir build ; cd build
  cmake .. && make
  ./ZED_Depth_Sensing
```

Q: CUDA_TOOLKIT_ROOT_DIR


Q: Docker環境の外
Docker環境の外
$ ls /usr/local/cuda
bin                EULA.txt  lib64  README   targets
compute-sanitizer  extras    nvml   samples  tools
DOCS               include   nvvm   share    version.json

Docker環境の内
ls /usr/local/cuda
include  lib64  targets
