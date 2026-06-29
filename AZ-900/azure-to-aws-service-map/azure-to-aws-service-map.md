# Azure → AWS サービス読み替え表｜SAA学習用

## 0. この表の使い方

この表は、AZ-900で学んだAzureの知識を、AWS Certified Solutions Architect - Associate（SAA）向けに読み替えるための対応表。

注意点：

- AzureとAWSは完全な1対1対応ではない
- 同じような役割でも、設計思想や責任範囲が違うことがある
- SAAでは「サービス名の暗記」より「要件に対して何を選ぶか」が重要
- Azureで覚えた概念を入口にして、AWSの設計パターンへつなげる

---

## 1. 全体構造・アカウント管理

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Microsoft Azure | Amazon Web Services（AWS） | クラウドサービス全体 |
| リージョン | リージョン | 地理的な提供地域。基本概念はほぼ同じ |
| 可用性ゾーン | アベイラビリティゾーン（AZ） | 同一リージョン内の独立したデータセンター群 |
| リージョンペア | 明確な固定ペアは基本なし | AWSでは自分でマルチリージョンDRを設計する |
| サブスクリプション | AWSアカウント | 請求・権限・リソース分離の単位として近い |
| 管理グループ | AWS Organizations / Organizational Units（OU） | 複数アカウントを階層管理する |
| リソースグループ | Resource Groups / タグ運用 | AWSではタグによる分類が特に重要 |
| リソース | AWSリソース | EC2、S3、VPCなどの個々のサービス実体 |
| グローバルサービス | グローバルサービス | IAM、Route 53、CloudFrontなどはグローバル寄り |
| SLA | AWS Service Level Agreement | サービスごとに可用性や補償条件がある |
| サービスクレジット | サービスクレジット | SLA未達時の利用料金クレジット |

---

## 2. クラウド概念・責任共有

| Azure | AWS | 読み替えポイント |
|---|---|---|
| 共有責任モデル | 共有責任モデル | クラウド事業者と利用者の責任分担 |
| IaaS | IaaS | EC2など。OS以上は利用者管理が多い |
| PaaS | マネージドサービス | Elastic Beanstalk、RDS、Lambdaなど |
| SaaS | SaaS | Amazon WorkSpaces、Amazon QuickSightなどサービスにより異なる |
| CAPEX | CAPEX | オンプレミスの設備投資 |
| OPEX | OPEX | クラウド利用料などの運用費 |
| 高可用性 | High Availability | 複数AZ、冗長化、ロードバランサーが重要 |
| フォールトトレランス | Fault Tolerance | 障害発生時も処理継続できる設計 |
| スケーラビリティ | Scalability | Auto Scaling、ELB、サーバーレスなど |
| 弾力性 | Elasticity | 需要に応じて自動的に増減する力 |
| 機敏性 | Agility | すばやくリソースを作成・変更できること |
| ハイブリッドクラウド | Hybrid Cloud | Direct Connect、VPN、Outposts、Storage Gatewayなど |
| マルチクラウド | Multi-Cloud | AWSとAzureなど複数クラウドを併用 |

---

## 3. 管理ツール・IaC

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Azure portal | AWS Management Console | WebベースのGUI管理画面 |
| Azure PowerShell | AWS Tools for PowerShell | PowerShellでAWSを操作 |
| Azure CLI | AWS CLI | コマンドラインでAWSを操作 |
| Azure Cloud Shell | AWS CloudShell | ブラウザー上でCLIを実行できる |
| Azure Resource Manager（ARM） | AWS API / CloudFormationの基盤操作 | AzureのARMのような単一名称では覚えにくい |
| ARMテンプレート | AWS CloudFormation | インフラをコードで定義して展開 |
| Bicep | AWS CDK / CloudFormation | より書きやすいIaCの位置づけ |
| Azure Arc | AWS Systems Manager / AWS Outposts / AWS Control Towerなど | 用途により対応が分かれる。完全一致ではない |
| Azure Mobile Apps | AWS Console Mobile Application | モバイルからリソース確認・簡易操作 |
| タグ | タグ | コスト配分・検索・自動化・権限管理で重要 |

---

