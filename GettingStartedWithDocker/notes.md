# Getting started with Docker

## We'll cover

- Why Docker is favourable
- we will not cover steps to install Docker and nvidia-docker...
- building your own image for dev setup

OS-level virtualization to deliver software in packages called containers.  
도커는 가상머신과 같이 하드웨어를 가상화하는 것이 아니라, 리눅스 운영체제에서 지원하는 다양한 기능을 사용해 컨테이너(하나의 프로세스)를 실행하기 위한 별도의 환경(파일 시스템)을 준비하고, 리눅스 네임스페이스와 다양한 커널 기능을 조합해 프로세스를 특별하게 실행시켜주빈다.

- 가상머신 사용경험?
- Git 사용경험?
- Docker 사용 경험?

## Why use Docker over Conda

- it virtualize does more than python. fully virtualized OS environment based on image
- portable once you set up an image; can keep your environment consistent across 2GPU <-> 4GPU servers
- makes sense in multi-user machines; when you need specific version of external C library such as CUDA/CuDNN
- you'll need it anyway when you reproduce others' works.
- enterprises love it.

## What it can't do

- vGPU is supported only in linux.

## Git vs. Docker

| Git | Docker |
|-----|--------|
|Code|Image|
|Process|Container|
|github.com<br>gitlab.com|hub.docker.com<br>ngc.nvidia.com|
|`git clone`|`docker pull`|
|-|`docker run`|
|-|`docker start`|
|-|`docker attach`|
|`git commit`|`docker commit`|
|`git push`|`docker push`|

## Tutorial: create your image and publish on Docker Hub

### Sign up on hub.docker.com

create a new repository and name it `pytorch`

### Two ways to create a docker image

- `docker commit` to a repository
- writing and running `Dockerfile` script

Once you set up your developer environment and turn it into a Docker image, you can launch it on any machine.

### pull and run a pytorch container from the docker hub

```bash
docker pull pytorch/pytorch
docker run --gpus all -it -p 5000:8888 --name MyContainerName pytorch/pytorch
```

### install your packages

```bash
apt update
apt install vim tmux git
pip install opencv-python pillow tqdm
```

### install and set up jupyter

```bash
pip install jupyterlab
jupyter notebook --generate-config
```

then open `~/.jupyter/jupyter_notebook_config.py` and set the value `c.NotebookApp.ip = '0.0.0.0'`  
you can add jupyter extensions.

### make sure your packages work

```python
import torch
print(torch.cuda.is_available())
```

```python
import cv2, PIL
```

```bash
jupyter-lab
```

### freeze and commit to image

press `CTRL + D` and exit container.

```bash
docker commit MyContainerName YourName/pytorch
docker push YourName/pytorch
```

## Docker examples

<https://github.com/feidfoe/learning-not-to-learn>

## 중요한 명령어

Run container

```bash
docker run --gpus all -it -v HostDir:/workspace -p HostPort:8888 --name ContainerName pytorch/pytorch
```

Exit container

`CTRL + D`

Detach from container

`CTRL + P + Q`

Show process status

```bash
docker ps -a
```

Start or stop container

```bash
docker start ContainerName
docker stop ContainerName
```

Attach to a running container

```bash
docker attach ContainerName
```

Remove container

```bash
docker rm ContainerName
```
