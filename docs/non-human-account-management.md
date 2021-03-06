# Non-human アカウント管理

Graham Williamson (Internet Commerce Austraila)著, André Koot (Nixu Corporation)著

## 概要

個人アカウント以外のアカウントは、堅牢なIAM環境の「アキレス腱」であることが多いです。IAM の専門家は、ユーザーアカウントのアイデンティティ、認証、RBAC、ABAC、ガバナンス、および監査の管理に関心を持っていますが、他の IT スタッフは、ハードワイヤードアカウント、公開サービス、および API を介して保護されたリソースへのアクセスが与えられたデバイスやサービスを展開しています。

非人間的なアカウント制御の管理は、ユーザーベースのアカウント管理と一貫していなければならず、高セキュリティアプリケーションへのユーザーアカウントアクセスに配置された制御は、非人間的なアカウントにも適用されなければなりません。

非人間アカウントを扱うための単一のソリューションはありません。IAMの専門家の中には、一貫したポリシーの展開を保証するために、すべてのアカウントを同じプロセスと同じインフラストラクチャで管理すべきだと提案する人もいます。この共通の管理は、IAMの展開が発生したときに、非人間アカウントが「取り残される」ことがないようにすべきだと彼らは主張しています。他の人は、これは非現実的であると考え、非人間アカウントのために目的別のプロセスを展開することを推奨しています。しかし、非人間アカウントを管理するために使用されるメカニズムに関係なく、それらが管理されていることを確認することが最重要です。そうでなければ、非人間アカウントは、企業施設へのアクセスを得るためにハッカーに好まれるサイバーセキュリティ攻撃のベクトルであり続けることになります。

Keywords: System accounts, IoT, Bots, Non-Human, server accounts, service accounts

## 序章

人ではない ID は、人間のユーザーではなく、サービスまたはデバイスに関連付けられている。この文脈での ID は、デバイスの識別子によって定義される。センサーからのデータを記録したり、アクチュエーターに信号を送信したりする企業システムと相互作用するためには、 デバイスを識別できなければならない。例えば、ビル管理システムは、環境データを企業の監視システムデータベースに書き込むために、定期的にコンピュータシステムにログインするかもしれない。

本明細書では、「非個人アカウント」とは、非営業時間中に実行されて生産データのオフラインコピーを作成するバックアップルーチンのような、個人に関連付けられていないコンピュータシステムアカウントを含む。このようなアカウントは、作成された特定の目的のために制限されるべきであり、対話的に使用される場合は一時停止されるべきです。

IAMの専門家は一般的にユーザーアカウントに焦点を当てていますが、これらの非個人アカウントは組織にとって潜在的な攻撃のベクトルとなり、組織がコンピュータシステムへのアクセスに関するポリシーを策定する際に含めるべきです。

| | 人間のIdentity | Non-personアイデンティティ |
| - | - | - |
| 利用範囲 | 多面的で、多くのアプリケーションや保護されたリソースへの複数のアクセス要件に対応する必要があります。| 目的に特化した、single requirement for each deployment |
| ライフサイクル | ユーザー参加型で作成され、要件の変更に応じて変更され、コンプライアンスのために継続的に監視され、サービスの停止時には無効化され、スタッフの退出時には削除されます　| デバイス/サービスのデプロイ時に作成され、終了時に削除されます。 |
| アクセス制御 | 動的 - 要求されたアプリケーションまたは保護されたリソースの保証レベル要件に合わせて、継続的にリスクを評価した認証。MFA は、認証の昇格に使用されます　| 静的 - アカウント作成時に決定されます。MFAの要件はありません。 |
| アクセスエンドポイント　| ユーザーは通常、スマートフォン、PC、ノートパソコンからインタラクティブにコンピュータサービスにアクセスします | エンドポイントは通常、デバイスまたはデバイスコントローラです。エンドポイントは、コンピュータ・アプリケーション、サービス・ルーチン、またはインターネット・ボットである場合もあります。 |

Table 1 - アカウント種別ごとの特性

IAM実務者が区別すべき非人用アカウントには、大きく分けて2つのカテゴリーがあります。

* 対話的に使用されることのないデバイスやサービスによって使用されるアカウント
* 個人に割り当てられていないシステム機能にインタラクティブにアクセスできるアカウント。

