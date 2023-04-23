# armv5tej-OpenVPN
Statically linked OpenVPN binary for armv5tej

# Building

Run below commands in Colab:

```
%%writefile compile.sh

apt-get install build-essential gcc-arm-linux-gnueabi ca-certificates

wget https://www.openssl.org/source/openssl-1.0.2j.tar.gz
tar -xvf openssl-1.0.2j.tar.gz
cd openssl-1.0.2j
./Configure gcc -static -no-shared --prefix=/home/user/vpn_compile --cross-compile-prefix=arm-linux-gnueabi-
make
make install

cd /content

wget http://www.oberhumer.com/opensource/lzo/download/lzo-2.09.tar.gz
tar -xvf lzo-2.09.tar.gz
cd lzo-2.09
./configure --prefix=/home/user/vpn_compile --enable-static --target=arm-linux-gnueabi --host=arm-linux-gnueabi --disable-debug
make
make install

cd /content

wget https://swupdate.openvpn.org/community/releases/openvpn-2.3.12.tar.gz
tar -xvf openvpn-2.3.12.tar.gz
cd openvpn-2.3.12
./configure --target=arm-linux-gnueabi --host=arm-linux-gnueabi --prefix=/home/user/vpn_compile --disable-server --enable-static --disable-shared --disable-debug --disable-plugins OPENSSL_SSL_LIBS="-L/home/user/vpn_compile/lib -lssl" OPENSSL_SSL_CFLAGS="-I/home/user/vpn_compile/include" OPENSSL_CRYPTO_LIBS="-L/home/user/vpn_compile/lib -lcrypto" OPENSSL_CRYPTO_CFLAGS="-I/home/user/vpn_compile/include" LZO_CFLAGS="-I/home/user/vpn_compile/include" LZO_LIBS="-L/home/user/vpn_compile/lib -llzo2" IFCONFIG=/sbin/ifconfig ROUTE=/sbin/route NETSTAT=/bin/netstat IPROUTE=/sbin/ip --enable-iproute2
make LIBS="-all-static"
make install
```

Then run:

```
!chmod -c 755 compile.sh
!./compile.sh
```

To checking binary file run:

```
!file /home/user/vpn_compile/sbin/openvpn
```

Check releases section also.
