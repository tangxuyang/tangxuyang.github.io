---
layout: post
title: 我的LFS之旅
date: 2017-12-27 12:00
---

### 在虚拟机中安装Ubuntu16.04.3
安装虚拟机很简单，这里就不在赘述了！  

### 检查需要的工具及版本

版本要求(取自LFS-8.1)：  
- Bash-3.2 (/bin/sh should be a symbolic or hard link to bash)
- Binutils-2.17 (Versions greater than 2.29 are not recommended as they have not been tested)
- Bison-2.3 (/usr/bin/yacc should be a link to bison or small script that executes bison)
- Bzip2-1.0.4
- Coreutils-6.9
- Diffutils-2.8.1
- Findutils-4.2.31
- Gawk-4.0.1 (/usr/bin/awk should be a link to gawk)
- GCC-4.7 including the C++ compiler, g++ (Versions greater than 7.2.0 are not recommended as they have not
been tested)
- Glibc-2.11 (Versions greater than 2.26 are not recommended as they have not been tested)
- Grep-2.5.1a
- Gzip-1.3.12
- Linux Kernel-3.2
- M4-1.4.10
- Make-3.81
- Patch-2.5.4
- Perl-5.8.8
- Sed-4.1.5
- Tar-1.22
- Texinfo-4.7
- Xz-5.0.0


{% highlight bash %}
# version-check.sh
#!/bin/bash
# Simple script to list version numbers of critical development tools
export LC_ALL=C
bash --version | head -n1 | cut -d" " -f2-4
MYSH=$(readlink -f /bin/sh)
echo "/bin/sh -> $MYSH"
echo $MYSH | grep -q bash || echo "ERROR: /bin/sh does not point to bash"
unset MYSH
echo -n "Binutils: "; ld --version | head -n1 | cut -d" " -f3-
bison --version | head -n1
if [ -h /usr/bin/yacc ]; then
echo "/usr/bin/yacc -> `readlink -f /usr/bin/yacc`";
elif [ -x /usr/bin/yacc ]; then
echo yacc is `/usr/bin/yacc --version | head -n1`
else
echo "yacc not found"
fi
bzip2 --version 2>&1 < /dev/null | head -n1 | cut -d" " -f1,6-
echo -n "Coreutils: "; chown --version | head -n1 | cut -d")" -f2
diff --version | head -n1
find --version | head -n1
gawk --version | head -n1
if [ -h /usr/bin/awk ]; then
echo "/usr/bin/awk -> `readlink -f /usr/bin/awk`";
elif [ -x /usr/bin/awk ]; then
echo awk is `/usr/bin/awk --version | head -n1`
else
echo "awk not found"
fi

gcc --version | head -n1
g++ --version | head -n1
ldd --version | head -n1 | cut -d" " -f2- # glibc version
grep --version | head -n1
gzip --version | head -n1
cat /proc/version
m4 --version | head -n1
make --version | head -n1
patch --version | head -n1
echo Perl `perl -V:version`
sed --version | head -n1
tar --version | head -n1
makeinfo --version | head -n1
xz --version | head -n1

echo 'int main(){}' > dummy.c && g++ -o dummy dummy.c
if [ -x dummy ]
then echo "g++ compilation OK";
else echo "g++ compilation failed"; fi
rm -f dummy.c dummy
{% endhighlight %}
直接粘贴自LFS。  

然后执行 bash version-check.sh  

以上是2017-12-27的  

跑了version-check.sh，会出现如下的错误，告知我们有些软件没装或版本不符合要求（这个结果是基于Ubuntu16.04.3的）

