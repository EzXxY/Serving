# Docker Images

([简体中文](Docker_Images_CN.md)|English)

This document maintains a list of docker images provided by Paddle Serving.

## Get docker image

You can get images in two ways:

1. Pull image directly from `registry.baidubce.com ` through TAG:

   ```shell
   docker pull registry.baidubce.com/paddlepaddle/serving:<TAG> # registry.baidubce.com
   ```

2. Building image based on dockerfile

   Create a new folder and copy Dockerfile to this folder, and run the following command:

   ```shell
   docker build -f ${DOCKERFILE} -t <image-name>:<images-tag> .
   ```



## Image description

Runtime images cannot be used for compilation.
If you want to customize your Serving based on source code, use the version with the suffix - devel.

**cuda10.1-cudnn7-gcc54 image is not ready, you should run from dockerfile if you need it.**

|                         Description                          |   OS    |             TAG              |                          Dockerfile                          |
| :----------------------------------------------------------: | :-----: | :--------------------------: | :----------------------------------------------------------: |
|                       CPU development                        | Ubuntu16 |         latest-devel         |        [Dockerfile.devel](../tools/Dockerfile.devel)         |
|              GPU (cuda10.1-cudnn7-tensorRT6-gcc54) development               | Ubuntu16 | latest-cuda10.1-cudnn7-gcc54-devel(not ready) | [Dockerfile.cuda10.1-cudnn7-gcc54.devel](../tools/Dockerfile.cuda10.1-cudnn7-gcc54.devel) |
|              GPU (cuda10.1-cudnn7-tensorRT6) development               | Ubuntu16 | latest-cuda10.1-cudnn7-devel | [Dockerfile.cuda10.1-cudnn7.devel](../tools/Dockerfile.cuda10.1-cudnn7.devel) |
|              GPU (cuda10.2-cudnn8-tensorRT7) development               | Ubuntu16 | latest-cuda10.2-cudnn8-devel | [Dockerfile.cuda10.2-cudnn8.devel](../tools/Dockerfile.cuda10.2-cudnn8.devel) |
|              GPU (cuda11.2-cudnn8-tensorRT7) development               | Ubuntu18 | latest-cuda11.2-cudnn8-devel | [Dockerfile.cuda11.2-cudnn8.devel](../tools/Dockerfile.cuda11.2-cudnn8.devel) |

**Java Client:**
```
registry.baidubce.com/paddlepaddle/serving:latest-java
```

**XPU:**
```
registry.baidubce.com/paddlepaddle/serving:xpu-arm # for arm xpu user
registry.baidubce.com/paddlepaddle/serving:xpu-x86 # for x86 xpu user
```

## Requirements for running CUDA containers

Running a CUDA container requires a machine with at least one CUDA-capable GPU and a driver compatible with the CUDA toolkit version you are using. 

The machine running the CUDA container **only requires the NVIDIA driver**, the CUDA toolkit does not have to be installed.

For the relationship between CUDA toolkit version, Driver version and GPU architecture, please refer to [nvidia-docker wiki](https://github.com/NVIDIA/nvidia-docker/wiki/CUDA).

# (Attachment) The List of All the Docker images

Develop Images:

| Env      | Version | Docker images tag            | OS        | Gcc Version |
|----------|---------|------------------------------|-----------|-------------|
|    CPU   | >=0.5.0 | 0.6.2-devel                 | Ubuntu 16 |  8.2.0       |
|          | <=0.4.0 | 0.4.0-devel                  | CentOS 7  | 4.8.5       |
| Cuda10.1 | >=0.5.0 | 0.6.2-cuda10.1-cudnn7-devel  | Ubuntu 16 |   8.2.0       |
|          | 0.6.2   | 0.6.2-cuda10.1-cudnn7-gcc54-devel(not ready)  | Ubuntu 16 |  5.4.0 |
|          | <=0.4.0 | 0.6.2-cuda10.1-cudnn7-devel    | CentOS 7  | 4.8.5     |
| Cuda10.2 | >=0.5.0 | 0.6.2-cuda10.2-cudnn8-devel  | Ubuntu 16 |   8.2.0       |
|          | <=0.4.0 | Nan                          | Nan       | Nan         |
| Cuda11.0 | >=0.5.0 | 0.6.2-cuda11.0-cudnn8-devel | Ubuntu 18 |    8.2.0       |
|          | <=0.4.0 | Nan                          | Nan       | Nan         |

Running Images:

Running Images is lighter than Develop Images, and Running Images are made up with serving whl and bin, but without develop tools like cmake because of lower image size. If you want to know about it, plese check the document [Paddle Serving on Kubernetes.](PADDLE_SERVING_ON_KUBERNETES.md).

| ENV                                      | Python Version | Tag                         |
|------------------------------------------|----------------|-----------------------------|
| cpu                                      | 3.6            | 0.6.2-py36-runtime          |
| cpu                                      | 3.7            | 0.6.2-py37-runtime          |
| cpu                                      | 3.8            | 0.6.2-py38-runtime          |
| cuda-10.1 + cudnn-7.6.5 + tensorrt-6.0.1 | 3.6            | 0.6.2-cuda10.1-py36-runtime |
| cuda-10.1 + cudnn-7.6.5 + tensorrt-6.0.1 | 3.7            | 0.6.2-cuda10.1-py37-runtime |
| cuda-10.1 + cudnn-7.6.5 + tensorrt-6.0.1 | 3.8            | 0.6.2-cuda10.1-py38-runtime |
| cuda-10.2 + cudnn-8.2.0 + tensorrt-7.1.3 | 3.6            | 0.6.2-cuda10.2-py36-runtime |
| cuda-10.2 + cudnn-8.2.0 + tensorrt-7.1.3 | 3.7            | 0.6.2-cuda10.2-py37-runtime |
| cuda-10.2 + cudnn-8.2.0 + tensorrt-7.1.3 | 3.8            | 0.6.2-cuda10.2-py38-runtime |
| cuda-11 + cudnn-8.0.5 + tensorrt-7.1.3   | 3.6            | 0.6.2-cuda11-py36-runtime   |
| cuda-11 + cudnn-8.0.5 + tensorrt-7.1.3   | 3.7            | 0.6.2-cuda11-py37-runtime   |
| cuda-11 + cudnn-8.0.5 + tensorrt-7.1.3   | 3.8            | 0.6.2-cuda11-py38-runtime   |

**Tips:**  If you want to use CPU server and GPU server (version>=0.5.0) at the same time, you should check the gcc version,  only Cuda10.1/10.2/11 can run with CPU server owing to the same gcc version(8.2).