## 4. コンピューティング

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Azure Virtual Machines | Amazon EC2 | IaaSの仮想サーバー |
| VMサイズ | インスタンスタイプ | vCPU、メモリ、用途別ファミリーを選ぶ |
| マネージドディスク | EBS | EC2に接続するブロックストレージ |
| OSディスク | ルートEBSボリューム | EC2起動用のディスク |
| データディスク | 追加EBSボリューム | データ保存用に追加する |
| 可用性セット | 近い直接対応なし | AWSでは複数AZ配置で考えることが多い |
| 障害ドメイン | AZ / ラック分散はAWS側抽象化 | SAAではAZ分散が重要 |
| 更新ドメイン | 近い直接対応なし | AWSではマネージドサービスや複数AZで影響を下げる |
| 可用性ゾーン | アベイラビリティゾーン | 同一リージョン内の障害分離単位 |
| Virtual Machine Scale Sets | Auto Scaling group | EC2台数を自動増減する |
| 自動スケール | Auto Scaling | 負荷やスケジュールに応じて増減 |
| スケールアップ | スケールアップ | インスタンスタイプを大きくする |
| スケールアウト | スケールアウト | EC2台数を増やす |
| Azure App Service | AWS Elastic Beanstalk / AWS App Runner | Webアプリをマネージドに実行 |
| App Serviceプラン | Beanstalk環境 / App Runner構成 | 実行基盤やサイズの考え方 |
| Azure Functions | AWS Lambda | サーバーレスでイベント駆動のコード実行 |
| トリガー | イベントソース | S3、EventBridge、SQS、API Gatewayなど |
| Web App for Containers | AWS App Runner / ECS Fargate | コンテナーWebアプリを実行 |
| Azure Container Instances（ACI） | ECS on Fargate / AWS App Runner | サーバー管理なしでコンテナー実行 |
| Azure Kubernetes Service（AKS） | Amazon EKS | Kubernetesのマネージドサービス |
| Azure Container Registry（ACR） | Amazon ECR | コンテナーイメージのレジストリ |
| Azure Virtual Desktop | Amazon WorkSpaces / AppStream 2.0 | 仮想デスクトップやアプリ配信 |
| ホストプール | WorkSpacesプール / AppStreamフリート | 完全一致ではないが、利用者環境をまとめる考え方 |

---

## 5. ネットワーク

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Azure Virtual Network（VNet） | Amazon VPC | クラウド内のプライベートネットワーク |
| サブネット | サブネット | VPC/VNet内を分割する範囲 |
| アドレス空間 | VPC CIDRブロック | VPC全体のIPアドレス範囲 |
| サブネットのアドレス範囲 | サブネットCIDR | サブネット単位のIP範囲 |
| プライベートIPアドレス | プライベートIPv4アドレス | 内部通信で使うIP |
| パブリックIPアドレス | パブリックIPv4 / Elastic IP | インターネット到達用IP |
| ネットワークインターフェイス | Elastic Network Interface（ENI） | VM/EC2をネットワークに接続する |
| NSG | セキュリティグループ / ネットワークACL | AWSでは2種類を使い分ける |
| NSG受信規則 | セキュリティグループのインバウンドルール | 入ってくる通信を制御 |
| NSG送信規則 | セキュリティグループのアウトバウンドルール | 出ていく通信を制御 |
| Azure Firewall | AWS Network Firewall | ネットワークファイアウォール |
| Azure Bastion | AWS Systems Manager Session Manager / EC2 Instance Connect | 踏み台なし接続。完全一致ではない |
| VNetピアリング | VPC Peering | 2つのVPCをプライベート接続 |
| グローバルVNetピアリング | 異なるリージョンのVPC Peering | リージョン間VPC接続 |
| VNet間接続 | Site-to-Site VPN / Transit Gateway | VPC同士をVPNや中継で接続 |
| VPNゲートウェイ | Virtual Private Gateway / Transit Gateway | VPN接続のAWS側ゲートウェイ |
| サイト間VPN | AWS Site-to-Site VPN | オンプレミスとAWSをVPN接続 |
| ポイント対サイトVPN | AWS Client VPN | 個人端末からVPCへVPN接続 |
| ExpressRoute | AWS Direct Connect | 専用線接続 |
| ローカルネットワークゲートウェイ | Customer Gateway | オンプレミス側ルーター情報 |
| サービスエンドポイント | VPCエンドポイント | AWSサービスへプライベート接続 |
| Private Link | AWS PrivateLink | サービスへプライベート接続 |
| ロードバランサー | Elastic Load Balancing（ELB） | 負荷分散 |
| Application Gateway | Application Load Balancer（ALB） | L7 HTTP/HTTPS負荷分散 |
| Azure Load Balancer | Network Load Balancer（NLB） | L4負荷分散 |
| Azure Front Door | CloudFront / Global Accelerator / ALB | グローバル配信・入口。用途で分かれる |
| CDN | Amazon CloudFront | コンテンツ配信 |
| DNSゾーン | Route 53 パブリックホストゾーン | インターネット向けDNS |
| プライベートDNSゾーン | Route 53 プライベートホストゾーン | VPC内部向けDNS |
| Aレコード | Aレコード | 名前をIPv4に対応付ける |
| CNAMEレコード | CNAMEレコード | 別名を定義 |
| MXレコード | MXレコード | メールサーバーを定義 |
| NSレコード | NSレコード | 権威DNSサーバーを示す |
| DNS委任 | DNS委任 | 上位DNSへNSレコードを登録 |

