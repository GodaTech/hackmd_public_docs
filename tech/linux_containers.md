# Linux Containers
###### tags: `Public`, `Technical`, `Configuration`, `Linux`, `Ubuntu`, `Container`, `Docker`, `LXD`

- [Link on GitHub](https://github.com/GodaTech/hackmd_public_docs/blob/main/tech/linux_containers.md)
- [Link on HackMD](https://hackmd.io/@z80020100/rysaFz0Wt)

## LXD
- `sudo apt install lxd `
- `lxd init`

---

## [Docker](https://docs.docker.com/)

### [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Install script](https://github.com/z80020100/scripts/tree/master/linux/docker)

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
- `echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

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
```
"data-root": "/mnt/uv500_raid0/docker"
```

#### Cross-platform emulators 
##### Install [Binfmt](https://github.com/tonistiigi/binfmt#installing-emulators)
Cross-platform emulator collection distributed with Docker images.
- [Install script](https://github.com/z80020100/scripts/tree/master/linux/docker)
- `docker run --privileged --rm tonistiigi/binfmt --install all`

### Command
- `docker system prune`
#### run
- `docker run -it --rm --platform linux/arm64 ubuntu /bin/bash`
- `docker run -it --rm --platform linux/amd64 ubuntu /bin/bash`
#### buildx
- `docker buildx ls`
- `docker buildx build --platform linux/amd64,linux/arm64 .`

### Dockerfile
- [Dockerfile templates](https://github.com/z80020100/dockerfile_templates_public)

---

## [Podman](https://podman.io/)
- TODO
