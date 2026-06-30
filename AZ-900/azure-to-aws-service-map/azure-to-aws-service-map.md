# Azure → AWS サービス読み替え表

AZ-900で学んだAzureの知識を、AWS SAA学習へつなげるための読み替え表。
完全に1対1対応するものばかりではないため、特に「注意ポイント」を意識する。

---

## 1. 基本概念・アカウント管理

| Azure                   | AWS                                         | ざっくり読み替え           | 注意ポイント                                                 |
| ----------------------- | ------------------------------------------- | ------------------ | ------------------------------------------------------ |
| Azureアカウント              | AWSアカウント                                    | クラウド利用の単位          | Azureは「テナント」「サブスクリプション」の考え方が強い。AWSはアカウント単位で分離することが多い。  |
| Microsoft Entra ID テナント | AWS Organizations / IAM Identity Center     | 組織・ID管理の土台         | 完全一致ではない。Entra IDは企業ID基盤、AWS OrganizationsはAWSアカウント管理。 |
| サブスクリプション               | AWSアカウント                                    | 課金・リソース管理の単位       | AzureのサブスクリプションはAWSアカウントに近い。                           |
| リソースグループ                | タグ / Resource Groups / CloudFormation Stack | リソースをまとめる単位        | AWSではAzureのリソースグループほど強制的な入れ物ではない。タグ管理が重要。              |
| リージョン                   | リージョン                                       | 地理的なデータセンター群       | 基本的な考え方は近い。                                            |
| 可用性ゾーン                  | Availability Zone                           | リージョン内の物理的に分離された場所 | AWSでも高可用性設計の基本。                                        |
| リージョンペア                 | 明確な同等サービスなし                                 | 災害対策用のリージョン組み合わせ   | AWSでは自分でマルチリージョン設計を考える。                                |

---

## 2. コンピューティング

| Azure                      | AWS                                                    | ざっくり読み替え        | SAAでの重要度 | 注意ポイント                                                   |
| -------------------------- | ------------------------------------------------------ | --------------- | -------- | -------------------------------------------------------- |
| Azure Virtual Machines     | Amazon EC2                                             | 仮想マシン           | 高        | SAAではEC2の購入オプション、Auto Scaling、EBSとの組み合わせが重要。             |
| Virtual Machine Scale Sets | Auto Scaling Group                                     | VMの自動増減         | 高        | 負荷に応じたスケーリングの基本。                                         |
| Azure App Service          | Elastic Beanstalk / App Runner                         | Webアプリ実行環境      | 中        | App ServiceはPaaS寄り。AWSではElastic BeanstalkやApp Runnerが近い。 |
| Azure Functions            | AWS Lambda                                             | サーバーレス関数        | 高        | SAAではイベント駆動、実行時間、他サービス連携が重要。                             |
| Azure Container Instances  | AWS Fargate                                            | サーバーレスコンテナ      | 中        | 単体コンテナ実行ならFargateのイメージ。                                  |
| Azure Kubernetes Service   | Amazon EKS                                             | マネージドKubernetes | 中        | SAAではECS/Fargateの方が出やすいこともある。                            |
| Azure Container Registry   | Amazon ECR                                             | コンテナイメージ保管      | 中        | ECS/EKS/Lambdaコンテナと組み合わせる。                               |
| Azure Batch                | AWS Batch                                              | バッチ処理           | 低〜中      | 大量ジョブ処理向け。                                               |
| Azure Bastion              | Systems Manager Session Manager / EC2 Instance Connect | 踏み台なし接続         | 中        | AWSでは踏み台サーバーを置かずSSMで接続する構成が重要。                           |

---

## 3. ストレージ

