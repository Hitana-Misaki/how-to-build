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
sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev libelf-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev python-is-python3
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

## Building
To build ROMs, you need to set up `WORKSPACE` dir for it, for example you want to build LineageOS 20:
```sh
mkdir lineage20
```
then you change the directory:
```sh
cd lineage20
```
after that, you need to fetch ROM sources (repo init) and sync it
>you can see the guide at LineageOS/android (depends with your ROM that you want to build, some ROMs manifest are located in manifest or android_manifest) github repository's README.md

If syncing ROM sources is done, next is clone device tree
```sh
# Android 15
git clone --depth=1 -b fifteen https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog

# Android 14
git clone --depth=1 -b fourteen-qpr3 https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog

# clone hardware/xiaomi as well
# remove the comment (#) in 'git clone ... hardware/xiaomi' line at vendorsetup.sh
```
After cloning your device tree, you need to adapt it to custom ROM that you want to build. To adapt, you need to edit `AndroidProducts.mk` and `ROMNAME*_fog.mk`

**ROMNAME is also need to renamed, in A14 tree, default ROMNAME is AOSP. for A13 tree, default ROMNAME is lineage.*

You can see other device tree for other devices as reference for ROM adaptation.
After you adapt device tree, next is building the ROM itself
```sh
# set up build env variables
. build/envsetup.sh
# since my tree now ship with vendorsetup.sh, envsetup will doing set up a vendor tree and kernel tree automatically
lunch ROMNAME_fog-userdebug
# depends with custom ROM, you can read their manifest README.md for 'make' command
m bacon -j$(nproc --all)
```

## Resources
NOTE: you can set specific branches by adding `-b $BRANCHNAME`
- Device tree :
  ```sh
  # default branch, which is for Android 14
  git clone --depth=1 https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog

  # use this branch if you want to build Android 15
  git clone --depth=1 -b fifteen https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog

  # use this branch if you want to build Android 14, and use OSS kernel
  git clone --depth=1 -b pixelbuilds https://github.com/alternoegraha/device_xiaomi_fog device/xiaomi/fog
  ```
- Vendor tree :
  ```sh
  # default branch, which is for Android 15
  git clone --depth=1 -b fifteen https://github.com/alternoegraha/vendor_xiaomi_fog vendor/xiaomi/fog

  # use this branch if you want to build Android 14
  git clone --depth=1 -b fourteen https://github.com/alternoegraha/vendor_xiaomi_fog vendor/xiaomi/fog
  ```
- Kernel prebuilt (do not clone if you're using OSS kernel // Avoid using it when not needed - OSS kernel is more stable, fp issue has been fixed) :
  ```sh
  git clone --depth=1 https://github.com/alternoegraha/device_xiaomi_fog-kernel device/xiaomi/fog-kernel
  ```
- Kernel headers :
  ```sh
  git clone --depth=1 https://github.com/alternoegraha/kernel_xiaomi_fog_header kernel/xiaomi/fog
  ```
- Kernel OSS :
  ```sh
  git clone --depth=1 -b another-reset https://github.com/alternoegraha/wwy_kernel_xiaomi_fog_rebase kernel/xiaomi/fog
  ```