---

## 6. ストレージ・データベース

| Azure | AWS | 読み替えポイント |
|---|---|---|
| ストレージアカウント | S3 / EFS / SQS / DynamoDBなどに分かれる | AWSにはAzure Storage Accountの完全対応はない |
| Azure BLOB | Amazon S3 | オブジェクトストレージ |
| BLOBコンテナー | S3バケット | オブジェクトを入れる単位 |
| BLOBオブジェクト | S3オブジェクト | 保存されるファイル実体 |
| Azure Files | Amazon EFS / Amazon FSx | ファイル共有。用途で選ぶ |
| Azure Queue | Amazon SQS | メッセージキュー |
| Azure Table | Amazon DynamoDB | キー値・NoSQL寄りのデータストア |
| マネージドディスク | Amazon EBS | VM/EC2用ブロックストレージ |
| 一時ディスク | インスタンスストア | 一時的なローカルストレージ |
| Azure SQL Database | Amazon RDS / Amazon Aurora | マネージドRDB |
| SQL Server on Azure VM | SQL Server on EC2 | 自分でDBサーバー管理 |
| Azure Cosmos DB | DynamoDB / DocumentDB / Keyspaces | APIやデータモデルにより対応が分かれる |
| Azure Cache for Redis | Amazon ElastiCache for Redis / Valkey | インメモリキャッシュ |
| Azure Data Lake Storage | Amazon S3 + Lake Formation | データレイク構成 |
| LRS | S3の単一リージョン内冗長性とは考え方が違う | AWSではS3標準で複数AZ冗長 |
| ZRS | S3 Standardの複数AZ冗長に近い | 厳密な仕組みは違う |
| GRS | S3 Cross-Region Replication | 別リージョンへ複製 |
| RA-GRS | S3 CRR先バケットの読み取り | AWSでは複製先バケットを直接読む設計 |
| ホットアクセス層 | S3 Standard | 頻繁アクセス向け |
| クールアクセス層 | S3 Standard-IA / One Zone-IA | 低頻度アクセス向け |
| アーカイブアクセス層 | S3 Glacier Flexible Retrieval / Deep Archive | 長期保管向け |
| リハイドレート | Glacier復元 | アーカイブから読み取り可能状態へ戻す |
| ライフサイクル管理ポリシー | S3 Lifecycle | ストレージクラス移行や削除を自動化 |
| Azure Storage Explorer | AWS Management Console / AWS Toolkit / S3用GUIツール | GUIでストレージ操作 |
| AzCopy | AWS CLI `aws s3 cp` / `aws s3 sync` | コマンドでコピー・同期 |
| Azure File Sync | AWS DataSync / AWS Storage Gateway File Gateway | オンプレミスとクラウドのファイル連携 |
| Azure Migrate | AWS Migration Hub / Application Migration Service（MGN） | 移行評価・サーバー移行 |
| Azure Data Box | AWS Snow Family | 物理デバイスによる大量データ移行 |
| Data Box Heavy | AWS Snowball Edge / Snowmobile | 大容量オフライン移行 |

