## オートスケールアウトのCVMにサービスのスタートアップ起動を設定する
### ユースケース
auto scaling でスケールアウトする場合、プロセス全体で手動による介入がないのは望ましいです。従って、オートスケールアウトのCVMに対して、起動後にサービスの自動起動を設定するのを強くお勧めします。例えば：

- **httpd** サービス
- **mysqld** サービス
- **php-fpm** サービス
- **tomcat** サービス
- など

設定はただ一分で完了できます——ファイル /etc/rc.d/rc.local を修正します！

### 設定方法（ centos を例に）：


#### step 1： ファイルrc.localを開く
入力します

    vim /etc/rc.d/rc.local

既存の設定を変更せず、ファイルの最後に設定を追加します。

>>操作 TIPS **（上級ユーザーはスキップできます）：**
>
>「i」を入力して、vim の insert モードに入り、設定を入力できます。この時、方向キー「↓」を押せば、ファイルの最後に移動できます。


#### step 2： 起動するサービスを記入する

この例の目的は構築したウェブサイトが起動する時に自動でhttpd、mysqld、php-fpmサービスを起動します。 従って、rc.local の最後に下記の内容を追加しました：

    service httpd start
    service mysqld start
    service php-fpm start

![](https://mc.qcloudimg.com/static/img/db828b166419cd933e13573c8838a6aa/image.jpg)

保存して終了します。その後、serverが起動した後、ウェブサイトが自動でアクセス可能になります。注意必要なのは、ウェブサイトによって、必要なサービスが異なるので、このステップは必要に応じて設定すれば良いです。


>操作 TIPS **（上級ユーザーはスキップできます）：**
>
>設定を入力した後、escキーを押し、続いて**shiftキー + zキー（2回）**を押して終了できます。つまり、ZZを入力します。


step 3:検証（オプション）
サーバを再起動します（rebootを入力して再起動、またはコンソールで再起動します）。サーバーを再起動した後、サーバーに入らなく、直接ウェブサイトの画面をリフレッシュして、レスポンスがあるかどうかを確認します。レスポンスがあれば、設定成功です。

#### step 4：このCVMを元にイメージを作成します。起動設定を作成する時にこのイメージを利用します。
この手順は簡単です、操作で問題がある場合は、以下のチュートリアルをご参照てください：

[カスタマイズイメージの作成](https://cloud.tencent.com/doc/product/213/%E9%95%9C%E5%83%8F%E6%93%8D%E4%BD%9C%E6%8C%87%E5%8D%97#1.-cvmインスタンスはカスタマイズイメージを作成します)

[起動設定の作成](https://intl.cloud.tencent.com/document/product/377/8544)


