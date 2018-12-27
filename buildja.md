---
layout: contents
language: ja
title: Build
short_desc: Consistent Hashing Mq inProcess data eXchange
lang_opp_file: build.html
lang_opp_word: To English
prev_url: usageja.html
prev_string: Usage
top_url: indexja.html
top_string: TOP
next_url: developerja.html
next_string: Developer
---
# ビルド

この章は3つの部分から構成されています。

* ローカル開発用に**CHMPX**を設定する方法
* ソースコードから**CHMPX**を構築する方法
* **CHMPX**のインストール方法

## 1. 前提条件をインストールする

**CHMPX**は主に、fullock、k2hashに依存します。それぞれの依存ライブラリとヘッダファイルは**CHMPX**をビルドするために必要です。それぞれの依存ライブラリとヘッダファイルをインストールする方法は2つあります。好きなものを選ぶことができます。

* GitHubを使う  
  依存ライブラリのソースコードとヘッダファイルをインストールします。あなたはそれぞれの依存ライブラリとヘッダファイルをビルドしてインストールします。
* packagecloud.ioを使用する  
  依存ライブラリのパッケージとヘッダファイルをインストールします。あなたはそれぞれの依存ライブラリとヘッダファイルをインストールするだけです。ライブラリはすでに構築されています。

### 1.1. GitHubから各依存ライブラリとヘッダファイルをインストールします。

詳細については以下の文書を読んでください。  
* [fullock](https://fullock.antpick.ax/build.html)
* [k2hash](https://k2hash.antpick.ax/build.html)  
SSL/TLSプロトコルを実装するアプリケーションは、1つのSSL/TLSライブラリを使用します。[K2HASH](https://k2hash.antpick.ax/build.html)をビルドするときには、必然的に同じSSL/TLSライブラリーを選択するでしょう。[K2HASH](https://k2hash.antpick.ax/build.html)がサポートしているSSL/TLSライブラリは、[K2HASH](https://k2hash.antpick.ax/build.html)を参照してください。

**注**: 後段で説明していますが、**CHMPX**も1つのSSL/TLSライブラリが必要であることを考慮する必要があります。これは、[K2HASH](https://k2hash.antpick.ax/build.html)ビルドオプションが**CHMPX**ビルドオプションに影響することを意味します。 表1は、可能な構成オプションを示しています。

表1. 可能な構成オプション:

| K2HASH configure options | CHMPX configure options | SSL/TLS library |
|:--|:--|:--|
| ./configure --prefix=/usr --with-openssl | ./configure --prefix=/usr --with-openssl | [OpenSSL](https://www.openssl.org/) |
| ./configure --prefix=/usr --with-gcrypt | ./configure --prefix=/usr --with-gnutls | [GnuTLS](https://gnutls.org/) |
| ./configure --prefix=/usr --with-nettle | ./configure --prefix=/usr --with-gnutls | [GnuTLS](https://gnutls.org/) |
| ./configure --prefix=/usr --with-nss | ./configure --prefix=/usr --with-nss | [Mozilla NSS](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS) |

### 1.2. packagecloud.ioから各依存ライブラリとヘッダファイルをインストールします。

このセクションでは、packagecloud.ioから各依存ライブラリとヘッダーファイルをインストールする方法を説明します。

**注**：前のセクションで各依存ライブラリとGitHubからのヘッダーファイルをインストールした場合は、このセクションを読み飛ばしてください。

**注**：前のセクションで説明したように、[K2HASH](https://k2hash.antpick.ax/build.html)ビルドオプションは**CHMPX**ビルドオプションに影響します。 前のセクションの表1を参照してください。

DebianStretchまたはUbuntu（Bionic Beaver）をお使いの場合は、以下の手順に従ってください。
```bash
$ sudo apt-get update -y
$ sudo apt-get install curl -y
$ curl -s https://packagecloud.io/install/repositories/antpickax/stable/script.deb.sh \
    | sudo bash
$ sudo apt-get install autoconf autotools-dev gcc g++ make gdb libtool pkg-config \
    libyaml-dev libfullock-dev k2hash-dev chmpx-dev gnutls-dev -y
$ sudo apt-get install git -y
```

Fedora28またはCentOS7.x（6.x）ユーザーの場合は、以下の手順に従ってください。
```bash
$ sudo yum makecache
$ sudo yum install curl -y
$ curl -s https://packagecloud.io/install/repositories/antpickax/stable/script.rpm.sh \
    | sudo bash
$ sudo yum install autoconf automake gcc gcc-c++ gdb make libtool pkgconfig \
    libyaml-devel libfullock-devel k2hash-devel chmpx-devel nss-devel -y
$ sudo yum install git -y
```

## 2. GitHubからソースコードを複製する

GitHubから**mdtor**のソースコードをダウンロードしてください。
```bash
$ git clone https://github.com/yahoojapan/chmpx.git
```

## 3. ビルドしてインストールする

以下の手順に従って**CHMPX**をビルドしてインストールしてください。 [GNU Automake](https://www.gnu.org/software/automake/)を使って**CHMPX**を構築します。

DebianStretchまたはUbuntu（Bionic Beaver）をお使いの場合は、以下の手順に従ってください。
```bash
$ cd chmpx
$ sh autogen.sh
$ ./configure --prefix=/usr --with-gnutls
$ make
$ sudo make install
```

Fedora28またはCentOS7.x（6.x）ユーザーの場合は、以下の手順に従ってください。
```bash
$ cd chmpx
$ sh autogen.sh
$ ./configure --prefix=/usr --with-nss
$ make
$ sudo make install
```

**CHMPX**を正常にインストールすると、**CHMPX**のヘルプテキストが表示されます。
```bash
$ chmpx -h
[Usage]
chmpx [-conf <file> | -json <json>] [-ctlport <port>] [-d [slient|err|wan|msg|dump]] [-dfile <debug file path>]
chmpx [ -h | -v ]

[option]
  -conf <path>         specify the configration file(.ini .yaml .json) path
  -json <json string>  specify the configration json string
  -ctlport <port>      specify the self contrl port(*)
  -d <param>           specify the debugging output mode:
                        silent - no output
                        err    - output error level
                        wan    - output warning level
                        msg    - output debug(message) level
                        dump   - output communication debug level
  -dfile <path>        specify the file path which is put debug output
  -h(help)             display this usage.
  -v(version)          display version.

[environments]
  CHMDBGMODE           debugging mode like "-d" option.
  CHMDBGFILE           the file path for debugging output like "-dfile" option.
  CHMCONFFILE          configuration file path like "-conf" option
  CHMJSONCONF          configuration json string like "-json" option

(*) if ctlport option is specified, chmpx searches same ctlport in configuration
    file and ignores "CTLPORT" directive in "GLOBAL" section. and chmpx will
    start in the mode indicated by the server entry that has beed detected.
```