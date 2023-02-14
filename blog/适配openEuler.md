缺少python环境，
yum install zlib
yum install zlib-devel
yum install sqlite
yum install sqlite-devel
yum install ncurses
yum install ncurses-devel
yum install openssl
yum install openssl-devel

yum install readline-devel
yum install gdbm-devel

yum install bzip2-devel


virtualbox
libGL
libICE
~libSDL
libSM
libXmu
libXt
~libasound
~libcrypto
~libopus


yara：
g++
pcre-devel

mkdir yara_compile_env
mkdir qt5-qtx11extras

yum install -y g++ pcre pcre-devel tar --downloadonly --allowerasing --downloaddir=./yara_compile_env

yum install -y qt5-qtx11extras.x86_64 --downloadonly --allowerasing --downloaddir=./qt5-qtx11extras