| Azure               | AWS                                      | ざっくり読み替え         | SAAでの重要度 | 注意ポイント                                                               |
| ------------------- | ---------------------------------------- | ---------------- | -------- | -------------------------------------------------------------------- |
| Azure Blob Storage  | Amazon S3                                | オブジェクトストレージ      | 最重要      | SAA頻出。静的Web、バックアップ、ライフサイクル、暗号化、バージョニング。                              |
| Azure Blob Archive  | S3 Glacier系                              | アーカイブストレージ       | 高        | Glacier Instant Retrieval / Flexible Retrieval / Deep Archiveの違いに注意。 |
| Azure Files         | Amazon EFS / FSx for Windows File Server | ファイル共有           | 高        | Linux系共有ならEFS、Windows SMBならFSx for Windows。                          |
| Azure Managed Disks | Amazon EBS                               | VM/EC2用ブロックストレージ | 高        | EC2にアタッチするディスク。スナップショットも重要。                                          |
| Azure Disk Storage  | Amazon EBS                               | ブロックストレージ        | 高        | EBSのgp3/io2/sc1/st1などの違いがSAAで出やすい。                                   |
| Azure Queue Storage | Amazon SQS                               | キュー              | 高        | 疎結合化の基本。標準キューとFIFOキューを区別。                                            |
| Azure Table Storage | Amazon DynamoDB                          | NoSQL Key-Value  | 中        | 完全一致ではないが、シンプルなNoSQLとして近い。                                           |
| Azure NetApp Files  | Amazon FSx for NetApp ONTAP              | NetApp系ファイルサービス  | 低        | エンタープライズ向け。                                                          |
| Azure Backup        | AWS Backup                               | バックアップ管理         | 中        | 複数サービスのバックアップを集中管理。                                                  |

---

## 4. データベース

| Azure                         | AWS                                           | ざっくり読み替え             | SAAでの重要度 | 注意ポイント                                         |
| ----------------------------- | --------------------------------------------- | -------------------- | -------- | ---------------------------------------------- |
| Azure SQL Database            | Amazon RDS / Amazon Aurora                    | マネージドRDB             | 高        | SQL ServerならRDS for SQL Server。AWSではAuroraも重要。 |
| Azure SQL Managed Instance    | Amazon RDS for SQL Server                     | SQL Server互換のマネージドDB | 中        | SQL Server移行文脈で考える。                            |
| Azure Database for PostgreSQL | Amazon RDS for PostgreSQL / Aurora PostgreSQL | PostgreSQL           | 高        | Auroraは高可用性・高性能のAWS独自DB。                       |
| Azure Database for MySQL      | Amazon RDS for MySQL / Aurora MySQL           | MySQL                | 高        | RDSとAuroraの違いに注意。                              |
| Cosmos DB                     | DynamoDB / DocumentDB                         | NoSQL                | 高        | Key-ValueならDynamoDB、MongoDB互換ならDocumentDB。     |
| Azure Cache for Redis         | ElastiCache for Redis / Valkey                | インメモリキャッシュ           | 高        | SAAではRDS負荷軽減、セッション管理で出やすい。                     |
| Azure Synapse Analytics       | Amazon Redshift                               | データウェアハウス            | 中        | 分析基盤。OLTPではなくOLAP。                             |
| Azure Data Factory            | AWS Glue / Step Functions                     | データ連携・ETL            | 中        | ETLならGlue、ワークフロー制御ならStep Functions。            |

---

## 5. ネットワーク

