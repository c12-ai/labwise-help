# Admin Operations Guide

Welcome to the "Admin Operations Guide". This document serves as a comprehensive manual designed to provide System
Administrators with essential information and step-by-step procedures for managing product accounts and conducting
installations.

## Account management

1. Type the domain.com:1337/admin and log-in with the admin account (will be provided to the system admin). In the
   Content Manager tab find the content “User”
   ![](../images/img_98.png)
2. Click the button “Create new entry”
   ![](../images/img_99.png)
3. Input:
    * The user name
    * The email
    * The provider **must** be local
    * Select TRUE for confirmed
    * Select False for blocked
    * The role **must** be pm-retro-only
    * You can leave the manager_id and personal project blank
      ![](../images/img_100.png)
    * Finally, click “Save”, then the account with the password can be used to the log-in Labwise.

4. Once an account is created, you can click the corresponding row to edit it
   ![](../images/img_101.png)

## Installation Guide

System requirement:
* Ram: ‎DDR4 64G with memory speed 3200MHz, or better
* CPU: ‎12th Generation Intel® Core™ i5 Processors, or better
* GPU: NVIDIA GeForce RTX 4060, or better
* Hard Drive: 512G or above
* Power supply: Output Wattage of 500, the other hardware mentioned above is upgraded, corresponding upgrades should also be made
* System: ubuntu 22.04(jammy)

### Install GPU driver
Execute commands sequentially in the terminal

```
sudo apt update
```
```
sudo apt install ubuntu-drivers-common
```
```
sudo apt install nvidia-driver-530
```
```
sudo ubuntu-drivers autoinstall
```
```
reboot
```

### Install nvidia-cuda
Execute commands sequentially in the terminal
```
export UBUNTU_VERSION=ubuntu2204/x86_64
```
```
wget https://developer.download.nvidia.com/compute/cuda/repos/$UBUNTU_VERSION/cuda-keyring_1.0-1_all.deb
```
```
sudo dpkg -i cuda-keyring_1.0-1_all.deb
```
```
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/$UBUNTU_VERSION/ /"
```
```
export NVIDIA_DRIVER_VERSION=535
```
```
export CUDA_DRIVER_VERSION=535.154.05-1
```
```
sudo apt install cuda-drivers-${NVIDIA_DRIVER_VERSION}=${CUDA_DRIVER_VERSION} cuda-drivers=${CUDA_DRIVER_VERSION}
```
```
sudo apt-get remove dkms && sudo apt-mark hold dkms
```

### Install docker 
Execute commands sequentially in the terminal
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt update
```
```
apt-cache policy docker-ce
```
```
sudo apt install docker-ce
```
```
sudo systemctl status docker
```
```
sudo usermod -aG docker ${USER}
```
```
sudo systemctl restart docker
```

### Install nvidia-docker
```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
&& curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
```
sudo sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
```
sudo apt-get update
```
```
sudo apt-get install -y nvidia-container-toolkit
```
```
sudo nvidia-ctk runtime configure --runtime=docker
```
```
sudo systemctl restart docker
```

### Install fresh

```
docker pull laurencecao/jack:1.0.1
```
```
docker run -d --name jack0 --rm -it laurencecao/jack:1.0.1 bash
```
```
mkdir works && docker cp jack0:/root/c12/jack_1.0.1.tar.gz works/
```
```
cd works && tar xvfz jack_1.0.1.tar.gz
```
```
./task install
```

