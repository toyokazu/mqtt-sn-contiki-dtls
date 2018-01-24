# mqtt-sn-contiki-dtls

## 1. セットアップ

- OS: Ubuntu 16.04 x64
- ホームディレクトリ(「~」)での作業を前提

### Contiki開発環境のセットアップ

```
$ sudo apt-get install build-essential binutils-msp430 gcc-msp430 msp430-libc binutils-avr gcc-avr gdb-avr avr-libc avrdude binutils-arm-none-eabi gcc-arm-none-eabi gdb-arm-none-eabi openjdk-8-jdk openjdk-8-jre ant libncurses5-dev doxygen srecord git autoconf
```

### gitリポジトリのクローン

```
$ git clone --recursive https://github.com/sk2-broken/mqtt-sn-contiki-dtls
```

### mspgcc-4.7.3をPATHに追加

```
$ export PATH=~/mqtt-sn-contiki-dtls/mspgcc-4.7.3/bin:$PATH
```

### border-routerをビルド

```
$ cd ~/mqtt-sn-contiki-dtls/contiki/examples/ipv6/rpl-border-router
$ make TARGET=z1
$ make TARGET=wismote
```

### tunslip6をビルド

```
$ cd ~/mqtt-sn-contiki-dtls/contiki/tools
$ make tunslip6
```

### mspsimをビルド

```
$ cd ~/mqtt-sn-contiki-dtls/contiki/tools/mspsim
$ make
```

### tinydtls(master版)をビルド

```
$ cd ~/mqtt-sn-contiki-dtls/org.eclipse.tinydtls
$ autoreconf
$ touch install-sh
$ ./configure --without-ecc
$ make
$ cp libtinydtls.a ../DTLS_RSMB/
```

### DTLS_RSMBをビルド

```
$ cd ~/mqtt-sn-contiki-dtls/DTLS_RSMB
$ rm *.o
$ rm broker_mqtts
$ make broker_mqtts
```

### mqtt_snをビルド

```
$ cd ~/mqtt-sn-contiki-dtls/contiki/mqtt_sn
$ make TARGET=z1
```

### tinydtls(0.8.2版)のサンプルプログラムをビルド

```
$ cd ~/mqtt-sn-contiki-dtls/contiki/apps/tinydtls
$ ./configure --with-contiki --without-ecc
$ cd examples/contiki
$ make TARGET=wismote
```

## 2. Contikiの実行

### mqtt_sn(非DTLS版)を実行する場合

#### Coojaを起動

```
$ cd ~/mqtt-sn-contiki-dtls/contiki/tools/cooja 
$ ant run
```

- ~/mqtt-sn-contiki-dtls/contiki/mqtt_sn/simulation_mqtt_sn.csc を開く

#### tunslip6を起動 (別コンソール)

```
$ cd ~/mqtt-sn-contiki-dtls/contiki/tools
$ sudo ./tunslip6 -a 127.0.0.1 aaaa::1/64
```

#### DTLS_RSMBを起動 (別コンソール)

```
$ cd ~/mqtt-sn-contiki-dtls/DTLS_RSMB
$ sudo ./broker_mqtts mqtts.conf
```

#### シミュレータを実行

- 「start」ボタンを押す
- tun0インターフェイスをwiresharkでキャプチャすればMQTT-SN通信を参照可能

### tinydtls(0.8.2版)のサンプルプログラムを実行する場合

mqtt_sn(非DTLS版)の実行手順と同様だが、シミュレーションファイルとして以下を開く

- ~/mqtt-sn-contiki-dtls/contiki/apps/tinydtls/examples/contiki/tinydtls-client-example.csc を開く

## メモ

- DTLS_RSMBのMQTT-SN over DTLSサーバに接続するが、クライアントはMQTT-SNではないため、ハンドシェイクに成功した後、止まる
- 今のところ、2-3回に1回ぐらいは、サーバからのパケットが届かず？、失敗することがある
- DTLS_RSMBはクライアントの状態を一定時間覚えてしまうので、動作がおかしくなれば再起動すること

