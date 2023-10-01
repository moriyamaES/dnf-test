# nodejsのテスト（rpmとdnf コマンドのテストを含む）

## 参考

- [nodejsのアーカイブダウンロードサイト【公式サイト】](https://nodejs.org/ja/download)

- [アーカイブでのnodejsのインストール手順【野良ブログ】](https://qiita.com/ymaru/items/a5e6e8559d2503a469c3)


- [19.9.0(SBと同じバージョン)のアーカイブ](https://nodejs.org/dist/v19.9.0/)

- [dnfコマンドでのnodejsのインストール手順【公式サイト】](https://github.com/nodesource/distributions/blob/master/README.md#enterprise-linux-based-distributions)

## nodejs をアーカイブでインストール

### インストール

- 19.9.0 のアーカイブをダウンロードし、解凍

    ```sh
    cd ~
    ```

    ```sh
    curl -O https://nodejs.org/dist/v19.9.0/node-v19.9.0-linux-x64.tar.gz
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    100 42.6M  100 42.6M    0     0  4900k      0  0:00:08  0:00:08 --:--:-- 5224k
    ```

    ```sh
    ll node-v19.9.0-linux-x64.tar.gz 
    -rw-rw-r-- 1 kazuhiro kazuhiro 44760474 10月  1 11:56 node-v19.9.0-linux-x64.tar.gz
    ```

    ```sh
    tar zxvf node-v19.9.0-linux-x64.tar.gz
    ```


    ```sh
    [kazuhiro@localhost ~]
    $ ll 
    合計 1245632
    -rw-r--r--  1 kazuhiro kazuhiro 1230762496  9月 27 12:44 nextcloud-container-image.tar
    drwxr-xr-x  6 kazuhiro kazuhiro        108  4月 11 04:36 node-v19.9.0-linux-x64
    -rw-rw-r--  1 kazuhiro kazuhiro   44760474 10月  1 11:56 node-v19.9.0-linux-x64.tar.gz
    drwxrwxr-x  3 kazuhiro kazuhiro         35 10月  1 10:37 nodejs-test-with-rpm-dnf-test
    drwxrwxr-x. 3 kazuhiro kazuhiro         35  9月 26 12:41 podman-test
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 ダウンロード
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 テンプレート
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 デスクトップ
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 ドキュメント
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 ビデオ
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 音楽
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 画像
    drwxr-xr-x. 2 kazuhiro kazuhiro          6  9月 25 23:05 公開
    [kazuhiro@localhost ~]
    $ cd node-v19.9.0-linux-x64/
    [kazuhiro@localhost ~/node-v19.9.0-linux-x64]
    $ ll
    合計 420
    -rw-r--r-- 1 kazuhiro kazuhiro 271788  4月 11 04:36 CHANGELOG.md
    -rw-r--r-- 1 kazuhiro kazuhiro 114948  4月 11 04:36 LICENSE
    -rw-r--r-- 1 kazuhiro kazuhiro  36430  4月 11 04:36 README.md
    drwxr-xr-x 2 kazuhiro kazuhiro     56  4月 11 04:36 bin
    drwxr-xr-x 3 kazuhiro kazuhiro     18  4月 11 04:36 include
    drwxr-xr-x 3 kazuhiro kazuhiro     26  4月 11 04:36 lib
    drwxr-xr-x 4 kazuhiro kazuhiro     28  4月 11 04:36 share
    [kazuhiro@localhost ~/node-v19.9.0-linux-x64]
    $ ll ./bin/
    合計 91112
    lrwxrwxrwx 1 kazuhiro kazuhiro       45  4月 11 04:36 corepack -> ../lib/node_modules/corepack/dist/corepack.js
    -rwxr-xr-x 1 kazuhiro kazuhiro 93298360  4月 11 04:36 node
    lrwxrwxrwx 1 kazuhiro kazuhiro       38  4月 11 04:36 npm -> ../lib/node_modules/npm/bin/npm-cli.js
    lrwxrwxrwx 1 kazuhiro kazuhiro       38  4月 11 04:36 npx -> ../lib/node_modules/npm/bin/npx-cli.js
    ```

- 実行ファイル `node` を PATH が通っているフォルダ `/usr/bin` にコピー

    ```sh
    sudo cp ~/node-v19.9.0-linux-x64/bin/node /usr/bin
    ```

    ```sh
    ll /usr/bin/node
    -rwxr-xr-x 1 root root 93298360 10月  1 12:11 /usr/bin/node
    ```

    - ※`sudo`で実行したので、オーナーが`root`になる

- バージョン表示し、インストール成功を確認する

    ```sh
    node -v
    v19.9.0
    ```
- mpn はインストールされない

    ```sh
    npm -v
    bash: npm: コマンドが見つかりませんでした...
    コマンド npm' を提供するためにパッケージ 'npm' をインストールしますか? [N/y] N
    ```

##＃ アーカイブでインストールした nodejs を案イントールする

    ```sh
    sudo rm -f /usr/bin/node
    ```

    ```sh
    ll /usr/bin/node
    ls: '/usr/bin/node' にアクセスできません: そのようなファイルやディレクトリはありません
    ```
    
    ```sh
    node -v
    bash: /usr/bin/node: そのようなファイルやディレクトリはありません
    ```

## dnf コマンドで`nodejs` をインストール

### インストール

- install build tools

    ```sh
    sudo yum install gcc-c++ make
    ```

    - 結果

        ```sh
        依存関係が解決しました。
        =====================================================================================================================================================================================================================
        パッケージ                                             アーキテクチャー                              バージョン                                              リポジトリー                                     サイズ
        =====================================================================================================================================================================================================================
        インストール:
        gcc-c++                                                x86_64                                        8.5.0-20.el8                                            appstream                                         12 M
        make                                                   x86_64                                        1:4.2.1-11.el8                                          baseos                                           498 k
        依存関係のインストール:
        cpp                                                    x86_64                                        8.5.0-20.el8                                            baseos                                            10 M
        gcc                                                    x86_64                                        8.5.0-20.el8                                            baseos                                            24 M
        glibc-devel                                            x86_64                                        2.28-236.el8.1                                          baseos                                            84 k
        glibc-headers                                          x86_64                                        2.28-236.el8.1                                          baseos                                           489 k
        isl                                                    x86_64                                        0.16.1-6.el8                                            appstream                                        841 k
        kernel-headers                                         x86_64                                        4.18.0-514.el8                                          baseos                                            11 M
        libstdc++-devel                                        x86_64                                        8.5.0-20.el8                                            appstream                                        2.2 M
        libxcrypt-devel                                        x86_64                                        4.1.1-6.el8                                             baseos                                            25 k

        トランザクションの概要
        =====================================================================================================================================================================================================================
        インストール  10 パッケージ

        ダウンロードサイズの合計: 62 M
        インストール後のサイズ: 141 M
        これでよろしいですか? [y/N]: y
        パッケージのダウンロード:
        (1/10): isl-0.16.1-6.el8.x86_64.rpm                                                                                                                                                  1.3 MB/s | 841 kB     00:00    
        (2/10): libstdc++-devel-8.5.0-20.el8.x86_64.rpm                                                                                                                                      1.6 MB/s | 2.2 MB     00:01    
        (3/10): gcc-c++-8.5.0-20.el8.x86_64.rpm                                                                                                                                              1.7 MB/s |  12 MB     00:07    
        (4/10): glibc-devel-2.28-236.el8.1.x86_64.rpm                                                                                                                                        763 kB/s |  84 kB     00:00    
        (5/10): cpp-8.5.0-20.el8.x86_64.rpm                                                                                                                                                  1.5 MB/s |  10 MB     00:07    
        (6/10): glibc-headers-2.28-236.el8.1.x86_64.rpm                                                                                                                                      1.3 MB/s | 489 kB     00:00    
        (7/10): libxcrypt-devel-4.1.1-6.el8.x86_64.rpm                                                                                                                                       339 kB/s |  25 kB     00:00    
        (8/10): make-4.2.1-11.el8.x86_64.rpm                                                                                                                                                 1.1 MB/s | 498 kB     00:00    
        (9/10): gcc-8.5.0-20.el8.x86_64.rpm                                                                                                                                                  2.1 MB/s |  24 MB     00:11    
        (10/10): kernel-headers-4.18.0-514.el8.x86_64.rpm                                                                                                                                    2.1 MB/s |  11 MB     00:05    
        ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
        合計                                                                                                                                                                                 4.2 MB/s |  62 MB     00:14     
        トランザクションの確認を実行中
        トランザクションの確認に成功しました。
        トランザクションのテストを実行中
        トランザクションのテストに成功しました。
        トランザクションを実行中
        準備             :                                                                                                                                                                                             1/1 
        インストール中   : kernel-headers-4.18.0-514.el8.x86_64                                                                                                                                                       1/10 
        scriptletの実行中: glibc-headers-2.28-236.el8.1.x86_64                                                                                                                                                        2/10 
        インストール中   : glibc-headers-2.28-236.el8.1.x86_64                                                                                                                                                        2/10 
        インストール中   : libxcrypt-devel-4.1.1-6.el8.x86_64                                                                                                                                                         3/10 
        インストール中   : glibc-devel-2.28-236.el8.1.x86_64                                                                                                                                                          4/10 
        scriptletの実行中: glibc-devel-2.28-236.el8.1.x86_64                                                                                                                                                          4/10 
        インストール中   : cpp-8.5.0-20.el8.x86_64                                                                                                                                                                    5/10 
        scriptletの実行中: cpp-8.5.0-20.el8.x86_64                                                                                                                                                                    5/10 
        インストール中   : libstdc++-devel-8.5.0-20.el8.x86_64                                                                                                                                                        6/10 
        インストール中   : isl-0.16.1-6.el8.x86_64                                                                                                                                                                    7/10 
        scriptletの実行中: isl-0.16.1-6.el8.x86_64                                                                                                                                                                    7/10 
        インストール中   : gcc-8.5.0-20.el8.x86_64                                                                                                                                                                    8/10 
        scriptletの実行中: gcc-8.5.0-20.el8.x86_64                                                                                                                                                                    8/10 
        インストール中   : gcc-c++-8.5.0-20.el8.x86_64                                                                                                                                                                9/10 
        インストール中   : make-1:4.2.1-11.el8.x86_64                                                                                                                                                                10/10 
        scriptletの実行中: make-1:4.2.1-11.el8.x86_64                                                                                                                                                                10/10 
        検証             : gcc-c++-8.5.0-20.el8.x86_64                                                                                                                                                                1/10 
        検証             : isl-0.16.1-6.el8.x86_64                                                                                                                                                                    2/10 
        検証             : libstdc++-devel-8.5.0-20.el8.x86_64                                                                                                                                                        3/10 
        検証             : cpp-8.5.0-20.el8.x86_64                                                                                                                                                                    4/10 
        検証             : gcc-8.5.0-20.el8.x86_64                                                                                                                                                                    5/10 
        検証             : glibc-devel-2.28-236.el8.1.x86_64                                                                                                                                                          6/10 
        検証             : glibc-headers-2.28-236.el8.1.x86_64                                                                                                                                                        7/10 
        検証             : kernel-headers-4.18.0-514.el8.x86_64                                                                                                                                                       8/10 
        検証             : libxcrypt-devel-4.1.1-6.el8.x86_64                                                                                                                                                         9/10 
        検証             : make-1:4.2.1-11.el8.x86_64                                                                                                                                                                10/10 

        インストール済み:
        cpp-8.5.0-20.el8.x86_64               gcc-8.5.0-20.el8.x86_64              gcc-c++-8.5.0-20.el8.x86_64         glibc-devel-2.28-236.el8.1.x86_64  glibc-headers-2.28-236.el8.1.x86_64  isl-0.16.1-6.el8.x86_64 
        kernel-headers-4.18.0-514.el8.x86_64  libstdc++-devel-8.5.0-20.el8.x86_64  libxcrypt-devel-4.1.1-6.el8.x86_64  make-1:4.2.1-11.el8.x86_64        

        完了しました!
        ```


    ```sh
    ll /etc/yum.repos.d/nodesource*.repo
    ls: '/etc/yum.repos.d/nodesource*.repo' にアクセスできません: そのようなファイルやディレクトリはありません
    ```

    ```sh
    sudo yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm
    ```

    - 結果

        ```sh
        nodesource-release-nodistro-1.noarch.rpm                                                                                               34 kB/s |  10 kB     00:00    
        依存関係が解決しました。
        ======================================================================================================================================================================
        パッケージ                                    アーキテクチャー                  バージョン                             リポジトリー                            サイズ
        ======================================================================================================================================================================
        インストール:
        nodesource-release                            noarch                            nodistro-1                             @commandline                             10 k

        トランザクションの概要
        ======================================================================================================================================================================
        インストール  1 パッケージ

        合計サイズ: 10 k
        インストール後のサイズ: 4.8 k
        これでよろしいですか? [y/N]: y
        パッケージのダウンロード:
        トランザクションの確認を実行中
        トランザクションの確認に成功しました。
        トランザクションのテストを実行中
        トランザクションのテストに成功しました。
        トランザクションを実行中
        準備             :                                                                                                                                              1/1 
        インストール中   : nodesource-release-nodistro-1.noarch                                                                                                         1/1 
        検証             : nodesource-release-nodistro-1.noarch                                                                                                         1/1 

        インストール済み:
        nodesource-release-nodistro-1.noarch                                                                                                                                

        完了しました!
        ```

    ```sh
    ll /etc/yum.repos.d/nodesource*.repo
    ```

    - 結果

        ```sh
        -rw-r--r-- 1 root root 504  8月 17 05:40 /etc/yum.repos.d/nodesource-nodistro.repo
        ```

        - 上記結果から、コマンド`sudo yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm`は、nodejs用のリポジトリをインストールするコマンドであることが分かる


    ```sh
    cat /etc/yum.repos.d/nodesource-nodistro.repo
    [nodesource-nsolid]
    name=Nsolid Packages for Linux RPM based distros - $basearch
    baseurl=https://rpm.nodesource.com/pub_16.x/nodistro/nsolid/$basearch
    priority=8
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/NODESOURCE-NSOLID-GPG-SIGNING-KEY-EL

    [nodesource-nodejs]
    name=Node.js Packages for Linux RPM based distros - $basearch
    baseurl=https://rpm.nodesource.com/pub_16.x/nodistro/nodejs/$basearch
    priority=9
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/NODESOURCE-NSOLID-GPG-SIGNING-KEY-EL
    ```

    - 上記結果からnodejsは、`dnf`コマンドでインストールする場合、インターネットにアクセスする

    ```sh
    sudo yum install nodejs --setopt=nodesource-nodejs.module_hotfixes=1
    ```
    
     - `nodesource-nodejs.module_hotfixes=1`はどういう意味？
	 - ChatGPTの回答：nodesource-nodejs.module_hotfixes=1 は、Node.jsをインストールする際にNodeSourceリポジトリから提供されるYumパッケージの設定の一部です。これは特定のモジュール（Node.jsモジュール）のホットフィックスを有効にするためのオプションです。ホットフィックスは、ソフトウェアの既知の問題やセキュリティの脆弱性を修正するための急ぎの修正を指します。nodesource-nodejs.module_hotfixes=1 を設定することで、NodeSourceリポジトリのNode.jsパッケージがホットフィックスを含むバージョンにアップグレードされる可能性が高まります。これにより、安定性やセキュリティの向上が期待できます。このオプションを指定しない場合、ホットフィックスが無効になり、バージョンの変更が制限されるかもしれません。ただし、ホットフィックスを有効にすることは、新しい問題が導入される可能性もあるため、慎重に選択する必要があります。通常、セキュリティと安定性を保つためにホットフィックスを有効にすることが推奨されます。


    - 結果

        ```sh
        Nsolid Packages for Linux RPM based distros - x86_64                                                                                  288  B/s | 257  B     00:00    
        Node.js Packages for Linux RPM based distros - x86_64                                                                                 1.6 MB/s | 732 kB     00:00    
        依存関係が解決しました。
        ======================================================================================================================================================================
        パッケージ                       アーキテクチャー                 バージョン                                       リポジトリー                                サイズ
        ======================================================================================================================================================================
        インストール:
        nodejs                           x86_64                           2:16.20.2-1nodesource                            nodesource-nodejs                            32 M

        トランザクションの概要
        ======================================================================================================================================================================
        インストール  1 パッケージ

        ダウンロードサイズの合計: 32 M
        インストール後のサイズ: 89 M
        これでよろしいですか? [y/N]: y
        パッケージのダウンロード:
        nodejs-16.20.2-1nodesource.x86_64.rpm                                                                                                 4.2 MB/s |  32 MB     00:07    
        ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
        合計                                                                                                                                  4.2 MB/s |  32 MB     00:07     
        Node.js Packages for Linux RPM based distros - x86_64                                                                                 1.6 MB/s | 1.7 kB     00:00    
        GPG 鍵 0x9B1BE0B4 をインポート中:
        Userid     : "NSolid <nsolid-gpg@nodesource.com>"
        Fingerprint: 6F71 F525 2828 41EE DAF8 51B4 2F59 B5F9 9B1B E0B4
        From       : /etc/pki/rpm-gpg/NODESOURCE-NSOLID-GPG-SIGNING-KEY-EL
        これでよろしいですか? [y/N]: y
        鍵のインポートに成功しました
        トランザクションの確認を実行中
        トランザクションの確認に成功しました。
        トランザクションのテストを実行中
        トランザクションのテストに成功しました。
        トランザクションを実行中
        準備             :                                                                                                                                              1/1 
        scriptletの実行中: nodejs-2:16.20.2-1nodesource.x86_64                                                                                                          1/1 
        インストール中   : nodejs-2:16.20.2-1nodesource.x86_64                                                                                                          1/1 
        scriptletの実行中: nodejs-2:16.20.2-1nodesource.x86_64                                                                                                          1/1 
        検証             : nodejs-2:16.20.2-1nodesource.x86_64                                                                                                          1/1 

        インストール済み:
        nodejs-2:16.20.2-1nodesource.x86_64                                                                                                                                 

        完了しました!
        ```

- インストール先の確認

    ```sh
    which node 
    /usr/bin/node
    ```

    ```sh
    node -v
    v16.20.2
    ```

- npmのインストールの確認

    ```sh
    npm -v
    8.19.4
    ```

    ```sh
    which npm 
    /usr/bin/npm
    ```

    -  上記結果からnodejsは、`dnf`コマンドでインストールした場合、`npm`コマンドもインストールされる。


### `dnf`コマンドでインストールしたnodejsをアンインストールする。

- 以下のコマンドを実行

    ```sh
    sudo yum remove nodejs
    ```

    - 結果

        ```sh
        依存関係が解決しました。
        ======================================================================================================================================================================
        パッケージ                       アーキテクチャー                 バージョン                                      リポジトリー                                 サイズ
        ======================================================================================================================================================================
        削除中:
        nodejs                           x86_64                           2:16.20.2-1nodesource                           @nodesource-nodejs                            89 M

        トランザクションの概要
        ======================================================================================================================================================================
        削除  1 パッケージ

        解放された容量: 89 M
        これでよろしいですか? [y/N]: y
        トランザクションの確認を実行中
        トランザクションの確認に成功しました。
        トランザクションのテストを実行中
        トランザクションのテストに成功しました。
        トランザクションを実行中
        準備             :                                                                                                                                              1/1 
        削除             : nodejs-2:16.20.2-1nodesource.x86_64                                                                                                          1/1 
        scriptletの実行中: nodejs-2:16.20.2-1nodesource.x86_64                                                                                                          1/1 
        検証             : nodejs-2:16.20.2-1nodesource.x86_64                                                                                                          1/1 

        削除しました:
        nodejs-2:16.20.2-1nodesource.x86_64                                                                                                                                 

        完了しました!
        ```

    ```sh
    sudo rm -r /etc/yum.repos.d/nodesource*.repo
    ```

    ```sh
    ll /etc/yum.repos.d/nodesource*.repo
    ls: '/etc/yum.repos.d/nodesource*.repo' にアクセスできません: そのようなファイルやディレクトリはありません
    ```

- yum は /var/cache/yum などにメタデータやパッケージファイルなどのキャッシュを蓄積する。 yum clean all とすると、すべてのキャッシュを削除する。 キャッシュなので、削除しても動作には影響しない。


    ```sh
     ll  /var/cache/yum
    ls: '/var/cache/yum' にアクセスできません: そのようなファイルやディレクトリはありません
    ```

    ```sh
    sudo yum clean all
    39 ファイルが削除されました
    ```
