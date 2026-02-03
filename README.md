This repo contains patches to enable an Orange Pi 3B to boot from SPI flash to a SATA M.2 SSD.

Note that the patches drop support for NVME M.2 SSDs as a consequence.

# How to apply patches to your Orange Pi 3B

Pick any one method (ordered from easiest to hardest):
1. [Use Armbian image with sata patches applied](#use-armbian-image-with-sata-patches-applied)
2. [Build patched image using Github Actions](#build-patched-image-using-github-actions)
3. [Import patches to your own Armbian build repo](#import-patches-to-your-own-armbian-build-repo)
4. [Install patched linux-u-boot and linux-dtb debs](#install-patched-linux-u-boot-and-linux-dtb-debs)

### Use Armbian image with sata patches applied
1. Download image from Releases > Current branch > Assets > [Armbian-unofficial_... .img.xz](https://github.com/dust-7/opi3b-sata/releases/download/v6.18/Armbian-unofficial_26.2.2_Orangepi3b_trixie_current_6.18.8.img.xz)
2. Flash image to SD card and boot.
3. From shell, type `nand-sata-install`.
4. Select `Boot from MTD Flash - system on SATA, USB or NVMe`.

### Build patched image using Github Actions
1. Clone this repo.
2. Build your own image using Github Actions.

### Import patches to your own Armbian build repo
1. Copy [userpatches](https://github.com/dust-7/opi3b-sata/tree/main/userpatches) folder into your build repo.
2. Build image. Note that patches only apply to Current branch (kernel v6.18). Example: `./compile.sh build BOARD=orangepi3b BRANCH=current BUILD_DESKTOP=no BUILD_MINIMAL=no KERNEL_CONFIGURE=no RELEASE=trixie`

### Install patched linux-u-boot and linux-dtb debs
1. Build your own Armbian image (must be Current branch). Example: `./compile.sh build BOARD=orangepi3b BRANCH=current BUILD_DESKTOP=no BUILD_MINIMAL=no KERNEL_CONFIGURE=no RELEASE=trixie`
2. Download debs from Releases > Current branch > Assets.
   * linux-u-boot: [linux-u-boot-... .deb](https://github.com/dust-7/opi3b-sata/releases/download/v6.18/linux-u-boot-orangepi3b-current_26.2.2_arm64__2024.10-Sf919-P4128-H8eee-Vb490-B2eb2-R448a.deb)
   * linux-dtb: [linux-dtb-... .deb](https://github.com/dust-7/opi3b-sata/releases/download/v6.18/linux-dtb-current-rockchip64_26.2.2_arm64__6.18.8-Sd905-D1d59-P1c42-C2cceHd6dd-HK01ba-Vc222-B052a-R448a.deb)
4. Install debs using `sudo dpkg -i <path to deb>`.
5. Flash newly-installed U-Boot to SD Card using `nand-sata-install`.
6. Reboot. After reboot you should be able to see your SATA SSD (e.g. from `lsblk`).
7. Flash U-Boot to SPI flash and system to SSD using `nand-sata-install`.