---

## 7. ID・アクセス管理

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Microsoft Entra ID（Azure AD） | IAM Identity Center / IAM / Cognito | 用途で分かれる。完全一致ではない |
| Azure ADユーザー | IAM Identity Centerユーザー / IAMユーザー | 人間ユーザー管理の考え方 |
| Azure ADグループ | IAM Identity Centerグループ / IAMグループ | 権限管理の単位 |
| Microsoftアカウント | AWSアカウントのルートユーザーとは別物 | 個人IDとクラウド管理者IDは混同注意 |
| 職場または学校アカウント | IAM Identity Centerユーザー | 組織ユーザーのSSOに近い |
| シングルサインオン | IAM Identity Center | 複数AWSアカウントやアプリへSSO |
| Azure AD Connect | IAM Identity Centerの外部IdP連携 / AD Connector | 既存ID基盤との連携 |
| AD DS | AWS Directory Service for Microsoft AD | マネージドMicrosoft AD |
| Azure AD DS | AWS Managed Microsoft AD | Kerberos/LDAPが必要なWindows系ワークロード |
| MFA | MFA | 多要素認証 |
| 条件付きアクセス | IAM Identity Center + 外部IdP条件 / Verified Accessなど | AWS単体ではAzure ADほど同じ形ではない |
| Identity Protection | GuardDuty / IAM Access Analyzer / Security Hubなど | IDリスク検出の完全対応ではない |
| Azure RBAC | IAMポリシー / IAMロール / SCP | AWSではIAMが中心 |
| ロール | IAMロール / IAMポリシー | 権限のまとまり |
| 所有者 | AdministratorAccessに近い | 強すぎる権限。SAAでは最小権限を意識 |
| 共同作成者 | PowerUserAccessに近い | IAM管理はできないが多くのリソース操作が可能 |
| 閲覧者 | ReadOnlyAccess | 参照のみ |
| カスタムロール | カスタマー管理ポリシー | 独自の権限定義 |
| セキュリティプリンシパル | IAMユーザー / IAMロール / フェデレーションユーザー | 権限を付与される主体 |
| IAM | IAM | Identity and Access Management |
| 最小権限 | 最小権限の原則 | SAAで頻出の考え方 |

---

## 8. セキュリティ

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Microsoft Defender for Cloud | Security Hub / GuardDuty / Inspector / Config | 複数サービスの組み合わせで考える |
| Defender CSPM | Security Hub / AWS Config / Security Hub CSPM系機能 | セキュリティ態勢管理 |
| Defender for Servers | Amazon Inspector / Systems Manager / GuardDuty | EC2やサーバー保護 |
| Defender for Storage | GuardDuty Malware Protection for S3 / Macie / Security Hub | S3保護やデータ検出 |
| Defender for SQL | RDS監視 / GuardDuty RDS Protection / Security Hub | DB保護は複数サービス |
| セキュアスコア | Security Hubのセキュリティスコア | セキュリティ状態の評価 |
| 推奨事項 | Security Hub Findings / Trusted Advisor | 改善項目の提示 |
| セキュリティアラート | GuardDuty Findings / Security Hub Findings | 脅威検知結果 |
| 規制コンプライアンスダッシュボード | AWS Artifact / Security Hub standards | 準拠状況確認 |
| Just-In-Time VMアクセス | Systems Manager Session Manager | SSH/RDPを開けずに管理接続する設計に近い |
| Key Vault | AWS KMS / Secrets Manager / CloudHSM | 鍵・シークレット・証明書管理 |
| Azure DDoS Protection | AWS Shield | DDoS対策 |
| Web Application Firewall | AWS WAF | Webアプリ保護 |
| Azure Firewall | AWS Network Firewall | ネットワークレベルの保護 |
| Microsoft Sentinel | Amazon Security Lake / Security Hub / GuardDuty / OpenSearch | SIEM/SOAR領域。完全一致ではない |
| ゼロトラスト | Zero Trust | AWSでも最小権限、継続検証、ネットワーク分離で実現 |
| 多層防御 | Defense in Depth | ID、ネットワーク、アプリ、データを多層で守る |

