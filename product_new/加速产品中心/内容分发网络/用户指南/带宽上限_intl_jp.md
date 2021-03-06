ドメイン名に対し帯域幅上限閾値を構成することができます。1つの統計期間（5分間）に発生したドメイン名の帯域幅が指定した閾値を越えた時に、お客様の構成によりすべてのアクセスをオリジンサーバーに戻すか、もしくは直接にCDNサービスを無効にした上ですべてのアクセスに対し404を戻します。

## 構成ガイド
1. [CDNコンソール](https://console.cloud.tencent.com/cdn)にログインし、左側のディレクトリの【ドメイン名管理】をクリックして、管理ページに入ります。リストで編集するドメイン名の所在する行を特定し、操作欄の【管理】をクリックします。
![](https://mc.qcloudimg.com/static/img/f92d2ef7e4be2b69185ab43228f025ef/1.png)
2. 【アドバンスト構成】タブをクリックすると、**帯域幅上限構成**モジュールが表示されます。デフォルトでは、帯域幅上限の構成は非アクティブ状態です。
![](https://mc.qcloudimg.com/static/img/0a53afbcf5f33dd59c2ec4e9cc7824c4/2.png)
3. 「帯域幅閾値及び超過処理」が空である場合、アクティブボタンをクリックします。
![](https://mc.qcloudimg.com/static/img/d534483a485993d979cbab4bec715d54/3.png)
アクティブにすると、構成ウィンドーが表示されます。デフォルトでは帯域幅閾値を**10Gbps**とすることをお勧めします。閾値に達すると、**アクセスに404を戻す**ことになります。
![](https://mc.qcloudimg.com/static/img/771ee9889b358b9b3cfe0a22f5db0f36/4.png)
構成した後に、帯域幅上限はアクティブ状態を保持し、関連する構成情報が表示されます。
表示される構成：帯域幅の閾値が**10Gbps**であり、閾値に達すると、**アクセスに404を戻す**ことになります。【編集】をクリックすると、帯域幅閾値、及び閾値超過後のユーザーリクエストに対する処理方法を構成することができます。
4. 構成していた場合は、**帯域幅閾値及び超過処理**のところに対応する構成情報が表示されます。手動により帯域幅上限をアクティブにすればよいです。
> ?
>- 強力なDDoS攻撃に対処することを目的とする場合、オリジンサーバーを守るためには、「アクセスに対し404を戻す」に構成することをお勧めします。
>- CDN費用を抑えることを目的とする場合、サービスを影響しないように、「アクセスをオリジンプルする」に構成することをお勧めします。
5. 上限閾値を超過すると、内部メッセージ、メール、ショートメッセージでお客様に通知しますので、CDNコンソールでドメイン名の状態を照会できます。「アクセスをオリジンプルする」にしても「アクセスに対し404を戻す」にしても、ドメイン名は非アクティブ状態になっています。「アクセスをオリジンプルする」 / 「アクセスに対し404を戻す」の実行の有効時間が5 ~15分間です。
> ?
> 帯域幅上限に達したことによりドメイン名 が非アクティブになった後に、引き続きCDNサービスを使いたい場合は、[CDN コンソール](https://console.cloud.tencent.com/cdn) にドメイン名加速を再開することができます。具体的な操作は [ドメイン名の操作](https://cloud.tencent.com/doc/product/228/5736)を参考してください。

## 構成の例
ドメイン名`www.test.com`が下記構成になっている場合は、
CDN は当該ドメイン名の最近の帯域幅統計・監視データを固定周波数でテストします。12:15:00の時点に12:05:00 時点（12:05:00 - 12:10:00の間に発生したデータを代表する）の帯域幅値が1Kbps以上になり上限を超過したことを発見した場合は、すぐに構成をデリバーし、リクエストをオリジンプルさせます。ネットワーク全体におけるCDNノードの構成は一括にデリバーされ、徐々に発効するので、帯域幅はだんだん下がり、12:20:00頃にネットワーク全体において発効します。
