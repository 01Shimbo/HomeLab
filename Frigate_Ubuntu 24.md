Frigate Docker container stack:


Coral TPU Driver Install (2024/10/22)
Add google tpu repo
<pre>
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
</pre>

attempt to install gasket THIS WILL MOST LIKELY NOT WORK
this will not work because gasket has been deprecated in >python3.10
<pre>
sudo apt-get install gasket-dkms libedgetpu1-std
</pre>

FIX:
install second python environment DO NOT DOWNGRADE CURRENT PYTHON VERSION (aptitude will not work anymore)
<https://idroot.us/install-pyenv-ubuntu-24-04/>
<pre>
sudo apt-get update && sudo apt upgrade -y
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
curl https://pyenv.run | bash
nano ~/.bashrc
</pre>
Add the following lines to the end of the file:
<pre>
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
</pre>
<pre>
exec "$SHELL"
</pre>
<pre>
pyenv install 3.10
pyenv global 3.10
</pre>

<pre>
sudo mkdir /lib/gasket-driver/
cd /lib/gasket-driver
sudo apt -y install debhelper devscripts
git clone https://github.com/google/gasket-driver.git
cd gasket-driver
debuild -us -uc -tc -b
sudo dpkg -i gasket-dkms_1.0-18_all.deb
</pre>
Confirm issue was resolved:
<pre>
sudo apt-get update && sudo apt upgrade -y
sudo apt-get install gasket-dkms libedgetpu1-std
</pre>

Create user and group
<pre>
sudo sh -c "echo 'SUBSYSTEM==\"apex\", MODE=\"0660\", GROUP=\"apex\"' >> /etc/udev/rules.d/65-apex.rules"
sudo groupadd apex
sudo adduser $USER apex
</pre>

REBOOT

check device is present and PCIE drivers are working
<pre>
lspci -nn | grep 089a
ls /dev/apex_0
</pre>

Install device drivers... if it works

<pre>
sudo apt-get install python3-pycoral
</pre>

if it does not work install the wheel

<pre>
cd ~
mkdir coral
cd coral/
wget https://github.com/hjonnala/snippets/raw/main/wheels/python3.10/pycoral-2.0.0-cp310-cp310-linux_x86_64.whl
wget https://github.com/hjonnala/snippets/raw/main/wheels/python3.10/tflite_runtime-2.5.0.post1-cp310-cp310-linux_x86_64.whl

<pre>
sudo apt install python3-pip
pip install tflite_runtime-2.5.0.post1-cp310-cp310-linux_x86_64.whl
pip install pycoral-2.0.0-cp310-cp310-linux_x86_64.whl
</pre>

git clone https://github.com/google-coral/pycoral.git
cd pycoral
bash examples/install_requirements.sh classify_image.py
python3 examples/classify_image.py --model test_data/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite --labels test_data/inat_bird_labels.txt --input test_data/parrot.jpg

</pre>



Ifnumpy is to0 new:
<pre>
pip install --upgrade numpy==1.26.4
</pre>