---

## 9. ガバナンス・コンプライアンス

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Azure Policy | AWS Config Rules / Organizations SCP / Control Tower Guardrails | ルールの種類により対応が分かれる |
| ポリシー定義 | Configルール / SCP | 1つのルール |
| イニシアチブ定義 | Conformance Packs / Control Tower controls | 複数ルールの集合 |
| 修復 | AWS Config Remediation | 非準拠リソースの自動修復 |
| リソースロック | 削除保護 / 終了保護 / S3 Object Lock / Backup Vault Lock | AWSではサービスごとに保護機能が違う |
| 削除ロック | Termination Protection / Deletion Protection | 削除防止 |
| 読み取り専用ロック | IAM/SCPで変更拒否 | AWSではIAMやSCPで制御することが多い |
| Azure Blueprints | AWS Control Tower / Service Catalog / CloudFormation StackSets | 標準環境を展開・統制 |
| Service Trust Portal | AWS Artifact | 監査レポートやコンプライアンス文書 |
| トラストセンター | AWS Trust Center | セキュリティ・コンプライアンス情報 |
| 管理グループへのPolicy割り当て | Organizations OUへのSCP適用 | 上位階層で統制する |
| RBACの継承 | IAM/SCP/Organizationsの階層適用 | AWSはSCPとIAMの組み合わせに注意 |

---

## 10. 監視・ログ・運用

| Azure | AWS | 読み替えポイント |
|---|---|---|
| Azure Advisor | AWS Trusted Advisor | ベストプラクティスに基づく推奨 |
| Azure Service Health | AWS Health Dashboard | AWSサービス障害や自分への影響を確認 |
| Azureの状態 | AWS Service Health Dashboard | 公開されるサービス状態 |
| サービス正常性 | AWS Health Dashboard | 自分のアカウントやサービスへの影響 |
| リソース正常性 | AWS Health / CloudWatch / Personal Health Dashboard | 個別リソースの状態は複数サービスで見る |
| Azure Monitor | Amazon CloudWatch | メトリック、ログ、アラームの中心 |
| メトリック | CloudWatch Metrics | 数値データ |
| ログ | CloudWatch Logs | テキストログ |
| アクティビティログ | AWS CloudTrail | 誰がいつ何をしたか |
| リソースログ | CloudWatch Logs / S3ログ / VPC Flow Logs | サービス固有ログ |
| 診断設定 | CloudWatch Logs出力設定 / CloudTrail設定 | ログ送信先の設定 |
| Log Analytics | CloudWatch Logs Insights / Athena / OpenSearch | ログ検索・分析 |
| Kusto Query Language | CloudWatch Logs Insights query language / SQL | クエリ言語は異なる |
| Azure Monitorアラート | CloudWatch Alarms / EventBridge | 条件に応じた通知・イベント処理 |
| アクショングループ | SNS / EventBridge targets | 通知先や自動処理 |
| Application Insights | CloudWatch Application Signals / X-Ray / RUM / Synthetics | アプリ監視・分散トレース |
| VM Insights | CloudWatch Agent / Systems Manager | EC2やOSレベルの監視 |
| Network Watcher | VPC Flow Logs / Reachability Analyzer | ネットワーク監視・疎通確認 |

---

## 11. コスト管理

| Azure | AWS | 読み替えポイント |
|---|---|---|
| 料金計算ツール | AWS Pricing Calculator | 利用前の料金見積もり |
| TCO計算ツール | Migration Evaluator / Pricing Calculator | 移行時のコスト比較 |
| コスト分析 | AWS Cost Explorer | 利用中・利用後のコスト分析 |
| 予算アラート | AWS Budgets | 予算超過や予測超過の通知 |
| Cost Management | AWS Billing and Cost Management | 請求・コスト管理 |
| タグによるコスト分類 | Cost Allocation Tags | タグ別コスト配分 |
| 予約 | Reserved Instances / Savings Plans | 長期利用割引 |
| Azureハイブリッド特典 | BYOL / License Manager / Dedicated Host | 既存ライセンス活用 |
| Spot VM | EC2 Spot Instances | 余剰キャパシティを安く使う |
| Advisorのコスト推奨 | Trusted Advisor / Cost Optimization Hub | コスト最適化の推奨 |

