## ネットワーク環境の作成

<details><summary>詳細を見る</summary><div>

## リソースグループの作成<br>
Azure Portalから「リソース グループ」と検索し、「作成」を押下する<br>
![alt text](/img/image.png)<br><br>

サブスクリプション名、リソースグループ名、作成するリージョンを入力して、作成する<br>
![alt text](img/image-1.png)

</div></details>

<br>

## 仮想ネットワークの作製

<details><summary>詳細を見る</summary><div>

Azure Portalから、「仮想ネットワーク」を検索し、作成を押下する
![alt text](img/image-2.png)

先ほど作成したリソースグループを指定し、任意の仮想ネットワーク名と、リソースグループと同じリージョンを選択する<br>
![alt text](/img/image-3.png)<br><br>

IPアドレスタブにて、作成する仮想ネットワークのIP範囲を設定する<br>
![alt text](./img/image-4.png)<br><br>

必要であれば、「サブネットの追加」より、defaultサブネットのIPアドレス範囲を/27にする<br>
![alt text](./img/image-5.png)<br><br>

サブネットの追加<br>
AzureFirewallサブネットの追加<br>
サブネットの追加→サブネットの種類「Azure Firewall」を選択し、追加する<br>
※自動でIPアドレス範囲が設定されます。<br>
![alt text](/img/image-6.png)<br><br>

Virtual Network Gatewayサブネットの追加<br>
サブネットの追加→サブネットの種類「Virtual Network Gateway」を選択し、追加する<br>
※IPアドレス範囲は/27を指定する<br>
![alt text](/img/image-7.png)<br><br>

レビューと作成タブにて、内容に問題なければ「作成」を押下する<br>
![alt text](/img/image-8.png)<br><br>

</div></details>

<br>

## 仮想マシンの作製

<details><summary>詳細を見る</summary><div>

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
<img width="960" height="516" alt="image" src="https://github.com/user-attachments/assets/b57e2bf7-ae31-495b-9558-2b56dd1976a2" /><br><br>


</div></details>

<br>

***

## ExpressRoute (ExpressRoute Circuit) 回線の作成
<details><summary>詳細を見る</summary><div>
Azure Portal で ExpressRoute で検索して、ExpressRoute circuitsを押下<br>

![alt text](/img/image-16.png)<br><br>


作成を押下<br>
![alt text](/img/image-17.png)<br>
<br>

基本タブにて、次のように入力する<br>

| 項目 | 入力方法 |
|:---------|:---------|
| サブスクリプション |作成したリソースグループが存在するサブスクリプション |
| リソース グループ | 作成したネットワークが存在するリソースグループ |
| 回復性 | 標準の回復性 |
| リージョン | 仮想ネットワークと同じリージョン |
| 回線名 | ExpressRoute回線の名前 |
| ポートの種類 | プロバイダー |
| ピアリングの場所 | Tokyo |
| プロバイダー | Oracle Cloud FastConnect |
| 帯域幅 | 50Mbps |
<br>

![alt text](/img/image-15.png)<br><br>

確認および作成で作成する<br><br>

</div></details>

### ExpressRouteGatewayのデプロイ
<details><summary>詳細を見る</summary><div>

Azure Portalで、「Virtual network gateways」と検索し、ExpressRoute gatewaysであることを確認して作成を押下する<br>
![alt text](/img/image-12.png)<br><br>


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

![alt text](/img/image-13.png)<br><br>

確認および作成で作成する<br>
![alt text](/img/image-14.png)

</div></details>

## ルーティングの設定
<details><summary>詳細を見る</summary><div>
rootテーブルの作製
Azure Portalでルート テーブルを検索し、作成を押下する
<img width="960" height="516" alt="image" src="https://github.com/user-attachments/assets/b5d31e1d-2aa5-484d-8671-a9cd55e81329" />

以下の通り入力して作成する
| 項目 | 入力方法 |
| -- | -- |
| サブスクリプション | 作成したリソースグループのあるサブスクリプション |
| リソース グループ | 作成した仮想ネットワークのあるリソースグループ |
| Name | ルートテーブル名 |
| Propagate gateway routes | Yes |
| リージョン | 仮想ネットワークと同じリージョン |

<br>

</div></details>

## ExpressRoutegatewayを経由するルートテーブルの作製

<details><summary>詳細を見る</summary><div>

作製したルートテーブルにて、「設定」→「ルート」より、「追加」を押下し、以下の通り作成する
※　ネクストホップをHub環境のAzureFirewallに指定します
| 項目 | 入力方法 |
| -- | -- |
| ルート名 | default |
| 宛先の種類 | IP アドレス |
| IP アドレス | OCI CloudのIPアドレス範囲 |
| ネクスト ホップの種類 | 仮想アプライアンス |
| Propagate gateway routes | No |
| ネクスト ホップ アドレス | ExpressRoutegatewayのプライベートIPアドレス |

Spoke環境のサブネットにルートテーブルを関連付け
作製したルートテーブルにて、「設定」→「サブネット」より、「関連付け」を押下し、Spoke環境のVMがあるサブネットを選択してOKする<br>
<img width="961" height="516" alt="image" src="https://github.com/user-attachments/assets/fe82944b-e74a-414b-acdf-6b80054b4ba4" /><br><br>

この状態でVM内からブラウザでGoogleを開こうとすると、接続できなくなる<br>
<img width="960" height="516" alt="image" src="https://github.com/user-attachments/assets/26adc189-aae0-4301-b22c-99584c72f670" /><br><br>

</div></details>

