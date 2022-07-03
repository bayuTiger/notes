# 概要
この記事は自分がAWS CLFの試験勉強をする際にとっていた`ノートをそのまま記載しています`

自分が勉強するとき・ノートを取るとき以下のことを意識しているので、よければ参考にしてください
1. まず全体像を掴む(座学、そこまで時間をかけない)
1. 演習問題に取り組み、そこで知らなかった知識をひたすら記述する(答えを見て既に知っていたものは、その単語だけを記載する)
1. 2をひたすら繰り返し、たまにノートを見返す

## 参考教材
https://explore.skillbuilder.aws/learn/course/external/view/elearning/134/aws-cloud-practitioner-essentials?lacp=tile&tile=dt

https://www.udemy.com/course/aws-4260/learn/quiz/4766842/results?expanded=519631452#overview

# CLF対策

> AWSの概要
> 
- クラウドコンピューティングのデプロイモデル３種
    - クラウドベース
        - クラウド移行や、最初からクラウド上で実装
    - オンプレミス（プライベートクラウド）
        - 仮想化ツールとリソース管理ツールの利用
    - ハイブリッド
        - クラウドベースからオンプレに繋げる
- クラウドコンピューティングの利点６種
    - 先行支出から変動支出（従量課金）
    - データセンターの維持管理費用が不要（クラウド上にあるから）
    - キャパシティの予測が不要（サービスをオンデマンドで利用できるから）
    - 圧倒的なスケールメリット（多くのユーザーによるクラウドの使用量が集約されるから、従量課金制の料金を低く抑えることができる）＝「規模の経済」（規模が大きいことによって、資源もトラブル対応もハイクオリティで行うことができる）
    - スピードと俊敏生を向上（クラウド上にあるから）
    - 数分でグローバルに展開（アプリケーションを低レイテンシーで提供）

> クラウドコンピューティング
> 

## 概要

- **マルチテナンシー**
    - 基盤となるハードウェアを、仮想マシン間で共有する（例：銀行）
- **ハイパーバイザー（仮想化OS）**
    - マルチテナンシーを調整する
- **垂直スケーリング可能**
    - EC2スペック自体をのあげる
    

## EC2（Amazon Elastic Compute Cloud）

- アンマネージドサービス（ユーザー側で基盤となる仮想インフラの完全な管理権限を保持することができる）

## インスタンスタイプ

1. **汎用**
    - バランス重視
    - 多様なワークロード
    - WEBサーバー
    - コードリポジトリ
2. **コンピューティング最適化**
    - コンピューティング負荷の高いタスク
    - ゲームサーバー
    - HPC
    - 科学モデリング
3. **メモリ最適化**
    1. メモリ負荷の高いタスク
    2. メモリはデータの一時的な保管場所（RAM）
    3. 高性能データベース
    4. メモリ内の大規模なデータセット処理
4. **高速コンピューティング**
    - 浮動小数点計算
    - グラフィック処理
    - データマッチングパターン
    - ハードウェアアクセラレータ（処理高速化支援機能）の利用
5. **ストレージ最適化**
    - ローカルに保存されたデータ向けのハイパフォーマンス
    - 具体的には、大規模なデータセットに対する高速シーケンシャルの読み取り、書き込み
    - ストレージはデータの恒久的な保管場所
    

## 料金

1. **オンデマンド**
    - 要求に応じて（その時使った時間分だけ）
2. **Amazon EC2 Savings Plans（確約）**
    - 1時間あたりの使用量を契約する（＝一定のコンピューティング使用量の確約）
    - 1年 or 3年
    - 最大72%節約
    - AWS Lambda,AWS Fargateにも割引を適用できる
3. **リザーブドインスタンス（確約）**
    - 長期的にキャパシティが一定だと予測できる時に便利
    - 1y or 3y
    - 最大75％節約
    - 全額前払い or 一部前払い or 前払い無し
4. **スポットインスタンス**
    - 開始時刻や終了時刻が決まっていなかったり、中断可能なものに最適（バッチ処理、テストとか）
    - つまり、使われていないEC2インスタンスを、売りに出すことができるし、それを買えば利用することができる
    - 需要と供給により値段が変動するので、途中でサーバーが停止したり（売る側）、いつまで経っても利用できなかったりする（買う側）
    - 最大90%節約
5. **Dedicated Hosts**
    - 自分専用の、EC2を備えた物理サーバーを使うことができる
    - もちろん、一番高い
    

## スケーリング（数と質）

- 拡張性と伸縮性

### Amazon EC2 Auto Scaling（スケール）

- これを設定する際には、必ず常に最低1つ以上のEC2インスタンスが稼働している必要があります
- スケールアップ + スケールダウン
- 動的スケーリング + 予測スケーリング(これらの同時利用が望ましい)
- 最小キャパシティ、（必要キャパシティ）、最大キャパシティ（Auto Scalingグループの設定に必要な引数）

## Elastic Load Balancing（トラフィック）

- ロードバランサー（負荷分散）
- リージョン単位（EC2毎ではない）
- EC2AutoScalingと組み合わせることで強力な効果を発揮する。

## メッセージングとキューイング

- 密結合
    - 単一障害でカスケード（連鎖的繋がり）の失敗が発生してしまう可能性がある
