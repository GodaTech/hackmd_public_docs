# Linux Containers
###### tags: `Public`, `Technical`, `Configuration`, `Linux`, `Ubuntu`, `Container`, `Docker`, `LXD`

## LXD
- `sudo apt install lxd `
- `lxd init`

## [Docker](https://docs.docker.com/)

### [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

#### Uninstall old versions
- `sudo apt-get remove docker docker-engine docker.io containerd runc`

#### Uninstall Docker Engine
- `sudo apt-get purge docker-ce docker-ce-cli containerd.io`

**The following command will delete all exists images, containers, and volumes!**
```
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

#### Install using the repository
##### Set up the repository
- `sudo apt-get update`
- `sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release`
- `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
- x86_64 / amd64
    - `echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

##### Install Docker Engine
- `sudo apt-get update`
- `sudo apt-get install docker-ce docker-ce-cli containerd.io`

##### [Setting up NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#setting-up-nvidia-container-toolkit)
```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```
- `curl -s -L https://nvidia.github.io/nvidia-container-runtime/experimental/$distribution/nvidia-container-runtime.list | sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list`
- `sudo apt-get update`
- `sudo apt-get install -y nvidia-docker2`
- `sudo systemctl restart docker`
- `sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi`

#### Manage Docker as a non-root user
- `sudo gpasswd -a $USER docker`

#### Change Docker data directory
- `sudo service docker stop`
- Add command to `/etc/docker/daemon.json`
```"data-root": "/mnt/uv500_raid0/docker"```