| Azure                           | AWS                                        | ざっくり読み替え           | SAAでの重要度 | 注意ポイント                                             |
| ------------------------------- | ------------------------------------------ | ------------------ | -------- | -------------------------------------------------- |
| Virtual Network                 | VPC                                        | 仮想ネットワーク           | 最重要      | AWSネットワークの土台。CIDR、サブネット、ルートテーブルが重要。                |
| サブネット                           | Subnet                                     | VPC内の分割            | 最重要      | AWSではサブネットは1つのAZに属する。                              |
| Network Security Group          | Security Group / Network ACL               | 通信制御               | 最重要      | Azure NSGはサブネット/NICに適用。AWS SGはインスタンス等、NACLはサブネット。  |
| Azure Firewall                  | AWS Network Firewall                       | マネージドファイアウォール      | 中        | Security Group/NACLより高度なネットワーク制御。                  |
| Azure VPN Gateway               | AWS Site-to-Site VPN                       | VPN接続              | 高        | インターネット経由で暗号化接続。                                   |
| ExpressRoute                    | AWS Direct Connect                         | 専用線接続              | 高        | インターネットを経由しない高品質接続。                                |
| Azure Load Balancer             | Network Load Balancer                      | L4負荷分散             | 高        | TCP/UDPなど。超高性能・低遅延。                                |
| Application Gateway             | Application Load Balancer                  | L7負荷分散             | 高        | HTTP/HTTPS、パスベースルーティング。                            |
| Front Door                      | CloudFront / Global Accelerator / Route 53 | グローバル配信・入口         | 中        | 完全一致しない。CDNならCloudFront、低遅延入口ならGlobal Accelerator。 |
| Traffic Manager                 | Route 53                                   | DNSベースルーティング       | 中        | レイテンシー、フェイルオーバー、加重ルーティング。                          |
| Azure DNS                       | Route 53                                   | DNS管理              | 高        | ドメイン管理・名前解決。                                       |
| Private Endpoint / Private Link | AWS PrivateLink / VPC Endpoint             | プライベート接続           | 高        | インターネットを通さずサービスへ接続。                                |
| NAT Gateway                     | NAT Gateway                                | プライベートサブネットから外向き通信 | 高        | プライベートサブネットのEC2がインターネットへ出る時に使う。                    |
| DDoS Protection                 | AWS Shield                                 | DDoS対策             | 中        | AWS Shield Standardは基本保護。Advancedは有料。              |
| Network Watcher                 | VPC Flow Logs / Reachability Analyzer      | ネットワーク監視・診断        | 中        | AWSでは複数サービスに分かれる。                                  |

---

## 6. ID・アクセス管理

| Azure                          | AWS                                                          | ざっくり読み替え           | SAAでの重要度               | 注意ポイント                                        |
| ------------------------------ | ------------------------------------------------------------ | ------------------ | ---------------------- | --------------------------------------------- |
| Microsoft Entra ID             | IAM / IAM Identity Center                                    | ID・アクセス管理          | 最重要                    | 完全一致しない。Entra IDは企業ID管理、IAMはAWSリソース権限管理。      |
| Azure RBAC                     | IAM Policy / IAM Role                                        | 権限管理               | 最重要                    | AWSではIAMポリシーをロールやユーザーに付与。                     |
| Managed Identity               | IAM Role                                                     | サービスに資格情報を持たせず権限付与 | 高                      | EC2ならInstance Profile、LambdaならExecution Role。 |
| MFA                            | MFA                                                          | 多要素認証              | 高                      | ルートユーザー・IAMユーザーで重要。                           |
| 条件付きアクセス                       | IAM Policy Condition / IAM Identity Center / Verified Access | 条件に応じたアクセス制御       | 中                      | Azureの条件付きアクセスと完全一致するAWS単体サービスはない。            |
| Privileged Identity Management | IAM Identity Center / 一時的な権限付与設計                             | 低〜中                | AWSでは一時認証情報やロール切替で考える。 |                                               |
| Entra ID アプリ登録                 | IAM Role / Cognito / IAM Identity Provider                   | アプリ連携・認証           | 中                      | 用途によって対応先が変わる。                                |

---

## 7. セキュリティ

