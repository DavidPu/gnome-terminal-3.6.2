downloaded the code by running below command on Ubuntu 14.04.5 LTS:
apt-get source gnome-terminal

build:
./configure --prefix=$PWD/out --enable-static --enable-silent-rules
