# how-to-build
> *ROM building guide for Xiaomi Redmi 10C (fog)*
> - the guide still work in progress i guess*
## System requirements
Before building ROM, you need a computer or server with these specifications:
- Ubuntu Linux OS (latest version is recommended, other distro can work but untested)
- 8 vCPU / 8 core CPU or higher
- 32GB RAM (64GB recommended)
- ZRAM enabled
- 1TB data disk storage

## Set up
Install required dependencies:
```sh
sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev libelf-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev
```
then install Repo:
```sh
sudo apt update
sudo apt install repo
```
if you using Ubuntu LTS, using `repo` from Ubuntu repositories is not recommended, so use this:
```sh
# taken from https://source.android.com/docs/setup/download#installing-repo

export REPO=$(mktemp /tmp/repo.XXXXXXXXX)
curl -o ${REPO} https://storage.googleapis.com/git-repo-downloads/repo
gpg --recv-keys 8BB9AD793E8E6153AF0F9A4416530D5E920F5C65
curl -s https://storage.googleapis.com/git-repo-downloads/repo.asc | gpg --verify - ${REPO} && install -m 755 ${REPO} ~/bin/repo
```
Done! you can build custom ROMs (make sure to read guide on manifest/android repo)
## Resources
NOTE: you can set specific branches by adding `-b $BRANCHNAME`
- Device tree :
  ```sh
  # default branch, which is for Android 14
  git clone https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog

  # use this branch if you want to build Android 13
  git clone -b lineage-20-new https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog

  # use this branch if you want to build Android 13, and use OSS kernel
  git clone -b lineage-20-oss https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog
  ```
- Vendor tree :
  ```sh
  # default branch, which is for Android 14
  git clone https://github.com/alternoegraha/vendor_xiaomi_fog vendor/xiaomi/fog

  # use this branch if you want to build Android 13
  git clone -b lineage-20-new https://github.com/alternoegraha/vendor_xiaomi_fog vendor/xiaomi/fog
  ```
- Kernel prebuilt (do not clone if you're using OSS kernel) :
  ```sh
  git clone https://github.com/alternoegraha/device_xiaomi_fog-kernel device/xiaomi/fog-kernel
  ```
- Kernel headers :
  ```sh
  git clone https://github.com/alternoegraha/kernel_xiaomi_fog_header kernel/xiaomi/fog
  ```
- Kernel OSS (unstable) :
  ```sh
  git clone https://github.com/alternoegraha/kernel_xiaomi_fog kernel/xiaomi/fog
  ```