- 疎結合
    - アプリケーションAB間にバッファ（メッセージキュー）を入れることで、どちらかの機能に障害が発生しても、どちらかは稼働できるようになる（バッファにキューを貯める事ができるから）

### Amazon SQS (Simple Queue Service)

- **Amazon SQSキュー**
    - メッセージ（ペイロード）が処理されるまで配置される場所
- オートスケールする

### Amazon SNS(Simple Notification Service)

- エンドユーザーに通知を送ることもできる
    - その仕組みはSQSと異なり、**Pub/Subモデル**と呼ばれている
- Amazon SNS トピック
    - 配信するメッセージのチャンネル
    - **サブスクライバーを構成**して、それらにメッセージを送る
        - サブスクライバーにはエンドポイントを使う事ができる
            - SQSキュー、AWS Lambda関数、HTTPS,HTTPウェブフック等
        - エンドユーザーに対しては、モバイルプッシュ、SMS,E-mail等を使える
        
    
    ---
    

## 主要なコンピューティングサービス

- そもそもEC2は
    1. インスタンスをプロビジョニング
    2. コードをアップロード
    3. アプリケーションの実行中、インスタンスを管理し続ける
- この3つ目が面倒なのよ。なんでかっていうと、新しいパッチとかが来たら適用しなければならず、管理コストが発生するから
- つまりEC2は「サーバー」「コード」の2つを管理しなければならない
- なお、AMI（ソフトウェア構成のテンプレート）を使うとEC2インスタンスからイメージを取得して、そこから新しいインスタンスを作成することができる
    - AMIは色々とできることが多い。テンプレートだから

- サーバーレス
    - 基盤となるインストラクチャの表示またはアクセスをする必要がない
    - つまり「コード」1つだけの管理をすれば良い

### AWS Lambda

- コードを配置して、トリガーを設置、するだけで、あとはトリガーに合わせて自動的にコードを実行してくれる
- もちろん柔軟にスケールもしてくれる
- 料金はコンピューティングに使用した時間のみ（コードが実行されている時間）
- つまり
    1. 短い実行時間
    2. サービス指向
    3. イベント駆動
    4. サーバーのプロビジョニングが不要
- の時におすすめできる
- ただ**実行プロセスは15分以内**（深層学習とかには向いていない）

### コンテナ

[AWSでDockerコンテナベースのワークロードを実行したい！]

1. オーケストレーションツール（コンテナの稼働を便利に操作してくれる）を選択する
2. どこでコンテナを実行するか選択する

[1]：ECS or EKS（扱うコンテナの規模の違い）

：**ECS(**Amazon Elastic Container Service)はDockerコンテナをサポートし、APIコールでDockerコンテナを起動、停止する（アプリそのものを実行できる）

：**EKS**(Amzon Elastic Kubernetes Service)はKubernetesをサポートしている。

[2]:EC2 or **AWS Fargate**

：AWS Fargate（コンテナ向けサーバーレスコンピューティングエンジン）はAWSの管理している、設定済みのサーバーを使用するので、サーバーのプロビジョニングと管理を行う必要がない

：料金はコンテナの実行に必要なリソースに対してのみ発生する

## ECR(Elastic Container Registry)

- Dockerコンテナイメージの管理
- AWSクラウドに保存・管理することができる

> グローバルインフラストラクチャと信頼性
> 
- 高可用性と耐障害性

## AWS グローバルインフラストラクチャ

### リージョン

- 高速ファイバーネットワーク
- 各リージョンは地理的に分離しているので、以下の用件を検討して、どのリージョンを使うのかを選択する事が重要
1. データガバナンスと法的要件の尊守（コンプライアンス）
2. ユーザーとの接近性（レイテンシー）
3. リージョン内での利用可能なサービス
4. 料金（原因は主に税金）

### AZ

- リージョン内の1つ、または複数のデータセンターのグループ
- アプリケーションは基本的に、**リージョン内の少なくとも2つのAZで実行する**

## エッジロケーション

- CDN(Contents Delivary Network)
    - **Amazon CloudFront**
    - リージョンから分離された世界中のエッジロケーションを使用
        - 顧客の近くにあるエッジロケーションに、必要な分のデータのキャッシュをコピーしておくことで、安定したテイレンテンシーでコンテンツを配信する事ができる
        - **Amazon Route53**というDNSサービスも、エッジロケーションで実行される
        - **APIGateway**と連携させる
    - **AWS Outposts**は、独自の建物内でAWSを使用できるサービス
        - ハイブリッドクラウドアプローチでうんフラストラクチャを運用できる
        - AWSが所有、運用するが、利用したい建物内でちゃんと分離されている
    - **AWS Global Accelerate**は、ユーザーの地理的に近いエンドポイントにトラフィックをルーティングしてくれる
        - アプリケーションエンドポイントを継続的に監視
    
    ## AWSリソースのプロビジョニング
    
    - APIを使用
        - AWS マネジメントコンソール
        - AWS CLI
        - AWS SDK
        - その他(cloudFormationとか)
    
    ### AWSマネジメントコンソール（手動）
    
    - ウィザードと自動化されたワークフローを利用して、AWSのサービスでタスクを実行する
        - テスト環境
        - AWS請求書の表示
        - モニタリングの表示
        - 非技術リソースで作業する
    - ただ、人間の手作業なので間違いが必ず起こる
    - そこでAPIコールのスクリプト化や、プログラミングを行えるツールを使用する

