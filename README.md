欢迎来到Redmi AX6的Openwrt源码仓库！
=


注意：
-
1. **不**要用 **root** 用户进行编译！！！
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1 密码 password


编译命令如下:
-
1. 首先装好 Ubuntu 64bit，推荐 Ubuntu 20.04 LTS x64

2. 命令行输入 `sudo apt-get update` ，然后输入
   `
   sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget curl swig rsync
   `


3. 下载源代码，更新 feeds 并选择配置/   [Redmi-AX6-config配置文件](./AX6config配置文件.config)。

   ```bash
   git clone https://github.com/yaya131/lede-for-XaioMi openwrt
   cd openwrt
   sed -i '$a src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
   sed -i '$a src-git small https://github.com/kenzok8/small' feeds.conf.default
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

4. Redmi-AX6编译最新Lede源码/   [Redmi-AX6-config配置文件](./AX6config配置文件.config)。

   ```bash
   git clone https://github.com/coolsnowwolf/lede openwrt
   svn co https://github.com/fichenx/OpenWrt/trunk/general/AX6
   cp -rf AX6/* openwrt/
   cd openwrt
   sed -i '$a src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
   sed -i '$a src-git small https://github.com/kenzok8/small' feeds.conf.default
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

5. 下载 dl 库，编译固件
（-j 后面是线程数，第一次编译推荐用单线程）

   ```bash
   make -j8 download V=s
   make -j1 V=s
   ```

6.更改Argon主题
   ```bash
   rm -rf feeds/luci/themes/luci-theme-argon && git clone -b 18.06 https://github.com/jerrykuku/luci-theme-argon.git feeds/luci/themes/luci-theme-argon
   ```
   
7.编译旧版openclash 0.44.29
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
  
  如果需要重新配置：

  ```bash
  rm -rf ./tmp && rm -rf .config
  make menuconfig
  make -j12 V=s
  ```

  编译完成后输出路径：bin/targets
  
  下载固件到MAC桌面：

  ```bash
  scp -r bin/targets/ipq807x/generic mac@192.168.10.204:/Users/mac/Desktop
  ```
  
  从虚拟机下载固件到桌面：

  ```bash
  scp -r parallels@192.168.10.230:/home/parallels/openwrt/bin/targets/ipq807x/generic /Users/mac/Desktop
  ```
  
  下载openclash内核到MAC桌面：

  ```bash
  scp -r root@192.168.10.3:/etc/openclash/core /Users/mac/Desktop
  ```
  
  上传openclash内核到路由器：

  ```bash
  scp /Volumes/共享磁盘/Redmi-AX6自己编译/openclash/* root@192.168.10.1:/etc/openclash/core
  ```
  
  清除ssh缓存：

  ```bash
  rm -rf ~/.ssh/known_hosts
  ```
  
 安装aliyundrive-webdav2.1.0：

  ```bash
  wget https://github.com/messense/aliyundrive-webdav/releases/download/v2.1.0/aliyundrive-webdav_2.1.0-1_aarch64_cortex-a53.ipk
wget https://github.com/messense/aliyundrive-webdav/releases/download/v2.1.0/luci-app-aliyundrive-webdav_2.1.0_all.ipk
wget https://github.com/messense/aliyundrive-webdav/releases/download/v2.1.0/luci-i18n-aliyundrive-webdav-zh-cn_2.1.0-1_all.ipk
opkg install aliyundrive-webdav_2.1.0-1_aarch64_cortex-a53.ipk
opkg install luci-app-aliyundrive-webdav_2.1.0_all.ipk
opkg install luci-i18n-aliyundrive-webdav-zh-cn_2.1.0-1_all.ipk
  ```
   
