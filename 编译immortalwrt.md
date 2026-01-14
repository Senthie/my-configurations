<!--
 * @Author: '浪川' '1214391613@qq.com'
 * @Date: 2025-07-10 11:33:02
 * @LastEditors: '浪川' '1214391613@qq.com'
 * @LastEditTime: 2025-09-13 12:21:47
 * @FilePath: \passiflora-edulis-simsc:\Users\ToPModelOffic\Desktop\编译immortalwrt.md
 * @Description: 
 * 
 * Copyright (c) 2025 by '1214391613@qq.com', All Rights Reserved. 
-->
# 编译 immortalwrt

```sh
# 拉取仓库
sudo git clone -b openwrt-25.12 --single-branch --filter=blob:none https://github.com/immortalwrt/immortalwrt

# 安装相关依赖
sudo apt update && sudo apt install -y build-essential clang flex bison g++ gawk gcc-multilib g++-multilib gettext git libncurses5-dev libssl-dev python3 python3-setuptools rsync unzip zlib1g-dev file wget mkisofs

sudo bash -c 'bash <(curl -s https://build-scripts.immortalwrt.org/init_build_environment.sh)'

# 设置权限
sudo chown -R $USER immortalwrt

cd immo*

# 
sudo chgrp -R $USER *

# 一键替换源
sed -i '1i src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
sed -i '2i src-git small https://github.com/kenzok8/small' feeds.conf.default
./scripts/feeds update -a

# 删除一些包
rm -rf feeds/luci/applications/luci-app-mosdns
rm -rf feeds/packages/net/{alist,adguardhome,mosdns,xray*,v2ray*,sing*,smartdns}
rm -rf feeds/packages/utils/v2dat
rm -rf feeds/packages/lang/golang

git clone https://github.com/kenzok8/golang -b 1.25 feeds/packages/lang/golang

# 移除 openwrt feeds 自带的核心库
rm -rf feeds/packages/net/{xray-core,v2ray-geodata,sing-box,chinadns-ng,dns2socks,hysteria,ipt2socks,microsocks,naiveproxy,shadowsocks-libev,shadowsocks-rust,shadowsocksr-libev,simple-obfs,tcping,trojan-plus,tuic-client,v2ray-plugin,xray-plugin,geoview,shadow-tls}

# 克隆passwall的包
git clone https://github.com/Openwrt-Passwall/openwrt-passwall-packages package/passwall-packages

# 移除 openwrt feeds 过时的luci版本
rm -rf feeds/luci/applications/luci-app-passwall
git clone https://github.com/Openwrt-Passwall/openwrt-passwall2 package/passwall-luci

./scripts/feeds install -a

make menuconfig
make download -j$(nproc)  V=s

# 编译immortalwrt
make -j$(nproc) V=s
# 或者
make -j1 V=s 2>&1 | tee build.log
```

## 常用软件源

[ ] luci-app-adgroudhome
[ ] luci-app-mosdns
[ ] luci-app-Openclash
[ ] luci-app-adgroudhome
[ ] luci-app-adgroudhome
[ ] luci-app-adgroudhome

## pve 安装 openwrt

```sh
# 方法1：直接使用 squashfs包
qm importdisk 105 /var/lib/vz/template/iso/immortalwrt-x86-64-generic-squashfs-combined-efi-by.img local
```

```sh
# 方法2：编译quem
# 1. 安装 编译工具
sudo apt install -y build-essential libncurses5-dev gawk git subversion libssl-dev gettext zlib1g-dev
sudo apt-get install qemu-utils
```
