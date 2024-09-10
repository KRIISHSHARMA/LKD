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