## 用語

### Bot

インターネットボットと呼ばれることもあります。「Robot」の略ですが、インターネット上で自動化されたタスクを実行するソフトウェアルーチンや、自律的なネットワークアプリケーションを指すウェブロボット、または特定の目的のために使用される自動化された、通常は反復的なタスクを指す単に「Bot」を指します。

例えば、ユーザのデジタル ID は、ユーザの銀行とは対照的に、職場環境では異なる定義を持つことになる。デバイス識別子は、そのアイデンティティと呼ばれることもあります。

### アイデンティティ

人間としてのユーザーの属性の集合体です。ドメインによって異なります。例えば、ユーザのデジタル ID は、ユーザの銀行とは対照的に、職場環境では異なる定義を持つことになる。デバイス識別子は、そのアイデンティティと呼ばれることもあります。

### CIA トライアド

機密性、完全性、可用性の観点からリソースのリスクを分類する、情報セキュリティの基本的な概念。

### 非個人アカウント

デバイス、サービス、サーバーに使用されるアカウントなど、特に個人に割り当てられていないアカウント。

### サーバーアカウント

サーバーの操作に対する特権的なアクセス権を持つアカウント。

### サービスアカウント

コンピュータアプリケーションが特定の目的のために他のアプリケーションやサービスにアクセスするために使用するアカウント。

### システムアカウント

システムレベルの機能に対する広範な権限を持つ特権アカウントの総称で、通常、新しいアプリケーションのインストール、システム更新の実行、または構成の変更を行うために使用されます。

## Non-person Access Control

IAM実務者にとっての重要な関心事は、人間が対話的に使用しないデバイスやサービスへのアクセス制御をどのように管理するかということです。これには、自動化されたプロセスで使用されることが増えているボットへのアクセス制御も含まれます。

### IoTデバイス

IoTデバイスは、センサまたはアクチュエータのいずれかであることができます。いくつかのケースでは、センサーは、リアルタイムで表示される連続的なデータストリームを提供したり、定期的な分析のためにデータベースに書き込まれる離散的な読み取り値を提供したりします。アクチュエータは、通常、プロセスを制御して何かをオンまたはオフにするために使用されるデバイスです。所望の開口部に到達するまでサーボモータを十分な回数パルスさせることでバルブを開閉するために使用されることがあります。多くの場合、装置は遠隔地に設置されており、コントローラを介して中央の場所にある監視システムに接続されます。

典型的なIoT構成では、3つのゾーンがあります。

* IoTデバイス（センサーとアクチュエーター）です。デバイスへのアクセスおよびデバイスからのアクセスを管理するには、必要なセキュリティレベルに合わせて、通信チャネルの暗号化（DNP3、MQTTなど）および／またはデジタル署名技術（PKIなど）の要件を課すポリシーによって管理する必要があります。低セキュリティ環境では、機器が廃棄されるまで使用される静的パスワードが使用されるかもしれません。機密性の高いアプリケーションでは、セキュリティ資格情報（パスワード、証明書など）は定期的にローテートされる。選択されたセキュリティ要件は、デバイスの能力と一致していなければなりません。IETF RFC 7228 は、3 つのクラスのデバイスを推薦しています。1
  * クラス 1 - 設定可能な認証をサポートする容量がありません。
  * クラス 2 - 鍵管理、トークンサポートなどの容量が限られています。
  * クラス 3 - 完全に構成可能で、動的な認証メカニズムをサポートできる。

* デバイスが接続されている）コントローラ。センサーデバイスのデータが、各センサーまたはアクチュエータをその制御ロジックにマッピングするデバイスコントローラによって集約されている場合、アクチュエータへのアクセス制御と、収集したデータをデータベースに書き込む際の保護を提供する必要があります（下記のサービスアカウントを参照）。

* IoTデバイスを監視または制御するコントローラアプリやSCADAアプリなどのヒューマン・マシン・インターフェース・アプリケーション（HMI）。場合によっては、センサーが直接データベースにデータを書き込み、SCADAアプリや類似のヒューマン・マシン・インターフェース（HMI）などの別のアプリケーションによって読み込まれます。これらのアプリケーションへのアクセスは人間が行うことになり、IDM環境を介して管理する必要があります。