---

## 12. SAAで特に重要な読み替え

### Azureの「可用性ゾーン」からAWSの「AZ」へ

Azureで可用性ゾーンを学んだら、AWSではさらに強く意識する。

SAAでは、次のような判断が頻出。

- EC2を複数AZに配置する
- RDS Multi-AZを使う
- ALBで複数AZに負荷分散する
- Auto Scaling groupで複数AZに展開する
- S3は標準で複数AZに保存される

---

### Azureの「リージョンペア」からAWSの「自分でマルチリージョン設計」へ

Azureにはリージョンペアの考え方がある。

AWSでは、決まったリージョンペアを選ぶというより、要件に応じて自分でマルチリージョン構成を設計する。

SAAで出やすい例：

- Route 53フェイルオーバールーティング
- S3 Cross-Region Replication
- DynamoDB Global Tables
- Aurora Global Database
- AWS Backupのクロスリージョンコピー
- CloudFrontによるグローバル配信

---

### Azureの「NSG」からAWSの「セキュリティグループとNACL」へ

AzureではNSGで通信制御を覚えた。

AWSでは、主に2つを使い分ける。

| AWS | 特徴 |
|---|---|
| セキュリティグループ | インスタンス単位。ステートフル。許可ルール中心 |
| ネットワークACL | サブネット単位。ステートレス。許可と拒否を設定可能 |

SAAでは、基本はセキュリティグループ、サブネット境界の明示的制御にはNACLと考える。

---

### Azureの「App Service」からAWSの「複数候補」へ

Azure App ServiceはWebアプリ用PaaSとして覚えやすい。

AWSでは要件により候補が分かれる。

| 要件 | AWS候補 |
|---|---|
| 簡単にWebアプリをデプロイしたい | Elastic Beanstalk |
| コンテナーWebアプリを楽に動かしたい | App Runner |
| サーバーレスAPIにしたい | Lambda + API Gateway |
| コンテナーを本格運用したい | ECS / EKS |
| 静的Webサイトを配信したい | S3 + CloudFront |

---

### Azureの「Storage Account」からAWSの「用途別サービス」へ

Azure Storage Accountは、BLOB、Files、Queue、Tableをまとめて扱う。

AWSではサービスが分かれる。

| Azure Storage | AWS |
|---|---|
| BLOB | S3 |
| Files | EFS / FSx |
| Queue | SQS |
| Table | DynamoDB |

SAAでは、ストレージの種類を次のように選ぶ。

- オブジェクト保存：S3
- EC2用ブロックストレージ：EBS
- 共有ファイル：EFS / FSx
- アーカイブ：S3 Glacier系
- NoSQL：DynamoDB

---

### Azureの「RBAC」からAWSの「IAM」へ

Azureでは、スコープにロールを割り当てる。

AWSでは、IAMポリシーをユーザー、グループ、ロールに付与する。

SAAで重要な考え方：

- ルートユーザーは日常利用しない
- IAMユーザーよりIAMロールを優先する場面が多い
- EC2からS3へアクセスするならアクセスキーではなくIAMロール
- 権限は最小権限にする
- 複数アカウント管理ではOrganizationsやSCPを使う

---

### Azureの「Azure Policy」からAWSの「SCPとConfig」へ

Azure Policyは、ルール適用と準拠評価をまとめて扱える。

AWSでは用途で分かれる。

| やりたいこと | AWS |
|---|---|
| アカウントやOU単位で操作を禁止したい | SCP |
| リソース設定がルールに準拠しているか評価したい | AWS Config |
| 非準拠を自動修復したい | AWS Config Remediation |
| 標準的な統制済み環境を作りたい | AWS Control Tower |

---

## 13. Azureで覚えた言葉をAWS風に言い換える

