# 画面の確認と事前準備
この Lab では、Control Plane のダッシュボード画面について確認します。  
また、事前準備としてサンプルのエージェントを作成します。  


## 事前準備
UI が英語になっている場合は、日本語に変更します。  
既に日本語になっている場合は、[ダッシュボードについて](#_3) セクションへ進んでください。  

1\. 画面右上にある自身のイニシャルアイコンをクリックし、**Setting** を選択します。  
   ![alt text](images/事前準備_GoSetting.png)

2\. **Platform Language** のタブを選択します。  
   **Add a language** のプルダウンで「Japanese / 日本語」を選択し、**Defaulf languages for all users** のプルダウンでも「Japanese / 日本語」を選択して **Save** を押下します。  
   ![alt text](images/事前準備_SelectLanguage.png)  
   "You're changing the default language for your users" というメッセージが出るので、**Confirm** を選択します。

3\. 設定が完了すると、上部のバーの **Engligh** と記載されている部分がプルダウンで選べるようになっているので、「Japanese / 日本語」を選択して **Apply** を押下します。  
   ![alt text](images/事前準備_SetJapanese.png)


## ダッシュボードについて
画面左上の **IBM watsonx Orchestrate** ロゴ、もしくは左上のハンバーガーメニューから **ホーム** をクリックすると、Control Plane のダッシュボードが表示されます。  
![alt text](images/事前準備_Dashboard-04.png) 

① エージェント作成メニュー：新しいエージェントを作成するウィンドウが開きます。 <br>
② カタログ：事前構築済みのエージェント（テンプレート）やツール（コネクター）等の一覧メニューに遷移します。 <br>
③ 概要：メッセージやフィードバック、エージェントの使用状況・運用動向、アラートを確認できます。 <br>
④ 利用状況：エージェントの導入状況、リーチ、およびエンゲージメントを追跡する。 <br>
⑤ 費用とFinOps： LLMのトークン消費量とコストに関する指標を監視する。 <br>
⑥ 品質：レスポンスの品質とツールの稼働状況を評価する。 <br>
⑦ 信頼性：実行時の障害やレイテンシの傾向を特定する。 <br>
⑧ セキュリティーとリスク：ガバナンスの適用範囲と制御の紐付けを確認する。<br>
⑨ Control Plane エージェント：このテナントで管理しているエージェントやアセットについて分析や洞察を質問することができる管理者用のエージェントです。<br>
![alt text](images/事前準備_Dashboard-Agent-02.png)  


## サンプルエージェントの作成
Control Plane で実際のデータを確認するために、サンプルのエージェントを作成します。

!!! note
    - このセクションでは、Agent Builder の基本的な説明や使い方は省略しています。初めての方は、まず <a href="https://ibm.github.io/ba-handson-jp/wxoagent/">こちらの基礎ハンズオン</a> を参照ください。

1\. 左上のメニューを開き、**エージェント** を選択し、**エージェントの作成** ボタンを押下します。  
&emsp;「最初から作成」を選んで、任意の名称と説明を付与したエージェントを作成してください。  
   ![alt text](images/事前準備_CreateAgent-02.png)  

2\. 以下2つの OpenAPI ツールをダウンロードし、エージェントにインポートしてください  
&emsp;① [Nominatim Geocoding ツール](files/nominatim_geocode.yaml)  
&emsp;&emsp;▶ OpenStreetMap の Nominatim を使って、地名・住所・施設名などのテキストから緯度経度（WGS84）を検索するツールです。  
&emsp;② [OpenStreetMap Overpass ツール](files/overpass_map_search.yaml)  
&emsp;&emsp;▶ OpenStreetMap の Overpass API を使って、指定した場所の周辺にある施設・地物を検索するツールです。  
   ![alt text](images/事前準備_AddTool-04.png)  
   ![alt text](images/事前準備_AddTool-05.png)  
   ![alt text](images/事前準備_AddTool-03.png)  

3\. 右側のプレビューチャットより、エージェントとのテスト会話をします。  
!!! note
    - 特定の地名から緯度経度を取得し、緯度経度の情報周辺の地図情報を取得できるようになったので、以下のような質問ができるようになっています。
    ```
    虎ノ門ヒルズ駅付近のカフェ情報を教えて
    ```
    - エージェントの回答から **理由の表示** を開くと、エージェントが2つのツールの機能を理解し適切に使い分けていることが分かります。