### AWS CLI(スクリプト化)

- マシン上のターミナルを利用してAPIコールを行う
- APIコールのスクリプト化、トリガーを設定して自動実行など、ヒューマンエラーを防ぐ事ができる
    - EC2インスタンスを作成して、特定のAutoScalingグループに接続することだってできる
- このようなオートメーションは、長期的安定性に不可欠

### AWS SDK(プログラム化)(Software Development Kit)

- さまざまなプログラミング言語を使用して、AWSリソースを操作する事ができる

## (管理ツール編)

### AWS Elastic Beanstalk

- EC2のベース環境をプロビジョニングするのに役立つツール
- アプリケーションコードと必要な設定を、EBに与えると、あとは自動でプロビジョニングしてくれる。
    - これにより個別の細かい設定を管理する必要性も無くなるし、可視性も備えているので、細かい修正も可能
- つまりEBはアプリの迅速な展開に優位性がある
- **しかも、ただデプロイするだけでなくデプロイメントの詳細（養老のプロビジョニング、負荷分散、Autoscaling、モニタリング）を処理してくれる**

### AWS CloudFormation

- 自動作成や反復可能なデプロイに役立つ
- アプリケーションの直接的な展開には利用できない
- Iacのサービスで、JSONかYAMLのテキストベースのドキュメントを使用する（CloudFormationテンプレート）
- 細かく厳密に定義する必要はない＝細かいところはCloudFormationエンジンがやってくれる
    - EC2以外にも、ストレージ、DB、分析、MLにも使える
    - 複数のアカウント、複数のリージョンで並行展開もできる
    
    ### OpsWorks
    
    - ChefやPuppetのマネージド型インスタンスを利用した構成管理サービス
    - 上記2つのインスタンスを使用して、EC2インスタンスの構成方法を自動化することができる
    
    ---
    
    ## ネットワーク
    
    ### VPC
    
    - IPアドレスの範囲を定義できる
    - 後述のさまざまなゲートウェイの存在により、1つのVPC内に多くのゲートウェイが存在する可能性があることに留意する
    
    ### サブネット
    
    - VPC内のIPアドレス群
    - ここに、パブリックとプライベートに分けて、AWSリソースを配置する事ができる
    
    ### ゲートウェイ
    
    - パブリックならインターネットゲートウェイ
    - 「VPC内にプライベートリソースのみが配置されている」なら**仮想プライベートゲートウェイ(VPN接続の確立)**
        - ただ、このままだと他の一般ユーザーと同じ帯域幅を使うことになるので、トラフィックの影響を受けてしまう
    
    ### AWS Direct Connect
    
    - そこで、データセンターとプライベートなサブネット（VPC内のコンポーネント）をAWS Direct Connectで繋げることで、完全にプライベートな専用ファイバー接続を確立する事ができる

## サブネットとネットワークアクセスコントロールリスト

### ネットワークACL（デフォ全許可）（カスタムネットワークACLだと全否定）

- ステートレス(ステート＝情報)
    - 常に独自のリストがチェックされる
    - 処理情報が記録されることはなく、常にチェックリストに当てはまるかどうかだけ確認している
- サブネット毎の入国審査官
- アウトバウンドも明示的許可が必要

### セキュリティグループ（デフォ：イン全否定・アウト全許可）

- ステートフル
- インスタンス毎のドアマン
- アウトバウンドはデフォルトで全許可
- 処理情報を記録しているので、リターントラフィックはそのまま入ってこれる

## グローバルネットワーク

### AWS Route53

- AWSが提供するDNSサービス
- 可用性に優れスケーラブル
- トラフィックを異なるエンドポイントに振り分けることもできる。これについては前述のCloudFrontとの連携が強力。
    1. 顧客が「oo.com」にアクセス
    2. Route53がDNS解決
    3. 顧客からのアクセスは、CloudFrontを介してエッジロケーションに送信
    4. 最後に受信パケットを処理
    
    ---
    

> ストレージとデータベース
> 

## インスタンスストアとAWS Elastic Block Store(EBS)

### インスタンスストア

- EC2インスタンスの、ブロックレベルの**一時ストレージを提供**
- EC2インスタンスのホストコンピュータに物理的にアタッチされているディスクストレージであるため、**存続期間がそのインスタンスと同じ**
- そのため重要なデータは入れない

### EBS

- 1つのAZにデータを保存
- **EC2起動時に必須要求**
- EC2インスタンスの、ブロックレベルストレージを提供（同一AZに存在している必要がある）
- EC2インスタンスが停止、削除されても、アタッチされたEBSボリュームの全てのデータは使用できる
- つまり重要なデータを入れることになるので、当然バックアップが必要。そこで↓

### EBS スナップショット

- 増分バックアップ
- データが増えた部分だけ、逐次バックアップをとっていくスタイル

## Amazon Simple Storage Service(S3)

- リージョンストレージだけど、標準でマルチAZ展開してる

### オブジェクトストレージ