歴史的にIoT環境は、運用技術（OT）を担当するチームによって管理されており、組織内の情報技術（IT）環境とはほとんど関係がありませんでした。IoT技術の専門性がこのような組織構造を正当化してきたため、IT環境を経由した潜在的な危害からOTを隔離することが企業ポリシーとなっていることが多い。しかし、セキュリティ技術の向上に伴い、隔離の要件は減少しています。ほとんどの産業用アプリケーションでは、IoTシステムをIAM環境に統合することで、アクセス制御能力が向上し、運用技術の展開に対する企業のガバナンスが向上します。

規制規制によって許可されている場合、OT環境とIT IAM環境との統合が行われるべきである。これにより、OT システムの権限が IAM システムを介して設定され、OT スタッフが特権アクセス管理システムを介して、潜在的には特権アクセス管理システムを介して、権限付与のために企業のクレデンシャルを使用することができるようになります。

サプライチェーン全体での IoT デバイスやトラッキングデバイスの実績に関する懸念が高まっており、「バックドア」アクセ スを展開する可能性のある変更が行われていないことを確認しています。IAM の実務者は、企業ポリシーが IoT デバイスに採用される認証プロセスを定義していることを確認することをお勧めします。

IoT デバイスのデータを保護することは、デバイス自体へのアクセスを保護することと同じくらい重要である。多くの場合、IoTデバイスを搭載したデータベースは、十分なセキュリティが確保されていません。このようなセキュリティの欠如は、建物環境のデバイスデータであればほとんど気になりませんが、産業スパイから十分に保護されていない工場の生産データであれば、重大な問題になる可能性があります。IAMの専門家は、産業用データストアに適切なアクセス制御が配置されていることを確認する必要があります。データベースの管理は、適切な保証レベルで認証された認可されたデータ管理者を介して行う必要があります。

#### IoTソリューションユースケース

IoT デバイスの管理における IAM 実務者の関与を決定することに関しては、「正解」はない。スペクトルの一端は、すべての IoT 展開と管理が OT 担当者の領域であるというユースケースである。この場合、IAMの関与はOTシステムにアクセスする人間のアカウントに制限されます。IoT システムを構成できるアカウントへの権限のグループ管理は、セキュリティのレベルを高めます。

スペクトルの中間点では、人間のアカウントがIoTデバイスの責任者となります。IAM プロビジョニング・ワークフローは、構成要求、および潜在的にはパスワード・ローテーション要求を責任者にルーティングします。IoTデバイスは、責任者への認証報告と、SOCや場合によってはSIEMに統合されたコンプライアンス管理に参加します。

もう一方の端では、デバイスのプロビジョニングが ID 管理インフラストラクチャに含まれている。IoT デバイスは、デバイスに「デジタル ID」を適用することで、個人と同じように扱われます。デバイスの権限は、アカウントプロビジョニングのための通常のワークフローを介して設定することができ、アクセス制御は同じプロトコルを使用することができます。ゲートウェイを含むほとんどの最新のAPIシステムは、マシン間通信にOAuthを使用しており、Open ID ConnectはIoTデバイスコントローラ認証に適しています。

### サービスアカウント

サービスアカウントには様々な種類があります。これらのアカウントは通常、UNIXのcronジョブやWindowsのタスクスケジューラなどを介して、自動化されたベースで定期的に実行されるプロセスで使用されます。これらのプロセスで使用されるアカウントは、ユーザーが対話的にアクセスしないため、監査人によって見落とされることがよくあります。ユーザーはこれらのアカウントにログインしないので、一般的には、制限された特権を持つ非常に基本的な単一目的のアカウントです。

例としては、以下のようなものがあります。

* データの毎晩のバックアップを実行するために使用されるアカウント
* 監視目的でHVACシステムへのアクセスを提供するアカウント
* ディレクトリインスタンス間でデータを複製するために使用されるアカウント

バッチアカウント」という用語は、サービスアカウントとして使われることもあります。これらは、システム機能を実行するために非生産時間中に定期的に実行される 1 つ以上のユーティリティ操作を指すことが多い。複数のバッチプロセスが単一のサービスアカウントを使用することもあります。

#### サービスアカウントユースケース

