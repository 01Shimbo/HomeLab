Installing NVIDIA drivers on Ubuntu server 24
https://ubuntu.com/server/docs/nvidia-drivers-installation
I used Building your own kernel modules using the NVIDIA DKMS package method
GPU is a P620 Quadro the driver branch is 535-server generic linux flavor

Start by updating
```
sudo apt update && apt upgrade -y
```

`sudo apt install linux-headers-generic -y
sudo apt install linux-headers-$(uname -r) -y
sudo apt install nvidia-dkms-535-server -y
sudo apt install nvidia-driver-535-server -y`


Installing Docker Tool Kit

`curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list`

`sudo sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list`
```sudo apt-get update```
```sudo apt-get install -y nvidia-container-toolkit```
```sudo nvidia-ctk runtime configure --runtime=docker```

Check if docker deamon.json was edited

`cat /etc/docker/daemon.json`

example out put:

`{
    "runtimes": {
        "nvidia": {
            "args": [],
            "path": "nvidia-container-runtime"
        }
    }
}`
if no output god speed.
`sudo systemctl restart docker`
