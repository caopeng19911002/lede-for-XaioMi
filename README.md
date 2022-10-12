欢迎来到Lean的Openwrt源码仓库！
=


注意：
-
1. **不**要用 **root** 用户进行编译！！！
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1 密码 password


编译命令如下:
-
1. 首先装好 Ubuntu 64bit，推荐 Ubuntu 20.04 LTS x64

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y ack antlr3 aria2 asciidoc autoconf automake autopoint binutils bison build-essential \
   bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
   git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
   libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
   mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip libpython3-dev qemu-utils \
   rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
   ```

3. 下载源代码，更新 feeds 并选择配置/   [Redmi-AX6-config配置文件](./AX6config配置文件.config)。

   ```bash
   git clone https://github.com/caopeng19911002/lede-for-XaioMi openwrt
   cd openwrt
   sed -i '$a src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
   sed -i '$a src-git small https://github.com/kenzok8/small' feeds.conf.default
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

4. 下载 dl 库，编译固件
（-j 后面是线程数，第一次编译推荐用单线程）

   ```bash
   make -j8 download V=s
   make -j1 V=s
   ```

5.更改Argon主题
   ```bash
   rm -rf feeds/luci/themes/luci-theme-argon && git clone -b 18.06 https://github.com/jerrykuku/luci-theme-argon.git feeds/luci/themes/luci-theme-argon
   ```
   
6.编译旧版openclash 0.44.29
   ```bash
   rm -rf feeds/kenzo/luci-app-openclash && git clone https://github.com/caopeng19911002/openclash-0.44.29.git feeds/kenzo/luci-app-openclash && ./scripts/feeds install -a
   ```
   
   
  本套代码保证肯定可以编译成功。里面包括了 R22 所有源代码，包括 IPK 的。

  你可以自由使用，但源码编译二次发布请注明我的 GitHub 仓库链接。谢谢合作！

  二次编译：

  ```bash
  cd openwrt
  make clean
  git pull
  ./scripts/feeds update -a
  ./scripts/feeds install -a
  make defconfig
  make -j8 download V=s
  make -j12 V=s
  ```
  
  二次编译旧版openclash0.44.29：

  ```bash
  cd openwrt
  make clean
  git pull
  ./scripts/feeds update -a
  rm -rf feeds/kenzo/luci-app-openclash && git clone https://github.com/caopeng19911002/openclash-0.44.29.git feeds/kenzo/luci-app-openclash && ./scripts/feeds install -a
  make defconfig
  make -j8 download V=s
  make -j12 V=s
  ```
   
   
  如果需要重新配置：

  ```bash
  rm -rf ./tmp && rm -rf .config
  make menuconfig
  make -j12 V=s
  ```

  编译完成后输出路径：bin/targets。