サービスアカウントは、多くの場合、静的なパスワードで確立されており、暗号化されていなければ、システム管理者であれば誰でも読むことができるため、多くの組織にとって重要な懸念材料となっています。そのサービスアカウントは、漏洩した場合、悪意のある行為者によって対話的に使用され、組織内の他のサーバへの横移動に使用される可能性があります（例えば、これらのアカウントを使用して他の環境へのステップアップ認証を行う）。サービスアカウントを、異常時の認証監視など、企業のデータ損失保護ツールに含めることで、このような脆弱性を防ぐことができます。より良い方法は、静的なサービスアカウントを、一般的に厳格なセキュリティと監視体制を課しているAPIに移行することです。

注: 「サービスアカウント」(非個人アカウント)という用語は、サービス担当者(HVAC技術者など)が定期的にアクセスするアカウントを説明するために誤って使用されることがあります。このようなアカウントはユーザーアカウントであり、このドキュメントでは、この担当者は会社の外部の人間であることが多く、したがってIAMデータストアには存在しないため、サービス会社のあらゆる人間が使用できるように「汎用アカウント」が確立されることがあるということに注意する以外には、このドキュメントでは対処していません。これはIAMの問題である。現代の組織にはジェネリックアカウントのための場所はなく、使用されるべきではありません。オプションには次のようなものがあります。

* サービス会社との統合認証
* 承認ワークフローによるセルフサービスのパスワード管理
* サービス会社へのMFA機器の発行
* サービス会社のアクセスを管理・監視するAPIの導入

### ボット

ボットという言葉は、反復的なプロセスのためにソフトウェア・ルーチンが配備されるプラント・オートメーションの分野で生まれたロボット・プロセス・オートメーション（RPA）から来ています。ボットは現在、ウェブサイトの利用情報を取得するためのクローラーからサービス妨害マルウェアまで、あらゆる用途で使用されています。ボットは、ビル情報管理データの検索や顧客取引データの統合など、反復的な作業を自動化するために組織で使用されることが増えています。これらの場合、ボットによるアクセスは特定の目的に限定されます。

ボットは通常、インターネットを利用して遠隔地のサービスやリソースにアクセスする。一般に公開されているウェブサイトは、ボットの活動を制限し、悪意のあるアクセスを避けるためのメカニズムを適用する必要があります。このようなメカニズムには、スクリーン・スクレーパー・コントロールの適用、人間による検証チェックの使用、DDOS保護などが含まれるかもしれません。悪意のある活動の一般的な形式は「クレデンシャル・スタッフィング」であり、ログイン認証情報がハッカーによって変更され、セッションを制御することができます。

組織は、ボットの外部利用に備える必要があります。IAMの実践者は、ユーザー行動分析を利用してアクセスの異常を特定することができます。ボットは、プロセスやサービスへの「通常の」非人のアクセスと比較すると、異なる特性を示します。

ボットの外部利用への対応に備えるには、ボットの利用状況を確認するプロセスの確立、導入前のテスト、ボットの利用パターンの分析などが含まれます。ボットの悪意ある破損は常に懸念されているため、監視は継続的な作業である。

#### ボットユースケース

ボット技術を企業に適用する場合、IAM実務者のタスクは、クレデンシャルに関する適切な管理が遵守され、PKI署名と暗号化が適切に使用されていることを確認することです。認可された活動のみが許可されるべきです。

例えば、ウェブサイトのデータにアクセスするボットは、通常、割り当てられたセッション・トークンを使用して HTTPS で認証します。セッショントークンを定期的に失効させることは良い習慣です。トークンの有効期間は、アクセスされるサービスやリソースの機密性に依存します。

## システムアカウントアクセス制御

厳密には非個人アカウントではありませんが、システムアカウントは、割り当てられた個人を持たないため、ここに含まれます。システム・アカウントは通常、システム/サーバのコミッショニング時に確立される管理アカウントを指します。このタイプのアカウントは、単一の個人に直接関連付けられていないため、通常、組織内のジョイナー・ムーバー・リーバー人事プロセスを介して管理されることはありません。システムアカウントは、人間が物理的または仮想的なシステムやサーバーへのアクセス権を与え、特権的なシステム機能への権限を付与します。IAMの実務家は、これらのアカウントの管理に関心を持たなければなりません。

### 管理者またはルートアカウント