| Azureでの言い方 | AWSでの言い方 |
|---|---|
| サブスクリプションを分ける | AWSアカウントを分ける |
| 管理グループでまとめる | OrganizationsのOUでまとめる |
| リソースグループでまとめる | タグやResource Groupsでまとめる |
| 可用性ゾーンに分散する | 複数AZに分散する |
| リージョンペアに複製する | マルチリージョンDRを設計する |
| NSGで通信制御する | セキュリティグループとNACLで制御する |
| VNetを作る | VPCを作る |
| VNetピアリングする | VPC Peeringする |
| ExpressRouteでつなぐ | Direct Connectでつなぐ |
| DNSゾーンを作る | Route 53 Hosted Zoneを作る |
| BLOBに保存する | S3に保存する |
| App ServiceでWebアプリを動かす | Elastic Beanstalk / App Runner / Lambdaなどを検討する |
| Functionsでイベント処理する | Lambdaでイベント処理する |
| VMSSで台数を増減する | Auto Scaling groupで台数を増減する |
| Azure Monitorで監視する | CloudWatchで監視する |
| アクティビティログを見る | CloudTrailを見る |
| Advisorの推奨を見る | Trusted Advisorを見る |
| Policyで統制する | SCP / Config / Control Towerで統制する |

---

## 14. SAAで迷いやすいAWSサービス選択

| 要件 | 選ぶAWSサービス |
|---|---|
| 仮想サーバーを作りたい | EC2 |
| EC2のディスクを追加したい | EBS |
| 一時的な高速ローカルディスクが欲しい | インスタンスストア |
| 静的ファイルや画像を保存したい | S3 |
| 複数EC2から共有ファイルにアクセスしたい | EFS |
| Windowsファイル共有が必要 | FSx for Windows File Server |
| マネージドRDBを使いたい | RDS / Aurora |
| NoSQLで高速・大規模に使いたい | DynamoDB |
| メッセージキューで疎結合にしたい | SQS |
| Pub/Sub通知をしたい | SNS |
| イベント駆動でつなぎたい | EventBridge |
| サーバーレスでコードを実行したい | Lambda |
| REST APIやHTTP APIを公開したい | API Gateway |
| コンテナーを簡単に動かしたい | ECS Fargate / App Runner |
| Kubernetesを使いたい | EKS |
| コンテナーイメージを保存したい | ECR |
| 負荷分散したい | ELB |
| HTTP/HTTPSでL7負荷分散したい | ALB |
| 超低遅延・L4負荷分散したい | NLB |
| CDNで配信したい | CloudFront |
| DNS管理したい | Route 53 |
| オンプレミスとVPN接続したい | Site-to-Site VPN |
| オンプレミスと専用線接続したい | Direct Connect |
| プライベートにAWSサービスへ接続したい | VPC Endpoint |
| 権限管理したい | IAM |
| 複数AWSアカウントを管理したい | Organizations |
| SSOしたい | IAM Identity Center |
| 操作履歴を確認したい | CloudTrail |
| メトリックやログを監視したい | CloudWatch |
| 脅威検出したい | GuardDuty |
| 脆弱性検出したい | Inspector |
| セキュリティ結果を集約したい | Security Hub |
| コンプライアンス文書を見たい | AWS Artifact |
| コストを分析したい | Cost Explorer |
| 予算通知したい | AWS Budgets |
| 料金を見積もりたい | AWS Pricing Calculator |

---

## 15. まず優先して覚えるAWS中核サービス

SAAの最初は、以下を優先して押さえる。

### 最優先

- IAM
- VPC
- EC2
- EBS
- S3
- ELB
- Auto Scaling
- RDS
- Route 53
- CloudWatch
- CloudTrail

### 次に重要

- Lambda
- API Gateway
- SQS
- SNS
- EventBridge
- DynamoDB
- CloudFront
- EFS
- ECR
- ECS
- EKS
- KMS
- Secrets Manager
- Systems Manager

### 設計問題で差がつきやすい