| Azure                        | AWS                                                         | ざっくり読み替え   | SAAでの重要度 | 注意ポイント                                 |
| ---------------------------- | ----------------------------------------------------------- | ---------- | -------- | -------------------------------------- |
| Azure Key Vault              | AWS KMS / Secrets Manager / Systems Manager Parameter Store | 鍵・シークレット管理 | 高        | 暗号鍵ならKMS、パスワード等ならSecrets Manager。      |
| Microsoft Defender for Cloud | Security Hub / GuardDuty / Inspector                        | セキュリティ統合管理 | 中        | AWSでは役割が複数サービスに分かれる。                   |
| Azure Firewall               | AWS Network Firewall                                        | ネットワーク防御   | 中        | ネットワーク境界での制御。                          |
| Web Application Firewall     | AWS WAF                                                     | Web攻撃対策    | 高        | ALB、CloudFront、API Gatewayなどと組み合わせる。   |
| Azure DDoS Protection        | AWS Shield                                                  | DDoS対策     | 中        | Shield Standardは自動適用。                  |
| Microsoft Sentinel           | Security Lake / OpenSearch / Security Hub連携                 | SIEM・ログ分析  | 低〜中      | SAAでは深追いしすぎなくてよい。                      |
| Azure Policyによるセキュリティ制御      | AWS Config / SCP                                            | 準拠・制御      | 中        | Configは状態評価、SCPはAWS Organizationsでの制限。 |

---

## 8. 監視・運用管理

| Azure                | AWS                                    | ざっくり読み替え  | SAAでの重要度 | 注意ポイント                  |
| -------------------- | -------------------------------------- | --------- | -------- | ----------------------- |
| Azure Monitor        | Amazon CloudWatch                      | 監視        | 最重要      | メトリクス、ログ、アラーム。          |
| Log Analytics        | CloudWatch Logs                        | ログ分析      | 高        | ログ収集・検索。                |
| Application Insights | CloudWatch Application Signals / X-Ray | アプリ監視     | 中        | 分散トレーシングならX-Ray。        |
| Azure Service Health | AWS Health Dashboard                   | クラウド側障害情報 | 中        | AWS側の障害・メンテナンス確認。       |
| Azure Advisor        | AWS Trusted Advisor                    | 推奨事項      | 中        | コスト、セキュリティ、耐障害性などの改善提案。 |
| Azure Automation     | Systems Manager Automation             | 運用自動化     | 中        | パッチ、コマンド実行、運用タスク自動化。    |
| Update Management    | Systems Manager Patch Manager          | パッチ管理     | 中        | EC2等のパッチ適用管理。           |

---

## 9. ガバナンス・IaC・コスト管理

| Azure                  | AWS                                                  | ざっくり読み替え            | SAAでの重要度 | 注意ポイント                                  |
| ---------------------- | ---------------------------------------------------- | ------------------- | -------- | --------------------------------------- |
| Azure Policy           | AWS Config / Organizations SCP                       | ルール適用・準拠管理          | 高        | Configは設定評価、SCPはアカウント単位の制限。             |
| Management Groups      | AWS Organizations OU                                 | 複数アカウント/サブスクリプション管理 | 中        | 組織階層管理。                                 |
| Azure Resource Manager | CloudFormation                                       | IaC・リソース展開          | 高        | AWSではCloudFormation、CDK、Terraformもよく使う。 |
| Bicep                  | CloudFormation / CDK                                 | IaC記述               | 中        | AWS CDKはプログラミング言語でIaCを書く。               |
| Azure Cost Management  | AWS Cost Explorer                                    | コスト分析               | 高        | SAAでもコスト最適化は重要。                         |
| Azure Budgets          | AWS Budgets                                          | 予算管理                | 中        | 予算超過アラート。                               |
| Azure Tags             | AWS Tags                                             | リソース分類              | 高        | コスト配賦・管理で重要。                            |
| Azure Blueprints       | AWS Control Tower / Service Catalog / CloudFormation | 環境標準化               | 低〜中      | Azure Blueprintsは現在は主流ではないため、概念として理解。   |

---

## 10. アプリ連携・メッセージング

