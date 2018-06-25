AWS-Study
# AWSが提供するセキュリティサービス
## IAM（Identity and Access Management）
### IAMとは
- 認証、認可、アクセス管理、ポリシー管理のサービス
- AWS操作のためのグループ・ユーザー・ロールの作成が可能
- グループ、ユーザーごとに、実⾏出来る操作を規定できる（IAMポリシー）

![image](https://user-images.githubusercontent.com/2520577/41504643-ed8d10d4-722f-11e8-9b2f-d0253398346c.png)


### IAMユーザ作成方法
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_users_create.html#id_users_create_console


### IAMグループ作成方法
- IAMグループの作成、およびポリシーのアタッチ
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_groups_create.html


- IAMグループにユーザ追加
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_groups_manage_add-remove-users.html


### IAMポリシー作成方法
- IAMポリシー作成
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/access_policies_create.html

- 【参考】ポリシー概要
https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/access_policies_understand.html

- 【参考】AWSが管理する既存のポリシーイメージ（一部のみ）
![image](https://user-images.githubusercontent.com/2520577/41831085-fad6039e-787f-11e8-9030-4bc25e161bb8.png)



### IAMロールについて
- EC2やLambdaなどのAWSサービスが、S3などの他のAWSサービスを利用する権限
![image](https://user-images.githubusercontent.com/2520577/41757333-7449bfce-761c-11e8-8dd6-f2464069db1d.png)

- IAMロールに対し、複数のIAMポリシーを設定できる。
![image](https://user-images.githubusercontent.com/2520577/41831149-5d23e778-7880-11e8-8d8e-cc25bf0ec948.png)



### 【参考】AWS CLI(Command Line Interface)を利用する際の認証について
- aws configureよりIAMユーザのアクセスキー、シークレットアクセスキー等を指定する。
https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration

- 追加の権限が必要な場合は、～/.aws/configファイルにIAMロールを指定する。
https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-roles.html

- 【注意】アクセスキー、シークレットキーを公開することは非常に危険ですので、扱いには注意ください


## KMS（Key Management Service）
- 暗号鍵の作成、管理、運⽤サービス
- S3, EBS, Redshift等のAWSサービスとの統合
- SDKとの連携でお客様の独⾃アプリケーションデータも暗号化
- AWS CloudTrail と連動したログの⽣成による組み込み型監査

![image](https://user-images.githubusercontent.com/2520577/41503547-276c2d44-7212-11e8-80c8-970ae7e6e229.png)


## Inspector
- EC2の自動セキュリティ診断サービス
    - EC2にエージェントを導入し、プラットフォームの脆弱性を診断するホスト型診断サービス。
- 以下の項目、ルールパッケージが用意されている


|項目|ルールパッケージ名|内容|注意点|
|:--|:--|:--|:--|
|共通脆弱性識別子|Common Vulnerabilities and Exposures|EC2 インスタンス が一般的な脆弱性や曝露 (CVE) に曝露されているかを確認する||
|CIS オペレーティングシステムのセキュリティ設定ベンチマーク|CIS Operating System Security Configuration Benchmarks|アメリカにあるインターネット・セキュリティー・センター（CIS）が定義したセキュリティチェックリストに沿って確認する||
|セキュリティのベストプラクティス|Security Best Practices|システムが安全に設定されているかどうかを判断するのに利用する|WindowsのEC2インスタンスでは結果が出ないので、LinuxのEC2インスタンスで利用すること。|
|実行時の動作の分析|Runtime Behavior Analysis|評価の実行中にインスタンスの動作を分析し、EC2 インスタンスのセキュリティを高めるためのガイダンスを提供する||


- 利用イメージ
    - AWS管理コンソールから設定（以下イメージ）
![image](https://user-images.githubusercontent.com/2520577/41504811-6b133100-7235-11e8-9184-f45c1dcefe9c.png)


- 費用について

|Inspector の利用開始から 90 日間|エージェント評価ごとの料金|
|:--|:--|
|最初の 250 回のエージェント評価|0.00 USD|

|任意の月|エージェント評価ごとの料金|
|:--|:--|
|最初の 250 回のエージェント評価|0.30 USD|
|次の 750 回のエージェント評価|0.25 USD|


## ACM（AWS Certificate Manager）
- AWSのサービスに対してパブリックSSL/TLS証明書の作成と管理を行うサービス。対象は以下のサービス。
    - Elastic Load Balancing
    - Amazon CloudFront
    - AWS Elastic Beanstalk
    - Amazon API Gateway
    - AWS CloudFormation

- ACMで管理するSSL/TLS証明書については料金が発生しない。

- 証明書の種類は「ドメイン認証」。

![image](https://user-images.githubusercontent.com/2520577/41504864-57761828-7236-11e8-84a1-16a36804a97e.png)



# ネットワークセキュリティ
## VPC(Virtual Private Cloud)
### VPCとは
- AWS上のプライベートネットワーク空間
- IPアドレス範囲を指定したサブネットを作成することにより、ネットワーク分離が可能
- VPCはAZをまたがることが可能

### VPCのネットワーキングコンポーネント
- ENI(Elastic Network Interface)
    - EC2のNICにあたるもので、これまではインスタンスに直接付与されていたIPアドレス等のネットワーク情報を、ENIに付与させることでより柔軟なネットワーク構成を実現するためのもの
    - ENIはVPCでのみ使用することができる
    - Lambda等のVPC外のサービスから、VPC内のリソース（RDS等）にアクセスする場合は、ENI経由で行う
- ルートテーブル
    - ネットワークトラフィックの経路を判断する際に使用される、一連のルール（パケットがどこに向かえばいいのかを示すもの）
- インターネットゲートウェイ
    - VPC のインスタンスとインターネットとの間の通信を可能にする
- Elastic IP
    - インスタンスに付いているネットワークインターフェイスと固定のPublicIPアドレスを紐付ける
- etc...
https://docs.aws.amazon.com/ja_jp/AmazonVPC/latest/UserGuide/VPC_Networking.html



### VPCのセキュリティ機能
#### セキュリティグループ
- EC2の仮想ファイアーウォールとして機能する

#### ネットワークACL(NACL)
- サブネットへのネットワークアクセスを制御する


## サブネット
### サブネットとは
- VPC内をさらに分割したネットワーク領域
- サブネットはAZをまたがれない


## VPC、サブネットの分割ポイント
### VPCの分割ポイント
- 個人情報などを扱うシステムは別VPCにするべき
- 本番環境、検証環境の単位。誤操作防止やIAMによる権限分離のため
- DirectConnectでオンプレミスと接続する場合は、VPCを増やすとコストも増加するため注意が必要

### サブネットの分割ポイント
- 本番環境、検証環境の単位。（VPCで分割していない場合）
- DMZ、APサーバ、DBサーバ等の単位。セキュリティ観点による分離
- AZ単位。AWSの制約のため

## VPC作成方法
https://docs.aws.amazon.com/ja_jp/AmazonVPC/latest/UserGuide/getting-started-ipv4.html#getting-started-create-vpc

## サブネット作成方法
https://docs.aws.amazon.com/ja_jp/AmazonVPC/latest/UserGuide/working-with-vpcs.html#AddaSubnet