- ブロックレベルのストレージ内のファイル変更は、変更した部分のみ更新されるが、オブジェクトレベルのストレージ内のファイル変更は、オブジェクト全体が更新される
1. データ（画像、テキスト...）
2. メタデータ（データの種類、使用方法...）
3. キー（一意の識別子）

### S3

- データをオブジェクトとしてバケットに保存する
- 最大5TBのオブジェクトをアップロード（ストレージ容量は無制限）
- オブジェクトのバージョン管理
- 複数のバケットを生成
    - 階層化やアクセス許可ができる、ということ
- ちなみに静的ウェブサイトのホスティングもできる（ブログにも最適）
- **S3 Transfer Acceleration**を使うと、長距離でも高速かつ安全にデータ送信ができる
- 標準SQLでS3ないのデータを分析する場合は、**Athena**を利用する

### S3 ストレージクラス

- 以下の2つを考慮する
    - データの取得頻度
    - データの可用性要件
1. S3標準
    - 頻繁にアクセスされるデータ用の設計
    - 最低3つのAZにデータを保存（しかも単体の耐久性はイレブンナイン）
2. S3標準-IA(低頻度アクセス)
    - アクセス頻度の低いデータに最適（バックアップ、災害データとか監査とか）
    - ストレージ料金は低いが、取り出し料金が高い
3. S3 1ゾーン-IA
    - 1つのAZにデータを保存
    - S3-IAよりも低料金
    - 「ストレージのコスト節約」＋「AZに障害が発生した際にデータの復元が容易」の場合に検討すること
4. S3 Glacier
    - データアーカイブ用に設計された低コストのストレージ
    - 数分から数時間以内にオブジェクトを取得可能（3つのオプションから選択）
    - Glacier系は、S3Glacierボールトロックポリシーを使用して、ボールトをロックすることで、データの書き換えとかをできなくし、データの安全保存を可能にすることができる（一回設定したら変更はできない）
        - 例えばWORM(Write Once Read Many)のようなコントロールを指定する
    - 一方でS3のライフサイクル管理を適用させることができる
        - クラス階層間での自動的なデータ移動を可能にする
            - 例えば、最初の120日はS3標準、次の30日はS3-IA、その後はS3Glacierとかの設定をすれば、データの移動をその通りに自動でやってくれる
5. S3 Glacier Deep Archive
    - 最も低コストのオブジェクトストレージクラスで、アーカイブに最適
    - 12時間以内にオブジェクトを取得可能
        - つまりS3Glacierとの違いは、データ取得にかかる時間
6. S3 Intelligent-Tiering
    - アクセスパターンが不明または変化するデータに最適
    - オブジェクトごとに月単位の少額のモニタリング及びオートメーション料金が必要
        - 30日間連続してアクセスがない場合にはS3-IA、S3-IaにアクセスがあるとオブジェクトはS3標準に移動される

## EBS vs S3

→ユーザーがアップロードした画像に対し、画像分析をかけるウェブアプリケーションを実行している（その画像に似た動物を提示する）場合

- S3
    - ウェブが有効（静的ウェブサイトに適応）
    - 地理的に分散（リージョンストレージであり、イレブンナインの耐久性）
    - コスト削減を実現
    - サーバーレス（EC2インスタンス不要）

→編集修正を行っている80Gバイトの動画ファイル

- EBS
    - そもそもオブジェクトストレージとブロックストレージの違いは、データに変更を加えた際に、データのどの部分を再アップロードする必要があるか、という点にある
    - オブジェクトストレージは、画像、テキスト等全てのデータをオブジェクトとして管理しているので、一つのデータの変更でオブジェクト全体を際アップロードする必要がある
    - 対してブロックストレージは、データを細かくコンポーネント化しているので、細かい修正を多数行っても、アップロードはその変更部分だけで良い
    - 従って動画編集という細かい変更が都度要求されるものはEBSが最適（つまり、変更が頻繁でない場合にはS3、複雑な読み取りや書き込み、関数の変更についてはEBSに軍配が上がる）
    
    ## Amazon Elastic File System(EFS)
    
    ### ファイルストレージ
    
    - 多数のサービスとリソースが同時に同じサービスにアクセスする必要があるユースケースに適している（共有フォルダをみんなで利用する感じ）
    
    ### EFS
    
    - 完全マネージド型NFS(Network File System)
    - 複数のインスタンスから同時にEFS内のデータにアクセスできる
    - **リージョンストレージ**（複数のAZにデータを保存）
    - 自動でスケーリング
    - オンプレサーバーからでもAWSDirectConnectでEFSにアクセスできる
    - NAS（Network Attached Storage）で、インターネットからアクセスできない
    
    ## Amazon Relational Datebase Service(RDS)
    
    - 主要なDBをサポート
    - 自動パッチ適用
    - 自動バックアップ
    - システム構成の冗長性のための**マルチAZ構成**(ユーザー側で設定する。自動ではない)（ただ設定さえすれば自動フェイルオーバーしてくれるよ！）
        - データ冗長性や災害復旧対応、コスト最適化の面では**リードレプリカ**という機能が有効
    - フェイルオーバー
    - 災害対策
    - フルマネージドサービスのためOSにログインすることはできない
    
    ### Amazon Aurora
    
    - 最もクラウドに特化したマネージド型RDB
    - MySQLとPostgreSQLに互換性がある
    - コストは商用DBの1/10
    - データレプリケーション（3つのAZ間に6つのデータコピー）
    - 最大十五のリードレプリカ（負荷分散）
    - S3への継続的なバックアップ
    - ポイントインタイムリカバリ（特定の期間からの復旧）
    
    ## AWS Storage Gateway
    
    - オンプレとAWSストレージを接続してくれる→ハイブリッドストレージサービス
        - バックアップ、アーカイブ、災害復旧、クラウドデータ処理、移行に利用することができる
    
    ## Amazon DynamoDB
    
    - サーバーレスDB
    - NoSQL(キーバリューペア)
        - JSON形式のデータを扱うドキュメント型DBとして活躍できるよ！
    - リージョンストレージ
        - クロスリージョンレプリケーションで、複数のAWSリージョンにまたがって自動的にレプリケートされるテーブルを作成することができる
            - しかしまずその前に、**DynamoDB streams**を有効にしなければならに
    - 自動マルチAZ展開（３箇所）
    - 自動スケーリング(10兆/day)
    - 継続バックアップは手動で有効にする必要がある
    - セッションデータの処理に向いている

