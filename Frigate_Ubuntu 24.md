Frigate Docker container stack:


Coral TPU Driver Install (2024/10/22)


Issue:
<pre>
Deprecated feature: REMAKE_INITRD (/var/lib/dkms/gasket/1.0/source/dkms.conf)

cat /var/crash/gasket-dkms.0.crash 
ProblemType: Package
DKMSBuildLog:
 DKMS make.log for gasket-1.0 for kernel 6.8.0-47-generic (x86_64)
 Tue Oct 22 06:45:23 PM CDT 2024
 make: Entering directory '/usr/src/linux-headers-6.8.0-47-generic'
 warning: the compiler differs from the one used to build the kernel
   The kernel was built by: x86_64-linux-gnu-gcc-13 (Ubuntu 13.2.0-23ubuntu4) 13.2.0
   You are using:           gcc-13 (Ubuntu 13.2.0-23ubuntu4) 13.2.0
   CC [M]  /var/lib/dkms/gasket/1.0/build/gasket_core.o
   CC [M]  /var/lib/dkms/gasket/1.0/build/gasket_ioctl.o
   CC [M]  /var/lib/dkms/gasket/1.0/build/gasket_interrupt.o
   CC [M]  /var/lib/dkms/gasket/1.0/build/gasket_page_table.o
   CC [M]  /var/lib/dkms/gasket/1.0/build/gasket_sysfs.o
   CC [M]  /var/lib/dkms/gasket/1.0/build/apex_driver.o
 /var/lib/dkms/gasket/1.0/build/gasket_interrupt.c: In function ‘gasket_handle_interrupt’:
 /var/lib/dkms/gasket/1.0/build/gasket_interrupt.c:161:17: error: too many arguments to function ‘eventfd_signal’
   161 |                 eventfd_signal(ctx, 1);
       |                 ^~~~~~~~~~~~~~
 In file included from /var/lib/dkms/gasket/1.0/build/gasket_interrupt.h:11,
                  from /var/lib/dkms/gasket/1.0/build/gasket_interrupt.c:4:
 ./include/linux/eventfd.h:87:20: note: declared here
    87 | static inline void eventfd_signal(struct eventfd_ctx *ctx)
       |                    ^~~~~~~~~~~~~~
 make[2]: *** [scripts/Makefile.build:243: /var/lib/dkms/gasket/1.0/build/gasket_interrupt.o] Error 1
 make[2]: *** Waiting for unfinished jobs....
 /var/lib/dkms/gasket/1.0/build/gasket_core.c: In function ‘gasket_register_device’:
 /var/lib/dkms/gasket/1.0/build/gasket_core.c:1841:41: error: passing argument 1 of ‘class_create’ from incompatible pointer type [-Werror=incompatible-pointer-types]
  1841 |                 class_create(driver_desc->module, driver_desc->name);
       |                              ~~~~~~~~~~~^~~~~~~~
       |                                         |
       |                                         struct module *
 In file included from ./include/linux/device.h:31,
                  from ./include/linux/cdev.h:8,
                  from /var/lib/dkms/gasket/1.0/build/gasket_core.h:11,
                  from /var/lib/dkms/gasket/1.0/build/gasket_core.c:12:
 ./include/linux/device/class.h:228:54: note: expected ‘const char *’ but argument is of type ‘struct module *’
   228 | struct class * __must_check class_create(const char *name);
       |                                          ~~~~~~~~~~~~^~~~
 /var/lib/dkms/gasket/1.0/build/gasket_core.c:1841:17: error: too many arguments to function ‘class_create’
  1841 |                 class_create(driver_desc->module, driver_desc->name);
       |                 ^~~~~~~~~~~~
 ./include/linux/device/class.h:228:29: note: declared here
   228 | struct class * __must_check class_create(const char *name);
       |                             ^~~~~~~~~~~~
 cc1: some warnings being treated as errors
 make[2]: *** [scripts/Makefile.build:243: /var/lib/dkms/gasket/1.0/build/gasket_core.o] Error 1
 make[1]: *** [/usr/src/linux-headers-6.8.0-47-generic/Makefile:1925: /var/lib/dkms/gasket/1.0/build] Error 2
 make: *** [Makefile:240: __sub-make] Error 2
 make: Leaving directory '/usr/src/linux-headers-6.8.0-47-generic'
DKMSKernelVersion: 6.8.0-47-generic
Date: Tue Oct 22 18:45:35 2024
DuplicateSignature: dkms:gasket-dkms:1.0-18:/var/lib/dkms/gasket/1.0/build/gasket_interrupt.c:161:17: error: too many arguments to function ‘eventfd_signal’
Package: gasket-dkms 1.0-18
PackageVersion: 1.0-18
SourcePackage: gasket-dkms
Title: gasket-dkms 1.0-18: gasket kernel module failed to build
</pre>

Solution compile driver:
<pre>
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

