## 概要

Tencent Cloudゲームマルチメディアエンジン（GME）SDKへようこそ。開発者がTencent Cloud GME製品のAPIを容易にデバッグして導入するために、ここではGME範囲ボイスの導入技術文書を紹介します。


GME範囲ボイスは、PUBGのようなゲームのために開発されたカスタム製品です。通常のチームボイスルームと区別するコア機能は以下の通りです。

#### 1. PUBGゲームの特有の「チームのみ」または「全員」のボイスモードを提供します。

#### 2. 範囲判断能力に基づき、同一のボイスルーム内で、多くのユーザーが同時にマイクをオンにして音声通話をすることをサポートします。



## 基本コンセプト

### 1. ルームに参加するときに指定されたTeamID != 0の場合、範囲ボイスルームモードに切り替えます。

### 2. 範囲ボイスルームに入ったとき、2つのボイスモードが利用可能です。

| ボイスモード | パラメータ名               | 機能                                         |
| -------- | ---------------------- | -------------------------------------------- |
| 全員   | RANGE_AUDIO_MODE_WORLD | 設定後プレーヤー付近の一定範囲内の者はこのプレーヤーの発言を聞こえます |
| チームのみ   | RANGE_AUDIO_MODE_TEAM  | チームメードだけが聞こえます                               |

### 3. ボイスモードによって、声音の到達状況は以下の通りです。

  #### 仮にプレーヤーAの状態は「全員」であれば、プレーヤーBが違うボイスモードでの声音到達状況は以下の通りです。

  |同一チームであるか	|範囲内にあるか	|ボイスモード	|AがBの声を聞こえるか	|BがAの声を聞こえるか	|
  | -----------------	| ------------ | ------------ |--------------------------	|--------------------------	|
  |同一チーム		|はい		 	|MODE_WORLD	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |同一チーム		|はい		 	|MODE_TEAM	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |同一チーム		|いいえ		 	|MODE_WORLD	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |同一チーム		|いいえ		 	|MODE_TEAM	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |違うチーム		|はい		 	|MODE_WORLD	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |違うチーム		|はい			|MODE_TEAM	|AがBの声を聞こえない	|BがAの声を聞こえない	|
  |違うチーム		|いいえ		 	|MODE_WORLD	|AがBの声を聞こえない	|BがAの声を聞こえない	|
  |違うチーム		|いいえ		 	|MODE_TEAM	|AがBの声を聞こえない	|BがAの声を聞こえない	|

  #### 仮にプレーヤーAが「チームのみ」であれば、プレーヤーBが違うボイスモードでの声音到達状況は以下の通りです。

  |同一チームであるか	|範囲内にあるか	|ボイス状態	|AがBの声を聞こえるか	|BがAの声を聞こえるか	|
  | -----------------	| ------------ | ------------ |--------------------------	|--------------------------	|
  |同一チーム		|はい		 	|MODE_WORLD	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |同一チーム		|はい		 	|MODE_TEAM	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |同一チーム		|いいえ		 	|MODE_WORLD	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |同一チーム		|いいえ		 	|MODE_TEAM	|AがBの声を聞こえる		|BがAの声を聞こえる		|
  |違うチーム		|はい		 	|MODE_WORLD	|AがBの声を聞こえない	|BがAの声を聞こえない	|
  |違うチーム		|はい			|MODE_TEAM	|AがBの声を聞こえない	|BがAの声を聞こえない	|
  |違うチーム		|いいえ		 	|MODE_WORLD	|AがBの声を聞こえない	|BがAの声を聞こえない	|
  |違うチーム		|いいえ		 	|MODE_TEAM	|AがBの声を聞こえない	|BがAの声を聞こえない	|


