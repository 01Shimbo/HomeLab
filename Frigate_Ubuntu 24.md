Frigate Docker container stack:


Coral TPU Driver Install (2024/10/22)
Add google tpu repo
<pre>
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
</pre>

attempt to install gasket THIS WILL MOST LIKLY NOT WORK
this will not work because gasket has been depricated in >python3.10
<pre>
sudo apt-get install gasket-dkms libedgetpu1-std
</pre>
FIX:
install secound python envorment DO NOT DOWNGRADE CURRENT PYTHON VERSION (aptitude will not work anymore)
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
pyenv install 3.9.7
pyenv global 3.9.7
</pre>

Solution compile driver:
<pre>
sudo apt -y install debhelper
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

