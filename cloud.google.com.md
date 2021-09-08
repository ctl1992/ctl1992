# 可控列表快捷方式, pin码六个六：

https://remotedesktop.google.com/access

# 被控端自动化脚本

https://cloud.google.com/architecture/chrome-desktop-remote-on-compute-engine#automating_the_installation_process

注意设置：

硬盘100G

INSTALL_CINNAMON=yes

INSTALL_FULL_DESKTOP=yes

```
#!/bin/bash -x
#
# Startup script to install Chrome remote desktop and a desktop environment.
#
# See environmental variables at then end of the script for configuration
#

function install_desktop_env {
  PACKAGES="desktop-base xscreensaver dbus-x11"

  if [[ "$INSTALL_XFCE" != "yes" && "$INSTALL_CINNAMON" != "yes" ]] ; then
    # neither XFCE nor cinnamon specified; install both
    INSTALL_XFCE=yes
    INSTALL_CINNAMON=yes
  fi

  if [[ "$INSTALL_XFCE" = "yes" ]] ; then
    PACKAGES="$PACKAGES xfce4"
    echo "exec xfce4-session" > /etc/chrome-remote-desktop-session
    [[ "$INSTALL_FULL_DESKTOP" = "yes" ]] && \
      PACKAGES="$PACKAGES task-xfce-desktop"
  fi

  if [[ "$INSTALL_CINNAMON" = "yes" ]] ; then
    PACKAGES="$PACKAGES cinnamon-core"
    echo "exec cinnamon-session-cinnamon2d" > /etc/chrome-remote-desktop-session
    [[ "$INSTALL_FULL_DESKTOP" = "yes" ]] && \
      PACKAGES="$PACKAGES task-cinnamon-desktop"
  fi

  DEBIAN_FRONTEND=noninteractive \
    apt-get install --assume-yes $PACKAGES $EXTRA_PACKAGES

  systemctl disable lightdm.service
}

function download_and_install { # args URL FILENAME
  curl -L -o "$2" "$1"
  dpkg --install "$2"
  apt-get install --assume-yes --fix-broken
}

function is_installed {  # args PACKAGE_NAME
  dpkg-query --list "$1" | grep -q "^ii" 2>/dev/null
  return $?
}

# Configure the following environmental variables as required:
INSTALL_XFCE=yes
INSTALL_CINNAMON=yes
INSTALL_CHROME=yes
INSTALL_FULL_DESKTOP=yes

# apt-get install --assume-yes $EXTRA_PACKAGES
EXTRA_PACKAGES="less bzip2 zip unzip tasksel wget python3-pip gedit git"

apt-get update

# Install backports version of libgbm1 on Debian 9/stretch
[[ $(/usr/bin/lsb_release --codename --short) == "stretch" ]] && \
  apt-get install --assume-yes libgbm1/stretch-backports

! is_installed chrome-remote-desktop && \
  download_and_install \
    https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb \
    /tmp/chrome-remote-desktop_current_amd64.deb

install_desktop_env

[[ "$INSTALL_CHROME" = "yes" ]] && \
  ! is_installed google-chrome-stable && \
  download_and_install \
    https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb \
    /tmp/google-chrome-stable_current_amd64.deb

echo "Chrome remote desktop installation completed"
```

继续设置

https://cloud.google.com/architecture/chrome-desktop-remote-on-compute-engine#configuring_and_starting_the_chrome_remote_desktop_service

# putty命令行工具链接

https://cloud.google.com/compute/docs/instances/connecting-advanced#windows-putty


# 开机继续安装软件

sudo passwd # 修改root密码，好像不能删除密码

sudo apt-get install git # 此命令后好像无法google remote desktop, 可能是因为git修改了ssh秘钥，或者安装冲突，卸载了google remote desktop

# 重新安装google remote desktop

sudo apt-get install --assume-yes --fix-broken

sudo dpkg --install /tmp/chrome-remote-desktop_current_amd64.deb

# 继续安装软件

sudo apt install python3-pip gedit git

mkdir Python3Projects

git clone https://github.com/ctl1992/gTTS_ixibanyayu.com.git

cd gTTS_ixibanyayu.com

python3 a1.py

pip3 install --user selenium gTTs num2words xlwt xlrd

pip3 install --user pandas #耗时较长


# 安装chromedriver

unzip chromedriver_linux64.zip

echo $PATH # 将chromedriver放置在/usr/bin下

sudo mv chromedriver /usr/bin

cd /usr/bin

ls -l | less
