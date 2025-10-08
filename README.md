# install-openwrt-to-pc-mainboard 
نصب سیستم عامل openwrt در مادربرد کامپیوتر   
بهمراه پکیج پسوال + روتینگ کامل سایتهای ایرانی  
**توضیحات فارسی-Persian**  
[برای دیدن توضیحات فارسی کلیک کنید](README.FA.md)

You will learn how to install the OpenWRT operating system and the Passwall 2 package along with the Iranian rootkit in this repository, along with a YouTube tutorial video.  

**install openwrt 23.05.3 to pc mainboard(normal bios not efi) and install passwall2 package**  

**installing openwrt video tutorial**  

## 📺 Video Tutorial
🎥 Watch the full installation and usage guide on YouTube:  
👉 [Click here to watch the video](https://www.youtube.com/watch?v=WZkUOihlim0)


# Now after installation we need to increase our disk size to install packages. Enter the commands as per the tutorial video.  

# Configure startup scripts   

-----  
```
cat << "EOF" > /etc/uci-defaults/70-rootpt-resize
if [ ! -e /etc/rootpt-resize ] \
&& type parted > /dev/null \
&& lock -n /var/lock/root-resize
then
ROOT_BLK="$(readlink -f /sys/dev/block/"$(awk -e \
'$9=="/dev/root"{print $3}' /proc/self/mountinfo)")"
ROOT_DISK="/dev/$(basename "${ROOT_BLK%/*}")"
ROOT_PART="${ROOT_BLK##*[^0-9]}"
parted -f -s "${ROOT_DISK}" \
resizepart "${ROOT_PART}" 100%
mount_root done
touch /etc/rootpt-resize
reboot
fi
exit 1
EOF
cat << "EOF" > /etc/uci-defaults/80-rootfs-resize
if [ ! -e /etc/rootfs-resize ] \
&& [ -e /etc/rootpt-resize ] \
&& type losetup > /dev/null \
&& type resize2fs > /dev/null \
&& lock -n /var/lock/root-resize
then
ROOT_BLK="$(readlink -f /sys/dev/block/"$(awk -e \
'$9=="/dev/root"{print $3}' /proc/self/mountinfo)")"
ROOT_DEV="/dev/${ROOT_BLK##*/}"
LOOP_DEV="$(awk -e '$5=="/overlay"{print $9}' \
/proc/self/mountinfo)"
if [ -z "${LOOP_DEV}" ]
then
LOOP_DEV="$(losetup -f)"
losetup "${LOOP_DEV}" "${ROOT_DEV}"
fi
resize2fs -f "${LOOP_DEV}"
mount_root done
touch /etc/rootfs-resize
reboot
fi
exit 1
EOF
cat << "EOF" >> /etc/sysupgrade.conf
/etc/uci-defaults/70-rootpt-resize
/etc/uci-defaults/80-rootfs-resize
EOF
```

------
# Install packages

```  
opkg update
```
```
opkg install parted losetup resize2fs
```
# Expand root partition/filesystem

```
sh /etc/uci-defaults/70-rootpt-resize
``` 

 # Now you can easily install the passwall.
 
# System Requirements :

- CPU : `+700 MHz ✅`

- RAM : `+256 MB ✅`

- ⚠️ Before Installation, Make sure `Wan Address` And `Lan address` are not same !
     
# INSTALL PASSWALL : 
Run this command in openwrt remote ssh to install passwall2 

🔴🔴🔴 The Routers With `128 MB RAM Memory` , Please `downgrade` to [OPENWRT 22.03.3](https://archive.openwrt.org/releases/22.03.3/targets/) 🔴🔴🔴

```
rm -f passwallx.sh && wget https://raw.githubusercontent.com/amirhosseinchoghaei/Passwall/main/passwallx.sh && chmod 777 passwallx.sh && sh passwallx.sh
```
Done !

# Types Support

[Mahsa Core](https://github.com/GFW-knocker/Xray-core/releases) ↩️

VLESS (XRAY ✅ SING-BOX ✅)

VMESS (XRAY ✅ SING-BOX ✅)

REALITY (XRAY ✅ SING-BOX ❌)

TROJAN (XRAY ✅ SING-BOX ✅)

HYSTERIA2 (XRAY ❌ SING-BOX ✅)

TUIC (XRAY ❌ SING-BOX ✅)

SHADOWSOCKS (XRAY ✅ SING-BOX ✅)

WIREGUARD (XRAY ✅ SING-BOX ✅)

SOCKS (XRAY ✅ SING-BOX ✅)

HTTP (XRAY ✅ SING-BOX ✅)


#

✅ Tested On : Xiaomi 4a Gigabit , TP-Link C6 v3 , Mikrotik Hap ac2 , GL.iNet AR300M (NOR) , Linksys ea7500. v1-v2 , Linksys ea8100 v1-v2 , ASUS RT-N66U , Xiaomi AX3600 , Xiomi AX3200 , Xiaomi AX6000 , Xiaomi AX3000T ,  Linksys EA8300 , Netgear r7800 , Super x7 Server Mainboars

# Features

⚡ Full Automatic installation Packages Just in one step

⚡ Install XRAY On Temp Space if You Don't Have Enough Disk Space (Smart)

⚡ IRAN IP & Domain Traffic Direct (100%)

⚡ Improve Performance

⚡ Server WARP Connection Fixed

⚡ Default Kill Switch

⚡ XRAY Fragment tlshello | 1-3