{% highlight bash %}
bash, version 4.3.48(1)-release
/bin/sh -> /bin/dash
ERROR: /bin/sh does not point to bash
Binutils: version-check.sh: line 9: ld: command not found
version-check.sh: line 10: bison: command not found
yacc not found
bzip2,  Version 1.0.6, 6-Sept-2010.
Coreutils:  8.25
diff (GNU diffutils) 3.3
find (GNU findutils) 4.7.0-git
GNU Awk 4.1.3, API: 1.1 (GNU MPFR 3.1.4, GNU MP 6.1.0)
/usr/bin/awk -> /usr/bin/gawk
version-check.sh: line 30: gcc: command not found
version-check.sh: line 31: g++: command not found
(Ubuntu GLIBC 2.23-0ubuntu9) 2.23
grep (GNU grep) 2.25
gzip 1.6
Linux version 4.4.0-87-generic (buildd@lcy01-31) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) #110-Ubuntu SMP Tue Jul 18 12:55:35 UTC 2017
version-check.sh: line 36: m4: command not found
version-check.sh: line 37: make: command not found
GNU patch 2.7.5
Perl version='5.22.1';
sed (GNU sed) 4.2.2
tar (GNU tar) 1.28
version-check.sh: line 42: makeinfo: command not found
xz (XZ Utils) 5.1.0alpha
version-check.sh: line 44: g++: command not found
g++ compilation failed
{% endhighlight %}

比对LFS8.1的软件包版本要求列表，发现：
- bash的版本符合要求，但是/bin/sh却是指向/bin/dash的，需要改一下： 
{% highlight bash %}
sudo rm /bin/sh
sudo ln -s /bin/bash /bin/sh # 创建软连
{% endhighlight %}
- binutils没有安装, sudo apt-get install binutils
- bison没有安装, sudo apt-get install bison
- gcc没有安装, sudo apt-get install gcc
- g++没有安装, sudo apt-get install g++
- make没有安装, sudo apt-get install make
- texinfo没有安装, sudo apt-get install texinfo

再次运行bash version-check.sh得到如下的结果，满足了要求  
{% highlight bash %}
bash, version 4.3.48(1)-release
/bin/sh -> /bin/bash
Binutils: (GNU Binutils for Ubuntu) 2.26.1
bison (GNU Bison) 3.0.4
/usr/bin/yacc -> /usr/bin/bison.yacc
bzip2,  Version 1.0.6, 6-Sept-2010.
Coreutils:  8.25
diff (GNU diffutils) 3.3
find (GNU findutils) 4.7.0-git
GNU Awk 4.1.3, API: 1.1 (GNU MPFR 3.1.4, GNU MP 6.1.0)
/usr/bin/awk -> /usr/bin/gawk
gcc (Ubuntu 5.4.0-6ubuntu1~16.04.5) 5.4.0 20160609
g++ (Ubuntu 5.4.0-6ubuntu1~16.04.5) 5.4.0 20160609
(Ubuntu GLIBC 2.23-0ubuntu9) 2.23
grep (GNU grep) 2.25
gzip 1.6
Linux version 4.4.0-87-generic (buildd@lcy01-31) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) #110-Ubuntu SMP Tue Jul 18 12:55:35 UTC 2017
m4 (GNU M4) 1.4.17
GNU Make 4.1
GNU patch 2.7.5
Perl version='5.22.1';
sed (GNU sed) 4.2.2
tar (GNU tar) 1.28
texi2any (GNU texinfo) 6.1
xz (XZ Utils) 5.1.0alpha
g++ compilation OK
{% endhighlight %}

### 创建新分区
使用虚拟机很容易添加一块新的硬盘。然后对这个硬盘进行分区，格式化。  
通过虚拟机添加一个新的IDE的硬盘。它的设备号是/dev/sda。虚拟机在安装操作系统时分配的硬盘是SCSI的，它的设备号是sda。但是添加IDE硬盘后，它就变成sdb。如果创建的新硬盘是SCSI的，那么这个新硬盘就是/dev/sdb

先用fdisk程序创建分区，这里我就把这个硬盘创建一个分区,/dev/sda1

{% highlight bash %}
fdisk /dev/sda #进入fdisk程序
n # 创建新的分区，接下来就按回车就好了
w # 保存分区
{% endhighlight %}

### 格式化
{% highlight bash %}
sudo mkfs -v -t ext4 /dev/sdb1 # 我创建的是SCSI的硬盘
{% endhighlight %}
### 挂载
创建一个环境变量LFS: export LFS=/mnt/lfs  
挂载
{% highlight bash %}
sudo mkdir -pv $LFS
sudo mount -v -t ext4 /dev/sdb1 $LFS
{% endhighlight %}
### 下载需要的软件包
- 创建sudo mkdir -v $LFS/sources来存放所有需要的软件包
- 执行sudo chmod -v a+wt $LFS/sources
- wget --input-file=file-name --continue --directory-prefix=$LFS/sources