### 4. ボイス距離範囲の補足説明：
  - ボイス距離範囲内にあるかは同一チームメンバー間の通話に影響しません。
  - ボイス受信距離範囲の設定はAPI：UpdateAudioRecvRangeを参照します
  - 範囲ボイスルームでリアルタイムボイスモードの切替をサポートしますが、ルーム内でTeamIDを切替えることはできません。TeamIDはルームに参加する前に指定する必要があります。




## 使用手順

通常のチームボイスルームと違って、範囲ボイス機能を使うとき、EnterRoomの前に以下の2つのAPIを呼び出す必要があります。

### 1. TeamIDの設定

- このメソッドによりチーム番号を設定するには、EnterRoomの前に呼び出しを行う必要があります。さもなければ、エラーコードAV_ERR_ROOM_NOT_EXITED(1202)がそのまま返されます

- ルーム退出のとき、このパラメータは0に自動リセットされることがありませんので、このボイスモードの呼び出しを決めると、毎回EnterRoomの前にこのメソッドを呼び出してTeamIDを設定してください

#### 関数プロトタイプ
```
ITMGContext SetRangeAudioTeamID(int teamID)
```

|パラメータ    | タイプ         |意味|
| ------------- |:-------------:|-------------
| teamID		|int    		| チーム番号であり、範囲ボイスで上りリンクと下りリンクのオーディオストリーム制御に使用されます。TeamIDが0の場合、通話モードは一般チームボイスで、デフォルト値は0です。

### 2. AudioModeの設定

- このメソッドによりボイスモードを変更するには、ルーム参加の前にも、ルーム参加の後にも呼び出しができます。

- ルーム参加の前にこのメソッドを呼び出すと、当メソッドは次のルーム参加に影響します

- ルーム参加の後にこのメソッドを呼び出すと、現在のユーザーのボイスモードを直接変更します。

- ルーム退出のとき、このパラメータはMODE_WORLDに自動リセットされることがありませんので、このメソッドの呼び出しを決めると、毎回EnterRoomの前にこのメソッドを呼び出してaudioModeを設定してください


関数プロトタイプ

  
```
ITMGRoom int SetRangeAudioMode(RANGE_AUDIO_MODE rangeAudioMode)
```

|パラメータ    | タイプ         |意味|
| ------------- |:-------------:|-------------|
| rangeAudioMode    |int     |0(MODE_WORLD)が「全員」を表し、1(MODE_TEAM)が「チームのみ」を表す|


### 3. ボイス受信距離範囲の設定

- このメソッドは受信したボイスの範囲（距離はゲームエンジンによる）を設定するために使用され、ルームに正常に参加した後に限り呼び出し可能です
  
- このメソッドは、音源方位をアップデートするUpdateSelfPositionに合わせて使用する必要があります

#### 関数プロトタイプ 

```
ITMGRoom int UpdateAudioRecvRange(int range)
```

|パラメータ    | タイプ         |意味|
| ------------- |-------------|-------------|
| range    |int         | オーディオ受信の最大許容範囲				|

#### サンプルコード  

```
ITMGContext.GetInstance().GetRoom().UpdateAudioRecvRange(300);
```

### 4. 音源方位のアップデート

- この関数は音源位置情報をアップデートします。ルームに正常に参加した後に限り呼び出し可能です

- この製品形態では、位置のみが必要です。音声の方向を指定する必要がありません。

- 音源とオーディエンスの間の距離を判定する場合、音源のSelfPostion、オーディエンスのSelfPostionとAudioRecvRangeを合わせて判定します。


#### 関数プロトタイプ

```
public abstract int UpdateSelfPosition(int position[3], float axisForward[3], float axisRight[3], float axisUp[3])
```

|パラメータ    | タイプ         |意味|
| ------------- |-------------|-------------|
| position   	|int[]		|ワールド座標系中の座標で、前、右、上の順序で表示|
| axisForward   |float[]  	|本製品で適用対象外です|
| axisRight    	|float[]  	|本製品で適用対象外です|
| axisUp    	|float[]  	|本製品で適用対象外です|