| Azure                   | AWS                  | ざっくり読み替え   | SAAでの重要度 | 注意ポイント                    |
| ----------------------- | -------------------- | ---------- | -------- | ------------------------- |
| Azure Service Bus Queue | Amazon SQS           | メッセージキュー   | 高        | 疎結合化。標準キュー/FIFOキューに注意。    |
| Azure Service Bus Topic | Amazon SNS + SQS     | Pub/Sub    | 高        | SNSで配信、SQSで受ける構成が頻出。      |
| Event Grid              | Amazon EventBridge   | イベントルーティング | 高        | イベント駆動アーキテクチャ。            |
| Event Hubs              | Kinesis Data Streams | ストリーミングデータ | 中        | 大量イベント・ログ収集。              |
| Logic Apps              | Step Functions       | ワークフロー     | 中        | サーバーレスの処理フロー制御。           |
| API Management          | Amazon API Gateway   | API公開・管理   | 高        | Lambdaやバックエンドサービスと組み合わせる。 |
| Azure App Configuration | AWS AppConfig        | アプリ設定管理    | 低〜中      | 設定値の管理・段階的反映。             |

---

## 11. DevOps・開発支援

| Azure                | AWS                                                    | ざっくり読み替え    | SAAでの重要度 | 注意ポイント                              |
| -------------------- | ------------------------------------------------------ | ----------- | -------- | ----------------------------------- |
| Azure DevOps         | CodePipeline / CodeBuild / CodeDeploy / GitHub Actions | CI/CD       | 中        | AWSでは複数サービスに分かれる。                   |
| Azure Repos          | GitHub / CodeCommit                                    | Gitリポジトリ    | 低        | CodeCommitは新規利用では扱いに注意。GitHub利用も多い。 |
| Azure Pipelines      | CodePipeline / CodeBuild                               | CI/CDパイプライン | 中        | ビルドはCodeBuild、デプロイはCodeDeploy。      |
| Azure Artifacts      | CodeArtifact                                           | パッケージ管理     | 低        | ライブラリ・パッケージ管理。                      |
| ARM Template / Bicep | CloudFormation / CDK                                   | IaC         | 高        | SAAではCloudFormationの役割を理解。          |

---

## 12. SAAで特に優先して覚える対応表

まずはここを優先して覚える。

| Azure               | AWS                            | 覚え方          |
| ------------------- | ------------------------------ | ------------ |
| Virtual Machine     | EC2                            | 仮想マシン        |
| Managed Disk        | EBS                            | EC2に付けるディスク  |
| Blob Storage        | S3                             | オブジェクトストレージ  |
| Azure Files         | EFS / FSx                      | ファイル共有       |
| Virtual Network     | VPC                            | 仮想ネットワーク     |
| NSG                 | Security Group / Network ACL   | 通信制御         |
| VPN Gateway         | Site-to-Site VPN               | インターネット経由VPN |
| ExpressRoute        | Direct Connect                 | 専用線          |
| Azure Load Balancer | Network Load Balancer          | L4負荷分散       |
| Application Gateway | Application Load Balancer      | L7負荷分散       |
| Azure Functions     | Lambda                         | サーバーレス関数     |
| App Service         | Elastic Beanstalk / App Runner | Webアプリ実行基盤   |
| AKS                 | EKS                            | Kubernetes   |
| Azure SQL Database  | RDS / Aurora                   | マネージドRDB     |
| Cosmos DB           | DynamoDB / DocumentDB          | NoSQL        |
| Azure Monitor       | CloudWatch                     | 監視           |
| Key Vault           | KMS / Secrets Manager          | 鍵・シークレット     |
| Azure Policy        | AWS Config / SCP               | 準拠・統制        |
| Entra ID            | IAM / IAM Identity Center      | ID・権限管理      |
| Service Bus         | SQS / SNS                      | メッセージング      |
| Event Grid          | EventBridge                    | イベント連携       |

---

## 13. 混同しやすいAWSサービス