## Amazon Redshift

- ビッグデータ分析に使用できるデータウェアハウジングサービス（履歴分析に特化）
- RDBサービスの一つ
- 単一のAPIコールを使用できる

## EMR(Elastic MapReduce)

- ビッグデータの分析処理に利用できるマネージド型クラスタープラットフォーム
- データフェアハウジングがなくてビッグデータ分析が出たら、RedshitではなくEMRを選択する

## AWS Database Migration Service(DMS)

1. ソースDBは移行中も通常通り運用できる
2. DBに依存するアプリケーションのダウンタイムは最小限
3. ソースとターゲットは同じタイプのDBでなくても良い
- 同種間の移行
- 異種間の移行
    - まず、スキーマ構造、データ型、DBコードが２種間で異なるため、AWS schema Conversion Toolを使用して、それらを変換する必要がある
- 開発及びテスト環境でのDB移行
- DBの統合
- 継続的なレプリケーション

## その他のDBサービス

- Amazon DocumentDB
    - MongoDBわークローdーをサポートするドキュメントDBサービス
    - コンテンツ管理、カタログ、ユーザープロファイルの管理に最適
- Amazon Neptune
    - ソーシャルウェブの追跡に最適
    - グラフデータベース
    - 不正検出のニーズの対応
- Amazon Quantum Ledger Database(QLDB)
    - 台帳DBサービス（イミュータブル）
    - 全ての変更履歴を確認することができる
- Amazon Managed Blockchain
    - 分散台帳システム
    - オープンソースフレームワークのブロックチェーンネットワークを作成、管理するために使用できる
- Amazon ElastiCache ・ AMazon DynamoDB Accelerator(DAX)
    - 2つともDBアクセラレータ
    - DB上にキャッシュレイヤーを追加することで、一般的なリクエストの読み込み時間を短縮する
        - DAXはDynamoDBに特化したDBアクセラレータ

---

> セキュリティ
> 

## 責任共有モデル

- 物理・ネットワーク・ハイパーバイザー → AWS（クラウドのセキュリティ）
- OS・アプリケーション・データ → 顧客（クラウド内のセキュリティ）

## ユーザーのアクセス許可とアクセス権

### AWS Identity and Access Management(IAM)

- IAMユーザー、グループ、ロール
    - IAMユーザーはデフォルトで全否定なので、必要なアクセス権を付与すること
    - グループはIAMユーザーの集合体。複数のユーザーに同一の権限を付与することができる
    - IAMロールは、権限の一時的な付与をすることができる。
        1. IAMユーザー、アプリケーション、サービスがIAMロールを引き受けられるようにするために、ロールに切り替える権限を事前に付与しておくこと
        2. IAMロールを引き受けると、以前のロールで持っていたアクセス許可は全て取り消され、新しいロールのアクセス許可が与えられる。
- IAMポリシー
    - AWSのサービスとリソースへのアクセスを許可または拒否するドキュメント
- 多要素認証
    - MFA
- IDフェデレーション
    - IAMユーザーを作成しなくても、AWSアカウントのリソースに対する安全なアクセス権が外部の認証に付与される
    - SSO（シングルサインオン）を有効にできる（googleアカウントによる認証のようなもの）

## AWS Organizations

- 一元化された管理
- アカウント作成の自動化
- 一括請求（コンソリデーティッドビリング）
    - ボリュームディスカウントもあるよ！
- アカウントの階層的なグループ化（OU）
- AWSのサービス及びAPIアクションのアクセスコントロール（SCP：サービスコントロールポリシー）
    - 組織のルート、個別のメンバーアカウント、OU（組織単位）に適用することができる（IAM系はIAMポリシーが適用される）
    

## コンプライアンス

### AWS Artifact

- AWS Artifact Agreements
    - 特定の種類の情報を使用することに関する、AWSとの契約の署名におけるサービス（医療保険や個人情報取扱とか）
- AWS Artifact  Reports
    - サードパーティーの監査人からのコンプライアンスレポートを提供
- カスタマーコンプライアンスセンター
    - ベストプラクティスとかチェックリストとかあってめっちゃ便利

## サービス妨害攻撃

