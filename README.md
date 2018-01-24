# mqtt-sn-contiki-dtls

## セットアップ

- OS: Ubuntu 16.04 x64
- ホームディレクトリ(「~」)での作業を前提

### Contiki開発環境のセットアップ

'''
$ sudo apt-get install build-essential binutils-msp430 gcc-msp430 msp430-libc binutils-avr gcc-avr gdb-avr avr-libc avrdude binutils-arm-none-eabi gcc-arm-none-eabi gdb-arm-none-eabi openjdk-8-jdk openjdk-8-jre ant libncurses5-dev doxygen srecord git autoconf
'''

### gitリポジトリのクローン

'''
$ git clone --recursive https://github.com/sk2-broken/mqtt-sn-contiki-dtls
'''

### mspgcc-4.7.3をPATHに追加

'''
$ export PATH=~/mqtt-sn-contiki-dtls/mspgcc-4.7.3/bin:$PATH
'''

### border-routerをビルド

'''
$ cd ~/mqtt-sn-contiki-dtls/contiki/examples/ipv6/rpl-border-router
$ make TARGET=z1
$ make TARGET=wismote
'''

### tunslip6をビルド

'''
$ cd ~/mqtt-sn-contiki-dtls/contiki/tools
$ make tunslip6
'''

### mspsimをビルド

'''
$ cd ~/mqtt-sn-contiki-dtls/contiki/tools/mspsim
$ make
'''

### mqtt_snをビルド

'''
$ cd ~/mqtt-sn-contiki-dtls/contiki/mqtt_sn
$ make all
'''

### tinydtls(master版)をビルド

'''
$ cd ~/mqtt-sn-contiki-dtls/org.eclipse.tinydtls
$ autoreconf
$ touch install-sh
$ ./configure --without-ecc
$ make
$ cp libtinydtls.a ../DTLS_RSMB/
'''

### DTLS_RSMBをビルド

'''
$ cd ~/mqtt-sn-contiki-dtls/DTLS_RSMB
$ rm *.o
$ rm broker_mqtts
$ make broker_mqtts
'''


