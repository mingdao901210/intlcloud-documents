## 概要

CDN加速は、COSバケット内のコンテンツのダウンロード、配布に適し、特に同じコンテンツの繰り返しダウンロードのシナリオに適します。

## 設定説明

ユーザーは、以下のドメイン名を管理し、バケット内のオブジェクトの迅速なダウンロードおよび配布を実現できます。
- デフォルトドメイン名：すなわち、COSオリジンサーバードメイン名であり、バケットを作成する時に、バケット名および地域に基づいてシステムによって自動的に生成され、デフォルトの加速ドメイン名と区別する必要があります。
- デフォルトの加速ドメイン名：CDN加速ノードを経由するドメイン名であり、システムによってデフォルトで生成され、ユーザーは有効化または無効化のいずれかを選択できます。
- カスタムドメイン名：ユーザーはバケットに対して登録されたカスタムドメイン名をTencent Cloudの中国内のCDN加速プラットフォームにバインディングして、カスタムメイン名を介してバケット内のオブジェクトにアクセスできます。

デフォルトの加速ドメイン名またはカスタムドメイン名に対してCDN加速を有効化した場合、オリジンサーバーがパブリック読み取りバケットであれば、システムにおいて、デフォルトでは、ユーザーはCDN加速ドメイン名またはカスタムドメイン名を介してオリジンサーバー内のオブジェクトに直接アクセスできます。オリジンサーバーがプライベート読み取りバケットであれば、ユーザーがCDNプル認証とCDN認証構成という2つのオプションを有効化することをお勧めします。

- プル認証（有効化の前提はCDNサービス権限が追加されたこと）：ユーザーによってリクエストされたデータがエッジノードでキャッシュにヒットしない場合、CDNはデータコンテンツを取得するためにプルされる必要があります。COSをオリジンサーバーとしてプル認証を有効化した後、CDNエッジノードは、特別なサービス身元（CDNサービス認証によってこの身元を取得する必要がある）を使用してCOSオリジンサーバーにアクセスすることで、プライベートアクセスバケット内のデータの取得およびキャッシュを実現します。
- CDN認証構成：ユーザーがエッジノードにアクセスすることでキャッシュデータを取得する際、エッジノードは認証構成ルールにより、アクセスするURLにおける身分認証フォールドをチェックすることで、未認証のアクセスを未然に防止し、ホットリンク保護を実現し、エッジノードのキャッシュデータの安全性と信頼性を向上させます。

CDN認証構成とCDNプル認証の使用は競合しませんが、両者の構成状態によってデータに対する保護効果は異なり、具体的な状況は以下のテーブルに示すとおりです：


| バケットアクセス権限      | CDNプル認証の有効化 | CDN認証構成の有効化 | CDN加速ドメイン名を介してオリジンサーバーにアクセス可能か  |COSオリジンサーバードメイン名を介してオリジンサーバーにアクセス可能か   | 適用シナリオ         |
| ------------------- | ------------ | ------------ | --------------- | --------------- | ------------ |
| パブリック読み取り              | 無効化         | 無効化         | アクセス可能          | アクセス可能          | 全サーバーパブリック読み取り |
| パブリック読み取り              | オフ         | オン         | URL認証が必要です | アクセス可能          | 推奨しません       |
| パブリック読み取り              | 有効化         | 無効化         | アクセス不可        | アクセス可能          | 推奨しない       |
| パブリック読み取り              | オン         | オン         | URL認証が必要です | アクセス可能          | 推奨しません       |
| プライベート読み取り+CDNサービス認証 | オン         | オン         | URL認証が必要です | COS認証が必要です | 完全リンクを保護します |
| プライベート読み取り+CDNサービス認証 | 無効化         | 有効化         | URL認証を使用する必要がある | COS認証を使用する必要がある | 推奨しない       |
| プライベート読み取り+CDNサービス認証 | 有効化         | 無効化         | アクセス可能          | COS認証を使用する必要がある |オリジンサーバー保護     |
| プライベート読み取り+CDNサービス認証 | オフ         | オフ         | アクセス不可        | COS認証が必要です | 推奨しません       |
| プライベート読み取り              |      無効化        |      有効化または無効化        | アクセス不可        | COS認証を使用する必要がある | CDNを使用できない |

>!
>- 第1行を例とし、オリジンサーバーバケットのアクセス権限がパブリック読み取りである場合、CDNプル認証もCDN認証構成も有効化しないと、CDNドメイン名を介してCDNエッジノードとオリジンサーバーバケットに直接アクセスでき、COSドメイン名を介してオリジンサーバーバケットに直接アクセスできます。
>- ユーザーがドメイン名に対してCDN加速を有効化した後、誰でもこのドメイン名を介してオリジンサーバーに直接アクセスできるため、データを非公開に保持するには、ぜび**認証構成**を介してオリジンサーバーのデータを保護してください。