- セキュリティグループ(UDPフラットやUDPリフレクトの対抗策)
- ELB（SLOWLORISの対抗策）

その他の攻撃に対する有効策

### AWS WAF(Web Application Firewall)

- 攻撃者のシグネチャで受信トラフィックを制限する
- 幅広い機械学習機能を持っているので進化し続ける
- Cloud Front,ALBと連動
- ACLを使用してAWSリソースを保護

### AWS Shield

- DDos攻撃にも対応してくれる

## その他のセキュリティサービス

### AWS KMS(Key Management Service)

- 保管時の暗号化と送信時の暗号化

### AWS CloudHSM

- 業界水準の暗号化対応実施を保証（KMSの上位互換かもしれん）

### Amazon Inspector

- AWSにデプロイしたアプリケーション(EC2)のセキュリティを、自動で評価してくれる
    - ネットワークの到達性に関する設定
    - Amzonエージェント
    - セキュリティ評価サービス

### Amazon GuardDuty

- 脅威検出サービス
- 他のAWSサービスとは独立して実行されるので、既存のワークロードの性能や可用性に無影響

### Amazon STS(Security Token Service)

- AWSリソースに対して一時的な認証情報を提供する

> モニタリングと分析
> 
- モニタリング
    - システムの監視
    - メトリクス（評価尺度）の収集
    - データを使用した意思決定
- つまり「システムパフォーマンスの測定」「異常発生時のアラート送信」に役立つ

## Amazon CloudWatch

- Amazon Cloud Watchアラーム
    - メトリクスにある変数の閾値を設定することで、その変数が閾値に達した時にアラームを鳴らして、アクションをトリガーする
- 一元的な場所から全てのメトリクスにアクセスできる
- アプリケーションインフラストラクチャサービスを可視化する
- MTTR（問題解決のための平均時間）を短縮しTCO（総所有コスト）を改善する
- アプリケーションと運用リソースの最適化に役立つインサイトを引き出す

## AWS CloudTrail

- 包括的なAPI監査ツール
    - 全てのリクエストがCloudTrailエンジンでログ(S3 無制限)に記録される
    - 異常なアカウントアクティビティモ自動的に検出してくれる
- CloudTrail Insightsを使用することで、次に実行すべきアクション（知見）も教えてくれる

## AWS System Manager

- 運用データを表示し、運用タスクを自動化する
    - AWSで利用しているインフラストラクチャを可視化して、制御する

## AWS Trusted Advisor

- 自動化されたアドバイスサービス
    - コスト最適化
    - パフォーマンス
    - セキュリティ
    - 耐障害性
    - サービスの制限
- 一部無料

---

> 料金とサポート
> 

## AWS 無料利用枠

- 無期限無料
    - AWSLambdaは月100万回の呼び出しが無料
    - DynamoDBでは毎月25GBのストレージを無料利用可能
- 12ヶ月間無料
    - AWSアカウントを作成した日から
    - オブジェクトストアサービスのS3は、最大5GBの標準ストレージが12ヶ月間無料
- トライアル
    - 一部のサービスは短期間の無料トライアルが可能
    - AWS Lightsailでは1ヶ月間のトライアルで最大750時間使用可能

## AWSの料金概念

- 従量課金
    - 大体そう
- 予約支払い
    - EC2SavingPlansとか
- ボリュームディスカウント
    - S3は使用量が大きくなるごとに1GBあたりの料金は下がってく

## 請求ダッシュボード

- 過去のデータとかも参照できるよ

## コンソリデーティッドビリング（一括請求）

- 無料の機能
- 請求プロセスの簡素化
- アカウント間で割引の共有(使用量を合算してディスカウントラインまで引き上げる、ってのはできないよ)

## AWS Budgets

- 使用量が予算を超えた場合に送信される、アラートを設定できる

## AWS Cost Explorer

- 時間経過に伴うAWSコストと使用量を可視化して把握し、管理できるツール
    - カスタムレポートを作成できる
    

## AWS料金計算ツール

- ユースケースのコスト見積もりを作成する

## AWSサポートプラン

- ベーシック
    - 無料
    - 24時間365日対応のカスタマーサービス
    - ドキュメント
    - ホワイトペーパー
    - サポートフォーラム
    - AWS Trusted Advisor
    - AWS Personal Health DashBoard
- デベロッパー
    - ベーシックサポート
    - カスタマーサポートへのEメールアクセス（12時間以内）
- ビジネス
    - デベロッパーサポート
    - AWS Trusted Advisorのベストプラクティスフルセット
    - クラウドサポートエンジニアへの直接電話アクセス（4時間以内）
    - インフラストラクチャイベント管理（キャンペーン・イベント支援）
- エンタープライズサポート
    - ビジネスサポート
    - ビジネスクリティカルなワークロードに対応する15分のSLA
    - TAM（専属のテクニカルアカウントマネージャー）サポート

## AWS Marketplace

- サードパーティから提供される、多数のソフトウェアで構成されるデジタルカタログ
- ほとんどのベンダーはAWSで使われることを許可してるし、従量課金制

> 移行とイノベーション
> 

## AWS CAF(Cloud Adoption Framework)