- AWS Organizations
- SCP
- AWS Config
- Trusted Advisor
- Security Hub
- GuardDuty
- Inspector
- WAF
- Shield
- Direct Connect
- Transit Gateway
- VPC Endpoint
- AWS Backup
- Storage Gateway
- DataSync
- Snow Family

---

## 16. Azure学習者向けのSAA注意点

### 1. AWSは「アカウント分離」がかなり重要

Azureではサブスクリプション分離で考えたものを、AWSではAWSアカウント分離で考えることが多い。

例：

- 本番アカウント
- 開発アカウント
- セキュリティアカウント
- ログ管理アカウント
- 共有ネットワークアカウント

OrganizationsとSCPはSAAでも重要。

---

### 2. AWSは「複数AZ設計」が超重要

SAAでは、単一AZより複数AZが基本。

例：

- ALBは複数AZ
- Auto Scaling groupも複数AZ
- RDSはMulti-AZ
- NAT GatewayもAZごとに置く設計がある
- EFSは複数AZから利用できる

---

### 3. AWSは「セキュリティグループ」と「NACL」を分けて覚える

AzureのNSGだけの感覚でいくと混乱しやすい。

| 項目 | セキュリティグループ | NACL |
|---|---|---|
| 適用先 | ENI / EC2 | サブネット |
| 状態 | ステートフル | ステートレス |
| ルール | 許可のみ | 許可と拒否 |
| 戻り通信 | 自動許可 | 明示的に必要 |
| よく使う場面 | EC2単位の制御 | サブネット境界の制御 |

---

### 4. AWSは「IAMロール」をかなり使う

Azure RBACのロールとは名前が似ていても、AWSのIAMロールは「一時的に引き受ける権限」として使う。

重要パターン：

- EC2にIAMロールを付けてS3へアクセス
- Lambdaに実行ロールを付ける
- クロスアカウントアクセスでロールを引き受ける
- アクセスキーを埋め込まない

---

### 5. AWSは「マネージドサービス選択」が試験の中心

SAAでは、要件に対して次のように選ぶ。

- 運用負荷を減らしたい → マネージドサービス
- 高可用性にしたい → 複数AZ / マルチリージョン
- 疎結合にしたい → SQS / SNS / EventBridge
- 静的コンテンツ配信 → S3 + CloudFront
- グローバルDB → DynamoDB Global Tables / Aurora Global Database
- サーバーレス → Lambda / API Gateway / DynamoDB / S3
- コスト最適化 → Savings Plans / Reserved Instances / Spot / S3 storage classes

---

## 17. 最後に覚える超重要対応

| Azure | AWS |
|---|---|
| Virtual Machines | EC2 |
| Managed Disk | EBS |
| Virtual Network | VPC |
| Subnet | Subnet |
| NSG | Security Group / NACL |
| Load Balancer | ELB |
| Application Gateway | ALB |
| ExpressRoute | Direct Connect |
| VPN Gateway | Site-to-Site VPN / VGW |
| DNS Zone | Route 53 Public Hosted Zone |
| Private DNS Zone | Route 53 Private Hosted Zone |
| Blob Storage | S3 |
| Azure Files | EFS / FSx |
| Queue Storage | SQS |
| Table Storage | DynamoDB |
| Azure SQL Database | RDS / Aurora |
| Cosmos DB | DynamoDB / DocumentDB |
| Functions | Lambda |
| App Service | Elastic Beanstalk / App Runner |
| AKS | EKS |
| ACR | ECR |
| Azure AD | IAM Identity Center / IAM |
| Azure RBAC | IAM |
| Key Vault | KMS / Secrets Manager |
| Defender for Cloud | Security Hub / GuardDuty / Inspector |
| Azure Policy | SCP / Config |
| Resource Lock | Deletion Protection / IAM・SCP制御 |
| Azure Advisor | Trusted Advisor |
| Azure Monitor | CloudWatch |
| Activity Log | CloudTrail |
| Log Analytics | CloudWatch Logs Insights |
| Service Health | AWS Health Dashboard |
| Pricing Calculator | AWS Pricing Calculator |
| Cost Analysis | Cost Explorer |
| Budget Alert | AWS Budgets |
| Azure Migrate | AWS Migration Hub / MGN |
| Data Box | Snow Family |