## デフォルトの加速ドメイン名の有効化
### 操作手順
1. バケット詳細画面の上方の【ドメイン管理】をクリックし、【編集】をクリックし、デフォルトの加速ドメイン名の現在のステータスを有効に設定します。通常、オリジンサーバーのタイプは、デフォルトで**デフォルトオリジンサーバー**であり、オリジンサーバーとするバケットが静的Webサイトを有効化し、静的Webサイトを加速しようとする場合、**静的Webサイトオリジンサーバー**を選択し、以下の図に示すとおりです：
![](https://main.qcloudimg.com/raw/c2a680b936c033d0ad00b058c7112bb1.png)
>!ユーザーがTencent Cloud CDNサービスを使用したことがない場合、【ドメイン名管理】に入ることはできず、先にCDNコンソールに入ってCDNサービスを有効化する必要があります。
2. バケットがパブリック読み取りである場合、**プル認証**を有効化する必要がなく、最後に【保存】ボタンをクリックしてCDN加速を有効化できます。
>!プライベート読み取りバケットに対して、プル認証とCDNサービス権限を同時に有効化すれば、CDNを介してオリジンサーバーにアクセスする時に署名を運ぶ必要がなくなり、CDNキャッシュリソースがパブリックネットワーク上に配布され、データのセキュリティが影響されるため、CDN認証を有効化することをお勧めします。
3. プル認証を有効化します。
 1. バケットがプライベート読み取りである場合、**プル認証**を（CDNサービス認証が追加された前提で）有効化する必要があります。[COSコンソール](https://console.cloud.tencent.com/cos5)にログインし、左側のメニューバー【バケットリスト】に入り、ドメイン名を構成する必要があるバケットをクリックし、バケット構成画面に入り、以下の図に示すとおりです：
![](https://main.qcloudimg.com/raw/b90ad17947a0ec530db87210f4b9027d.png)
 2. バケット詳細画面の上方の【ドメイン名管理】をクリックし、【編集】をクリックし、【プル認証】の有効化を選択し（CDNサービス認証が追加されたことを確認する必要あり）、【保存】をクリックしてCDN加速を有効化でき、以下の図に示すとおりです：
![](https://main.qcloudimg.com/raw/439f408aae267d3052758a1fdaa93743.png)
 3. 【保存】をクリックすると、デフォルトの加速ドメイン名が構成されていることがわかります。同時に、下部に**CDN認証**のステータスプロンプトが表示され、【認証構成】をクリックしてCDN認証の構成を開始し、以下の図に示すとおりです：
![](https://main.qcloudimg.com/raw/8ed30acab9085f97f052e9eda83e6740.png)
 4. ジャンプ画面で、「認証構成」ボタンをオンにし、認証Keyと有効期間を入力し、以下の図に示すとおりです：
![](https://main.qcloudimg.com/raw/7b8f499321fbe7a61e304397a945215f.png)
 5. 【認証計算機】をクリックし、前のステップで構成した場合、認証Keyと有効期間が自動的に入力され、通常、アクセスする必要があるオブジェクトのPathを入力するだけでよく、その後、【作成】をクリックして認証URLを取得し、認証URLを使用してターゲットオブジェクトに直接アクセスできます。この前入力していない場合、認証Key、有効時間およびターゲットパスを認証計算機に入力する必要があり、以下の図に示すとおりです：
![認証計算機](https://main.qcloudimg.com/raw/572b32410086d49cbfc00a650eb6f514.png)

## カスタム加速ドメイン名の有効化

>!
>- ここでは、COSコンソールによるカスタムドメイン名の追加およびCDN加速の有効化のみを紹介し、COSコンソールからカスタムドメイン名を追加する必要がある場合、CDNの[ドメイン名の追加](https://cloud.tencent.com/document/product/228/5734)ドキュメントを参照してください。
>- COSコンソールから追加されるカスタムドメイン名の上限は10個です。

### 操作手順

1. [COSコンソール](https://console.cloud.tencent.com/cos5)にログインし、左側のメニューバー【バケットリスト】に入り、ドメイン名を構成する必要があるバケットをクリックし、バケット構成画面に入り、以下の図に示すとおりです：
![](https://main.qcloudimg.com/raw/b90ad17947a0ec530db87210f4b9027d.png)
2. バケットページの上方から【ドメイン名管理】に入り、「カスタム加速ドメイン名」で【ドメイン名の追加】をクリックし、バインディングされるカスタムドメイン名（例えば、`www.example.com`）を入力し、プル認証の有効化を選択し、右側の【保存】をクリックしてドメイン名の追加を完了でき、以下の図に示すとおりです：
![](https://main.qcloudimg.com/raw/0bfbd7e43e19f9bf8e0d42e2957fc1b8.png)
>!プライベート読み取りバケットに対して、プル認証とCDNサービス権限を同時に有効化すれば、CDNを介してオリジンサーバーにアクセスする時に署名を運ぶ必要がなくなり、CDNキャッシュリソースがパブリックネットワーク上に配布され、データのセキュリティが影響されるため、CDN認証を有効化し、ステップ3に進むことをお勧めします。
3. カスタムドメイン名を保存した後、CDN認証機能有効化ボタンがCDN認証バーに表示され、カスタムドメイン名のCDN認証を有効化できます。
4. [CDNコンソール](https://console.cloud.tencent.com/cdn/access)にログインし、左側のメニューバー【ドメイン名管理】に入り、構成する必要があるドメイン名の【管理】をクリックし、【セキュリティ構成】を選択します。認証Keyと有効期間を入力し、設定が完了した後、【OK】をクリックし、以下の図に示すとおりです：
![詳細な認証構成](https://main.qcloudimg.com/raw/61310b0c5960b4846a946bbacbc9fd00.png)
5. 【認証計算機】をクリックし、前のステップで構成した場合、認証Keyと有効期間が自動的に入力され、通常、アクセスする必要があるオブジェクトのPathを入力するだけでよく、その後、【作成】をクリックして認証URLを取得し、認証URLを使用してターゲットオブジェクトに直接アクセスできます。この前入力していない場合、認証Key、有効時間およびターゲットパスを認証計算機に入力する必要があり、以下の図に示すとおりです：
![認証計算機](https://main.qcloudimg.com/raw/ad4a7703e469269bbf299e19869d00d6.png)

## 注意事項
ユーザーがドメイン名に対してCDN加速を有効化した後、誰でもこのドメイン名を介してオリジンサーバーに直接アクセスできるため、データを非公開に保持するには、ぜび**認証構成**を介してオリジンサーバーのデータを保護してください。
