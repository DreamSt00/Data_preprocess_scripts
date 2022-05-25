# Data Scripts

## Preparation

### Install the docker image of openpose 
Docker repository：https://hub.docker.com/r/cwaffles/openpose   
Docker common commands：https://www.runoob.com/docker/docker-tutorial.html  
**step1: install docker**  
Automatic installation using official installation script:   
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun   
  
**step2: Install NVIDIA CONTAINER RUNTIME** 
Create a new script file vim nvidia.sh and fill in the following content:   
sudo curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
sudo curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
sudo apt-get update

执行脚本 sh nvidia.sh

安装 nvidia-container-runtime:
sudo apt-get install nvidia-container-runtime

**step3:Download image**

docker pull cwaffles/openpose

Create a container from an image：
sudo docker run --gpus all --name openpose -it cwaffles/openpose:latest /bin/bash
Enter the container (the container will be automatically entered if the creation is successful)：
docker exec -it openpose /bin/bash


**step4:Test the demo of openpose**

＃only body   
./build/examples/openpose/openpose.bin --video examples/media/video.avi --write_json output/ --display 0 --render_pose 0   
#Body + face + hands   
./build/examples/openpose/openpose.bin --video examples/media/video.avi --write_json output/ --display 0 --render_pose 0 --face --hand
###  Mount the repository and video files in the openpose docker container as data volumes

docker run -it -v  /host path directory: /directory in the container image name
docker run -idt -v --name openpose  /home/$USER/share:/openpose/share cwaffles/openpose:latest 
docker exec -it  openpose /bin/bash 

### Install python related packages

pip3 install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple


### skating_convert.py:

Call openpose to extract the skeleton points in the video and package the results of each video into a json file

### skating_gendata.py

Organize json files into npy files and save
