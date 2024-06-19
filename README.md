# Modulus

## Install Nvidia Docker

From the [NVidia Install Guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install -y nvidia-container-toolkit
```

### Configure Docker to use Nvidia Container Toolkit


I am using Rootless mode

```bash
nvidia-ctk runtime configure --runtime=docker --config=$HOME/.config/docker/daemon.json
```

```bash
systemctl --user restart docker
```

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

```bash
sudo nvidia-ctk config --set nvidia-container-cli.no-cgroups --in-place
```


## Install Modulus Dev Container

```bash
docker pull nvcr.io/nvidia/modulus/modulus:24.04
```

## Using Modulus Dev Container

To open a shell in the container

```bash
docker run  --privileged --gpus all --shm-size=1g --ulimit memlock=-1 --ulimit stack=67108864 --runtime nvidia \
--rm -it nvcr.io/nvidia/modulus/modulus:24.04 bash
```

You should now be NVida Modules Dev Container

```
modulus:24.04 bash

====================
== NVIDIA Modulus ==
====================

NVIDIA Release 24.04 (build 14160669)
Modulus PyPi Version 0.6.0 (Git Commit: eff54e6)
Modulus Sym PyPi Version 1.5.0 (Git Commit: cab5b30)
Container image Copyright (c) 2024, NVIDIA CORPORATION & AFFILIATES. All rights reserved.
Copyright (c) 2014-2024 Facebook Inc.
Copyright (c) 2011-2014 Idiap Research Institute (Ronan Collobert)
Copyright (c) 2012-2014 Deepmind Technologies    (Koray Kavukcuoglu)
Copyright (c) 2011-2012 NEC Laboratories America (Koray Kavukcuoglu)
Copyright (c) 2011-2013 NYU                      (Clement Farabet)
Copyright (c) 2006-2010 NEC Laboratories America (Ronan Collobert, Leon Bottou, Iain Melvin, Jason Weston)
Copyright (c) 2006      Idiap Research Institute (Samy Bengio)
Copyright (c) 2001-2004 Idiap Research Institute (Ronan Collobert, Samy Bengio, Johnny Mariethoz)
Copyright (c) 2015      Google Inc.
Copyright (c) 2015      Yangqing Jia
Copyright (c) 2013-2016 The Caffe contributors
All rights reserved.

Various files include modifications (c) NVIDIA CORPORATION & AFFILIATES.  All rights reserved.

This container image and its contents are governed by the NVIDIA Deep Learning Container License.
By pulling and using the container, you accept the terms and conditions of this license:
https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license

NOTE: CUDA Forward Compatibility mode ENABLED.
  Using CUDA 12.4 driver version 550.54.14 with kernel driver version 470.239.06.
  See https://docs.nvidia.com/deploy/cuda-compatibility/ for details.

root@480339601b19:/workspace#

```

```
#nvidia-smi
Fri Jun  7 17:36:03 2024
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.239.06   Driver Version: 470.239.06   CUDA Version: 12.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Quadro P1000        Off  | 00000000:01:00.0  On |                  N/A |
| N/A   49C    P8    N/A /  N/A |    394MiB /  4015MiB |     13%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+
```

## Run an example

```
git clone https://github.com/NVIDIA/modulus.git
```

```
pip install warp-lang # install NVIDIA Warp to run the darcy example
```

```
cd modulus/examples/cfd/darcy_fno/

```

```
python train_fno_darcy.py
```