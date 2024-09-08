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
git branch -a | grep linux-5
    remotes/origin/linux-5.12.y
    remotes/origin/linux-5.11.y
    remotes/origin/linux-5.10.y

​git checkout linux-5.12.y
```
- Latest stable branch as of 08/09/2024 
- [List of all tree's latest branches](https://www.kernel.org/)
