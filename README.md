# LEARNING LINUX KERNEL DEVELPOMENT 
## Build essintial packages (might already be installed)
```
sudo apt-get install build-essential vim git cscope libncurses-dev libssl-dev bison flex
sudo apt-get install git-email
```
- [check minimal requirment](https://www.kernel.org/doc/html/latest/process/changes.html)

# Cloning the Stable Kernel Git
```
git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git linux_stable
cd linux_stable
git branch -a | grep linux-6
  remotes/origin/linux-6.0.y
  remotes/origin/linux-6.1.y
  remotes/origin/linux-6.10.y
  remotes/origin/linux-6.2.y
  remotes/origin/linux-6.3.y
  remotes/origin/linux-6.4.y
  remotes/origin/linux-6.5.y
  remotes/origin/linux-6.6.y
  remotes/origin/linux-6.7.y
  remotes/origin/linux-6.8.y
  remotes/origin/linux-6.9.y

​git checkout linux-6.9.y
```
- Latest stable branch as of 08/09/2024 is linux-6.9.y
- [List of all tree's latest branches](https://www.kernel.org/)

# Copying the Configuration for Current Kernel from /boot

- copy the kernel configuration file from your existing system to the kernel tree to update the configuration

``` 
ls /boot                                                                              
config-6.8.0-44-generic  grub  initrd.img  initrd.img-6.8.0-44-generic  initrd.img.old  lost+found  System.map-6.8.0-44-generic  vmlinuz  vmlinuz-6.8.0-44-generic  vmlinuz.old

cp /boot/config-6.8.0-44-generic .config
```

# Building the kernel

```
make oldconfig
```
- This command updates your existing kernel configuration based on the latest changes in the source code. It prompts you only for new options that were added since the last configuration file was created. This is useful if you already have a .config file (from a previous build or your system's default kernel configuration) and just want to bring it up to date.

```
lsmod > /tmp/my-lsmod
make LSMOD=/tmp/my-lsmod localmodconfig
```
- This command creates a new .config file based on the modules currently loaded on your system. It examines the modules loaded by the running kernel (lsmod) and builds a configuration file tailored to only those modules. This trims down the kernel by disabling drivers and features that your system doesn’t need, resulting in a leaner kernel with potentially faster boot times and less resource usage.

```
make -j3 all
```
-  If you skip the make oldconfig or make localmodconfig step and directly run make all, the build process will likely invoke make oldconfig internally. However, it might prompt you interactively during the build to make choices about new kernel options. This interrupts the build process and can be confusing if you're unfamiliar with kernel configuration options.

## Error 

- It was likely because I copied a configuration file from a different Linux tree into my latest Linux stable version.

## Solution 

- Make sure you have all the necessary development tools and kernel build dependencies installed.
make defconfig

```
sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev
```

- Use the default configuration for your system . Once the configuration is done, start the build process. You can specify the number of parallel jobs with -j to speed up the build (use nproc to detect the number of available cores).

```
make defconfig
make -j$(nproc)
```

- If everyting goes well you will recieve logs similar to this

```
Kernel: arch/x86/boot/bzImage is ready  (#1)
```

# Installing the New Kernel

- Installing the new kernel 

```
su -c "make modules_install install"

make modules_install: This step installs the kernel modules (driver files).
make install: This installs the compiled kernel, copies the necessary files to /boot, and updates the bootloader configuration.
```

- capture and filter dmesg logs (kernel ring buffer messages) into different files, each with a specific focus.

```
sudo su 

dmesg -t > dmesg_current
dmesg -t -k > dmesg_kernel
dmesg -t -l emerg > dmesg_current_emerg
dmesg -t -l alert > dmesg_current_alert
dmesg -t -l crit > dmesg_current_crit
dmesg -t -l err > dmesg_current_err
dmesg -t -l warn > dmesg_current_warn
dmesg -t -l info > dmesg_current_info
```

- In general, dmesg should be clean, with no emerg, alert, crit, and err level messages. If you see any of these, it might indicate some hardware and/or kernel problem.
- If the dmesg_current is zero length, it is very likely that secure boot is enabled on your system. When secure boot is enabled, you won’t be able to boot the newly installed kernel, as it is unsigned. You can disable secure boot temporarily on startup with MOK manager. Your system should already have mokutil.

**When secure boot is enabled, you won’t be able to boot the newly installed kernel, as it is unsigned. You can disable secure boot temporarily on startup with MOK manager. Your system should already have mokutil , if not install it `sudo apt install mokutil`**





