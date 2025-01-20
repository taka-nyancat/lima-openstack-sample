# lima-openstack

## 概要

Limaは軽量なVM環境をMac上で構築するためのツールです。このガイドは、Limaを使用してDevStackを構築する方法を説明します。初学者がOpenStack CLIを学ぶために作成しました。

## 必要要件
- macOS (Apple Silicon)
- Homebrew
- Lima

## Limaのインストール
```bash
brew install lima
```

## LimaでdevstackのVMを構築

DevStackの構築のための設定を`./lima/devstack.yaml`に記述しています。DevStackの仕様上、SSHに使用するインターフェイスと外部ネットワークを別にする必要があるため、MacVLANでインターフェイスを分離しています。Ubuntuは22.04を使用しています。これはOpenStack Yogaが22.04までをサポートしているためです。

### 1. LimaでVMを起動 (MacOSで実行)
```bash
limactl create ./lima/devstack.yaml --tty=false # VM起動
limactl shell devstack                          # コンソール接続
```

### 2. Stackユーザの作成 (VMで実行)
```bash
sudo useradd -s /bin/bash -d /opt/stack -m stack
sudo chmod +x /opt/stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
```

### 3. DevStackのダウンロード (VMで実行)
```bash
git clone https://opendev.org/openstack/devstack
cd devstack
```

### 4. local.confの設定 (VMで実行)

以下の場所に、このリポジトリの`./devstack/local.conf`を参考に`local.conf`を作成してください。

Path:
```bash
stack@lima-devstack:~/devstack$ pwd
/opt/stack/devstack
```

コピペやSCPを使用して`local.conf`をアップロードします。

### 5. DevStackの起動 (VMで実行)
```bash
./stack.sh
```

### 6. OpenStack ClientやHorizonから試す

Horizonを使用する際は、SSHやVSCodeの拡張機能を使用して、80番ポートをポートフォワードしてください。