- クラウド支援するためのドキュメント
- 誰が、なんの役割で、何に焦点をあてて取り組むのか
- 6つのパースペクティブ（ビジネス・会社のさまざまな側面から、）
    - ビジネス
    - 人員
    - ガバナンス
    - プラットフォーム
    - セキュリティ
    - オペレーション

## 移行戦略

- 6つのR
    - リホスト
        - リフトアンドシフトとも言われる
        - 何も変更せずに、アプリを移行する
    - リプラットフォーム
        - リフトティンカーシフトとも言われる
        - 基本的にはリホストだが、いくつかのクラウド最適化を行う（基幹のコードは変えない）
    - リファクタリング
        - アーキテクチャの再設計を行う
        - 最も初期コストが高い
    - 再購入
        - 古いベンダーとの契約を終了し、新しいベンダーに切り替える
        - もちろん時間がかかるものもある
    - 保持
        - ソース環境でビジネスに重要なアプリケーションを維持する
    - リタイア
        - 10％〜20％の不要になったアプリケーションを削除する

## AWS Snowファミリー

- データをAWSに物理的に転送できる**物理デバイス（ネットワークを経由しない）**
- AWS Snowcone
    - 最大8TB
    - エッジコンピューティング（EC2とか）のデータも送れる
    - 普通はS3に保存される
- AWS Snowball Edge
    - Storage Optimized
        - 80TB
    - Compute Optimized
        - 42TB
- AWS Snowmobile
    - 100PBまで乗っけれるトラックが来るよ！

## AWSでのイノベーション

- サーバーレス
    - AWS Lambda
- IoT
    
    Kinesis → IoTデータのストリーミング処理を実施してデータを収集する
    
- 人工知能
    - Amazon Transcribe→音声をテキストに
    - Amazon Comprehend→テキストパターンを検出
    - Amazon Fraud Detector→不正可能性のあるオンラインアクティビティを検出
    - Amazon Lex→音声及びチャットbotを構築
- 機械学習
    - Amazon SageMaker
    

---

> クラウドジャーニー
> 

## AWS Well-Architectedフレームワーク

- 運用上の優秀性
- セキュリティ
- 信頼性
- パフォーマンス効率
- コスト最適化

---

# 問題演習

> 基礎
> 
- フォールトトレランス
- CloudTrail — CloudWatch
- S3バケット名の命名規則
    - 世界中の全てのAWSリージョンで一意
    - 3〜63文字以内
- 可用性
- IDフェデレーション
    - IAMユーザーを作成しなくても、権限付与ができる
    - SSOを有効にできる
- AWS内データの削除方法
    - 物理メディアはそのままで、ストレージブロックを未割り当てとする
- スケールメリット＝規模の経済
- S3＝リーションストレージ
- CloudFrontは単一障害点にはならないが、既存の単一障害点を解決することには役立たない。適切なエンドポイントにルーティングするRoute53、ELB、Autoscalingは単一障害点を解消することができる
- カーブアウト(VPCができる)
    - 企業が事業の一部を切り出して、その事業を社外事業の1つとして独立させること
- ALB → Application Load Balancer
- NLB → Network Load Balancer
- CLB → Classic Load Balancer（ELBの古い版）
- ELB → Elastic Load Balancer
    - ALB + CLB = ELB
    - CLBは特殊なケースを除いて基本使わない
    - NLBは高性能だが、利用すべき専用の用件が定義されていない限りWEBアプリケーション用のELBはALBを使う
- EC2インスタンスのログ系は「CloudWatchログ」
- マネージド型サービスにおいてユーザー側が実行すべきセキュリティ手順は「SSL/TLS（データ送信の暗号化）」＋「ログ管理」
- 見積もり→AWS料金計算ツール
- S3はIPアドレスつけられないけど、URL経由で保存データはダウンロードできるよ（静的ウェブサイトホスティングできるからね）
- RDS読み取り＝リードレプリカ（読み取り処理の負荷軽減）
    - オフロード＝負荷分散（システムの一部分を外部システムに渡す）
- AWSプロフェッショナルチーム
    - ビジネス成果を実現
- AWSコンシェルジュ
    - 請求とアカウントの専門
- Route53はWEBサーバーのヘルスチェックに基づいた監視に利用できる
    - だから正常なエンドポイントにトラフィックをルーティングして負荷分散を実現している
- VPCフローログ
    - VPCのトラフィックログを取得する
- Fargateはコンテナサービスのエンジンだからサーバーレス
- メトリクス（評価尺度）→CloudWatch
- AWS Config
    - AWSリソース設定の継続的モニタリング＋設定に対する記録の評価
- Snowballはダメ→Snowball edgeならOK
- データレイヤー.＝.DB
- スナップショット＝簡易的なバックアップ（的な認識。EC2とかEBSで使う）
- **AWS CodeCommit（ソース管理）**
    - アプリケーションリソースをアプリケーションコードと一緒に保存できる
- **AWS CodePipeline（デリバリーサービス）**
    - フルマネージド型デリバリーサービス
    - アプリとインフラのアップデート用のパイプラインリリースの自動化
- **AWS CodeBuild（ビルド＆テスト）**
    - クラウド内のコードのビルド及びテストができるよ
- **AWS CodeDeploy（デプロイ）**
    - EC2,Fargate,Lambda、オンプレのサーバーなど、さまざまなコンピューティングサービスへのソフトウェアデプロイを自動化する
    - フルマネージド型のサービス
