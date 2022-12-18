# ORB_SLAM3 docker

This docker is based on ros melodic ubuntu 18.

There are two versions available:
- CPU based (Xorg Nouveau display)
- Nvidia Cuda based. 

To check if you are running the nvidia driver, simply run `nvidia-smi` and see if get anything.

Based on which graphic driver you are running, you should choose the proper docker. For cuda version, you need to have [nvidia-docker setup](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) on your machine.

---

## Compilation and Running

Install docker:
* [Install docker](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
* If curl install gives you an error, try:
 - `sudo snap install curl`
* To run using docker use `sudo docker` or create a [user group](https://docs.docker.com/engine/install/linux-postinstall/)

Steps to compile the Orbslam3 on the sample dataset:

- `cd ~ # an arbitrary folder outside your ROS workspace`
- `git clone https://github.com/jahaniam/orbslam3_docker.git`
- `cd orbslam3_docker` 
- `./download_dataset_sample.sh`
- `./build_container_cpu.sh` or `build_container_cuda.sh` depending on your machine.

** I build fails, you need more cpu cores and it closes processes duiring the build. To avoid that you can modify `build.sh` and build from inside the docker.
- `sudo docker exec -it orbslam3 bash`
- `sudo nano build.sh`
- remove all `-j` after `make -j` to limit using one core during the build.
- build: `chmod +x build.sh
          ./build.sh`
          
Now you should see ORB_SLAM3 is compiling. 
To run a test example:
- `sudo docker exec -it orbslam3 bash`
- `cd /ORB_SLAM3/Examples&&./euroc_examples.sh`
- `cd /ORB_SLAM3/Examples
./Stereo/stereo_euroc ../Vocabulary/ORBvoc.txt ./Stereo/EuRoC.yaml /Datasets/EuRoC/MH01 ./Stereo/EuRoC_TimeStamps/MH01.txt dataset-MH01_stereo`
---

You can use vscode remote development (recommended) or sublime to change codes.
- `docker exec -it orbslam3 bash`
- `subl /ORB_SLAM3`
