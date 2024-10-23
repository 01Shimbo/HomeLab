Frigate Docker container stack:


Coral TPU Driver Install (2024/10/22)


Issue:
Deprecated feature: REMAKE_INITRD (/var/lib/dkms/gasket/1.0/source/dkms.conf)

Solution compile driver:
<pre> git clone https://github.com/google/gasket-driver.git
cd gasket-driver
debuild -us -uc -tc -b
sudo dpkg -i gasket-dkms_1.0-18_all.deb </pre>
Confirm issue was resolved:
   sudo apt-get update && sudo apt upgrade -y
   sudo apt-get install gasket-dkms libedgetpu1-std


