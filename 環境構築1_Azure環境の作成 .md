## ネットワーク環境の作成

## リソースグループの作成<br>
Azure Portalから「リソース グループ」と検索し、「作成」を押下する<br>
![alt text](/img/image.png)<br><br>

サブスクリプション名、リソースグループ名、作成するリージョンを入力して、作成する<br>
![alt text](img/image-1.png)

<br>

## 仮想ネットワークの作製
Azure Portalから、「仮想ネットワーク」を検索し、作成を押下する
![alt text](img/image-2.png)

先ほど作成したリソースグループを指定し、任意の仮想ネットワーク名と、リソースグループと同じリージョンを選択する<br>
![alt text](/img/image-3.png)<br><br>

IPアドレスタブにて、作成する仮想ネットワークのIP範囲を設定する<br>
![alt text](./img/image-4.png)<br><br>

必要であれば、「サブネットの追加」より、defaultサブネットのIPアドレス範囲を/27にする<br>
![alt text](./img/image-5.png)<br><br>

Virtual Network Gatewayサブネットの追加<br>
サブネットの追加→サブネットの種類「Virtual Network Gateway」を選択し、追加する<br>
※IPアドレス範囲は/27を指定する<br>
![alt text](/img/image-7.png)<br><br>

レビューと作成タブにて、内容に問題なければ「作成」を押下する<br>
![alt text](/img/image-8.png)<br><br>

<br>

## 仮想マシンの作製

下記の方法で２台作成する

Azure Portalで「仮想マシン」を検索し、「作成」を押下する
<img width="960" height="516" alt="image" src="https://github.com/user-attachments/assets/d1d7ce42-8310-48c8-b181-f128d66a7a0b" />

基本タブにて、次の通りに入力する
| 項目 | 入力方法 |
| -- | -- |
| サブスクリプション | Spoke環境のリソースグループがあるサブスクリプション |
| リソース グループ | Spokeの仮想ネットワークがあるリソースグループ |
| 仮想マシン名 | 任意の仮想マシン名 |
| リージョン | (Asia Pacific) Japan East |
| 可用性オプション | インフラストラクチャ冗長は必要ありません |
| セキュリティの種類 | Standard |
| イメージ | Windows Server 2025 Datacenter: Azure Edition - x64 Gen2 |
| サイズ | デフォルト |
| ユーザー名 | 仮想マシンの管理者アカウント名 |
| パスワード | 管理者アカウントのパスワード |
| パブリック受信ポート | なし |

![alt text](/img/image-9.png)<br><br>


ネットワークタブにて、下記の通り入力する
| 項目 | 入力方法 |
| -- | -- |
| 仮想ネットワーク | 作成した仮想ネットワーク |
| サブネット | default |
| パブリック IP | なし |
| NIC ネットワーク セキュリティ グループ | Basic |
| パブリック受信ポート | なし |

![alt text](/img/image-10.png)<br><br>


確認及び作成で、作成を押下する<br>
![alt text](/img/image-11.png)<br><br>

仮想マシンにBastionを使用してログイン
作製した仮想マシンをAzure Portalで開き、接続→Bastonを押下する
<img width="960" height="516" alt="image" src="https://github.com/user-attachments/assets/1281a975-e2dd-4f0c-8a85-d08db0c23839" />

作成時に入力したユーザ名とパスワードを入力して、接続する
<img width="960" height="516" alt="image" src="https://github.com/user-attachments/assets/e0640044-79cc-4331-806d-c34023b17f5c" />

自動的にBastionがデプロイされ、接続される<br>
<img width="960" height="516" alt="image" src="https://github.com/user-attachments/assets/b57e2bf7-ae31-495b-9558-2b56dd1976a2" /><br>

<br>


***

### ExpressRouteGatewayのデプロイ
Azure Portalで、「Virtual network gateways」と検索し、ExpressRoute gatewaysであることを確認して作成を押下する<br>
![alt text](/img/image-12.png)<br>

<br>

基本タブにて、次のように入力する

| 項目 | 入力方法 |
|:---------|:---------|
| サブスクリプション |作成したリソースグループが存在するサブスクリプション |
| リソース グループ | 作成したネットワークが存在するリソースグループ |
| インスタンスの名前 | ExpressRouteGatewayの名前 |
| リージョン | 仮想ネットワークと同じリージョン |
| ゲートウェイの種類 | ExpressRoute |
| SKU | Standard |
| 仮想ネットワーク | 作成した仮想ネットワーク |
<br>

![alt text](/img/image-13.png)<br>

<br>

確認および作成で作成する<br>
![alt text](/img/image-14.png)