Windows、Linux、Unixサーバの管理者またはルートアカウントは、高度な権限を持つアカウントは、それぞれのプラットフォーム上のシステムレベルの操作にアクセスできるようになっています。

* 最高レベルの権限を持っています。
* プラットフォーム上で実行されているすべてのファイルとプロセスにアクセスできます。
* 「root」または「admin」アカウントは、システム操作を設定し、それによってプラットフォームの動作に影響を与える権限を持っています。
* システムからのログは、通常、実行されたコマンドと表示された応答を表示します。
* アカウントの運用上の利用状況を継続的に監視する必要があります。

注：仮想化およびハイパーバイザープラットフォーム（VMware、Citrix、Xen）やコンテナプラットフォーム（Docker、Openshift、DCOS、Kubernetes）には、適切に管理されていない場合に攻撃のベクトルを提供する管理アカウントがあります。

### スーパーユーザーアカウント

「スーパーユーザー」という用語は、標準ユーザーアカウントよりも高い権限を持つビジネス情報システムまたはアプリケーションアカウントに適用されます。これは、システムがデプロイされたときに、システムのコミッショニングプロセスの一部として生成されます。スーパーユーザーアカウントは、構成を変更する権限を持っており、情報システムにおけるミッションクリティカルなアカウントとなっています。

### サーバーアカウント

WindowsやLinuxのオペレーティングシステム環境で実行されるDBMS、ESB、その他のICTコンポーネントのようなミドルウェアプロセスのアカウントは、サーバーアカウントと呼ばれることがあります。これらは、リソースの所有者に管理者アクセスを与えるためのDBMSなどのアプリケーションの特権アカウントです。

### コンシューマーデバイス

インターネットに接続しているコンシューマ機器の脆弱性に関する懸念が高まっています。最近の事件には以下のようなものがあります。

* オーディオまたはビデオキャプチャ機能を持つデバイスによるプライバシー侵害。この例では、機密データが監視機関にフィードバックされています。
* 一般的に公開されている管理者パスワードが使用され、ハッカーに消費者デバイスへのアクセス権が与えられます。その後、マルウェアがこれらのデバイスにインストールされ、DDOS 攻撃に利用される可能性があります。

現在、ほとんどの法域では、製品が適切な基準に準拠することを要求していますが、その中には一般的に以下のようなものが含まれます。

* デフォルトのパスワードを使用しない。すべてのデバイスには、共通のデフォルト設定にリセットできない固有のパスワードが同梱されています。
* ソフトウェアのアップデートを提供すること。脆弱性が検出された場合に容易に更新できない固定ファームウェアを搭載したデバイスを出荷すべきではない。
* すべての認証情報は、暗号化保護および／または信頼できるストレージ機構を用いて安全に保存されるべきである。
* 攻撃面は最小限に抑えるべきである。未使用のポートは閉じ、公開されているサービスは機能的に必要なものだけに制限し、ソフトウェアはシステムの運用に必要な最低レベルの特権で実行するべきである。
* 個人識別情報（PII）をデバイスに保存してはならない。対象地域のプライバシー規制を遵守する。

### システムアカウントの特性

システムアカウントは単一のアイデンティティに割り当てられていないので、IAMソリューションで完全に管理することはできません。例えば、管理者権限を持つ人が組織を離れると、そのようなアカウントが削除されるのは適切ではありません。一般的な慣行は、管理されたグループを介して特権アカウントへのアクセスを提供することであり、例えば、グループ内の任意のユーザーにアカウントへのアクセスが許可される。しかし、IAM環境外での管理はやはり必要です。グッドプラクティスには次のようなものがあります。

* サーバー/サービスが属するIDの属性として登録されている構成管理データベースを使用する。
* アカウントの使用について説明責任を負うアカウント所有者を割り当てる。システム所有者が定義されていない場合は、IT 部門の責任者が説明責任者となるべきである。
* インタラクティブアカウントは、インフラストラクチャの変更や災害時にのみ使用されるべきです。管理者権限は、ユーザーのアカウントを介して、例えば、適切な管理者グループのメンバーシップを介して付与されるべきです。
* 管理者/ルートアカウントのパスワードは厳密に管理する必要があります。これらのパスワードは、手動の手順、パスワード保管庫、または特権アカウント管理システムを介して保護することができます。

#### システムアカウントソリューションのユースケース