{% highlight bash linenos%}
http://download.savannah.gnu.org/releases/acl/acl-2.2.52.src.tar.gz
http://download.savannah.gnu.org/releases/attr/attr-2.4.47.src.tar.gz
http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.xz
http://ftp.gnu.org/gnu/automake/automake-1.15.1.tar.xz
http://ftp.gnu.org/gnu/bash/bash-4.4.tar.gz
http://ftp.gnu.org/gnu/bc/bc-1.07.1.tar.gz
http://ftp.gnu.org/gnu/binutils/binutils-2.29.tar.bz2
http://ftp.gnu.org/gnu/bison/bison-3.0.4.tar.xz
http://www.bzip.org/1.0.6/bzip2-1.0.6.tar.gz
https://github.com/libcheck/check/releases/download/0.11.0/check-0.11.0.tar.gz
http://ftp.gnu.org/gnu/coreutils/coreutils-8.27.tar.xz
http://ftp.gnu.org/gnu/dejagnu/dejagnu-1.6.tar.gz
http://ftp.gnu.org/gnu/diffutils/diffutils-3.6.tar.xz
http://dev.gentoo.org/~blueness/eudev/eudev-3.2.2.tar.gz
http://downloads.sourceforge.net/project/e2fsprogs/e2fsprogs/v1.43.5/e2fsprogs-1.43.5.tar.gz
http://prdownloads.sourceforge.net/expat/expat-2.2.3.tar.bz2
http://prdownloads.sourceforge.net/expect/expect5.45.tar.gz
ftp://ftp.astron.com/pub/file/file-5.31.tar.gz
http://ftp.gnu.org/gnu/findutils/findutils-4.6.0.tar.gz
https://github.com/westes/flex/releases/download/v2.6.4/flex-2.6.4.tar.gz
http://ftp.gnu.org/gnu/gawk/gawk-4.1.4.tar.xz
http://ftp.gnu.org/gnu/gcc/gcc-7.2.0/gcc-7.2.0.tar.xz
http://ftp.gnu.org/gnu/gdbm/gdbm-1.13.tar.gz
http://ftp.gnu.org/gnu/gettext/gettext-0.19.8.1.tar.xz
http://ftp.gnu.org/gnu/glibc/glibc-2.26.tar.xz
http://ftp.gnu.org/gnu/gmp/gmp-6.1.2.tar.xz
http://ftp.gnu.org/gnu/gperf/gperf-3.1.tar.gz
http://ftp.gnu.org/gnu/grep/grep-3.1.tar.xz
http://ftp.gnu.org/gnu/groff/groff-1.22.3.tar.gz
http://ftp.gnu.org/gnu/grub/grub-2.02.tar.xz
http://ftp.gnu.org/gnu/gzip/gzip-1.8.tar.xz
http://anduin.linuxfromscratch.org/LFS/iana-etc-2.30.tar.bz2
http://ftp.gnu.org/gnu/inetutils/inetutils-1.9.4.tar.xz
http://launchpad.net/intltool/trunk/0.51.0/+download/intltool-0.51.0.tar.gz
https://www.kernel.org/pub/linux/utils/net/iproute2/iproute2-4.12.0.tar.xz
https://www.kernel.org/pub/linux/utils/kbd/kbd-2.0.4.tar.xz
https://www.kernel.org/pub/linux/utils/kernel/kmod/kmod-24.tar.xz
http://www.greenwoodsoftware.com/less/less-487.tar.gz
http://www.linuxfromscratch.org/lfs/downloads/8.1/lfs-bootscripts-20170626.tar.bz2
https://www.kernel.org/pub/linux/libs/security/linux-privs/libcap2/libcap-2.25.tar.xz
http://download.savannah.gnu.org/releases/libpipeline/libpipeline-1.4.2.tar.gz
http://ftp.gnu.org/gnu/libtool/libtool-2.4.6.tar.xz
https://www.kernel.org/pub/linux/kernel/v4.x/linux-4.12.7.tar.xz
http://ftp.gnu.org/gnu/m4/m4-1.4.18.tar.xz
http://ftp.gnu.org/gnu/make/make-4.2.1.tar.bz2
http://download.savannah.gnu.org/releases/man-db/man-db-2.7.6.1.tar.xz
https://www.kernel.org/pub/linux/docs/man-pages/man-pages-4.12.tar.xz
http://www.multiprecision.org/mpc/download/mpc-1.0.3.tar.gz
http://www.mpfr.org/mpfr-3.1.5/mpfr-3.1.5.tar.xz
http://ftp.gnu.org/gnu//ncurses/ncurses-6.0.tar.gz
http://ftp.gnu.org/gnu/patch/patch-2.7.5.tar.xz
http://www.cpan.org/src/5.0/perl-5.26.0.tar.xz
https://pkg-config.freedesktop.org/releases/pkg-config-0.29.2.tar.gz
http://sourceforge.net/projects/procps-ng/files/Production/procps-ng-3.3.12.tar.xz
https://sourceforge.net/projects/psmisc/files/psmisc/psmisc-23.1.tar.xz
http://ftp.gnu.org/gnu/readline/readline-7.0.tar.gz
http://ftp.gnu.org/gnu/sed/sed-4.4.tar.xz
https://github.com/shadow-maint/shadow/releases/download/4.5/shadow-4.5.tar.xz
http://www.infodrom.org/projects/sysklogd/download/sysklogd-1.5.1.tar.gz
http://download.savannah.gnu.org/releases/sysvinit/sysvinit-2.88dsf.tar.bz2
http://ftp.gnu.org/gnu/tar/tar-1.29.tar.xz
http://sourceforge.net/projects/tcl/files/Tcl/8.6.7/tcl-core8.6.7-src.tar.gz
http://ftp.gnu.org/gnu/texinfo/texinfo-6.4.tar.xz
http://www.iana.org/time-zones/repository/releases/tzdata2017b.tar.gz
http://anduin.linuxfromscratch.org/LFS/udev-lfs-20140408.tar.bz2
https://www.kernel.org/pub/linux/utils/util-linux/v2.30/util-linux-2.30.1.tar.xz
ftp://ftp.vim.org/pub/vim/unix/vim-8.0.586.tar.bz2
http://cpan.metacpan.org/authors/id/T/TO/TODDR/XML-Parser-2.44.tar.gz
http://tukaani.org/xz/xz-5.2.3.tar.xz
http://zlib.net/zlib-1.2.11.tar.xz
http://www.linuxfromscratch.org/patches/lfs/8.1/bash-4.4-upstream_fixes-1.patch
http://www.linuxfromscratch.org/patches/lfs/8.1/bzip2-1.0.6-install_docs-1.patch
http://www.linuxfromscratch.org/patches/lfs/8.1/coreutils-8.27-i18n-1.patch
http://www.linuxfromscratch.org/patches/lfs/8.1/glibc-2.26-fhs-1.patch
http://www.linuxfromscratch.org/patches/lfs/8.1/kbd-2.0.4-backspace-1.patch
http://www.linuxfromscratch.org/patches/lfs/8.1/sysvinit-2.88dsf-consolidated-1.patch
{% endhighlight %}

### 创建tools目录
sudo mkdir -v $LFS/tools  
ln -sv $LFS/tools /

### 创建LFS用户
groupadd lfs  
useradd -s /bin/bash -g lfs -m -k /dev/null fs  
passwd lfs #设置密码

-s表示指定shell, -g表示指定group, -m创建home文件夹  
把$LFS/tools和$LFS/sources所有者设置为lfs, sudo chown -v lfs $LFS/tools, sudo chown -v lfs $LFS/sources  
以lfs登录su - lfs

### 设置环境 
{% highlight bash %}
su - lfs
# ~/.bash_profile
exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
# ~/.bashrc
set +h
umask 022
LFS=/mnt/lfs
LC_ALL=POSIX
LFS_TGT=$(uname -m)-lfs-linux-gnu
PATH=/tools/bin:/bin:/usr/bin
export LFS LC_ALL LFS_TGT PATH


source ~/.bash_profile
{% endhighlight %}

### 构建临时系统
这个临时系统的用处是把构建最终系统的构建工具都放到这个临时系统中，这样最终系统的构建其实是不依赖宿主系统的。  
构建分为两部分：
- 构建一个新的，宿主无关的工具链（编译器、汇编器、链接器，库和一些有用的工具）