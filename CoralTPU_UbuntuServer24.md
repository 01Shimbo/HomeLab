Frigate Docker container stack:


Coral TPU Driver Install (2024/10/22)
Add google tpu repo
```
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
```

attempt to install gasket THIS WILL MOST LIKELY NOT WORK
this will not work because gasket has been deprecated in >python3.10
```
sudo apt-get install gasket-dkms libedgetpu1-std
```

FIX:
install second python environment DO NOT DOWNGRADE CURRENT PYTHON VERSION (aptitude will not work anymore)
<https://idroot.us/install-pyenv-ubuntu-24-04/>
```
sudo apt-get update && sudo apt upgrade -y
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
curl https://pyenv.run | bash
nano ~/.bashrc
```
Add the following lines to the end of the file:
```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
```
Save Exit nano
To restart shell:
```
exec "$SHELL"
```
install the a lower version of python:
```
pyenv install 3.10
pyenv global 3.10
```

gasket builder github
https://github.com/jnicolson/gasket-builder

```
sudo mkdir -p ~/packages/coraltpu
cd ~/packages/coraltpu
sudo apt -y install debhelper devscripts dh-dkms
git clone https://github.com/google/gasket-driver.git
cd gasket-driver
debuild -us -uc -tc -b
cd ..
sudo dpkg -i gasket-dkms_*_all.deb
```
Confirm issue was resolved:
```
sudo apt-get install gasket-dkms libedgetpu1-std
sudo apt-get update && sudo apt upgrade -y
```

Create user and group
```
sudo sh -c "echo 'SUBSYSTEM==\"apex\", MODE=\"0660\", GROUP=\"apex\"' >> /etc/udev/rules.d/65-apex.rules"
sudo groupadd apex
sudo adduser $USER apex
```

REBOOT

check device is present and PCIE drivers are working
```
lspci -nn | grep 089a
ls /dev/apex_0
```

Install device drivers... if it works

```
sudo apt-get install python3-pycoral
```

if it does not work install the wheel

```
cd ~
mkdir -p coral
cd coral/
wget https://github.com/hjonnala/snippets/raw/main/wheels/python3.10/pycoral-2.0.0-cp310-cp310-linux_x86_64.whl
wget https://github.com/hjonnala/snippets/raw/main/wheels/python3.10/tflite_runtime-2.5.0.post1-cp310-cp310-linux_x86_64.whl
pip install --upgrade numpy==1.26.4
```

  
```
sudo apt install python3-pip
pip install tflite_runtime-2.5.0.post1-cp310-cp310-linux_x86_64.whl
pip install pycoral-2.0.0-cp310-cp310-linux_x86_64.whl
```

```
git clone https://github.com/google-coral/pycoral.git
cd pycoral
bash examples/install_requirements.sh classify_image.py
python3 examples/classify_image.py --model test_data/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite --labels test_data/inat_bird_labels.txt --input test_data/parrot.jpg
```


test if docker image can use TPU
```
 cd ~ \
 && apt-get -y update \
 && apt-get -y install curl \
 && mkdir -p test_data \
 && cd test_data \
 && curl -LO https://coral.ai/static/docs/images/parrot.jpg \
 && curl -LO https://raw.githubusercontent.com/google-coral/test_data/master/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite \
 && curl -LO https://raw.githubusercontent.com/google-coral/test_data/master/inat_bird_labels.txt \
 && cd .. \
 && curl -LO https://raw.githubusercontent.com/google-coral/pycoral/master/examples/classify_image.py \
 && python3 classify_image.py \
  --model test_data/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite  \
  --labels test_data/inat_bird_labels.txt \
  --input test_data/parrot.jpg
  ```