- Neptune→グラフアプリケーション
- Cognito→ウェブ・モバイルアプリにユーザーのアクセスコントロール機能を追加する
- RDSはアプリケーション向けのRDB、EC2向けはEBSかEFS（単一・複数のEC2インスタンスにアタッチするかで変わる）
- IAMエンティティにポリシーは含まれない（ユーザー、グループ、ロールの3つ）（ポリシーだけドキュメントだから、それを元に仲間外れにする）
- S3は自動バックアップをしていない（複数のAZに自動的にレプリケートしているが、これは厳密にはバックアップではない）
- プログラム呼び出しを認証→アクセスキー＋シークレットアクセスきー

---

> 応用（知らないものがめっちゃ多かった）
> 
- 運用データの確認と運用データの自動化
    - **SystemManager と CloudWatch**(CloudWatchAgentとSystemManagerとの連携が前提)
- アーキテクチャのスケーリングに関するガイダンス
    - **IEM(Infrastructure Event Management)**
- 侵入テスト
    - **AWSからの許可なしに顧客がテストできる**
        - EC2インスタンスに対するセキュリティなどは、顧客の責任範囲であるため
- CLIの初期設定には、アクセスキーが求められる（他にはシークレットアクセスキー＋AWSリージョン＋出力形式）
- AWS Organizationは、**S3を統合して包括的なディスカウントを受けることのできる可能性がある**
- DynamoDBは、**複雑なトランザクション処理が発生する業務システムには向いていない**（銀行の振り込み処理とか）
    - トランザクションは、分割できない一連の処理のこと
    - つまり単純な記録の蓄積・処理には向いている
- ユーザー側の責任の範囲は、**クラウド内（OS、ネットワーク、データ）**である
- マイクロサービスアーキテクチャで構成されるアプリケーション（分散型アプリケーション）（ブロックチェーンみたいなもん）の分析・デバッグは**AWS X-Ray**で行う
- Inspectorは、事前に定義されたセキュリティテンプレートを参照してEC2インスタンスを分析する
- 信頼性＝「障害の発生のしにくさ」＝「可用性の高さ」
    - システムの中断からの復旧、中断を緩和させる
        - 障害からの早期復旧＋Autoscalingを活用した自動プロビジョニング
- Snowball Edgeのもう1つの顔
    - 「データ移行(Storage)」＋**「エッジコンピューティング(Compute)」**
        - 切断された環境（軍事、海軍とか）における高度な機械学習＋フルモーションビデオ分析とか
- 不正処理は不正使用対策チームへご連絡を
- 閾値を超えた時にトリガーされる＝CloudWatch+SNS
    - 請求額が予算を超えそうな時とか
- CloudFrontの料金
    - トラフィック分散
    - リクエスト
    - データ転送アウト
- インスタンスを停止させてもEBSのデータ保存料を支払う必要がある
    - 逆に言えば、インスタンスを料金は支払わずに、データを保存しておくことができる
- AWSの設計原則
    - スケーラビリティ
    - デプロイ可能なリソース
    - 環境の自動化
    - 疎結合化
    - サーバー代わりのマネージドサービス（サーバーレス）
    - 柔軟なストレージオプション
- コンタクトセンターソリューション＝**Amazon Connect**
- Configは設定を外れると自動的にSNSがトリガーされる
    - コンプライアンスや脆弱性への有効策
- SystemManagerはインフラとAWSサービスを可視化して制御するためのサービス
    - コンプライアンス・脆弱性向きではない
- 各フェーズに応じたカスタムコンソール＝**AWSリソースグループ**
- RDSはオブジェクトを使用していません
- RDSは途中でインスタンスタイプを縮小できる
    - ちなみにデータ容量だけはスケーリングできるよ（キャパシティは無理）
- DAXを忘れないで！
    - ElastiCacheのDynamoDB版。処理の高速化よ！
- DynamoDBにリードレプリカはないよ！
    - その代わりDAXクラスターにはある
- CloudHSM(ハードウェアセキュリティモジュール)（モジュール＝システムの構成要素）
- AWS Trusted Advisor
    - コスト、パフォーマンス、セキュリティ
        - スケーリングはIEMへ
- DynamoDB＝ドキュメント型DBとして利用できる
- リザーブドできるやつ
    - EC2インスタンス
    - RDSインスタンス
    - ElastiCacheノード
    - DynamoDBキャパシティ
    - Redshiftノード
        - S3やEFSは、データ使用量（＋データリクエスト量）に対して料金を支払う
- **MCS(Managed Apache Cassandra Service)**
    - DynamoDBはAWS専用であるのに対して、MCSはオープンソースとの互換性がある

---

## 基本問題

- **RDSは自動バックアップ**してくれるけど、マルチAZ展開は手動
    - DynamoDBとS3は自動マルチAZ展開
        - 「初めからマルチAZ展開」＝リージョンストレージ
- 見積もり→AWS料金計算ツール
    - 簡易見積もりツールは、AWS料金計算ツールに統合される
- リザーブドインスタンスは、途中で利用をやめようと思えば、お金は返ってこないがマーケットプレイスで売りに出せる
- **S3**は**自動バックアップをしているわけではない**