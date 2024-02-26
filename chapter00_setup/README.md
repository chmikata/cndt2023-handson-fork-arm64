# 事前準備

**12/8に実施されているハンズオンに参加される方はこのステップは不要です。**

## VM の作成

下記の手順に従って、VMを作成してください。
MacはMultiPass、WindowsはWSL2を利用している想定で記載しています。
RancherDesktopやOrbStackなどを利用していも問題ありませんが、OSのバージョンは揃えてください。

### Macの場合
```
# MultiPassのインストール
$ brew install --cask multipass

# VMの作成
$ multipass launch --name <VM名> --cpus 4 --mem 8G --disk 32G --timeout 3600 22.04

# VMの起動確認
$ multipass shell <VM名>

# VMのIPアドレスの確認
$ multipass ls
```

### WSL2の場合
```
# WSL2のアップデート
> wsl --update  --web-download

# VMの作成
> wsl --install --web-download -d Ubuntu-22.04

# VMの起動確認
> wsl -s Ubuntu-22.04
> ws -l
```

出力されるIPアドレスは「名前解決の設定」の手順で`YOUR_VM_IP_ADDRESS`を置き換えて利用してください。
WSL2の場合は、IPアドレスは`127.0.0.1`です。

## VM のセットアップ
演習で利用するVMに最低限のパッケージをインストールします。

```
sudo apt-get update
sudo apt-get install -y curl vim git unzip gnupg lsb-release ca-certificates dstat
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

## 名前解決の設定

今回の演習では、ローカル端末のhostsファイルを利用して名前解決を行います。

> **Info**  
> 利用しているOSに応じてhostsファイルに設定を書き込んでください。
> - Windows：`C:\Windows\System32\drivers\etc\hosts`
> - Linux, Mac: `/etc/hosts`

hostsファイルには、この演習を通して利用するIPアドレスとドメインの紐付けを設定してください。YOUR_VM_IP_ADDRESSはこの演習で利用するマシンのIPアドレスを指定してください。

```
YOUR_VM_IP_ADDRESS    app.example.com
YOUR_VM_IP_ADDRESS    prometheus.example.com
YOUR_VM_IP_ADDRESS    grafana.example.com
YOUR_VM_IP_ADDRESS    jaeger.example.com
YOUR_VM_IP_ADDRESS    argocd.example.com
YOUR_VM_IP_ADDRESS    app.argocd.example.com
YOUR_VM_IP_ADDRESS    dev.kustomize.argocd.example.com
YOUR_VM_IP_ADDRESS    prd.kustomize.argocd.example.com
YOUR_VM_IP_ADDRESS    helm.argocd.example.com
YOUR_VM_IP_ADDRESS    app-preview.argocd.example.com
YOUR_VM_IP_ADDRESS    kiali.example.com
YOUR_VM_IP_ADDRESS    kiali-ambient.example.com
YOUR_VM_IP_ADDRESS    app.cilium.example.com
YOUR_VM_IP_ADDRESS    hubble.cilium.example.com
```

## リポジトリのClone

マニフェストなどをVMにも配置するために、VM上でリポジトリをCloneしておいてください。
なお、事前に```https://github.com/chmikata/cndt2023-handson-fork/```をForkしておき、それをCloneしてください。

```shell
git clone https://github.com/<YOUR_GITHUB_ID>/cndt2023-handson-fork.git
```