IAM実務者は、システムアカウントへのアクセスを保護するために支援すべきである。UNIX 環境では、「etc/passwd」ファイルの削除や、特権昇格のための SUDO の使用によって、これが可能になるかもしれません。Windows 環境では、特権アクセス管理 (PAM) システムが一般的な解決策です。この場合、システムパスワードは特別に複雑なものにして、必要に応じて回転させます。このようなアカウントへのアクセスは、PAM システムを介して行われ、適切な権限を持つ特定の個人へのアクセスを制限し、すべてのアクセスイベントをログに記録します。

PAM を使用しない場合は、管理者への通知を伴うアカウント権限の時間制限付き昇格が Windows 環境でサポートされています。システムとサーバのアカウントが適切に使用され、管理されていることを確認するために手動で介入することも、企業監査にサーバのアカウントを含めることと同様に、良い実践となります。このような介入を行うには、サーバーアカウントのために企業ポリシーを確立する必要があり、アカウント管理慣行の可視性を高める必要があります。

アプリケーションがクラウドサービス上に展開されるケースが増えており、各展開に合わせたアクセス制御環境が必要とされています。このようなデプロイメント固有のアクセス制御モデルは、マスターアカウントの特権を保護するためにリソースマネージャを構成したり、アプリケーションがマスターアカウントをデータベースへのアクセスに使用しないようにポリシーを設定することを意味する場合があります。

## 今後
IoTデバイスのユビキタス化が進む。デバイスは企業とコンシューマの世界の両方にまたがることになり、IoTデバイスとデータフローの統合は新たな企業リスクとなるでしょう。機械学習や人工知能を活用した自動化の導入が進み、アクセス制御環境の複雑さが増していくでしょう。APIゲートウェイ、データベースゲートウェイ、サービスメッシュ、ポリシーベースのアクセス制御ソリューションの使用を介したIAM環境との統合を検討する必要があります。

APIは、マシン・ツー・マシン（M2M）通信に使用されることが増えています。APIは、通信チャネルに一貫したセキュリティ制御を適用し、管理目的で監視する機能を提供します。ゲートウェイ・アプローチを採用している企業は、各サービス・インスタンスが個別に展開されている場合には事実上不可能な、M2M通信全体での一貫性を提供することができます。

クラウドサービスの採用が加速し続ける中、マイクロサービスやコンテナ化の利用が普及していくでしょう。IAMの実務者は、アイデンティティデータを通信するサービス間の通信を保護するために、適切な情報セキュリティソリューションを導入する必要があります。

また、ボットの使用は今後も加速し、行動分析とゲートウェイ技術の導入を検討する必要があります。米国国土安全保障省2は次のようにアドバイスしています。

* 極悪なボット開発者は、新しいIoTデバイスが市場にリリースされると、脆弱性を狙って、マルウェアを展開するために互いに競い合うようになるだろう。
* ボットのコードサイズは、検出を回避するために小さくなり、防御を挫くために巧妙になる。
* ボットネットは拡張され、ソーシャルメディアプラットフォームへのインターフェースを介して収益化されるようになるでしょう。
* ボットネット運営者は、地域の脆弱性を利用してグローバルに活動するようになるでしょう。外国の国民国家からの攻撃が増加する。

人ではないエンティティへのアクセス制御は、リスクを回避する組織にとって重要な能力です。デバイスやボットが適切に本人確認を行い、一貫したセキュリティと監視制御が可能なAPIへの移行、行動分析ツールなどのデータ損失防止技術の導入がますます重要になっています。

## 結論
あまりにも多くの場合、IAM の実務者は、非個人アカウント管理から隔離され、ユーザーアカウントに関連するプロビジョニングとアクセス制御にのみ焦点を当てています。このような限定的なアプローチは、サイバーセキュリティに対するホスト組織のリスク管理アプローチを断片化し、ガバナ ンス業務を挫折させてしまうので、残念なことです。少なくとも、IAM実務者は、IoTデバイスがどのように保護されているか、サーバ・アカウントがどのように管理されているか、悪意のあるボットを阻止するためにどのような防御策が講じられているかについて、適切な質問をする必要があります。組織内の IAM チームと InfoSec チームは、企業ポリシーに沿ったサイバーセキュリティ対策の一貫した適用を確保するために協力する必要があります。