SAAでは、Azureとの読み替えよりもAWSサービス同士の違いが重要になる。

| 混同しやすい組み合わせ                             | 違い                                                 |
| --------------------------------------- | -------------------------------------------------- |
| S3 / EBS / EFS                          | S3はオブジェクト、EBSはブロック、EFSはファイル共有                      |
| ALB / NLB                               | ALBはHTTP/HTTPSのL7、NLBはTCP/UDPのL4                   |
| Security Group / Network ACL            | SGはステートフル、NACLはステートレス                              |
| NAT Gateway / Internet Gateway          | NAT Gatewayはプライベートサブネットから外向き、IGWはVPCとインターネット接続     |
| SQS / SNS / EventBridge                 | SQSはキュー、SNSは通知配信、EventBridgeはイベントルーティング            |
| RDS / Aurora / DynamoDB                 | RDSは一般的なRDB、AuroraはAWS独自高性能RDB、DynamoDBはNoSQL      |
| CloudWatch / CloudTrail / Config        | CloudWatchは監視、CloudTrailは操作履歴、Configは設定評価          |
| KMS / Secrets Manager / Parameter Store | KMSは鍵、Secrets Managerはシークレット、Parameter Storeは設定値管理 |
| IAM Role / IAM User / IAM Policy        | Roleは一時的に引き受ける権限、Userは人や長期認証、Policyは権限定義           |

---

## 14. 学習方針メモ

AZ-900で学んだ内容をSAAへつなげる場合、次の順番で進める。

1. Azureで知っている概念をAWSサービス名へ読み替える
2. AWSサービス同士の違いを比較表で整理する
3. 問題演習で「どの要件ならどのサービスか」を判断できるようにする

SAAでは単純暗記だけでなく、以下の観点が重要。

* 可用性
* スケーラビリティ
* コスト最適化
* セキュリティ
* 運用負荷の低減
* 疎結合化
* マネージドサービスの活用

---

## 15. まず覚える一言まとめ

| AWSサービス          | 一言                   |
| ---------------- | -------------------- |
| EC2              | 仮想マシン                |
| S3               | オブジェクトストレージ          |
| EBS              | EC2用ディスク             |
| EFS              | 複数EC2で共有できるファイルストレージ |
| VPC              | AWS内の仮想ネットワーク        |
| Security Group   | インスタンス単位のファイアウォール    |
| Network ACL      | サブネット単位のファイアウォール     |
| IAM              | AWSの権限管理             |
| Lambda           | サーバーレス関数             |
| RDS              | マネージドRDB             |
| Aurora           | AWS独自の高性能RDB         |
| DynamoDB         | フルマネージドNoSQL         |
| SQS              | キュー                  |
| SNS              | 通知・Pub/Sub           |
| EventBridge      | イベント連携               |
| CloudWatch       | 監視                   |
| CloudTrail       | 操作ログ                 |
| Config           | 設定評価                 |
| KMS              | 暗号鍵管理                |
| Secrets Manager  | パスワード・シークレット管理       |
| Direct Connect   | 専用線                  |
| Site-to-Site VPN | VPN接続                |
| Route 53         | DNS                  |
| CloudFront       | CDN                  |
| API Gateway      | API公開                |
| Step Functions   | ワークフロー制御             |

---

## 16. neo用メモ

AZ-900で覚えた内容はSAAでもかなり使える。

特に以下はAzureで学んだ考え方をそのままAWSへ持ち込める。

* 可用性ゾーン
* リージョン
* 負荷分散
* スケーリング
* VPNと専用線
* IDと権限管理
* オブジェクトストレージ
* マネージドDB
* 監視
* コスト管理
* ガバナンス

ただし、SAAでは「このサービスは何か」だけでなく、
「この要件ならどのサービスを選ぶか」が問われる。

そのため、最終的には

* サービス概要の暗記
* 比較表で違いを整理
* 問題演習で判断力をつける

の3段階で進める。
